# 在本地使用 CPU 运行 Llama 2 进行文档问答

> 原文：[`towardsdatascience.com/running-llama-2-on-cpu-inference-for-document-q-a-3d636037a3d8?source=collection_archive---------0-----------------------#2023-07-18`](https://towardsdatascience.com/running-llama-2-on-cpu-inference-for-document-q-a-3d636037a3d8?source=collection_archive---------0-----------------------#2023-07-18)

## 清晰的指南，介绍如何在 CPU 上运行量化的开源 LLM 应用程序，使用 Llama 2、C Transformers、GGML 和 LangChain

[](https://kennethleungty.medium.com/?source=post_page-----3d636037a3d8--------------------------------)![Kenneth Leung](https://kennethleungty.medium.com/?source=post_page-----3d636037a3d8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3d636037a3d8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3d636037a3d8--------------------------------) [Kenneth Leung](https://kennethleungty.medium.com/?source=post_page-----3d636037a3d8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdcd08e36f2d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frunning-llama-2-on-cpu-inference-for-document-q-a-3d636037a3d8&user=Kenneth+Leung&userId=dcd08e36f2d0&source=post_page-dcd08e36f2d0----3d636037a3d8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3d636037a3d8--------------------------------) ·11 分钟阅读·2023 年 7 月 18 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3d636037a3d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frunning-llama-2-on-cpu-inference-for-document-q-a-3d636037a3d8&user=Kenneth+Leung&userId=dcd08e36f2d0&source=-----3d636037a3d8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3d636037a3d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frunning-llama-2-on-cpu-inference-for-document-q-a-3d636037a3d8&source=-----3d636037a3d8---------------------bookmark_footer-----------)![](img/887641ba170cdfdd4548d8d2553f96b1.png)

[NOAA](https://unsplash.com/@noaa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 的照片，来自 [Unsplash](https://unsplash.com/s/photos/computing-cloud?orientation=landscape&license=free&utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

像 OpenAI 的 GPT4 这样的第三方商业大语言模型（LLM）提供商通过简单的 API 调用使 LLM 使用变得民主化。然而，团队可能仍然需要在企业范围内进行自我管理或私有部署以满足数据隐私和合规性等各种原因。

开源 LLM 的普及幸运地为我们提供了广泛的选择，从而减少了对这些第三方提供商的依赖。

当我们在本地或云端托管开源模型时，专用计算能力成为关键考虑因素。虽然 GPU 实例可能是最方便的选择，但成本很容易失控。

在这本易于跟随的指南中，我们将探讨如何在本地 CPU 推理上运行开源 LLM 的量化版本，以实现检索增强生成（即文档问答）功能，使用 Python 编程。特别是，我们将利用最新的高性能 [Llama 2](https://ai.meta.com/llama/) 聊天模型。

# 内容

> ***(1)*** *量化的快速入门*…
