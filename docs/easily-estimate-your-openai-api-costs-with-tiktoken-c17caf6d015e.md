# 使用 Tiktoken 轻松估算你的 OpenAI API 成本

> 原文：[`towardsdatascience.com/easily-estimate-your-openai-api-costs-with-tiktoken-c17caf6d015e?source=collection_archive---------3-----------------------#2023-08-01`](https://towardsdatascience.com/easily-estimate-your-openai-api-costs-with-tiktoken-c17caf6d015e?source=collection_archive---------3-----------------------#2023-08-01)

## 计算你的令牌，避免因使用 OpenAI API 而破产

[](https://medium.com/@iamleonie?source=post_page-----c17caf6d015e--------------------------------)![Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----c17caf6d015e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c17caf6d015e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c17caf6d015e--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----c17caf6d015e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feasily-estimate-your-openai-api-costs-with-tiktoken-c17caf6d015e&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----c17caf6d015e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c17caf6d015e--------------------------------) ·6 分钟阅读·2023 年 8 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc17caf6d015e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feasily-estimate-your-openai-api-costs-with-tiktoken-c17caf6d015e&user=Leonie+Monigatti&userId=3a38da70d8dc&source=-----c17caf6d015e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc17caf6d015e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feasily-estimate-your-openai-api-costs-with-tiktoken-c17caf6d015e&source=-----c17caf6d015e---------------------bookmark_footer-----------)![](img/286da1233aab3f57c2e2d49f77d8d96e.png)

新鲜的令牌！每千克 $0.0015！

我认识的很多人对使用 OpenAI 的大型语言模型（LLMs）感兴趣。但托管 LLMs 的费用很高，因此像 OpenAI 的应用编程接口（API）这样的推理服务并非免费。但在不知道推理成本会积累到多少的情况下输入支付信息可能会让人感到有些不安。

通常，我喜欢在文章的说明中包含 API 成本的小提示，这样读者就能了解预期，并对推理成本有个概念。

本文介绍了我用来估算 OpenAI 基础模型推理成本的`tiktoken`库。

# 什么是 tiktoken？

`tiktoken`是 OpenAI 开发的开源字节对编码（BPE）分词器，用于对其大型语言模型中的文本进行分词。它允许开发者在调用 OpenAI 接口之前计算文本中的令牌数量。

因此，它有助于估算使用 OpenAI API 的相关费用，因为其费用是**以 1,000 个令牌为单位计费**的，详细信息请参见[OpenAI 的定价页面](https://openai.com/pricing) [1]。
