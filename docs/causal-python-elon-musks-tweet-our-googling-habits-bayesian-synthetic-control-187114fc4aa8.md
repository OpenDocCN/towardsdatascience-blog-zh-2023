# 因果 Python——埃隆·马斯克的推文，我们的搜索习惯，以及贝叶斯合成控制

> 原文：[`towardsdatascience.com/causal-python-elon-musks-tweet-our-googling-habits-bayesian-synthetic-control-187114fc4aa8`](https://towardsdatascience.com/causal-python-elon-musks-tweet-our-googling-habits-bayesian-synthetic-control-187114fc4aa8)

## 使用带有贝叶斯改进的合成控制量化推文的影响（使用 CausalPy）

[](https://aleksander-molak.medium.com/?source=post_page-----187114fc4aa8--------------------------------)![Aleksander Molak](https://aleksander-molak.medium.com/?source=post_page-----187114fc4aa8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----187114fc4aa8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----187114fc4aa8--------------------------------) [Aleksander Molak](https://aleksander-molak.medium.com/?source=post_page-----187114fc4aa8--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----187114fc4aa8--------------------------------) ·阅读时间 11 分钟·2023 年 1 月 8 日

--

![](img/aeed401fdb4cd97fd78b62ea9d006cd6.png)

图片由[Tolga Aslantürk](https://www.pexels.com/@tolgaaslanturk/)在[Pexels](https://www.pexels.com/photo/a-bird-flying-in-the-sky-11017085/)提供

2022 年 10 月给 Twitter 旧金山总部（以及一个水槽）带来了许多新变化。特斯拉和 SpaceX 的首席执行官埃隆·马斯克于 10 月 27 日成为公司新任所有者和首席执行官。

一些观众热烈欢迎这一变化，而另一些则保持怀疑态度。

一天后，即 10 月 28 日，马斯克推特上发了“*鸟儿被释放了*”。

推文的威力有多大？

让我们看看吧！

![](img/a6ce1b8901be7f13324a66b0f23244aa.png)

图片由[Laura Tancredi](https://www.pexels.com/@laura-tancredi/)在[Pexels](https://www.pexels.com/photo/curving-shaped-fragment-of-modern-building-7078717/)提供。

# 目标

在这篇博文中，我们将使用[**CausalPy**](https://causalpy.readthedocs.io/en/latest/)——来自[PyMC Developers](https://medium.com/u/7c6b7b6803cd?source=post_page-----187114fc4aa8--------------------------------)（[`www.pymc-labs.io`](https://www.pymc-labs.io)）的全新 Python 因果包，来估计马斯克的推文对我们搜索行为的影响，运用一种强大的因果技术叫做**合成控制**。我们将讨论该方法的基本原理，逐步实施，并分析我们方法的潜在问题，同时链接到额外的资源。

准备好了吗？

# 介绍

-   2022 年 11 月初，我安排了一次会议演讲，讲解如何量化时间序列数据中的干预效果。我认为在演讲中使用一个真实世界的例子会很有趣，于是我想到了马斯克的推文。关于 Twitter 收购案在互联网上引发了很多讨论，我想知道这样的推文在多大程度上能影响我们的行为，超越传统的社交媒体活动，例如它如何影响我们搜索“*Twitter*”的频率？

-   **嵌入 1.** 埃隆·马斯克的推文。

-   但首先要讲清楚一件事。

# -   因果关系与实验

-   因果分析旨在识别和/或量化干预（也称为处理）对感兴趣结果的影响。我们在世界上改变一些东西，想要理解我们行动的结果如何改变其他东西。例如，一家制药公司可能对确定新药对特定患者群体的效果感兴趣。这可能因为多种原因而具有挑战性，但最基本的原因是无法在同一时间观察到一个患者同时服用药物和不服用药物（这被称为因果推断的基本问题）。

-   人们找到许多聪明的方法来克服这个挑战。如今被认为是黄金标准的方法称为随机实验（或随机对照试验；**RCT**）¹。在 RCT 中，参与者（或一般情况下有时称为 ***单位***) 被随机分配到治疗组（接受治疗）或对照组（不接受治疗）²。

-   [](/causal-kung-fu-in-python-3-basic-techniques-to-jump-start-your-causal-inference-journey-tonight-ae09181704f7?source=post_page-----187114fc4aa8--------------------------------) ## 因果 Python — 3 种简单技术，助力你今天开始因果推断之旅

### -   学习 3 种因果效应识别技术，并在 Python 中实现它们，而无需耗费几个月、几周或几天的时间…

-   towardsdatascience.com

-   我们期望在设计良好的 RCT 中，随机化将平衡治疗组和对照组在 [**混杂因素**](https://causalpython.io/#confounding) 和其他重要特征方面的差异，这种方法通常相当成功！

-   不幸的是，由于经济、伦理或组织等多种原因，实验并不总是可用的。

-   如果我们…

![](img/7bf9992cbe8c8c61f93f098de58ad53a.png)

-   图片由 [Engin Akyurt](https://www.pexels.com/@enginakyurt/) @ [pexels.com](https://www.pexels.com/photo/red-synthetic-carpet-texture-14823277/)

# -   合成控制

如果我们只能观察处理下的结果，而控制组不可用呢？阿尔贝托·阿巴迪和哈维尔·加尔德亚萨巴尔在评估巴斯克地区冲突的经济成本时遇到了这种情况（Abadie & Gardeazabal, 2003）。他们的论文孕育了我们今天讨论的方法——合成控制。

这个方法背后的基本思想很简单——如果我们没有控制组，就创造一个！

如何？

一个解决方案是预测它。

如果我们选择一些*某种程度上相似*于我们的处理单位（但保持未处理的）并将它们用作预测变量呢？这就是合成控制（几乎完全）所做的！

这些未处理的单位有时被称为***捐赠池***。记住，我们处于时间序列数据的领域，基本的合成控制估计器是未处理单位的加权和。我们将使用一个额外的权重约束，强制权重在**0**和**1**之间，并且加起来等于一²。

每个权重调整每个未处理单位对结果的贡献。你可以把它看作是一个时间上的约束线性回归。

我们在处理前观察数据上拟合模型，并预测处理后的结果值。这一逻辑基于一个假设，即捐赠池变量没有受到处理的影响。当这个假设成立时，预测的处理后控制组应该保持所有处理前特征不变（假设捐赠池变量足够好地预测结果）。

> 如果你想看到合成控制的逐步实现以及整洁呈现的数学，查看[Matteo Courthoud](https://medium.com/u/666130fb420f?source=post_page-----187114fc4aa8--------------------------------)’s 博客文章和/或[Matheus Facure](https://medium.com/u/5a3f80e369d3?source=post_page-----187114fc4aa8--------------------------------)’s [章节](https://matheusfacure.github.io/python-causality-handbook/15-Synthetic-Control.html)关于合成控制。如果你想要更多应用研究的背景，请查看 Scott Cunningham 的“[因果推断——混合带](https://amzn.to/3MOINqp)”。对于贝叶斯实现（我们这里使用的），请查看[CausalPy 源代码](https://github.com/pymc-labs/CausalPy/blob/main/causalpy/pymc_models.py)。

[](https://aleksander-molak.medium.com/yes-six-causality-books-that-will-get-you-from-zero-to-advanced-2023-f4d08718a2dd?source=post_page-----187114fc4aa8--------------------------------) [## 是的！六本因果关系书籍将使你从零基础到高级（2023）

### …如果你愿意，可以完全免费获得其中的三本书！🤗

aleksander-molak.medium.com](https://aleksander-molak.medium.com/yes-six-causality-books-that-will-get-you-from-zero-to-advanced-2023-f4d08718a2dd?source=post_page-----187114fc4aa8--------------------------------)

# 假设

回到我们的推文。我假设马斯克广泛讨论的推文（“*小鸟自由了*”）使人们对 Twitter 本身以及相关新闻产生了更多兴趣。因此，我们期望观察到相对于其他社交媒体平台，“*Twitter*”的搜索量有所增加。

实际上，这一假设很难验证，因为结果可能不仅受到马斯克推文的影响，还可能受到*其他因素*（例如媒体对 Twitter 收购的报道）的影响。请注意，这实际上是一个很好的例子，说明了[**混杂**](https://causalpython.io/#confounding)如何在**合成控制**分析中发生³（Twitter 收购导致马斯克的推文*以及*引发对该平台的兴趣增加）。你认为哪种规范（**推文**作为原因或**收购**作为原因）更合理？请在评论中告诉我！

由于这是一个有趣的帖子，我们将*假设*马斯克推文对搜索行为的影响没有混杂，并且我们可以安全地进行估计。如果你决定自己估计 Twitter 收购对“*Twitter*”搜索数量的影响，请随时通过[LinkedIn](https://www.linkedin.com/in/aleksandermolak/)与我分享你的结果，或者加入 Causal Python 社区（https://causalpython.io），直接将结果回复到我们的一封每周邮件中。

# 数据中的马斯克推文

我们使用[Google Trends](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&cd=&cad=rja&uact=8&ved=2ahUKEwiJ9YDPxa78AhVtSPEDHf1QD4MQFnoECBAQAQ&url=https%3A%2F%2Ftrends.google.com%2Ftrends%2F&usg=AOvVaw1Iv7kLi18t1S2QG52bC3rn)作为代表全球每日搜索量的时间序列数据来源。我们对“[*Twitter*](https://twitter.com/AleksanderMolak)”的搜索变化感兴趣，因此我们收集了这个搜索的数据，同时也收集了“*TikTok*”、“*Instagram*”和“[*LinkedIn*](https://www.linkedin.com/in/aleksandermolak/)*”*的数据，以用作我们的***捐赠池***。

我们将使用 2022 年 5 月 15 日至 11 月 11 日之间的数据。

让我们看看图表。

![](img/5ee1ea5a90878e1b900514f031bc3516.png)

**图 1\.** *Twitter*、*LinkedIn*、TikTok 和 Instagram 搜索的数据。图像由本人提供。

我们可以看到 Twitter 和 Instagram 是搜索量最多的平台。它们之间存在一定的相关性。我们还可以看到 LinkedIn 的搜索量具有非常强的季节性特征，周末的搜索量明显较少，这与该网站的职业性质相符。

马斯克在 10 月 28 日发布了他的“*小鸟自由了*”推文。让我们把这个信息添加到图表中。

![](img/dc4b532f1fb1c9b16ad6fe77fb65896f.png)

**图 2.** 包括处理（黑色虚线）的*Twitter*、*LinkedIn*、TikTok 和 Instagram 搜索的数据。图像由本人提供。

我们看到 Twitter 搜索量的急剧增加与马斯克推文的发布日一致。

让我们看看在合成产生的对照组下效果有多强。

# 让我们建模吧！

我们从导入开始。

**代码块 1。** 导入库。

我们遵循[CausalPy 文档](https://causalpy.readthedocs.io/en/latest/)的惯例，并将库导入为`cp`。我们导入`pandas`来读取数据，并导入`matplotlib`来帮助我们绘图。

我们读取数据并将索引转换为日期时间（这帮助我们生成了上面的图表，并使得索引治疗更容易，但并非必要）。

**代码块 2。** 读取数据并将索引更改为日期时间类型。

让我们简单看一下数据。

![](img/8b3c485ce3b271f5b53bb4a9cfe794f7.png)

**图 3。** 我们数据集的前五行。图片由我本人提供。

正如预期的那样，我们看到四个变量和一个日期时间索引。我们将使用“*LinkedIn*”、“*TikTok*”和“*Instagram*”搜索作为捐赠池信号。

让我们将治疗日期存储在一个变量中，并实例化模型。

**代码块 3。** 将治疗日期存储在变量中并实例化模型。

我们使用`WeightedSumFitter`模型，这将允许我们为每个捐赠池变量找到权重，以生成最佳拟合的合成控制。你可能还记得我们之前说过，我们对这些权重使用了两个约束：

+   它们应总和为**1**。

+   它们应在**0**和**1**之间。

> 请注意，如果第一个条件为真，则第二个条件可以用更不严格的非负约束代替；我们使用了更严格的条件，因为它可能对一些读者更直观。

满足这些约束可以通过多种方式实现。如果你查看了我们上面提到的其中一个参考资料（Matteo 的博客或 Matheus 的书），你可能会注意到他们都使用了约束优化来实现这个目标。由于我们使用贝叶斯方法，我们需要在分布层面上对这些约束进行编码。与我们所需约束非常匹配的分布是[Dirichlet 分布](https://en.wikipedia.org/wiki/Dirichlet_distribution)。Dirichlet 分布的样本总和为**1**且非负。如果这让你想起了贝塔分布，那是一个很好的直觉！Dirichlet 是贝塔分布的（多维）推广。

CausalPy 将在后台负责初始化和拟合分布。我们现在准备好定义和拟合模型了！

CausalPy 支持 R 风格的公式来定义模型。公式`twitter ~ 0 + tiktok + linkedin + instgram`表示我们想要将 Twitter 搜索随时间的变化建模为“*TikTok*”、“*LinkedIn*”和“*Instagram*”搜索的函数。公式开头的零告诉模型我们不想拟合截距。

**代码块 4。** 定义和拟合模型。

我们使用`SyntheticControl`实验对象，它将负责模型拟合和结果生成。我们向构造函数传递四个参数：数据集、治疗索引、定义模型的公式和模型对象（我们选择了`WeightedSumFitter`）。

如果你自己运行代码，你会注意到初始化采样器并采样[链条](https://en.wikipedia.org/wiki/Markov_chain_Monte_Carlo)需要一些时间，但大约过一分钟后我们应该可以开始绘制结果。

[](https://amzn.to/3NiCbT3?source=post_page-----187114fc4aa8--------------------------------) [## Python 中的因果推断与发现：解锁现代因果机器学习的秘密…

### 《Python 中的因果推断与发现》：通过 DoWhy、EconML 解锁现代因果机器学习的秘密……

amzn.to](https://amzn.to/3NiCbT3?source=post_page-----187114fc4aa8--------------------------------)

# 结果

让我们来检查结果！`results`对象有一个非常方便的方法叫做`.plot()`，可以有效地以图形方式总结结果。

**代码块 5。** 绘制结果。

这给出了以下输出：

![](img/bd446e359ac0a9c30c50af953c15e303.png)

**图 4。** 我们模型的结果。图片由本人提供。

在图的顶部，我们看到处理前贝叶斯***R²***（Gelman 等，2018）的打印输出，量化了我们的捐赠者池变量对 Twitter 处理前搜索次数的预测效果。

最上面的面板展示了结果变量的实际观察（黑点）、处理前结果的预测（深蓝色线）、捐赠者池变量（灰色）、我们生成的合成控制（绿色）、干预时间（垂直红线）和干预效果（阴影蓝色区域）。

在中间面板中，我们看到**预测的因果影响**在处理前后的情况。

最后，底部面板展示了**累积因果效应**。

# 总结一下！

贝叶斯***R²***为**0.385**表明模型的处理前拟合效果不是很好（完美拟合的***R²***为**1**）⁴。考虑到我们的捐赠者池较小，这并不令人惊讶。许多从业者建议作为经验法则，捐赠者池中变量至少应在 5 到 25 个之间。我们只有 3 个。

另一方面，我们可以非常确定我们没有过拟合，这在捐赠者池较大的情况下可能会发生（*参见* Abadie，2021）。

如果我们认为分析中没有隐藏的混杂因素，埃隆·马斯克的推文的处理后效果相对较大，表明他的推文足够强大，能够暂时改变我们的搜索行为！

注意，另一种假设（Twitter 获取而非推文作为处理）看起来很有前景——你有没有注意到在干预前“*Twitter*”的搜索次数有所增加？

如果你决定检验这个假设，**与我和** **社区**分享你的结果吧！

# 关于 CausalPy

**CausalPy** 仍处于初期阶段，但正在稳步成长。我收到图书馆创建者的消息，表示一些令人兴奋的新功能正在开发中，包括对合成控制的用户定义先验的支持。此外，图书馆的功能不仅限于这一种方法。确保查看最新版本和更新，请访问这里：[`github.com/pymc-labs/CausalPy`](https://github.com/pymc-labs/CausalPy)

> **代码**和**conda 环境**文件可在这里获取：

[](https://github.com/AlxndrMlk/blogs-code/tree/main/Causal%20Python%20-%20Did%20Elon%20Musk%27s%20Tweet%20Change%20Our%20Googling%C2%A0Habits?source=post_page-----187114fc4aa8--------------------------------) [## blogs-code/Causal Python - Elon Musk 的推文是否改变了我们的搜索习惯?]

### 目前您无法执行该操作。您在另一个标签或窗口中已登录。您在另一个标签或窗口中已注销...

[github.com](https://github.com/AlxndrMlk/blogs-code/tree/main/Causal%20Python%20-%20Did%20Elon%20Musk%27s%20Tweet%20Change%20Our%20Googling%C2%A0Habits?source=post_page-----187114fc4aa8--------------------------------)

# 脚注

¹ 虽然存在多个处理和/或多个对照组的实验设计，但在这里我们保持简单。

² 请注意，这些约束并不是必要的，但当***捐赠者池***变量的值既**高于** *又* **低于**结果变量的值时，这会迫使模型不对超出我们观察到的值的范围进行外推。在我们的案例中，这很有意义——参见图 1，其中*Instagram*（大多数时候）高于*Twitter*，其他平台则低于。允许模型进行外推并不错误，但它存在模型*幻觉*变量如何在其观察范围之外表现的风险。如果这让您想起了积极性假设——那是一个很好的直觉！有关积极性和外推的更多信息，请访问：[`causalpython.io/#positivity`](https://causalpython.io/#positivity)

³ 请注意，我们也可以说这种情况违反了[SUTVA](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&cd=&cad=rja&uact=8&ved=2ahUKEwiGheHtmbb8AhVBTaQEHQgMDvkQFnoECC0QAQ&url=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FRubin_causal_model&usg=AOvVaw1k-P4Hoq71MjTTd0XFjId-)假设中的无多重处理版本部分，但我认为混杂视角更清晰、更直观。

⁴ 需要记住的是，使用***R²***作为拟合优度指标会带来自身的挑战。

## 了解更多关于 Python 中因果关系的内容：

[](/causal-kung-fu-in-python-3-basic-techniques-to-jump-start-your-causal-inference-journey-tonight-ae09181704f7?source=post_page-----187114fc4aa8--------------------------------) [## Causal Python: 3 个简单技巧来快速启动您的因果推断之旅]

### 学习 3 种因果效应识别技巧，并在 Python 中实现它们，不用花费几个月、几周或几天的时间...

towardsdatascience.com [](/beyond-the-basics-level-up-your-causal-discovery-skills-in-python-now-2023-cabe0b938715?source=post_page-----187114fc4aa8--------------------------------) [## Causal Python: 提升你在 Python 中的因果发现技能 [超越基础！] (2023)

### …并挖掘 Python 中最优秀且最被低估的因果发现包的潜力！

towardsdatascience.com [](https://aleksander-molak.medium.com/yes-six-causality-books-that-will-get-you-from-zero-to-advanced-2023-f4d08718a2dd?source=post_page-----187114fc4aa8--------------------------------) [## 是的！六本因果关系书籍将带你从零到高级 (2023)

### …而且如果你愿意的话，可以完全免费获得其中的三本！🤗

[aleksander-molak.medium.com](https://aleksander-molak.medium.com/yes-six-causality-books-that-will-get-you-from-zero-to-advanced-2023-f4d08718a2dd?source=post_page-----187114fc4aa8--------------------------------)

# 参考文献

Abadie, A. (2021). 使用合成控制法：可行性、数据要求和方法论方面。*经济文献杂志*。

Abadie, A., & Gardeazabal, J. (2003). 冲突的经济成本：以巴斯克地区为例。*公共选择与政治经济学电子期刊*。

Athey, S., & Imbens, G. (2017). [应用计量经济学的现状——因果关系和政策评估](https://arxiv.org/pdf/1607.00699.pdf)。*经济学视角杂志 32*(2)。

Gelman, A., Goodrich, B., Gabry, J., & Vehtari, A. (2018). 贝叶斯回归模型的 R 平方。*美国统计学家*。
