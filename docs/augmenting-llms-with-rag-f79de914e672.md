# 增强 LLM 的 RAG

> 原文：[`towardsdatascience.com/augmenting-llms-with-rag-f79de914e672?source=collection_archive---------1-----------------------#2023-10-10`](https://towardsdatascience.com/augmenting-llms-with-rag-f79de914e672?source=collection_archive---------1-----------------------#2023-10-10)

## 一个端到端的示例，展示了 LLM 模型在回答与 Amazon SageMaker 相关问题时的表现

[](https://ram-vegiraju.medium.com/?source=post_page-----f79de914e672--------------------------------)![Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----f79de914e672--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f79de914e672--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f79de914e672--------------------------------) [Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----f79de914e672--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6e49569edd2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faugmenting-llms-with-rag-f79de914e672&user=Ram+Vegiraju&userId=6e49569edd2b&source=post_page-6e49569edd2b----f79de914e672---------------------post_header-----------) 发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----f79de914e672--------------------------------) ·9 分钟阅读·2023 年 10 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff79de914e672&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faugmenting-llms-with-rag-f79de914e672&user=Ram+Vegiraju&userId=6e49569edd2b&source=-----f79de914e672---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff79de914e672&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faugmenting-llms-with-rag-f79de914e672&source=-----f79de914e672---------------------bookmark_footer-----------)![](img/553cc94765b38c8f5437dbc15e3856a6.png)

图片来源于[Unsplash](https://unsplash.com/photos/lUSFeh77gcs)

我在 Medium 上写了不少关于不同技术主题的博客，特别是关于[Amazon SageMaker 上的机器学习（ML）模型托管](https://ram-vegiraju.medium.com/list/amazon-sagemaker-f1b06f720fba)。最近，我对不断发展的生成式 AI/大型语言模型生态系统也产生了兴趣（就像行业中的其他人一样，哈哈）。

这两个不同的垂直领域引发了我一个有趣的问题。**我的 Medium 文章在教授 Amazon SageMaker 方面效果如何？** 为了回答这个问题，我决定实现一个生成式 AI 解决方案，该解决方案利用了 [Retrieval Augmented Generation (RAG)](https://docs.aws.amazon.com/sagemaker/latest/dg/jumpstart-foundation-models-customize-rag.html) 并访问我的一些文章，以查看它在回答一些 SageMaker 相关问题时的表现。

在本文中，我们将探讨构建一个端到端的生成式 AI 解决方案，并利用一些流行的工具来使这一工作流变得可操作化：

+   [**LangChain**](https://www.langchain.com/): LangChain 是一个流行的 Python 框架，它通过提供现成的模块来简化生成式 AI 应用，这些模块帮助进行 Prompt Engineering、RAG 实现和 LLM 工作流编排。

+   [**OpenAI**](https://openai.com/): LangChain 将负责我们生成式 AI 应用的编排，但大脑仍然是模型。在这种情况下，我们使用…
