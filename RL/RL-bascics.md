



[核心算法及其实现 — Spinning Up 文档](https://spinningup.readthedocs.io/zh_CN/latest/user/algorithms.html)

https://huggingface.co/learn/deep-rl-course/unit4/hands-on



# policy gradient

| 类别            | 代表算法       | 是否用RL梯度 | 特点              |
| --------------- | -------------- | ------------ | ----------------- |
| Policy Gradient | PPO, REINFORCE | ✅            | 最常见，用于 RLHF |



### ppo

依赖 **reward model + value model**

需要 **rollout**

1. 用当前 policy rollout（on-policy）
2. Reward model 打分
3. Critic 估计 value → 算 advantage.  
4. PPO clip / KL 约束更新
5. 重复

为什么没人说 PPO 就是一个 Trick， 核心贡献不过是 clip 了一下？ - ChenShawn的回答 - 知乎
https://www.zhihu.com/question/1997333117237736115/answer/1997679386053341605



v 是作为基线, gt 本身方差会太大, 所以要减去 v. 

通过广义优势估计（GAE）来计算每个动作的“优势”，能够直接优化累积奖励。



# Preference-based RL

### DPO (Direct Preference Optimization)

- **思路**：**不显式建 reward、不采样 rollouts、不做 advantage**， 只通过最大化 “preferred response 比 rejected response 概率更高” 来隐式等价优化策略。
- **公式近似等价于**：优化 KL-regularized RL objective，但无需 rollouts。
- **优点**：稳定、无需 RL 框架。
- **代表论文**：
  - *“Direct Preference Optimization: Your Language Model is Secretly a Reward Model” (2023)*
  - 后续：GRPO 



### GRPO 

重要性采样的本质是：我们希望在新的分布下计算期望，但数据却来自旧分布。为此，我们使用新旧策略在同一动作上的概率比作为修正权重.

这样就可以利用离线数据（来自旧策略）来评估新策略的期望，避免每次更新都重新采样，从而降低成本。然而，如果新旧策略差异过大，权重的方差会非常高，容易导致训练不稳定。

```
# 标准 PPO clipping
ratio = pi_new(token) / pi_old(token)  # 新旧策略的概率比. pi_old(token)：更新前的策略，对这个 token 的输出概率
pi_new(token)：更新后的策略，对这个 token 的输出概率. 
直觉：ratio 衡量"策略变化了多少"
ratio = 1 → 策略没变
ratio = 2 → 新策略认为这个 token 概率翻倍了

clipped_ratio = torch.clamp(ratio, 1 - eps, 1 + eps)  # 通常 eps=0.2

loss = -min(ratio * advantage, clipped_ratio * advantage)
```



GRPO 的思想是，在一个批次（group）内，对同一个问题生成的多个候选回答进行比较，计算它们相对于该批次平均奖励的“相对优势”（relative advantage）。性能越好的回答（相对优势越高），其生成概率被提升；性能差的回答则被抑制。



- **PPO** (Proximal Policy Optimization) - 最常用的策略优化算法
- **GRPO** (Group Relative Policy Optimization)
- **DAPO** (Data-efficient Adaptive Policy Optimization)
- **Reinforce++** - 增强版 REINFORCE 算法
- **SAC** (Soft Actor-Critic) - 连续动作空间算法
- **CrossQ** - 交叉 Q 学习算法
- **RLPD** (Reinforcement Learning with Prior Data)
- **SAC-Flow** - 基于流匹配的 SAC 变体

### sac: Soft Actor-Critic

既要**分高**，又要**动作尽可能随机（熵最大化）**。

SAC 会同时训练两个 Critic（Double Q-learning），取打分最低的那个，以防止盲目乐观（Overestimation bias）。

**样本效率极高 (Sample Efficient)：** 它是 **Off-policy**（异策略）算法。这意味着它可以把过去存下来的经验（Replay Buffer）反复拿出来学习，不需要像 PPO 那样每走几步就把旧数据扔了。这对做机器人仿真（如 ManiSkill）甚至真机训练非常重要，因为数据很珍贵。

**抗干扰强：** 因为它追求“最大熵”，学出来的策略往往容错率更高。

**超参数少：** 早期的 SAC 需要手动调一个叫“温度系数 (Temperature, $\alpha$)”的参数，后来的版本（SAC-Auto）可以自动调节这个参数，使得它非常容易上手，不像有些算法那样很难调参。

直接用 SAC 训练 Flow Policy 非常困难。因为 Flow 模型生成动作的过程涉及多步积分（ODE Solver），这在反向传播时等价于一个**深层的残差 RNN（Recurrent Neural Network）**。这会导致严重的**梯度爆炸或消失**问题，使得训练无法收敛。

*SAC Flow: Sample-Efficient Reinforcement Learning of Flow-Based Policies via Velocity-Reparameterized Sequential Modeling*, 2025

提交给 Iclr26.

由于路径太长（K步非线性变换），**精确的对数似然和熵在实践中无法高效计算和求导**——尤其是需要反向传播时，路径梯度非常难处理。

有两个关键设计：

- 每一步都加了独立的高斯噪声（扩散项）
- 同时修改了 drift（漂移项）$ b_\theta $ 来“矫正”分布，确保最终的 **边缘分布 $ p(a|s) $ 不变**

这就像在模拟一个 SDE（随机微分方程）路径。每一步转移概率都是高斯分布：

看不懂了,太数学了. https://www.alphaxiv.org/abs/2509.25756?chatId=019be34a-3fbe-7f2b-899a-a5756e84bb1c