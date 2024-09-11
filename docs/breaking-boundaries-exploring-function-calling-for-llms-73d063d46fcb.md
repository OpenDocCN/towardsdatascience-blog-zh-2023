# 突破边界：探索 LLM 的函数调用

> 原文：[https://towardsdatascience.com/breaking-boundaries-exploring-function-calling-for-llms-73d063d46fcb?source=collection_archive---------11-----------------------#2023-08-10](https://towardsdatascience.com/breaking-boundaries-exploring-function-calling-for-llms-73d063d46fcb?source=collection_archive---------11-----------------------#2023-08-10)

## 函数调用如何为大型语言模型与外部工具和 API 的无缝集成铺平道路

[](https://dpoulopoulos.medium.com/?source=post_page-----73d063d46fcb--------------------------------)[![迪米特里斯·波洛普洛斯](../Images/ce535a1679779f5a2ec8b024e6691e50.png)](https://dpoulopoulos.medium.com/?source=post_page-----73d063d46fcb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----73d063d46fcb--------------------------------)[![数据科学的前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----73d063d46fcb--------------------------------) [迪米特里斯·波洛普洛斯](https://dpoulopoulos.medium.com/?source=post_page-----73d063d46fcb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7cc87df5b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbreaking-boundaries-exploring-function-calling-for-llms-73d063d46fcb&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=post_page-7cc87df5b1----73d063d46fcb---------------------post_header-----------) 发表在 [数据科学的前沿](https://towardsdatascience.com/?source=post_page-----73d063d46fcb--------------------------------) ·8 分钟阅读·2023年8月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F73d063d46fcb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbreaking-boundaries-exploring-function-calling-for-llms-73d063d46fcb&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=-----73d063d46fcb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F73d063d46fcb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbreaking-boundaries-exploring-function-calling-for-llms-73d063d46fcb&source=-----73d063d46fcb---------------------bookmark_footer-----------)![](../Images/c2db4889afc4841fc06a779401048f6c.png)

图像生成自 SDXL

当我发现大型语言模型（LLMs）获得了与外部工具和 API 互动的能力时，我知道一切都将发生改变。

这是否使我们更接近实现人工通用智能（AGI）？也许不是，但无疑开启了AI的全新时代：一个LLMs可以执行你放入函数中的任何东西的时代。现在，这可能看起来有些异端，但对我来说，这让我们更接近我对AGI的愿景，因为我不幻想有意识的机器或其他科幻叙事。

想象一下：一个AI代理完全负责一个任务，与其他在线代理进行沟通，获取数据，并带回你所期望的结果。这种变革性的能力不仅重新定义了我们与互联网的互动，还重塑了我们的思维方式。

快进几年：想象一下你要计划一次度假。第一步不会是在线搜索票务，而是指示一个AI代理来规划和组织一切，从头到尾。你只会在几个人…
