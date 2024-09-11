# GPT 与 BERT：哪个更好？

> 原文：[`towardsdatascience.com/gpt-vs-bert-which-is-better-2f1cf92af21a?source=collection_archive---------0-----------------------#2023-06-23`](https://towardsdatascience.com/gpt-vs-bert-which-is-better-2f1cf92af21a?source=collection_archive---------0-----------------------#2023-06-23)

## 比较两个大规模语言模型：方法和示例

[](https://pranay-dave9.medium.com/?source=post_page-----2f1cf92af21a--------------------------------)![Pranay Dave](https://pranay-dave9.medium.com/?source=post_page-----2f1cf92af21a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2f1cf92af21a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2f1cf92af21a--------------------------------) [Pranay Dave](https://pranay-dave9.medium.com/?source=post_page-----2f1cf92af21a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F89d4bb7ead78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgpt-vs-bert-which-is-better-2f1cf92af21a&user=Pranay+Dave&userId=89d4bb7ead78&source=post_page-89d4bb7ead78----2f1cf92af21a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2f1cf92af21a--------------------------------) · 6 分钟阅读·2023 年 6 月 23 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2f1cf92af21a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgpt-vs-bert-which-is-better-2f1cf92af21a&user=Pranay+Dave&userId=89d4bb7ead78&source=-----2f1cf92af21a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2f1cf92af21a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgpt-vs-bert-which-is-better-2f1cf92af21a&source=-----2f1cf92af21a---------------------bookmark_footer-----------)![](img/948d3d929fc0378773816d8614d45deb.png)

图像由 DALLE 创建，PPT 由作者制作 ([`labs.openai.com/s/qQgpAQbLi0srYlZHOHQjGKWh`](https://labs.openai.com/s/qQgpAQbLi0srYlZHOHQjGKWh))

生成式 AI 的流行也导致了大规模语言模型数量的增加。在这个故事中，我将对比两种模型：GPT 和 BERT。GPT（生成预训练变换器）由 OpenAI 开发，基于仅解码器架构。而 BERT（双向编码器表示变换器）由 Google 开发，是仅编码器的预训练模型。

两者在技术上有所不同，但它们有一个相似的目标——执行自然语言处理任务。许多文章从技术角度对这两者进行比较。然而，在这个故事中，我将根据它们的目标质量进行比较，也就是自然语言处理。

# 比较方法

如何比较两种完全不同的技术架构？GPT 是仅解码器架构，而 BERT 是仅编码器架构。因此，对仅解码器与仅编码器架构的技术比较就像是在比较法拉利与兰博基尼——两者都很出色，但底盘下的技术完全不同。

然而，我们可以基于两者都能执行的共同自然语言任务的质量进行比较——那就是……
