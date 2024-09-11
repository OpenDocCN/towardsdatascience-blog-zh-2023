# Meta AI 推出了革命性的图像分割模型，经过 10 亿个掩模的训练。

> 原文：[https://towardsdatascience.com/meta-ai-introduces-revolutionary-image-segmentation-model-trained-on-1-billion-masks-8f13c86a13a2?source=collection_archive---------4-----------------------#2023-04-06](https://towardsdatascience.com/meta-ai-introduces-revolutionary-image-segmentation-model-trained-on-1-billion-masks-8f13c86a13a2?source=collection_archive---------4-----------------------#2023-04-06)

## Segment Anything - 最佳深度学习模型用于图像分割。

[](https://medium.com/@gkeretchashvili?source=post_page-----8f13c86a13a2--------------------------------)[![Gurami Keretchashvili](../Images/4da78f113a0046c2deb8224e09dd9e3d.png)](https://medium.com/@gkeretchashvili?source=post_page-----8f13c86a13a2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8f13c86a13a2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8f13c86a13a2--------------------------------) [Gurami Keretchashvili](https://medium.com/@gkeretchashvili?source=post_page-----8f13c86a13a2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fba1f382fdaca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmeta-ai-introduces-revolutionary-image-segmentation-model-trained-on-1-billion-masks-8f13c86a13a2&user=Gurami+Keretchashvili&userId=ba1f382fdaca&source=post_page-ba1f382fdaca----8f13c86a13a2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8f13c86a13a2--------------------------------) ·7 分钟阅读·2023年4月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8f13c86a13a2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmeta-ai-introduces-revolutionary-image-segmentation-model-trained-on-1-billion-masks-8f13c86a13a2&user=Gurami+Keretchashvili&userId=ba1f382fdaca&source=-----8f13c86a13a2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8f13c86a13a2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmeta-ai-introduces-revolutionary-image-segmentation-model-trained-on-1-billion-masks-8f13c86a13a2&source=-----8f13c86a13a2---------------------bookmark_footer-----------)

# **介绍**

在 OpenAI 的 ChatGPT 在自然语言处理领域取得革命性进展之后，人工智能的进步持续推进，Meta AI 在计算机视觉领域取得了令人惊讶的进展。Meta AI 研究团队介绍了名为 Segment Anything Model (SAM) 的模型以及一个包含 10 亿个掩模和 1100 万张图像的数据集。图像分割是识别图像中哪些像素属于一个对象的过程。

提议的项目主要包括三个支柱：**任务**、**模型**和**数据**。

# **1\. 分割任何任务**

Meta AI 团队的主要目标是创建一个可以处理用户输入提示的图像分割模型，就像 ChatGPT 一样。因此，他们提出了解决方案，将用户输入与图像集成，以生成分割掩码。分割提示可以是任何指示图像中需要分割内容的信息。例如，前景或背景点的集合、一个框、自由格式文本等。因此，模型的输出是给定任何用户定义的提示的有效分割掩码。

# 2\. 分割任何模型

可推广的分割任何模型（SAM）有三个组件，如下图所示。
