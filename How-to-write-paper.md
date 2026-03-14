# 画图技巧

- 粗一点的线会好看点,太细了不好看

nips是单列, 所以图一般都是横着, 

所有字体都粗一点, 字号大一点! 默认的字体都太细, 不好看 . 

画图, 角一般用圆角, 方形太尖锐.  需要加一些icon.

需要阴影, 看起来不会很廉价, 就有种高级感,  .柱状图全都用淡色

首字母要大写

数字放到bar上面

线要足够粗, 字体要足够大, 字要少

科研太需要画图能力, 画图很难被ai 替代. 

可以 python 画了, 用 ppt 加字体, 

大大大! 字体能多大就多大, 弄到最大为止. 

截个图问问gpt.可以怎么改进图, 怎么改进表. 

iconfont来找矢量图. 

spiderweb chart, matplotlib 里面有. 

把亮点集中在一个图, realworld, overallbenchmark,  不同 modality.

一张图的信息量要大, 去参考优秀论文怎么画的.

```
plt.xticks(tau_values, fontsize=20)
plt.yticks(fontsize=20)都要大字体.
颜色不能太相似. 不用红色和绿色, 因为红绿色盲不友好. 
线稍微粗一点. 
fontsize=20.
```

导出矢量图

macOS 矢量作图小贴士 - 陈乐群的文章 - 知乎
https://zhuanlan.zhihu.com/p/37664365

可以直接复制别人的幻灯片, 还可以截图矢量图!

ppt 和 drawio 都可以, 记得用 

### drawio

subfigure  mini figure

drawio 先导入图, 然后画个长方形, 自己用取色器, 输入文字, 做一个 legend. drawio 可以输入 latex.

https://www.latexlive.com/   来验证我们的公式正不正确

下载, 选 裁剪. 不然会有大量白边. 

ggplot2 很不错, 比 seaborn 细粒度.

### wandb 
wandb 怎么 把图导出 合并? 

wandb 保存 pdf 都是黑框, svg   在 ppt  会变成透明的, 下载 svg 甚至是网页截图,  网页卡顿的转圈都会被他保存下来. wandb 导出图的功能非常差, 基本不可用. 

wandb 可以隐藏. 
怎么显示这个波动的阴影呀?  Smoothing 就可以, 

只能在一根curve里做运算, 没法减别的. https://docs.wandb.ai/models/app/features/panels/line-plot/reference#expressions  
custom chart 里面用 vega 应该能算出新的曲线  
有个report 和 compare 的功能, 

wandb  val, 之后, 输出几个视频看看.


### nanobanaba

https://mp.weixin.qq.com/s/8MhSvE5pWhnofezxYfCzlg

画个快图还是可以的, 放论文不太行. 

```
You are an expert ML illustrator.
Draw a clean, NeurIPS/ICLR-style scientific figure using Nano Banana Pro.

GOAL:
Create a professional, publication-quality diagram that exactly follows the
structure and logic provided in the MODULE LIST below.
Do not invent components, do not reinterpret, do not add creativity.
Strictly follow the logical flow.

GLOBAL RULES:

- Flat, clean NeurIPS style (no gradients, no gloss, no shadows)
- Consistent thin line weights
- Professional pastel palette
- Rounded rectangles for blocks
- Arrows must clearly indicate data flow
- No long sentences, only short labels
- Keep spacing clean and balanced
- All modules must appear exactly once unless specified

LAYOUT:

- Horizontal left → right layout (recommended)
- Or vertical top → bottom if modules are inherently sequential
- Align components cleanly in straight lines
- Respect the module order exactly as listed

MODULE LIST (FILL THIS WITH YOUR PAPER'S CONTENT):

1. Input(s):
   - Initial state image (first_frame.png): shows the starting point or problem to be solved
   - Text answer (goal.txt): provides the text answer to the question
   - Text prompt (prompt.txt): provides natural language instructions for the video model

2. Preprocessing / Encoding / Embedding:
   - none

3. Core Architecture / Stages / Blocks:
   - video model,  like sora2

4. Special Mechanisms (optional):
   - none

5. Output Head:
   - video 
   
   6.   VLM
   -   VLM input is last frame of video, output is score.

NOTES (Optional but useful):

- Specify any required two-branch or multi-branch flow
- Specify “A and B must merge here”
- Specify “keep this as a single tall block with submodules”
- If experimental plot → replace section above with structured numbers

STYLE REQUIREMENTS:

- NeurIPS 2024 visual tone
- Very light background
- Text left-aligned inside blocks
- Arrows short and clean
- Use consistent vertical spacing

Generate the final diagram.
```





