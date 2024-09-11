# 预训练上下文才是你所需要的

> 原文：[`towardsdatascience.com/pre-training-context-is-all-you-need-f457ffa8a358?source=collection_archive---------3-----------------------#2023-11-27`](https://towardsdatascience.com/pre-training-context-is-all-you-need-f457ffa8a358?source=collection_archive---------3-----------------------#2023-11-27)

## 现代变换器模型的驱动力在很大程度上源于其相关数据，这使得强大的上下文学习能力成为可能。

[](https://medium.com/@benjamin.thuerer?source=post_page-----f457ffa8a358--------------------------------)![Benjamin Thürer](https://medium.com/@benjamin.thuerer?source=post_page-----f457ffa8a358--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f457ffa8a358--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f457ffa8a358--------------------------------) [Benjamin Thürer](https://medium.com/@benjamin.thuerer?source=post_page-----f457ffa8a358--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcd27eb9661fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpre-training-context-is-all-you-need-f457ffa8a358&user=Benjamin+Th%C3%BCrer&userId=cd27eb9661fd&source=post_page-cd27eb9661fd----f457ffa8a358---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f457ffa8a358--------------------------------) ·6 min read·2023 年 11 月 27 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff457ffa8a358&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpre-training-context-is-all-you-need-f457ffa8a358&user=Benjamin+Th%C3%BCrer&userId=cd27eb9661fd&source=-----f457ffa8a358---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff457ffa8a358&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpre-training-context-is-all-you-need-f457ffa8a358&source=-----f457ffa8a358---------------------bookmark_footer-----------)

生成型人工智能及其流行的变换器模型如今随处可见，每小时都有新模型发布（见 [AI 的膨胀](https://medium.com/towards-data-science/the-inflation-of-ai-is-more-always-better-8ea1be75e0aa)）。在这个快速发展的人工智能领域，这些模型可能带来的价值似乎是无穷无尽的。像 [chatGPT](http://chat.openai.com) 这样的语言模型已经成为每位工程师的资源库的一部分，作家用它们来支持他们的文章，设计师则利用计算机视觉模型的结果来创作首批视觉作品或寻求灵感。

> 如果这不是魔法，那么究竟是什么让这些令人印象深刻的变换器模型运转？

然而，尽管这些成就和实用性非常高，生成式 AI 提高了生产力，但重要的是要记住，现代机器学习模型（如 LLMs 或 VisionTransformers）根本没有施展任何魔法（类似于 ML 或统计模型总体上从未有过魔法）。即便模型的显著能力可能被认为是*类似魔法*的，一些领域的专家甚至谈论模型的*幻觉*，但每个模型的基础依然只是数学和统计概率（有时复杂，但仍然…
