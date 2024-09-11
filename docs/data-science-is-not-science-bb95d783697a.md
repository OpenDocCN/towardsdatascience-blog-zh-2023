# 数据科学不是科学

> 原文：[`towardsdatascience.com/data-science-is-not-science-bb95d783697a?source=collection_archive---------7-----------------------#2023-01-17`](https://towardsdatascience.com/data-science-is-not-science-bb95d783697a?source=collection_archive---------7-----------------------#2023-01-17)

## 如何在数据科学分析中融入科学过程

[](https://conorosullyds.medium.com/?source=post_page-----bb95d783697a--------------------------------)![Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----bb95d783697a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bb95d783697a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb95d783697a--------------------------------) [Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----bb95d783697a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ae48256fb37&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-science-is-not-science-bb95d783697a&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=post_page-4ae48256fb37----bb95d783697a---------------------post_header-----------) 在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb95d783697a--------------------------------)上发布 · 8 分钟阅读 · 2023 年 1 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbb95d783697a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-science-is-not-science-bb95d783697a&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=-----bb95d783697a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbb95d783697a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-science-is-not-science-bb95d783697a&source=-----bb95d783697a---------------------bookmark_footer-----------)![](img/cca5ee3484d862d642dfb4909eae09b4.png)

图片由[Greg Rakozy](https://unsplash.com/@grakozy?utm_source=medium&utm_medium=referral)提供，发布于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

商业科学、运动科学、食品科学……我们喜欢在其他词汇后面加上“科学”这个词。这让这些领域听起来，更加*科学*。然而，如果我们不小心，它们会与伪科学有更多的共同点。

**数据科学也没有什么不同。**

表面上看，它可能接近科学。两者都是寻求知识的过程。从本质上讲，机器学习也是一个重复的过程。我们不断收集新数据并尝试改进之前的结果。然而，在许多情况下，数据科学偏离了科学过程。

了解为什么我们讨论：

+   科学过程

+   数据科学与科学不同的方面

+   我们如何使数据科学更接近科学

科学是我们理解自然世界的最佳工具。它可能在不同的条件下操作于数据科学，但我们仍然可以采用其过程。这样做将导致更可靠的分析。

在你继续阅读之前，你可能对这个话题的概述感兴趣：

# 什么是科学？

科学涵盖了大量的人类活动。大致来说，它既包括我们已经知道的东西，也包括我们知道这些东西的方法。后者被称为科学过程。这是一种获取新知识的系统方法论。该系统中最重要的部分是假设。

> 科学可以被看作既是知识的体系，又是获取新知识的过程（通过观察和实验——测试和假设）。
> 
> — 澳大利亚科学院

## 假设

假设是一种关于事物运作方式的有根据的假设或猜测。例如，你可能睡得不好。一个好的假设是：“我睡前喝的双倍浓缩咖啡让我无法入睡。”这是一个好的假设，因为它既可测试又有证据支持。

## 可测试性

事实上，假设*必须*是可测试的。这意味着我们能够设计一个实验来提供数据。对这些数据的分析将支持或反驳假设。我们上面的假设是可测试的。你可以简单地在睡前停止喝咖啡，然后观察你的睡眠情况。

## 证据

在科学中，我们永远不能以 100%的确定性宣称一个假设是真的。我们可以通过更多来自多个独立实验的证据逐渐接近这一目标。要宣称咖啡对睡眠有负面影响，我们需要比一次实验更严格。这需要多个人、不同的咖啡因水平、双盲测试等……

这就是科学过程——做观察，制定假设，设计实验，收集证据，得出结论并重复！这是我们获取新知识的最佳工具。然而，它不是唯一的工具。

# 为什么数据科学不是科学

数据科学也寻求获取新知识。正如我们所提到的，它以偏离上述过程的方式进行。在我担任数据科学家期间，我遇到过这方面的例子。

## 数据讲述

首先是数据讲述。实际上，这就是以一种不会让非技术观众感到乏味的方式重新框定分析。我们使用漂亮的图表和相关的故事来说服他们我们的结论是正确的。

> 数据讲述的概念是基于复杂的数据和分析构建一个引人入胜的叙述，帮助讲述你的故事并影响和告知特定的观众。
> 
> — 微软（Power BI）

数据讲述的一个关键部分是建立对模型的信任。我们需要说明为何使用某些特征或这些特征的参数。换句话说，我们需要解释模型捕捉的潜在关系。

问题是，这些理由是在模型构建后才被想到的。我们从数百，甚至数千个特征开始。经过多次迭代，我们将这些特征缩减到最终集。只有到那时，我们才会为将这些特征纳入模型提供令人信服的商业理由。这正好与假设检验相反。

## 黑箱模型

在某些情况下，我们甚至不关心模型是如何做出预测的。我们只关心它们做出预测的效果如何。算法被视为黑箱，我们没有产生新的知识。这与科学的最终目标相悖。

## 机器学习的回音室

更糟糕的是，这种方法可能会从行业的数据科学转到学术界。也就是说，机器学习研究者往往会创造自己的生态圈。研究的目标甚至整个社区的目标是提升在基准数据集上的表现。性能改进的递减被视为科学贡献，原始的研究问题则成为次要考虑。

一个例子来自我自己的研究。我的硕士论文研究了[使用机器学习和自然语言处理技术预测法律案件的结果](https://arxiv.org/abs/1912.10819)。这旨在帮助欧洲人权法院（ECHR）提高判决速度。

我的唯一目标是提高现有方法的准确度。回顾过去，这实际上对 ECHR 并没有帮助。法官会相信模型的结果吗？

与其挤出额外的 1%准确度，我本可以解释模型或提供更深入的结果分析。这样法院可以用来辅助决策，而不是完全取代决策。

## 快速决策

最后的原因与环境差异有关。学术界有重新测试、收集新数据集或甚至以全新的方式测试假设的特权。工业界则没有这种特权。你需要立即做出决策，基于你所拥有的任何证据。

这引出了一个问题，*数据科学*是否应该更像科学？通常答案是否定的。用与科学假设相同的严谨度来处理所有商业问题会适得其反。在做出决定所需的时间内根本无法创造出多条证据链。

# 数据科学如何更像科学

然而，与此同时，科学仍然是我们揭示真相的最佳方法。将数据科学更接近科学可以使我们的结果更可靠。那么，我们可以通过哪些方式来实现这一目标呢？

## 融入领域知识

与非技术同事提前讨论期望是很重要的。这可以包括数据中的趋势或你期望哪些模型特征是重要的。这些可以作为非正式假设。

你的分析将提供支持或反对这些期望的证据。如果你发现相反的证据，现在你不仅仅需要对观察到的趋势提供令人信服的理由。你还需要解释为何现有的期望是错误的。这将增加你理由的负担，使其更具可靠性。

## 使用多个指标和可视化

在提供证据时，不要仅仅依赖一个指标或可视化，因为它们都有自己的局限性。数据科学家们已经这样做了。在评估模型性能时，我们不仅仅看准确性。我们使用精确度、召回率，甚至通过 ROC 曲线来可视化性能。

进行分析时也应采取相同的方法。例如，假设我们想评估某人的财务状况。我们可以计算他们前一年的总收入。这会告诉我们很多，但一个数字不足以完全捕捉他们的状况。我们还需要查看开支和现有债务。除了总数外，我们还可以查看月度变化。

这类似于为一个结论提供多条证据。尽管这并不等同于收集新数据或重新运行实验。我们仍然使用一个基础数据集。所有指标都将受到该数据集中的偏差的影响。我们只是去除了由指标本身引入的偏差。

## 预先定义成功指标

更进一步，我们可以正式定义指标及其截断值。也就是说，我们将在分析中使用哪些指标，以及哪些值将被视为成功。这是为了防止你挑选支持先入为主观念的指标。

一个例子来自下面关于算法公平性的文章。这里我们提供了公平性的不同定义。一种定义，**平等机会**，通过比较特权组和非特权组的假阳性率（FPR）来工作。如果 FPR 的差异在某个截断值之内，则模型被认为是公平的。

[](/analysing-fairness-in-machine-learning-with-python-96a9ab0d0705?source=post_page-----bb95d783697a--------------------------------) ## 机器学习中的公平性分析（使用 Python）

### 进行探索性公平性分析，并使用平等机会、平等化概率和差异化...

towardsdatascience.com

假设我们没有定义截断值或选择公平性的定义。在计算 FPR 差异后，我们可以争论一个更高的截断值。如果差异显著，我们甚至可以争论使用完全不同的定义。面对几个月的建模工作，这可能是诱人的。

## 可解释的机器学习

我们还可以努力不仅仅使用这些评估指标。这就是 IML 的作用。它旨在构建可以被人类理解的模型。像 SHAP、LIME、PDPs 和 ICE 图这样的 IML 方法允许你窥视黑箱，了解它的工作原理。

使用这些方法使我们更接近科学的目标——理解我们的自然世界。我们从了解模型预测的效果到了解它们如何做出这些预测。在这个过程中，我们可以学到一些关于我们数据的新知识。

[## 从黑箱模型中可以学到什么

### 使用非线性模型进行数据探索和知识生成

[了解我们可以从黑箱模型中学到什么](https://conorosullyds.medium.com/membership?source=post_page-----bb95d783697a--------------------------------)

## 对你的结论保持自信

直到现在，我们讨论了与数据科学相关的问题。然而，许多问题实际上是由于其最佳实践的曲解造成的。数据科学家往往不是做出决策的人。你是负责提供基于数据的信息，以支持决策的人。你应该努力提供尽可能可靠的信息。

在组织中工作时，这说起来容易做起来难。你会处理多个利益相关者，他们可能对决策有不同的偏好。无论是想让模型上线的经理，还是希望销售更多保险的业务主管，都会对你的结论施加压力。因此，为了避免从 DS 变成 BS，你需要坚定立场，依靠数据所告诉你的信息。

数据科学可以通过许多方式变得更像科学。我们没有涉及的一点是科学的协作性质。我认为数据科学家已经很擅长这一点。无论是分享 git 代码、Kaggle 上的数据集还是 Medium 上的文章，我们都喜欢分享我们的想法。也许我们可以对这种做法稍微更具科学性。

希望你喜欢这篇文章！你可以通过成为我的[**推荐会员**](https://conorosullyds.medium.com/membership)来支持我**:)**

[## 使用我的推荐链接加入 Medium — Conor O’Sullivan](https://conorosullyds.medium.com/membership?source=post_page-----bb95d783697a--------------------------------)

### 作为 Medium 会员，你的一部分会员费用会分给你阅读过的作者，你将可以完全访问每一个故事……

[加入 Medium，使用我的推荐链接 — Conor O’Sullivan](https://conorosullyds.medium.com/membership?source=post_page-----bb95d783697a--------------------------------)

| [Twitter](https://twitter.com/conorosullyDS) | [YouTube](https://www.youtube.com/channel/UChsoWqJbEjBwrn00Zvghi4w) | [Newsletter](https://mailchi.mp/aa82a5ce1dc0/signup) — 免费注册以获得 [Python SHAP 课程](https://adataodyssey.com/courses/shap-with-python/)

## 参考资料

伯克利，**科学理解 101** [`undsci.berkeley.edu/understanding-science-101/`](https://undsci.berkeley.edu/understanding-science-101/)

科学委员会，**我们对科学的定义**，[`sciencecouncil.org/about-science/our-definition-of-science/`](https://sciencecouncil.org/about-science/our-definition-of-science/)

澳大利亚科学院，**什么是科学？** [`www.science.org.au/curious/people-medicine/what-science`](https://www.science.org.au/curious/people-medicine/what-science)

可汗学院，**科学方法** [`www.khanacademy.org/science/biology/intro-to-biology/science-of-biology/a/the-science-of-biology`](https://www.khanacademy.org/science/biology/intro-to-biology/science-of-biology/a/the-science-of-biology)

微软 Power BI，**什么是数据讲述？** [`powerbi.microsoft.com/en-us/data-storytelling/`](https://powerbi.microsoft.com/en-us/data-storytelling/)
