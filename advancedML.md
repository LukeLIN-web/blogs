

https://karpathy.github.io/2019/04/25/recipe/
karpathy的经验。
教你怎么在ML debug。
1. 与数据融为一体
有一次我发现数据里有重复的例子。还有一次我发现图片/标签损坏了。我会寻找数据不平衡和偏见。我通常也会关注自己对数据的分类流程，这暗示了我们最终将探索的架构类型。举个例子——非常局部的特征足够吗，还是我们需要全局背景？变化有多大，形式如何？哪些变异是虚假的，可以预处理去除？空间位置重要吗，还是我们想要平均分配？细节有多重要？我们能承受多大程度的下采样？

写一些简单的代码，用你能想到的任何方式搜索/筛选/排序（比如标签类型、注释大小、注释数量等），并可视化它们的分布和任意轴上的离群值。尤其是异常值，几乎总是会发现数据质量或预处理上的漏洞。

2. 建立端到端的培训/评估骨架 + 获取模糊基线
修正随机种子 。一定要使用固定的随机种子，这样可以保证运行代码两次后结果会一致。这样可以减少变异因素，有助于保持理智。
简化 。一定要关闭任何不必要的花哨功能。举个例子，这个阶段一定要关闭任何数据增强。数据增强是一种规范化策略，我们可能会以后加入，但目前这只是另一个引入愚蠢漏洞的机会。
init well. Initialize the final layer weights correctly. E.g. if you are regressing some values that have a mean of 50 then initialize the final bias to 50. If you have an imbalanced dataset of a ratio 1:10 of positives:negatives, set the bias on your logits such that your network predicts probability of 0.1 at initialization. Setting these correctly will speed up convergence and eliminate “hockey stick” loss curves where in the first few iteration your network is basically just learning the bias.
输入-独立基线 。训练一个输入无关的基线，（比如最简单的就是把所有输入都设为零）。这应该比你直接插入数据但不归零时表现得差。是吗？也就是说，你的模型是否学会从输入中提取任何信息？
过拟合一批 。对仅有少数样本（例如最小两个）的单批次进行过拟合。为此，我们增加模型容量（例如添加层或过滤器），并验证能否达到最低的损失（例如零）。我还喜欢在同一张图中同时可视化标签和预测值，确保当我们达到最小损失时，它们能完美对齐。如果没有，说明某处有 bug，我们无法继续下一阶段。
visualize just before the net. The unambiguously correct place to visualize your data is immediately before your y_hat = model(x) (or sess.run in tf). That is - you want to visualize exactly what goes into your network, decoding that raw tensor of data and labels into visualizations. This is the only “source of truth”. I can’t count the number of times this has saved me and revealed problems in data preprocessing and augmentation.
可视化预测动态 。我喜欢在训练过程中可视化固定测试批次的模型预测。这些预测的“动态”会让你对训练进展有极好的直觉。很多时候，如果数据晃动过大，可能会感觉到网络“难以适应”你的数据，从而暴露出不稳定。非常低或非常高的学习率也很容易从抖动的程度中显现出来。
推广一个特殊情况 。这其实是个比较通用的编程建议，但我经常看到有人因为承担太多事情，从零开始写一个相对通用的功能，结果就出了 bug。我喜欢写一个非常具体的函数来配合我现在正在做的事情，让它工作，然后再泛化，确保得到相同的结果。这通常适用于向量化代码，我几乎总是先写出完全循环的版本，然后再逐循环转换成向量化代码。


3. Overfit
我喜欢采用的寻找好模型的方法有两个阶段：首先，模型足够大以至于可以过拟合（即专注于训练损失），然后适当正则化它（放弃部分训练损失以改善验证损失）。我喜欢这两个阶段的原因是，如果任何型号都无法达到低错误率，这可能再次说明存在问题、漏洞或配置错误。
我总是建议大家找最相关的论文，复制粘贴他们最简单的架构，这样能实现良好的性能。比如，如果你在分类图像，不要做英雄，第一次运行时直接复制粘贴 ResNet-50。你可以以后做更自定义的游戏来通关。

complexify only one at a time. If you have multiple signals to plug into your classifier I would advise that you plug them in one by one and every time ensure that you get a performance boost you’d expect. Don’t throw the kitchen sink at your model at the start. There are other ways of building up complexity - e.g. you can try to plug in smaller images first and make them bigger later, etc.

do not trust learning rate decay defaults. ImageNet would decay by 10 on epoch 30. If you’re not training ImageNet then you almost certainly do not want this. If you’re not careful your code could secretely be driving your learning rate to zero too early, not allowing your model to converge. In my own work I always disable learning rate decays entirely (I use a constant LR) and tune this all the way at the very end.

 

