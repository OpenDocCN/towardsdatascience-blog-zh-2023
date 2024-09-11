# 多模态入门

> 原文：[https://towardsdatascience.com/getting-started-with-multimodality-eab5f6453080?source=collection_archive---------3-----------------------#2023-12-27](https://towardsdatascience.com/getting-started-with-multimodality-eab5f6453080?source=collection_archive---------3-----------------------#2023-12-27)

![](../Images/ae8e7dbea616220acfe1963d16441f60.png)

图像由 Microsoft Designer 创建

## 了解大型多模态模型的视觉能力

[](https://valentinaalto.medium.com/?source=post_page-----eab5f6453080--------------------------------)[![Valentina Alto](../Images/888b8aa17759d8dd5332d8fd4653cf05.png)](https://valentinaalto.medium.com/?source=post_page-----eab5f6453080--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eab5f6453080--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----eab5f6453080--------------------------------) [Valentina Alto](https://valentinaalto.medium.com/?source=post_page-----eab5f6453080--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F341264d69dd4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgetting-started-with-multimodality-eab5f6453080&user=Valentina+Alto&userId=341264d69dd4&source=post_page-341264d69dd4----eab5f6453080---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eab5f6453080--------------------------------) · 9分钟阅读 · 2023年12月27日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Feab5f6453080&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgetting-started-with-multimodality-eab5f6453080&user=Valentina+Alto&userId=341264d69dd4&source=-----eab5f6453080---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Feab5f6453080&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgetting-started-with-multimodality-eab5f6453080&source=-----eab5f6453080---------------------bookmark_footer-----------)

最近在生成 AI 领域的进展使得大型多模态模型（LMMs）的发展成为可能，这些模型可以处理和生成各种类型的数据，如文本、图像、音频和视频。

LMMs 与“标准”大型语言模型（LLMs）共享通常的大型基础模型的泛化和适应能力。然而，LMMs 能够处理超越文本的数据，包括图像、音频和视频。

大型多模态模型最显著的例子之一是 GPT4V(ision)，这是生成预训练变换器（GPT）家族的最新迭代版本。GPT-4 可以执行各种任务，这些任务既需要自然语言理解也需要计算机视觉，例如图像标注、视觉问答、文本生成图像和图像生成文本。

GPT4V（及其更新版本 GPT-4-turbo vision）展示了卓越的能力，包括：

+   对数值问题的数学推理：

![](../Images/22d6be0bb9790388f39c6d37bdf13ce6.png)

作者提供的图像

+   从草图生成代码：
