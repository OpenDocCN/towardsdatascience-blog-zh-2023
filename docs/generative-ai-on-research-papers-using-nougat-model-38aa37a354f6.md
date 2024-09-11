# 使用 Nougat 模型的研究论文生成式 AI

> 原文：[`towardsdatascience.com/generative-ai-on-research-papers-using-nougat-model-38aa37a354f6?source=collection_archive---------5-----------------------#2023-09-20`](https://towardsdatascience.com/generative-ai-on-research-papers-using-nougat-model-38aa37a354f6?source=collection_archive---------5-----------------------#2023-09-20)

## 用数据做一些酷炫的事情！

[](https://priya-dwivedi.medium.com/?source=post_page-----38aa37a354f6--------------------------------)![Priya Dwivedi](https://priya-dwivedi.medium.com/?source=post_page-----38aa37a354f6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----38aa37a354f6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----38aa37a354f6--------------------------------) [Priya Dwivedi](https://priya-dwivedi.medium.com/?source=post_page-----38aa37a354f6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb040ce924438&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerative-ai-on-research-papers-using-nougat-model-38aa37a354f6&user=Priya+Dwivedi&userId=b040ce924438&source=post_page-b040ce924438----38aa37a354f6---------------------post_header-----------) 发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----38aa37a354f6--------------------------------) ·9 min read·2023 年 9 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F38aa37a354f6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerative-ai-on-research-papers-using-nougat-model-38aa37a354f6&user=Priya+Dwivedi&userId=b040ce924438&source=-----38aa37a354f6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F38aa37a354f6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerative-ai-on-research-papers-using-nougat-model-38aa37a354f6&source=-----38aa37a354f6---------------------bookmark_footer-----------)![](img/286ebca3250788f30b4f28ba9521ee43.png)

照片由[Dan Dimmock](https://unsplash.com/@dandimmock?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，来源于[Unsplash](https://unsplash.com/photos/3mt71MKGjQ0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

最近，大型语言模型（LLMs）如 GPT-4 在生成连贯文本方面展现了令人印象深刻的能力。然而，准确解析和理解研究论文仍然是 AI 面临的极具挑战性的任务。研究论文包含复杂的格式、数学方程、表格、图形和领域特定语言。信息密度非常高，重要的语义也被编码在格式中。

在这篇文章中，我将展示 Meta 公司新模型[Nougat](https://facebookresearch.github.io/nougat/)如何帮助准确解析研究论文。然后，我们将其与一个 LLM 管道结合，该管道提取和总结论文中的所有表格。

这里的潜力巨大。很多数据/信息被锁定在未被正确解析的研究论文和书籍中。准确的解析能够使这些数据在包括 LLM 再训练在内的许多不同应用中得到利用。

我制作了一个 YouTube 视频，更详细地解释了代码和我的实验。请查看[这里](https://youtu.be/rYxaijVlc-A)。

# Nougat 模型

Nougat 是 Meta AI 研究人员开发的一种视觉变换器模型，可以将文档页面的图像转换为结构化文本[1]。它能够…
