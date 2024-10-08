# 如何使用 Mage 简化数据管道中的沟通

> 原文：[`towardsdatascience.com/how-to-streamline-communication-in-data-pipelines-using-mage-1ab2a885dd73`](https://towardsdatascience.com/how-to-streamline-communication-in-data-pipelines-using-mage-1ab2a885dd73)

## 让机器人处理困难的沟通问题

[](https://medium.com/@xiaoxugao?source=post_page-----1ab2a885dd73--------------------------------)![Xiaoxu Gao](https://medium.com/@xiaoxugao?source=post_page-----1ab2a885dd73--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1ab2a885dd73--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1ab2a885dd73--------------------------------) [Xiaoxu Gao](https://medium.com/@xiaoxugao?source=post_page-----1ab2a885dd73--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1ab2a885dd73--------------------------------) ·6 分钟阅读·2023 年 7 月 13 日

--

![](img/34e20992bb781ce6ecdc54e0203739a7.png)

照片由 [Volodymyr Hryshchenko](https://unsplash.com/@lunarts) 拍摄，来自 [Unsplash](https://unsplash.com/)

你是否曾遇到过这样一种情况：你的下游数据管道因为一个小小的 Google Sheets 手动错误而被阻塞？有时候，这个表格甚至不在你的团队名下，因此你无能为力，只能追着表格的拥有者让他们修复。与此同时，许多其他关键的管道也因此而失败，你还需要处理这些问题。

你感到精疲力竭。最糟糕的是，作为工程师你几乎无能为力。这一切都涉及到无尽的沟通和利益相关者管理。Google Sheets 问题只是各种规模的源问题的一个例子。花一点时间暂停一下，考虑一下一个与你产生共鸣的问题，随着我们深入文章。

改善这种情况的一个关键是**自动化你数据管道中的沟通过程**。如果你的管道已经有了警报机制，那就已经是一个好的开始。然而，警报主要针对数据工程团队，而不是外部团队。

根据我的经验，与源团队或最终用户建立主动沟通同样至关重要，以确保他们对当前情况有足够了解，并可以采取相应措施。在本文中，我将使用 Mage 进行实现，Mage 是一种现代的 Airflow 替代品，因其在解决这些问题方面的有效功能而受到称赞。

## 自动化沟通

工程师的使命之一是自动化。这可以节省我们未来的时间，并且很有趣。没有人喜欢不断地追着数据源团队解决数据问题，或在出现问题时向最终用户逐一解释情况。我们更愿意让机器人来做这些工作。我们可以实现两个层次的自动化：

***及时反馈给数据源团队 —*** 与其手动通知源团队数据问题，不如通过机器人建立一种自动且一致的沟通方式。每当数据测试失败时，类似回调的函数将被触发，通过电子邮件或 Slack 通知源团队，提供详细的失败原因。对于定期运行，将在每次执行时一致地发送警报，直到问题解决。这些警报的重复性质应提高团队的警觉性，并促使他们在无需人工干预的情况下解决问题。

![](img/fc2564729c681c42e8cc48defb7703c6.png)

及时反馈给数据源团队（由作者创建）

***数据用户的数据服务状态页面 —*** 另一方面，我们需要在停机期间处理数据用户的请求。数据团队在数据停机期间经常会收到大量重复的问题。数据服务状态页面的目的是提供一个集中平台来宣布数据问题。鼓励用户首先查看该页面，因为自助服务页面应提供有关当前情况和预期结果的全面而简洁的信息。目标是减少重复票据的提交，从而使数据工程师能够优先处理和集中于最关键的任务。

![](img/0560dda5b170504a25c44b35595be0dd.png)

数据用户的数据服务状态页面（由作者创建）

> 我有一篇专门讲解[数据服务状态页面](https://medium.com/towards-data-science/status-page-for-data-products-we-all-need-one-5a493092059a)的文章。欢迎你自己阅读以了解更多内容。

## 使用回调块在 Mage 中实现

> 以下示例使用 Mage v0.8.100。

接下来，让我们看看如何在数据管道中集成沟通生命周期，使用 Mage。如果你还不熟悉 Mage，Mage 被视为 Airflow 的现代替代品，旨在为数据工程团队带来最佳的开发者体验和最佳的工程实践。Mage 解决了 Airflow 的一些痛点，如本地测试和任务之间的数据传递。它还拥有非常直观的 UI，帮助工程师在几分钟内构建数据管道。

![](img/6bdb3384c88d2876b791a658287937ac.png)

Mage UI（由作者创建）

Mage 中的管道由几种类型的块组成：@data_loader、@transformer、@data_exporter、@sensor、@callback 等。在本文中，我们将使用 [回调块](https://docs.mage.ai/development/blocks/callbacks/overview)。它是一个特殊的块，因为它不会作为单独的步骤运行，而是在父块之后运行。你可以从“附加组件”创建一个回调块，它的外观如下所示。

![](img/0c12f5849e210eae6a2e2ee343be9434.png)

Mage 中的回调块（由作者创建）

每个回调块有两个函数：@callback(‘success’) 和 @callback(‘failure’)。在我们的案例中，@callback(‘failure’) 可用于发送通信消息。`parent_block_data` 包含诸如管道和块名称的元数据，这些数据可用于自定义消息正文。此外，Mage 努力将开发者体验优化到**极致**。UI 使得在不同的块中重用回调块变得非常简单。所选的块将显示在底部。

![](img/1e3edd537cff42db9193bc88368cc5a2.png)

Mage 中的回调块（由作者创建）

相应地，回调块也将在每个父块的底部显示。每个父块可以有一个或多个回调块。

![](img/6f482e7c7191be3a7df46dca186d5a75.png)

Mage 中的回调块（由作者创建）

在父块完成后，如果块运行成功，则触发 `@callback('success')` 函数，否则执行 `@callback('failure')`。

> 值得注意的是，在 v0.8.100 中，@callback(‘failure’) 只有在 @loader、@transformer 或 @data_exporter 执行失败时才会被调用，不包括 @test 部分。如果 @test 失败，@callback(‘failure’) 将不会被触发。但请在你当前使用的版本中验证这一点。

@callback 块简化了数据管道中的通信生命周期的创建。它弥合了数据团队与外部团队之间的沟通差距。在宣布重大数据更改或数据问题时，拥有一致和及时的沟通方式无疑是有利的。但在庆祝成功时，是时候释放你的创造力并邀请用户参与庆祝了。

## 结论

确保数据团队与利益相关者之间的一致和适当沟通是具有挑战性的，因为每个人都有自己喜欢的沟通风格。本文的关键是，自动化沟通可以显著提升数据团队的工作效率。

首先，我们可以为各种类型的数据相关通信创建模板，例如表格弃用或表格模式更改。模板创建好后，下一步是探索自动化过程的方法，确保消息在适当的时刻传递，并且尽可能减少人工干预，这正是我们在本文中看到的。

我希望这个想法能引起你的共鸣，并激发你对在数据团队中实施类似解决方案的兴趣。*我很想知道你对此的看法*，请在下方留言，让我们知道。祝好！
