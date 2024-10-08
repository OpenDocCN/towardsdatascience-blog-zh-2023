# 使用公共表表达式改进你的 SQL 逻辑

> 原文：[`towardsdatascience.com/improving-your-sql-logic-with-common-table-expressions-26f04967fc86`](https://towardsdatascience.com/improving-your-sql-logic-with-common-table-expressions-26f04967fc86)

## 使逻辑更容易编写、阅读、解释和维护

[](https://medium.com/@elliottstam?source=post_page-----26f04967fc86--------------------------------)![Elliott Stam](https://medium.com/@elliottstam?source=post_page-----26f04967fc86--------------------------------)[](https://towardsdatascience.com/?source=post_page-----26f04967fc86--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----26f04967fc86--------------------------------) [Elliott Stam](https://medium.com/@elliottstam?source=post_page-----26f04967fc86--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----26f04967fc86--------------------------------) ·阅读时长 6 分钟·2023 年 12 月 5 日

--

![](img/6a63bf368229748a912d02f23f3d9489.png)

[Sam Moghadam Khamseh](https://unsplash.com/@sammoghadamkhamseh?utm_source=medium&utm_medium=referral) 的照片，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

把衣物丢进一堆里并寄希望于最好的方法可能不是管理衣物的长期策略。这样很难做到：

`SELECT socks FROM pile` 随后执行。

你肯定会遇到困难：

`SELECT shirt FROM pile WHERE wrinkles = false` 当你需要一件衬衫时。

就像管理你的衣物一样，积极管理 SQL 逻辑的组织有明显的好处。

在这篇文章中，我们将探讨公共表表达式（CTEs）如何改进你的 SQL 逻辑。意大利面条代码很少是好的代码，这在 SQL 中和任何编程语言中都是一样的。在数据管理和工程中没有灵丹妙药。然而，保持你的逻辑有序且易于审查只会对你有帮助。那么，让我们深入了解吧！

## 一个公共表表达式是什么样的？

我有时称它们为 `WITH` 子句。为什么？让我们看一个例子：

一个公共表表达式的示例。

我称它们为 `WITH` 语句，因为它们总是以 … `WITH` 开头。

一目了然，理解发生了什么有多容易？有三个部分：

1.  一个识别“高价值客户”的查询。

1.  一个识别“新客户”的查询。

1.  最终查询阶段执行一个内连接操作，将“高价值客户”和“新客户”进行连接，提供一个结果集，包含仅存在于这两个类别中的客户。

## 一种替代的方法

在我了解公共表表达式（CTEs / `WITH` 语句）之前，这就是我可能会如何处理这个查询：

在有人向我展示 CTE 的价值之前，这就是我编写所有查询的方式。

上面的查询没有什么本质上的问题。实际上，它似乎只是代码的重新组织。

但有时顺序很重要。

顺序，事情的顺序有时确实很重要（谢谢，尤达）。

我的观点是，CTE 方法更容易处理和解释。在阅读 CTE 查询时，我清楚地知道故事是：

1.  我们有高价值客户。

1.  我们有新客户。

1.  我们的结果将返回两个客户集的内连接。

这是一个简单明了的故事。第二个查询也不复杂，但这是我阅读它的体验：

1.  我们有一个客户。

1.  我们有总收入（可能是针对客户的）。

1.  我们有自上次见面以来的天数（可能是针对客户的）。

1.  我们有一个子查询派生出满足某些收入门槛的客户。

1.  我们将对另一个子查询执行内连接……

1.  另一个子查询派生出在过去 90 天内首次出现的客户。

那个故事仍然足够简单易懂，但我需要六个思考步骤来拼凑这个故事，而只需三个步骤就能拼凑出 CTE 版本。我是个简单的人——将工作量减少一半对我来说意义重大。

但说真的，实际上我发现 CTE 方法将逻辑模块化，使得定义 SQL 拼图的小部分变得容易，从而能够在最终查询阶段轻松连接这些拼图。

我们上面看到的是一个玩具示例。在现实世界中，复杂问题通常需要更复杂的逻辑。寻找将复杂问题（或复杂查询）拆分成更小部分的方法是有帮助的。这样，一个复杂的问题可能变成几个简单的问题。如果你能够轻松解决几个简单的问题，那么你就成功地为复杂问题开发了一个简单的解决方案。

## 总结好处

根据我的经验，这些是将公共表表达式纳入 SQL 工作流程的核心好处：

1.  逻辑的组织。

1.  逻辑和查询目的的可读性。

1.  逻辑的可维护性。

1.  逻辑的可解释性。

1.  性能和可扩展性（这是什么？）

## 1：逻辑的组织

公共表表达式为我们的查询提供了结构。类似于段落为书页提供结构，房间为房子提供结构，函数为程序提供结构，或者……你懂我的意思。

拥有可靠和可预测的模式在提高查询效率方面是绝对有利的。

## 2：逻辑和查询目的的可读性

查询中的结构组织有助于使其更具可读性。你是否尝试过阅读一本 1800 年代的书？做个快速的 Google 搜索。那些句子有的可能会延续好几*页*。

无结构的 SQL 查询就像非常非常长的句子：很难跟踪全部内容。一种思路在哪里结束，另一种思路从哪里开始？

保持逻辑的可读性是一个巨大的胜利。这对你自己以及任何将来查看你查询的人来说都是一个胜利。你的查询中的逻辑不应是一个让艾伦·图灵夜不能寐的加密算法。它们应该是如此简单，以至于非技术人员会想，“嘿，我想我知道这在做什么”。

## 3: 逻辑的可维护性

当你的查询易于阅读时，它们也容易维护。你不想在写完查询三个月后回来，却要花几个小时来解读这个该死的东西是怎么工作的。

即使是好的查询也很可能需要随着时间的推移进行适应和演变。好的查询应该容易理解、调试，并能扩展以供进一步使用。

公共表表达式使你可以轻松地将逻辑拆分为小块。这提高了查询的可维护性，因为你可以隔离逻辑的特定组件。例如，你可能有一个 500 行的查询，但由于你组织得很好，你知道实现一个新功能只需编辑你 CTE 的特定部分，而这仅涉及 20 行代码。

## 4: 逻辑的可解释性

结构化、可读的逻辑往往比一堆意大利面条更容易解释。无论是文档、项目交接还是好奇的业务用户挑剔你的实现：如果你的逻辑做了任何重要的事情，你总会需要解释它。

公共表表达式使你可以轻松地将逻辑拆分为小而易于解释的部分。当你的利益相关者理解这些小部分时，你就非常接近赢得他们对整体大局的理解。

## 5: 性能和可扩展性

从技术上讲，公共表表达式的性能并不比一系列内联子查询更好。我们之前的两个示例在性能上应该是相等的。

但在性能的战场上，我们并不讲究公平。这将使你在数据工程师中脱颖而出：你是否知道如何在你的 favor 中不公平地倾斜天平？公共表表达式可以帮助显而易见地识别在许多工作流中实现的公共查询组件。这为自动化或 ETL 优化的效率打开了大门，允许一次执行该查询，并在多个地方重用其结果集。

你还会发现，在采用公共表表达式后，你会有一个“库”来存储查询组件。你上周写的一部分查询可能与今天你正在处理的新业务用例相关。当你的查询使用公共表表达式组织和结构化得很好时，很容易从你已经解决的问题中提取相关的逻辑片段，并将其插入到需要解决的新问题中。

## 接下来是什么？

使 SQL 查询正常工作只是开始。设置它以在其生命周期内提供最大价值（和最小头痛）才是关键所在。

回顾一下你以前的一些查询。它们易于理解吗？易于解释和维护吗？如果答案是否定的，那么不妨尝试一下我们在这里讨论的内容。

将你的逻辑组织成一个公共表表达式。将其拆分成更易于管理的逻辑片段。将你的更改给同事看看，看看他们的反应。取用对你有效的部分并加以运用！

## 总结

本文到此结束。既然你读到了这里，希望你觉得这篇文章有用且有趣！祝你在编写高质量 SQL 时好运。

如果你想要更多内容，请查看我的[YouTube 频道](https://www.youtube.com/@devyx)！给我留言，告诉我你想探索哪些其他数据工程主题。
