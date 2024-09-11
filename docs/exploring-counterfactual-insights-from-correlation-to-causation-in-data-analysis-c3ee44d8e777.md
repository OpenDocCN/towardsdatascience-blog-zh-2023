# 探索反事实洞见：从数据分析中的相关性到因果关系

> 原文：[`towardsdatascience.com/exploring-counterfactual-insights-from-correlation-to-causation-in-data-analysis-c3ee44d8e777?source=collection_archive---------4-----------------------#2023-10-06`](https://towardsdatascience.com/exploring-counterfactual-insights-from-correlation-to-causation-in-data-analysis-c3ee44d8e777?source=collection_archive---------4-----------------------#2023-10-06)

## 反事实在数据科学中的知情决策使用

[](https://medium.com/@MahamsMultiverse?source=post_page-----c3ee44d8e777--------------------------------)![Maham Haroon](https://medium.com/@MahamsMultiverse?source=post_page-----c3ee44d8e777--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c3ee44d8e777--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c3ee44d8e777--------------------------------) [Maham Haroon](https://medium.com/@MahamsMultiverse?source=post_page-----c3ee44d8e777--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F398c9514a58b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-counterfactual-insights-from-correlation-to-causation-in-data-analysis-c3ee44d8e777&user=Maham+Haroon&userId=398c9514a58b&source=post_page-398c9514a58b----c3ee44d8e777---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c3ee44d8e777--------------------------------) ·12 min read·2023 年 10 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc3ee44d8e777&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-counterfactual-insights-from-correlation-to-causation-in-data-analysis-c3ee44d8e777&user=Maham+Haroon&userId=398c9514a58b&source=-----c3ee44d8e777---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc3ee44d8e777&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-counterfactual-insights-from-correlation-to-causation-in-data-analysis-c3ee44d8e777&source=-----c3ee44d8e777---------------------bookmark_footer-----------)![](img/08ac230de223b22fe21ecb9575cf8842.png)

照片由 [Daniele Franchi](https://unsplash.com/@daniele_franchi?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/GbAEJUJKJ88?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

想象一下：一个世界，天空呈现出宁静的柠檬黄色，鸟儿们恢复了理智，用流利的英语优雅地交谈，而果树则违背了重力的法则，展示着深紫罗兰色和电光紫色的叶子，同时在你每一次的愿望下奉上最美味的水果。

于是你会想，终于！这个世界有意义了。

你好啊！

让我们回到现实中来，不过别担心，我们即将踏上一个同样迷人的旅程——反事实的世界。虽然我们最初的愿景可能只是一个愉快的幻想，反事实却打开了一扇通向另一种奇迹的大门，让我们能够探索自己世界中的‘如果’。

‘反事实’这个词可能一开始听起来很复杂，但它的意思只是考虑那些与实际事件相反的情景。尽管这个词本身是在[1946](https://www.merriam-webster.com/dictionary/counterfactual#word-history)年创造的，但这个概念可以追溯到几个世纪前，当时人类首次开始思考‘如果’的情景。

在心理学中，反事实思维常用于探讨与实际情况不同的情景…