# 写作技巧

  https://github.com/yzhao062/cs-paper-checklist 

画图写论文可以用 Claude Scholar：AI学术研究全流程助手 - Dawn的文章 - 知乎
https://zhuanlan.zhihu.com/p/2005373649767658909

https://github.com/hzwer/WritingAIPaper/blob/main/NotGoodIdeas.md

https://github.com/hzwer/WritingAIPaper/blob/main/README.md 

用figma把矢量图图表字体调大，确保不放大看图也能一眼看清楚图表x轴y轴。疯狂用cursor让他以何凯明老师的风格，把abs和intro写到非常清晰，用斜体加粗一句话说明解决什么领域gap问题. 最关键是要icl，要拿你觉得比较好的论文的相关部分先丢给他，让他学一下写法.  kaiming暗通道的论文的写法

找人一起写 paper. 就问他们有没有兴趣一起写paper。 作者顺序要最早定好不要之后换, 因为别人会根据这个来努力, 

method要有总起段落.不断总结. % 3个创新点, 2个技术上创新,  一个实验上的结果. 

% 先写这两个.   

% 放实验结果证明

 remove all the citation in his methodology section}, 不然说明我们没novelty

only one section is allowed for the explanation of all experiments}

有时候需要显示作者名字, 就需要

```
\PassOptionsToPackage{numbers, compress}{natbib}
一起用这样就可以有压缩模式
{
\small # 要有small
\bibliographystyle{plainnat}# 默认会显示全部人名
\bibliography{ref}
}
```

what is the novelty of your method in the section?

appendix有很多内容，但是正文没有提，需要在正文中对应的位置提下，更多的细节在appendix中.

result analysis is the most unimportant ,表和图最重要. 写作大部分时间都在画图, 表可以用gemini 生成. 

不能只写出latency和吞吐量 ,还要写speed up.

methodology可以不引用, 说明创新性.

only one section is allowed for the explanation of all experiments

图的字体要和paper字体一样大. 

在最后一天再调vspace , 因为内容一直在变化, 过早调整高度没有意义.

method 不要讲太多别人的方法,  先写自己的方法, 专门在一个advantage 段落比一下别人的方法. 

related work 也没人看, 不重要. 

results的文字,  只会看table和图, 没人会看文字.  文字最后写, 最不重要,  所以写论文 做好图和表之前, 不要写任何文字. 

Gpt, prompt让他保持一致, 否则他会一个词用多种表达. 

放到其他地方写了，这里写没用，这是background，没人认真看的 , 我 专门写一个章节列出 advantages. 写出 motivation, 总结两点. 

Figures, drawings, tables, and photographs should be placed throughout the paper on the page (or the subsequent page) where they are first discussed. Do not group them together at the end of the paper. 

总结就不要写太复杂不要写细节, 写的很傻瓜奶奶也能懂.

写的时候, 不断总结, 先总结在 section 开头, 再总结在 introduction, 再总结到 abstract.

同一个名词在论文里要对齐. 

Writing like this will make readers feel that two separate techniques are combined in the paper. Better to highlight the connection between the two techniques so that they are combined comprehensively.   Space may be limited. We should still use the original paper architecture. We can make some paragraphs short. But remove the whole paragraph may change the paper architecture.



# 实验心得

你需要强调的重点在哪儿呢？？

有大的想法变动先尝试大的, 因为大的有用的话, 小的都不用做了就整个方向都抛弃了. 

- 不断尝试不同的方法, 每个实验都可以作为消融实验, 都不会白做的, 发的论文可能会启发某个人, 所以也不是没用的. 
- 快速尝试, 不要犹豫, 犹豫的时间都足够跑一次实验了. 不断实验调整, 不要纠结能不能成功.  
- 只看论文没有用, 要想一个idea, 去做, 中途不断看论文. 快速调整 
- 跑上实验之后要检查一遍gpu vram.

