# 我们能否阻止 LLMs 产生幻觉？

> 原文：[`towardsdatascience.com/can-we-stop-llms-from-hallucinating-17c4ebd652c6?source=collection_archive---------6-----------------------#2023-08-24`](https://towardsdatascience.com/can-we-stop-llms-from-hallucinating-17c4ebd652c6?source=collection_archive---------6-----------------------#2023-08-24)

## 意见

## *普及 LLMs 的最大障碍之一可能是本质上难以解决的。*

[](https://medium.com/@juras.jursenas?source=post_page-----17c4ebd652c6--------------------------------)![Juras Juršėnas](https://medium.com/@juras.jursenas?source=post_page-----17c4ebd652c6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----17c4ebd652c6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----17c4ebd652c6--------------------------------) [Juras Juršėnas](https://medium.com/@juras.jursenas?source=post_page-----17c4ebd652c6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3041473d9e3c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-we-stop-llms-from-hallucinating-17c4ebd652c6&user=Juras+Jur%C5%A1%C4%97nas&userId=3041473d9e3c&source=post_page-3041473d9e3c----17c4ebd652c6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----17c4ebd652c6--------------------------------) · 阅读时间 5 分钟·2023 年 8 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F17c4ebd652c6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-we-stop-llms-from-hallucinating-17c4ebd652c6&user=Juras+Jur%C5%A1%C4%97nas&userId=3041473d9e3c&source=-----17c4ebd652c6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F17c4ebd652c6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-we-stop-llms-from-hallucinating-17c4ebd652c6&source=-----17c4ebd652c6---------------------bookmark_footer-----------)![](img/09bc28622320bd28b5b369807b75f40d.png)

图片由 [Google DeepMind](https://unsplash.com/@googledeepmind?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

虽然大型语言模型（LLMs）已经引起了几乎所有人的关注，但由于它们存在一个相当恼人的方面——这些模型有时会“幻觉”，因此这种技术的大规模应用略显受限。简单来说，它们有时会编造信息，而且最糟的是，这些内容往往看起来非常令人信服。

幻觉，无论频繁与否，都带来了两个主要问题。它们无法直接应用于许多敏感或脆弱的领域，其中一个错误可能会非常昂贵。此外，它还会造成普遍的不信任，因为用户被期望验证 LLM 输出的所有内容，这在一定程度上违背了这项技术的初衷。

学术界似乎也认为幻觉是一个重大问题，因为 2023 年有很多研究论文讨论并试图解决这个问题。然而，我[倾向于同意 Yann LeCun](https://www.youtube.com/watch?v=mViTAXCg1xQ&feature=youtu.be)，Meta 的首席 AI 科学家，他认为幻觉根本无法解决。我们需要对技术进行彻底的重构以消除这个问题。

# 幻觉虚假陈述

我认为，有两个重要方面使得幻觉问题无法解决。首先是相当明显的技术基础，LLM 像其他机器学习模型一样，具有随机性。简单来说，它们做出预测。

虽然它们确实比“被吹捧的自动完成”要先进得多，但底层技术仍然使用关于令牌的统计预测。这既是 LLM 的优点也是缺点。

在强项方面，我们已经看到它们在预测输入后的内容方面是多么出色（假设没有故意破坏输出的尝试）。用户可能会犯几种类型的错误，比如留有错别字、误解单词的意思等，而 LLM 仍然可能给出正确的输出。

当初，首批基于文本的游戏被创造时，用户需要准确输入命令，不能有任何错误或解释的空间。比如，“move north”命令如果用户输入为“move morth”就会出错。然而，LLM 可能能够推测两者的意思。从这个角度来看，这项技术确实非常迷人。

然而，这也展示了一个弱点。任何输入都有一个广泛的令牌选择决策树。简单来说，模型生成输出的方式总是有很多种。在那种广泛的选择范围中，相对较小的部分是“正确”的决定。

尽管有许多[优化选项](https://github.com/hwchase17/langchain)可供选择，但问题本身是不可解决的。例如，如果我们增加提供某个特定答案的可能性，LLM 会变成一个查找表，所以我们希望保持平衡。底层技术仅基于随机预测，因此必须为更广泛的输出令牌提供一些空间。

但 LLMs 在当前状态下不能解决另一个问题。这涉及到更为抽象和虚幻的认识论问题，即研究知识本质的哲学领域。表面上看，这个问题很简单——我们如何知道哪些陈述是真实的，我们如何获得这种知识？毕竟，幻觉只是一些虚假的陈述*事后*产生的，因此如果我们能为模型创建一种方法来验证其是否做出了虚假的陈述并将其删除，这将解决问题。

# 分离幻觉和真实陈述

借鉴哲学的思路，我们可以区分两种可能的陈述——分析性和综合性。前者是通过定义而真实的陈述（最常见的例子之一是“单身汉是未婚男子”）。简单来说，我们可以通过分析语言本身找到真实的陈述，而无需外部经验。

综合性陈述是指通过某种形式的经验来判断真实的陈述，例如“桌子上有一个苹果”。在没有直接经验的情况下，无法知道这样的陈述是否真实。纯粹的语言分析对于判断其真假无济于事。

我应该指出，这些陈述之间的区别在几百年来一直备受争议，但这一讨论对于 LLMs（大型语言模型）来说基本上不相关。正如它们的名字所示，它们是高度先进的语言分析和预测机器。

根据这两种类型的区别，我们可以看到 LLMs 在分析性陈述方面几乎不会有问题（至少不会比人类有更多问题）。然而，它们无法获取经验或大范围的世界知识。它们无法知道某些陈述是否因为事件的存在而真实。

主要问题在于，分析性陈述的数量远小于所有综合性陈述的集合。由于 LLMs 无法验证这些陈述是否真实，我们作为人类必须向它们提供这些信息。

因此，LLMs 面临一个挑战。所有可能输出的集合中总会包含一些综合性陈述，但对于模型来说，它们都是不具备真值的。简单来说，“尤利乌斯·凯撒的刺客是布鲁图斯”（虽然有很多，但在此案例中无关紧要）和“尤利乌斯·凯撒的刺客是亚伯拉罕·林肯”对于模型来说是等同的。

反驳意见可能是我们对这些事件也没有直接经验。我们只是从书籍中读到它们。但对陈述真实性的发现是基于对幸存记录的重建以及广泛的考古证据。

一个简单的（虽然相关性较低）例子是“今天在下雨。” 对于大型语言模型来说，这样的陈述是无法确定其真实性的，因为它需要在查询时接触到现实世界的经验。

从某种意义上说，认识论问题是自我解决的。我们的文学语料库会使“凯撒的刺客是布鲁图斯”这种输出变得显著更可能，因为它出现得更频繁。然而，再次强调的是，这种自我解决的方法依赖于在绝对所有可用文本信息上训练大型语言模型，这显然是不可能的。此外，这也会使其他不那么真实的输出仍然存在于所有可能输出的集合中。

因此，数据质量变得非常重要，但这种质量只能由人类观察者来判断。即使模型在大量数据上进行训练，仍然会有一定的选择过程，这意味着合成陈述的错误率无法消除。

# 结论

我认为，阻止模型产生幻觉的问题是无法解决的。一方面，技术本身基于随机过程，这不可避免地会在大量输出中导致错误的预测。

除了技术难题，还有一个问题是大型语言模型是否可以对陈述做出真实性判断，我再次认为这是不可能的，因为它们无法接触到现实世界。这个问题在很多大型语言模型现在提供的各种搜索引擎功能中稍微得到了缓解，这些功能可以验证某些陈述。

然而，可能会有一种方法是收集一个可以测试陈述的数据库，但这需要超出技术本身的东西，这将我们带回到最初的问题。
