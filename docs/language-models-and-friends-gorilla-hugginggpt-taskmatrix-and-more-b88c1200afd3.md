# 语言模型及其相关：Gorilla、HuggingGPT、TaskMatrix 以及更多

> 原文：[`towardsdatascience.com/language-models-and-friends-gorilla-hugginggpt-taskmatrix-and-more-b88c1200afd3?source=collection_archive---------9-----------------------#2023-09-04`](https://towardsdatascience.com/language-models-and-friends-gorilla-hugginggpt-taskmatrix-and-more-b88c1200afd3?source=collection_archive---------9-----------------------#2023-09-04)

## 当我们让大型语言模型（LLMs）访问成千上万的深度学习模型时会发生什么？

[](https://wolfecameron.medium.com/?source=post_page-----b88c1200afd3--------------------------------)![Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----b88c1200afd3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b88c1200afd3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b88c1200afd3--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----b88c1200afd3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flanguage-models-and-friends-gorilla-hugginggpt-taskmatrix-and-more-b88c1200afd3&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----b88c1200afd3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b88c1200afd3--------------------------------) ·18 分钟阅读·2023 年 9 月 4 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb88c1200afd3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flanguage-models-and-friends-gorilla-hugginggpt-taskmatrix-and-more-b88c1200afd3&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----b88c1200afd3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb88c1200afd3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flanguage-models-and-friends-gorilla-hugginggpt-taskmatrix-and-more-b88c1200afd3&source=-----b88c1200afd3---------------------bookmark_footer-----------)![](img/6d46238c8cde54e36f1961a4a4509706.png)

（照片由 [Mike Arney](https://unsplash.com/@mikearney?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/rJ5vHo8gr2U?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)）

最近，我们见证了基础模型在深度学习研究中的流行。预训练的大型语言模型（LLMs）引领了一种新范式，在这种范式中，单一模型可以用来——以令人惊讶的成功——解决许多不同的问题。尽管通用 LLMs 很受欢迎，但以任务特定方式微调的模型往往优于利用基础模型的方法。简单来说，*专业化模型仍然很难被超越*！话虽如此，我们可能会开始思考基础模型和专业化深度学习模型的力量是否可以结合。在这次概述中，我们将研究近期将 LLMs 与其他专业化深度学习模型结合的研究，通过学习调用它们相关的 API。所得到的框架将语言模型作为一个集中控制器，用于制定解决复杂 AI 相关任务的计划，并将解决过程中的专业部分委派给更合适的模型。

> “通过仅提供模型描述，HuggingGPT 可以持续且便捷地集成来自 AI 社区的各种专家模型，而无需更改任何结构或提示…