- 中间就消融, 不要删除ckpt.尽量多留着.  所以每个实验, 参数都应该和baseline setting一样, 这样才可以作为消融实验. 先和baseline setting一样,  再尝试其他的. 

写论文的时候, 要补的实验, 列一个表, 一个个做 

我测的参数太多方向了, 反而没有一个方向能用来写. 需要把一个表做透.  要把整个表列出来一个个做, 不能拍脑袋想测啥就测啥. 

先把自己的调到最好, 再做消融实验.

windows 的本地端ppt 功能很强大, 每个组件都可以拖动调节留白,  wps 的excel 图表不能导出pdf, google sheet图表不能加阴影. 

不要follow 一个较好的论文setting, 要用到最好的论文setting.

有大的想法变动先尝试大的, 因为大的有用, 小的各种超参数都不用做了就整个方向都抛弃了. 就一开始学习率大, 最后实在没时间了再学习率小微调. 

都先尝试最好的方法, 有好的方法都加上直接训练, 不要在不好的设置上继续. 

不要在第二好的setting上重复微调降低学习率, 要先把重要的几个参数换了,大幅度更换. 找到最好的再小幅度更换.  把自己做到最好, 做极限, 能好就不断放大, 不断加, 直到上不去为止. 

合作的话要写好代码分享给别人, 不能只讲一遍, 不然两边不一致.  要全部写好了让他跑, 不能让他改任何地方, 让他改一定会改错. 

应该用梯度累积, 把实验bs做一样, bs不一样就不能直接放在一个表里. 就会有人说你不公平比较. 

怎么用一个图例 ,然后分开三个轴画图? 

把图例都删掉, 然后自己画个方块, 写上one token, two token.图例要放中间. 用一行,

table字体要弄好. 

倍数是 `$\times$` 不是x.

经验
- 之前训练错的话, 就不能用ckpt 改了代码继续训练, 必须换ckpt.
- 所有表格都可以放在附录里, 都不会白做的.
- 不要和别人较差的setting一模一样, 就选最好的设置, outperform 他们就行. 不过根据上一条, 也可以说公平比较. 当做消融实验就行.   但是时间有限的情况下, 先测最好的设置.
- 要验证想法对不对, 可视化出来看是否确实选另外两个action 更好. 
- 变厉害了, 你就要用啊! 为什么不用呢. 厉害的方法都要用起来, 直到不能变好为止, 模块全都加上.

下次 测的时候, 记得要 测标准差或标准误差.

先自己干几次, 干多了之后就要让 cursor 自动化. 

代码管理: https://zhuanlan.zhihu.com/p/1973526373718065845



# latex技巧

dont forget the'~'  ,your should write ~\cite or ~\citet  ,  it is half space and it does not let the number gets seperated from 'Table'
它是半空间，不会让数字与“表”分开。 in case of an enter:
this is correct: 
"Table 1"

This is incorrect:
"Table
1"  

in some cases you should use \citet instead of \cite ,  to show the firt author's name et. al.

https://ctan.math.illinois.edu/macros/latex/contrib/natbib/natnotes.pdf

```
多个人写作的话,  用: 
\newcommand{\wang}[1]{\textcolor{blue}{#1}}
\wang{it is a method}
% 就可以代表是小王写的颜色.
```

不要把碎碎念写tex不然传arxiv会被看到 

不要png, 非要png的话先拼接png为pdf.

如果交了appendix一部分的话 另外一部分就没人看了. 

你不会下载pdf之后把后面的删掉？这样正文里就不会错误显示了. 

 输入 \mathcal{a} (小写) 通常会报错或者无法正确显示。\mathcal 通常用来表示更抽象的概念，如集合、集合的类别或数学算子。 

bm和 mathbf 都可以, 统一了就行. 

Check your log file!} You must fix any overflow into the margin (that means no overfull boxes in \LaTeX{}). \textbf{Nothing is permitted to intrude into the margin or gutter.}

## table

把table 上下移动一下, 这样可以移动表的位置, 更接近文字. 要按文中出现的顺序来. 

`\setlength{\abovecaptionskip}{2pt} ` 表格和文字之间,  距离太远也是可以缩短的. 

如果只有两种, 可以把 “Platform” 这一列去掉，但通过**分块标题**（如 “A6000” 和 “Orin”）来区分不同平台.

