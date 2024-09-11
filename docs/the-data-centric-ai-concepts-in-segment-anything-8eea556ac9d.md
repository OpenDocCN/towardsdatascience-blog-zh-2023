# 《Segment Anything中的数据中心AI概念》

> 原文：[https://towardsdatascience.com/the-data-centric-ai-concepts-in-segment-anything-8eea556ac9d?source=collection_archive---------8-----------------------#2023-05-31](https://towardsdatascience.com/the-data-centric-ai-concepts-in-segment-anything-8eea556ac9d?source=collection_archive---------8-----------------------#2023-05-31)

## 解读《Segment Anything》中使用的数据中心AI概念，这是第一个用于图像分割的基础模型

[](https://medium.com/@a0987284901?source=post_page-----8eea556ac9d--------------------------------)[![Henry Lai](../Images/eaa1b4eb6f6cebc131f4cf0cfdd4cda7.png)](https://medium.com/@a0987284901?source=post_page-----8eea556ac9d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8eea556ac9d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8eea556ac9d--------------------------------) [Henry Lai](https://medium.com/@a0987284901?source=post_page-----8eea556ac9d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd5548707b59&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-data-centric-ai-concepts-in-segment-anything-8eea556ac9d&user=Henry+Lai&userId=d5548707b59&source=post_page-d5548707b59----8eea556ac9d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8eea556ac9d--------------------------------) ·7分钟阅读·2023年5月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8eea556ac9d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-data-centric-ai-concepts-in-segment-anything-8eea556ac9d&user=Henry+Lai&userId=d5548707b59&source=-----8eea556ac9d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8eea556ac9d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-data-centric-ai-concepts-in-segment-anything-8eea556ac9d&source=-----8eea556ac9d---------------------bookmark_footer-----------)![](../Images/48aef715050a81cdac17d39e62afc269.png)

Segment Anything数据集的构建。图像来源于论文 [https://arxiv.org/pdf/2304.02643.pdf](https://arxiv.org/pdf/2304.02643.pdf)

人工智能（AI）取得了显著的进展，特别是在开发基础模型方面，这些模型通过大量数据进行训练，并能够适应广泛的下游任务。

基础模型的一个显著成功是[大型语言模型 (LLMs)](https://medium.com/towards-data-science/what-are-the-data-centric-ai-concepts-behind-gpt-models-a590071bb727)。这些模型可以以极高的精度执行复杂任务，如语言翻译、文本摘要和问答。

基础模型也开始在计算机视觉领域改变游戏规则。Meta的Segment Anything是一个引起轰动的最新发展。

Segment Anything的成功可以归因于其大规模标注数据集，这在实现其卓越性能中发挥了至关重要的作用。正如[Segment Anything论文](https://arxiv.org/pdf/2304.02643.pdf)中所描述的，该模型架构出奇地简单且轻量。

在本文中，基于我们最近的调查论文[1,2]的见解，我们将通过[数据驱动的人工智能](https://github.com/daochenzha/data-centric-AI)这一数据科学社区中日益增长的概念，更深入地探讨Segment Anything。

## Segment Anything能做什么……
