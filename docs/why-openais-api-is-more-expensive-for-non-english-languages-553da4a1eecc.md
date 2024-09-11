# 为什么 OpenAI 的 API 对非英语语言更贵

> 原文：[`towardsdatascience.com/why-openais-api-is-more-expensive-for-non-english-languages-553da4a1eecc?source=collection_archive---------4-----------------------#2023-08-16`](https://towardsdatascience.com/why-openais-api-is-more-expensive-for-non-english-languages-553da4a1eecc?source=collection_archive---------4-----------------------#2023-08-16)

## 超越文字：字节对编码和 Unicode 编码如何影响定价差异

[](https://medium.com/@iamleonie?source=post_page-----553da4a1eecc--------------------------------)![Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----553da4a1eecc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----553da4a1eecc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----553da4a1eecc--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----553da4a1eecc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-openais-api-is-more-expensive-for-non-english-languages-553da4a1eecc&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----553da4a1eecc---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----553da4a1eecc--------------------------------) · 7 分钟阅读 · 2023 年 8 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F553da4a1eecc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-openais-api-is-more-expensive-for-non-english-languages-553da4a1eecc&user=Leonie+Monigatti&userId=3a38da70d8dc&source=-----553da4a1eecc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F553da4a1eecc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-openais-api-is-more-expensive-for-non-english-languages-553da4a1eecc&source=-----553da4a1eecc---------------------bookmark_footer-----------)![](img/7dbc785874f6574c212f692b9a203709.png)

为什么“Hello world”在英语中有两个标记，而在印地语中有 12 个标记？

在发布了[关于如何估算 OpenAI API 成本的近期文章](https://medium.com/towards-data-science/easily-estimate-your-openai-api-costs-with-tiktoken-c17caf6d015e)后，我收到了一条有趣的评论，评论者注意到，OpenAI API 在其他语言中的费用远高于英语，例如使用中文、日文或韩文（CJK）字符的语言。

![](img/8e0c5de69c8cc1d758147b74a591882f.png)

读者对[我最近关于如何估算 OpenAI API 成本的文章](https://medium.com/towards-data-science/easily-estimate-your-openai-api-costs-with-tiktoken-c17caf6d015e)上的评论提到`[tiktoken](https://medium.com/towards-data-science/easily-estimate-your-openai-api-costs-with-tiktoken-c17caf6d015e)` [库](https://medium.com/towards-data-science/easily-estimate-your-openai-api-costs-with-tiktoken-c17caf6d015e)。

我之前没有意识到这个问题，但很快意识到这是一个活跃的研究领域：在今年年初，Petrov 等人发表了一篇名为“语言模型分词器在语言之间引入不公平”的论文[2]，该论文显示“同一文本翻译成不同语言可能会有极大的分词长度差异，在某些情况下差异达到 15 倍”。

作为复习，分词是将文本拆分成一系列标记的过程，这些标记是文本中的常见字符序列。