每个长标题分两行，第二行放单位或箭头符号说明（例如 ms、GB、Hz 等），并保持居中对齐：

### 附录

是可以在附录里写相关工作的，有论文也是这样干的，不过正文的introduction部分最好也要放一点文献。附录是单独的文件，需要单独上传. 这次没有appendix这一说法，都属于附加材料，不会收录的，格式随意

附录如果放在单独的pdf里，正文可以\ref对应的附录章节吗?

直接in the appendix 肯定是可以的.

先完整弄, 然后pdf裁剪掉附录.

和代码一起打包.传补充材料.  

在适当的情况下，鼓励作者将有关每个可重复性标准的详细信息作为其技术附录的一部分。审稿人将被要求评估论文中报告的结果的可重复程度，并在对每篇论文做出最终决定时权衡该评估。

### 代码

readme越全越好，核心代码越缺越好

### checklist

直接放reference后面, 不算页数. 

# 会议要求

http://aideadlines.org/?sub=ML,CV,NLP,RO,SP,DM,AP,KR,HCI,IRSM,MISC

##dac

The manuscript (up to 6 pages plus an additional 1 page for references only) is due by November 18, 2025 (5:00pm US Pacific Time). Each submitted manuscript must discuss original work that has not been previously published in other indexed research databases. It must not be actively reviewed at another conference or journal at the time of submission.

手稿（最多 6 页加上另外 1 页 reference）截止日期为 2025 年 11 月 18 日（美国太平洋时间下午 5：00）。每篇提交的稿件都必须讨论以前未在其他索引研究数据库中发表过的原创作品。在提交时不得在其他会议或期刊上积极审查。

在 2025 年 11 月 18 日截止日期之后，将不允许添加新的合著者或重新排序作者。

https://dac.com/2026/program/research-manuscript-submissions

https://softconf.com/dac26/research/

https://ieeexplore.ieee.org/xpl/conhome/11132383/proceeding
https://ieeexplore.ieee.org/document/11133060 也有 hardware.

https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=11132779 

 https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=11133396 他没有 hardware.

是用 sigconf 模板

acmart.cls 是格式.

https://www.overleaf.com/gallery/tagged/acm-official#.WOuOk2e1taQ  

其实就是给了一个模板的不同编译方式。根据后缀不同可区分为

| 模板             | 核心区别                                                     |
| ---------------- | ------------------------------------------------------------ |
| authordraft      | 传统 BibTeX 文献管理，适合常规投稿。                         |
| biblatex         | 使用 BibLaTeX，支持高级引用（如软件、数据集），适合复杂文献需求。 |
| i13n             | 多语言支持，标题、摘要可翻译为多种语言。                     |
| lualatex/xelatex | 针对特定 TeX 引擎优化，支持 Unicode、系统字体或高级排版。    |

总结来说 如果你不知道该选哪个 就选第一个 authordraft。。。

 sample-sigconf.tex 和 sample-sigconf-authordraft.tex 的区别 是前者是中稿之后要改的格式 后者是投稿用的格式

可以加附录吗? 不能

一千个录取 200 个. 

有的人说  期刊不用写满，我都是尽可能写少点，防止超页缴费

dac 2026换成acm了 . https://dl.acm.org/doi/proceedings/10.1145/3768292

在顶会（包括 ACM 系列会议）里，**摘要通常必须使用模板默认的格式**，不能自行加粗、加色或做其它强调。ACM 的官方 `acmart` 模板里，abstract 环境的正文是“正常字体、正常大小、正常粗细”，作者不应该修改样式，否则会违反格式规范。

有人说,  ccs标签不能删, 其他随便

permission MM, 官方说可删，但是建议别删。因为删了被录用的话这段也得加回来，留着方便最终版排版。

dac 17%  11.18

### icme

https://2026.ieeeicme.org/icme-2026-call-for-papers/

30% 

12.12

https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=11209792

https://dblp.org/db/conf/icmcs/index.html

### ijcai

January 19th. **Full paper submission deadline**  

https://github.com/lixin4ever/Conference-Acceptance-Rate

15% in 2023 https://www.ijcai.org/proceedings/2022/preface.pdf

 ijcai 是A 会, 比别的A 简单, 但是还是很难中, 15%.  据说很多内部交易  ,,而且ijcai好像每年就收几百篇, 