4. 正则化
据我所知，增加更多数据几乎是唯一能保证单调地提升配置良好神经网络性能的方法。另一个是ensemble（如果你负担得起的话），但这最多只能用~5 个模型。
数据增强 。仅次于真实数据的方法是半假数据——尝试更激进的数据增强。
stick with supervised learning. Do not get over-excited about unsupervised pretraining  (though NLP seems to be doing pretty well with BERT and friends these days, quite likely owing to the more deliberate nature of text, and a higher signal to noise ratio).很有趣。
输入维度更小 。移除可能包含虚假信号的特征。任何额外的虚假输入，如果你的数据集很小，都可能更容易过度拟合。同样，如果低层次细节不太重要，试着输入更小的图像。
模型尺寸较小 。在很多情况下，你可以利用网络的领域知识约束来缩小其规模。举例来说，过去在 ImageNet 的主干网顶部使用完全连通图层是流行的，但现在这些图层被简单的平均池化取代，过程中消除了大量参数。
减少batch size 。Due to the normalization inside batch norm smaller batch sizes somewhat correspond to stronger regularization. This is because the batch empirical mean/std are more approximate versions of the full mean/std so the scale & offset “wiggles” your batch around more.
drop. Add dropout. Use dropout2d (spatial dropout) for ConvNets. Use this sparingly/carefully because dropout does not seem to play nice with batch normalization.
weight decay. Increase the weight decay penalty.
early stopping. Stop training based on your measured validation loss to catch your model just as it’s about to overfit.

5. tune
神经网络通常对某些参数的敏感度远高于其他参数。在极限情况下，如果参数 a 很重要但改变 b 没有影响，那么你宁愿更彻底地采样 a，而不是多次在几个固定点采样。
参考chatgpt什么是random search。  https://jmlr.csail.mit.edu/papers/volume13/bergstra12a/bergstra12a.pdf




## Fundamental concepts of statistical learning

Uniform convergence (learning finite, realizable hypothesis spaces)

#### 迁移学习

- How does the difference between β(1) and β(2) affect transfer learning performance?   这个叫model shift.

- How does the difference between the feature vectors of source task and target task affect transfer learning?  这个叫 covariate shift

**Hard Transfer** assumes a single shared parameter vector (β\betaβ) for both tasks.

**Soft Transfer** allows separate parameters for each task but aligns them through regularization

Probably Approximately Correct Learning,  PAC learning

h：是一个理论上的候选假设，可能不一定是最佳的。

h^\hat：是通过训练优化得到的假设，用来对新数据进行预测。

Empirical Risk Minimization, ERM

## HW1

神经切线核

McDiarmid. 有界差分不等式，是概率论中的强大工具。它提供了一种方法来限制独立随机变量函数显著偏离其预期值的概率

Proof of McDiarmid’s inequality

Martingale Definition

A martingale is like a "fair game": no matter what happens up to a given point, your expected future value is equal to your current value.

Di的期望是0 .  因为 Zi会等于 Zi-1.

Rademacher Complexity 是一种用于衡量假设类（hypothesis class）容量的统计学习理论工具。它用于量化模型在随机标签（噪声数据）上的拟合能力，进而用于评估模型的泛化能力。

若 **Rademacher Complexity 低**，说明该假设类对随机噪声拟合能力弱，模型更可能具有较好的泛化能力。

在深度学习中，Rademacher Complexity 也用于分析神经网络的复杂度，并指导如何使用正则化（如 Dropout、权重衰减）来控制过拟合。







## matrix completion.

矩阵的 **核范数**（Nuclear Norm），也叫做 **迹范数**，是矩阵的奇异值的和。给定一个矩阵，其核范数定义为矩阵的所有奇异值的和. 

直观上它可以用来控制矩阵的低秩结构。较小的核范数表示矩阵的秩较低。

我们通常希望找到一个低秩矩阵来近似一个部分已知的矩阵。然而，优化矩阵的秩是一个非凸优化问题，难以直接求解。

为了简化这个问题，我们使用 **核范数松弛**，即将目标函数中的矩阵秩用核范数来代替。通过最小化矩阵的核范数，我们可以得到一个近似的低秩矩阵。

1. **矩阵元素有界**：
   - 这是一个合理的假设，因为在实际应用中（如电影评分），矩阵元素通常是有界的（例如评分在 1 到 5 之间）。

对于极端情况（如矩阵中只有一个非常大的元素），如果没有采样到关键元素，恢复可能会失败。因此，合理的假设（如元素有界性和不相干性）是核范数最小化成功的关键。

## 网络泛化误差

激活函数, 一般都是 1-Lipschitz, Sublinear function, 不能增长太快. 

### **与 VC 维的关系**

- VC 维（Vapnik-Chervonenkis 维度）也是衡量假设类复杂度的工具，但 Rademacher Complexity 具有更强的适用性，尤其适用于神经网络等假设类容量较大的情况。
- 一般来说，较高的 VC 维和较高的 Rademacher Complexity 都意味着较差的泛化能力。



对于  n  个样本  Z = \{z_1, z_2, \dots, z_n\} ，假设函数族  F  的每个函数将这些样本映射到二值集合  \{0, 1\} 。如果函数族  F  可以产生所有可能的  2^n  种二分类标签，那么我们说函数族  F  可以“碎裂”这  n  个样本。

**VC维度（Vapnik-Chervonenkis Dimension）**：函数族的 VC 维度是指该函数族可以“碎裂”的最大样本点数。如果一个函数族能够“碎裂”  n  个点(也就是 样本)，但不能“碎裂”  n+1  个点，那么该函数族的 VC 维度就是  n 。









## project

See the current list of paper reading here: [https://docs.google.com/document/d/1uqmO_KRcX_Wy_6csd7bOqoNK8hgQU8oDBa6v9ZgyVPc/edit?usp=sharingLinks to an external site.](https://docs.google.com/document/d/1uqmO_KRcX_Wy_6csd7bOqoNK8hgQU8oDBa6v9ZgyVPc/edit?usp=sharing)





CLIP ,模型有多大? 可以做什么? 



https://github.com/stanford-crfm/helm



https://github.com/web-arena-x/visualwebarena





