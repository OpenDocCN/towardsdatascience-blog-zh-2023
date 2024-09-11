# 是时候开始讨论 LLM 中的提示架构了吗？

> 原文：[`towardsdatascience.com/prompt-architecture-bd8a07117dab?source=collection_archive---------0-----------------------#2023-10-28`](https://towardsdatascience.com/prompt-architecture-bd8a07117dab?source=collection_archive---------0-----------------------#2023-10-28)

## 从提示工程到提示架构

[](https://donatoriccio.medium.com/?source=post_page-----bd8a07117dab--------------------------------)![Donato Riccio](https://donatoriccio.medium.com/?source=post_page-----bd8a07117dab--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bd8a07117dab--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd8a07117dab--------------------------------) [Donato Riccio](https://donatoriccio.medium.com/?source=post_page-----bd8a07117dab--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe384fc71d292&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprompt-architecture-bd8a07117dab&user=Donato+Riccio&userId=e384fc71d292&source=post_page-e384fc71d292----bd8a07117dab---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd8a07117dab--------------------------------) ·7 分钟阅读·2023 年 10 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbd8a07117dab&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprompt-architecture-bd8a07117dab&user=Donato+Riccio&userId=e384fc71d292&source=-----bd8a07117dab---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbd8a07117dab&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprompt-architecture-bd8a07117dab&source=-----bd8a07117dab---------------------bookmark_footer-----------)![](img/f118c2e6e264afa9690d8ddeaca23aba.png)

图片由作者提供。 (AI 生成)

> 总结。

一切始于一个词。

对结果不满意，我们再次尝试。

> 总结文章中的最重要点*。*

提示工程教会我们，更具体的提示更好。

> 确定文章中提出的三个最重要的论点，并根据提供的证据评估作者的论证力度。是否有任何点你觉得论证可以更强或更有说服力？

随着时间的推移，我们学会了包含更多细节，以指导我们最喜欢的 LLM 提供最佳答案。

![](img/4eafc6b535fb758a0ee39002ebc806d7.png)

最近的一种提示架构叫做“从最少到最多提示”。 [1]

提示工程技术变得越来越复杂和精细，有时由许多组件构成。*提示工程*的定义可能在定义这些复杂系统时显得有限。

> 在本文中，我想提出一个更准确的标签，用于多组件系统的接口…