ijcnn,  25 年 **39%**. 接受率.  Ijcnn是 ccf c,  中国人认为没有用, 回国没人看.不过投的人越来越多了, 23 年甚至 54% 接受率.  

icip, ddl 2 月 4 日.   44% 接受率.

### icann



https://e-nns.org/icann2026/call-for-papers-2/

https://cmt3.research.microsoft.com/User/Login?ReturnUrl=%2F

https://cmt3.research.microsoft.com/ICANN2026/Submission/Index



## aaai

- 只能交一个tex, 没有用到的图不要交.
- 不能图太大等式太长侵占margin
- listing 用来放真实代码片段, algorithm是伪代码

https://arxiv.org/html/2309.00779v2

https://aaai.org/conference/aaai/aaai-26/submission-instructions/

Do not include references in your abstract!

reference不需要单独一页. 直接放后面. 

aaai section默认没有, 所以没法ref.

72 dpi不行, 图至少300 dpi

## PMLR

### tmlr

这个和 iclr 一样会公开review

Submissions may be any length, but a paper’s length should be justified by its content 

12 页以上审稿时间更长. 

适合实验非常丰富的论文. 

不应被接受的论文包括 :  在没有经验或严格证据支持的情况下做出大胆陈述的论文、没有明确写作的论文、错误地声称比现有已发表作品具有新颖性的论文，以及仅仅重新实施以前已经复制的想法的论文。请参阅验[收标准](https://jmlr.org/tmlr/acceptance-criteria.html)以进行更彻底的讨论。

- Affinity Score: 0.986 最相关的
- Custom Max Papers: 自己设置最多的
- Assignment: 已有的

72 【被真实了一年的MagicPose4D终于中了TMLR - 养猪科学家 | 小红书 - 你的生活兴趣社区】 😆 zv3S168cGXOCAWq 😆 https://www.xiaohongshu.com/discovery/item/685094540000000020028c45?source=webshare&xhsshare=pc_web&xsec_token=ABKFHqpZgBWTAPPf0BxDH3clSk1g3Gvue27jGLaS2gub4=&xsec_source=pc_share





## 期刊

期刊一般好像都有来回改的余地

### jmlr

jmlr 都是很长的,其中一篇91页，七十几页推导，还有一篇在投，80页左右.

https://jmlr.org/tmlr/ *使用双盲评审*

https://www.jmlr.org/author-info.html  JMLR 发表有关机器学习理论和方法的论文，但不发表机器学习在其他领域的应用。 好像是单盲的. 

TPAMI  https://ieeexplore.ieee.org/xpl/RecentIssue.jsp?punumber=34    期刊主要是要很久tpami. 被拒花了得8/9个月  

### Nature Machine Intelligence

nature 都是要从0到1的问题，从无到有的。

第一次投不需要太正式的模板，只要章节顺序和正式发表都一致就行. 投稿后编辑会卡两周左右给回复，如果送审的话大概一个半月出一审意见。全流程下来差不多7个月左右吧.

图有限制, 所以他们一个图都是一堆图组合

https://www.springernature.com/gp/authors/campaigns/latex-author-support 为啥模板都是单列的? 

我初稿图表都是放在正文了 和ai会议差不多的排版格式 但是一定要按intro result discussion method appendix这个流程写

Nmi 模板为啥是一列不是两列? 应该用什么模板?我认为所有 Springer-Nature 期刊至少在接受和最终编辑之前都使用标准的 Springer-Nature 模板。

- `sn-jnl.cls`（必须用）
- `sn-nature.bst`（参考文献格式用这个）

#### article

An Article is a substantial novel research study, with a complex story often involving several techniques or approaches. 

https://www.nature.com/natmachintell/content

**Format**

- Main text – up to 3,500 words, excluding abstract, Methods, references and figure legends.
- Abstract – up to 150 words, unreferenced. 
- Display items – up to 6 items (figures and/or tables). 
- Article should be divided as follows: 
  - Introduction (without heading)  要写一段 **引言部分**，但是**不要加 “Introduction” 这个标题**。
  - Results 似乎他们也不用大标题. 
  - Discussion  . 
  - Methods. 
- Results and Methods should be divided by topical subheadings; the Discussion does not contain subheadings. 
- References – as a guideline, we typically recommend up to 50.
- Articles include received/accepted dates. 
- Articles may be accompanied by supplementary information. 
- Articles are peer reviewed.

可以选双盲 https://www.nature.com/natmachintell/submission-guidelines/dapr  ,默认不匿名. 

 12 页, 图超级大, 都是一个图嵌套很多小图.  因为最多就 6 个 display item.

Result 好像就是 experiment.

https://www.nature.com/articles/s42256-025-01090-y

discussion , 讨论我们的优点,  和 future work

methods 好像主要写 setting. 

## 做 ppt

https://slideslive.com/icml-2024

做 ppt 最需要人手把手教, 比文字更难传播.

ai 审稿 prompt:

```
[System Role]
You are an  experienced reviewer for top-tier ML/AI venues (AAAI/NewcAPS/ICLR style) -
Produce a text-only, structured review with NO scores, ratings, or accept/reject decision.
[Critical Constraints]
1) Use EXACTLY these section headings in this order (no extras, no omissions):
- Synopsis of the paper
- Summary of Review
- Strengths
- Weaknesses
- Suggestions for Improvement
- References
2) Do NOT output any scores, ratings, or accept/reject verdict.
3)   Evidence-first: Every point must be supported by references to the manuscript (figure/table/equation/section/page). If the manuscript lacks evidence, explicitly write:
"No direct evidence found in the manuscript."
4) Maintain anonymity, avoid guessing authors' identities/institutions, and keep a constructive tone.
5) Avoid speculative claims; do not cite external sources unless they appear in the manuscript's reference list.
(Input]
- Full
anonymous manuscript (plain text or OCR output).
[Output Template]
Write the review using the six headings and only those headings:
1) Synopsis of the paper
becty ve State the or anya, catton- oe languributions, and main results (5150 words).
2) Summary of Review
- After each reason, add an evidence anchor (e-g., "see Table 2; Sec. 4.1; Eq. (5)").
- 3-6 bullet points focusing on novelty, technical soundness, experimental rigor, clarity, and potential impact.
- Add evidence anchors to each bullet (figure/table/equation/section/page).
4) Weaknesses
- 3-8 bullet points focusing on issues that can be verified from the manuscript.
- Typical aspects: relation to closest prior work; breadth of experiments (datasets/metrics/ablations/statistical significance);
anchorse each buntes e/spe ex cat sate the o.
5) Suggestions for Improvement
- 4-8 concrete, actionable recommendations (eg., add specific ablations, unify baseline settings and tuning budgets, report meantstd or confidence intervals, include reliability diagrams or additional metrics, release code and seeds).
- Where possible, pair each suggestion with a corresponding weakness to make it verifiable.
References
- List ONLY items that you explicitly cite within this review AND that appear in the manuscript's reference list; use a concise format (e.g.,
- If you do not
"[Author et al., Year]" or the manuscript's numbering style).
cite anything or the manuscript's reference list is unavailable, write "None".
[Style & Length]
Tone: objective, polite, and constructive.
- Suggested total length: 800-1200 words (adjust as needed to match manuscript
```

 

### 读论文的 prompt



```
讲了啥?
模型结构是啥? 
训练多久? 多少 gpu?
```

### 寻求合作

第一封邮件, 介绍自己, 问问能不能未来合作. 

介绍自己, 也让他们介绍一下他们的工作. 

一般 last year student 都很忙, 实习, 很多去工业界不愿意发论文, 但是聊聊依然很有好处. 

如果他们感兴趣, 那第二次 meeting 介绍自己的 idea.

也让他们付出时间, 不愿意付出时间的合作者毫无用处. 

一个项目可以多个合作者, 可以就需要的时候拉会, 根据贡献给 author list.

如果上来就要求你写所有的代码, 上来就要求你给他做个 ppt, 多半不太好. 

### rebuttal

We sincerely thank you for your constructive feedback.  

应该可以，再多写点，引用些论文中的数据，比如 as demonstrated in  section xxx （or table xxx）, we achieve an acceleration of XXX on XXX device (or memory reduction) for which model, 把跟这个track相关的性能提升的performance 数据列一列

you can use gpt. give the conference information and your paper it can come up with a coherent strong answer
