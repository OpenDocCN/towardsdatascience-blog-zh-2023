# 我们是否应该更依赖数据？有时候。

> 原文：[https://towardsdatascience.com/should-we-be-more-data-driven-sometimes-3dcf5e2753ae?source=collection_archive---------3-----------------------#2023-08-17](https://towardsdatascience.com/should-we-be-more-data-driven-sometimes-3dcf5e2753ae?source=collection_archive---------3-----------------------#2023-08-17)

## 何时应该依赖数据，何时数据依赖反而会成为障碍。

[](https://ryi.medium.com/?source=post_page-----3dcf5e2753ae--------------------------------)[![Robert Yi](../Images/69f5d0c5860a73ec291f42d34a74d147.png)](https://ryi.medium.com/?source=post_page-----3dcf5e2753ae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3dcf5e2753ae--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3dcf5e2753ae--------------------------------) [Robert Yi](https://ryi.medium.com/?source=post_page-----3dcf5e2753ae--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8ac2da8b0742&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshould-we-be-more-data-driven-sometimes-3dcf5e2753ae&user=Robert+Yi&userId=8ac2da8b0742&source=post_page-8ac2da8b0742----3dcf5e2753ae---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3dcf5e2753ae--------------------------------) ·6分钟阅读·2023年8月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3dcf5e2753ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshould-we-be-more-data-driven-sometimes-3dcf5e2753ae&user=Robert+Yi&userId=8ac2da8b0742&source=-----3dcf5e2753ae---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3dcf5e2753ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshould-we-be-more-data-driven-sometimes-3dcf5e2753ae&source=-----3dcf5e2753ae---------------------bookmark_footer-----------)

我在Airbnb担任数据科学家时，Covid-19爆发了。正如你所料，Covid-19对于一个依赖良好人际互动的业务来说特别残酷。当世界正在形成孤立的[社交圈](https://www.washingtonpost.com/lifestyle/wellness/pandemic-pod-winter-covid/2020/10/14/214ed65c-0d63-11eb-b1e8-16b59b92b36d_story.html)时，想要让人们住在陌生人的家中是非常困难的。因此，正如你所预期，我们的指标急剧下滑——我们的核心指标下降到了*个位数*的同比值。没有人再预订Airbnb了，*可以肯定的是*，也没有人愿意开设新的Airbnb。

当我们面临那突如其来的指标悬崖时，我们的首席执行官布赖恩迅速作出了令人钦佩的回应。尽管我们都在设置家庭办公室，并从好市多囤积卫生纸和罐头食品，布赖恩却召开了紧急全员大会。他明确告诉我们：“我们所知的旅行已经结束。”他没有明确的下一步计划，但在风暴中却有一个灯塔般的指示：停止一切非关键工作，弄清楚如何在疫情中生存下来。

随后发生的事情令人印象深刻。公司有效地*转变了方向*，在如此大规模的公司中参与其中是非常激动人心的。我们在创纪录的时间内推出了Airbnb在线体验。我们以“近在咫尺即为远方”为新的口号，策划并推动人们前往那些在疫情期间适合作为避难所的地点。明显不符合未来方向的新举措被关闭（我曾参与一个名为“社交住宿”的团队，尽管投入巨大，我们还是迅速终结了这一项目）。我们进行了新的融资，重组了公司。公司每天做出数百甚至上千个决定，因此，成功地在疫情最严重时期游刃有余，表现出尽可能好的灵活性。

话虽如此，尽管业务变动颇具趣味，我实际上更想在这篇文章中讨论这一时期数据的作用以及我们可以从中获得的经验教训。我最令人震惊的认识是：数据，曾经在几乎所有战略对话中扮演关键角色，却在一夜之间变成了次要因素。那时，为了争取“数据驱动决策”而奋斗将会是可笑的——不是因为数据在这一过渡期*没有用*，而是因为在危机中数据不应成为主导。接下来，我将讨论这种思维方式转变的根本原因：紧迫性。让我们考虑不同的决策情况，然后讨论我们应该如何利用数据。是时候真正谈谈“数据驱动”应该意味着什么了。

# 决策的划分

你可以通过两个轴来清晰地划分决策过程：**决策的紧迫性**和**决策的重要性**。根据你的决策在帕内特方格中的位置，分析的参与程度可以并且应该有所不同。

![](../Images/f8f94bf0e663ab823fa8396f3d0b8353.png)

图片由作者提供。

# 低紧迫性，高重要性

一方面，当一个决定极其重要但并不特别紧急时，我们可以按照理想的方式进行分析——与利益相关者紧密迭代，以更好地导航可能的行动空间。例如，假设你公司的高管想要彻底修改你的登陆页面，但他们希望你支持决定应该放置什么内容。你团队中的机器学习软件工程师跳转到卡片分类解决方案，但你和你的利益相关者知道，更关键的决定是是否首先要应用这种分类解决方案。

![](../Images/08fc788d6e6e88c57c6c453f306d6ce0.png)

作者提供的图片。

当前的主页运行良好，因此所需的更改并不紧急，但决定的影响很大——你的更改将影响每一个访客的体验。因此，应该利用分析来更好地导航决策空间：你可以筛选过去的实验，汇总可能有助于当前决策的学习；你可以进行小规模的机会大小检查，以查看任何更改的范围；你可以提供人口统计/渠道/其他分布数据，以更好地了解你可能需要重点关注的内容。

利益相关者必须处理大量的选项，而你可以帮助他们以一种有度量、以假设驱动的方式进行。这就像你在买车一样。花时间进行市场调研是一个好的投资。

# 高紧迫性，高重要性

另一方面，让我们重新考虑上面的 Covid-19 Airbnb 情况。公司正处于危机状态，领导层已经确定了前进的最佳行动方案：我们需要确定一些市场，推向那些对 Covid 隔离所具有吸引力的市场。你可以像之前的例子那样采取相同的方法——仔细分析细分市场，筛选过去的实验结果等。但每推迟一天做出选择，你将失去两样东西：

1.  有机会利用新市场。

1.  有机会进行测试并学到东西。

因此，你提出了一个简单的假设：如果你选择一些与主要城市相对接近的地点，那么你将最大化预订量，因为客人将（a）感到足够隔离于 Covid，但也（b）足够接近以便在紧急情况下能回到家中与朋友和家人团聚。你在几小时内回到高管那里，他们发起了一个推进这些地点的倡议，你发现一些地点效果更佳，从而为你的第二批选择提供了参考。

![](../Images/0ec4c7928f5b16ec9b8b0c5a5f3b1041.png)

作者提供的图片。

在这里分析的最佳参与程度与低紧急情况有所不同——你仍在帮助你的利益相关者在思想迷宫中导航，但所做的决策大多是由直觉驱动的，因此你的参与自然较为浅薄。这并不是说你应该盲目顺从，强化[反应性先例](https://win.hyperquery.ai/p/the-bad-loop-ruining-analytics)——仍然要理解原因，但接受你的参与将会是较少结构化和严谨的。尽管你可以在足够的时间里帮助利益相关者做出*更好的*决策，但你没有足够的时间，**现在**做出80%正确的决策远比明天做出90%正确的决策要有价值得多。

你遇到车祸了。获取一些数据来评估你和对方司机的健康状况，以及到最近医院的最佳路线是有用的，但你可能不应该花几个小时阅读医院评价。

# 低重要性

最后，有时决策实际上并没有那么重要。你在用户支持页面上移动一个按钮，实验没有收敛，但你的利益相关者想知道结果的真相。这时候你应该反驳——分析确实可以提供答案，但结果会改变什么行动？你会学到什么？利益相关者已经*知道*这是一个更好的体验，他们询问是为了确认，但你知道在这种实验曝光水平下，确定性是不可能的。

如果我们的决策没有因为我们的数据工作而[发生变化](https://georgexing.substack.com/p/what-it-means-to-be-data-driven)，或至少我们没有从探索数据中学到些什么，我们可能根本不应该做这项工作。学会预测你的工作的影响——[如果你帮助做出这个决策，潜在的提升是什么？](https://georgexing.substack.com/p/what-it-means-to-be-data-driven)——然后据此行动。

# 最终评论

为了明确，我并不主张在这里做一个严格的截断，但在选择适合任务的分析时，**速度**和**重要性**应当被考虑。当决策非常紧急时，数据几乎总是应该退居二线，依赖直觉。当决策极为重要时，数据应当被更仔细地使用来验证假设，并对直觉进行监督。当决策不重要时，你不应该花很多时间担心这个决策，因此任何分析工作都应该在完成前重新考虑。

*👋 你好！我是罗伯特，* [*Hyperquery*](https://www.hyperquery.ai/) *的首席产品官，曾是数据科学家和分析师。此帖最初发布在* [*Win With Data*](https://win.hyperquery.ai)*，我们每周讨论如何最大化数据的影响。如果你想了解更多关于 Hyperquery 如何帮助你最大化影响的信息，请随时联系我。你可以在* [*LinkedIn*](https://www.linkedin.com/in/robert-yi/) *或* [*Twitter*](https://twitter.com/imrobertyi)*上找到我。*
