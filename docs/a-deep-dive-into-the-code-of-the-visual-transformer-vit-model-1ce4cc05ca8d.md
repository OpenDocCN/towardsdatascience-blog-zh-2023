# 深入探讨视觉变换器（ViT）模型的代码

> 原文：[https://towardsdatascience.com/a-deep-dive-into-the-code-of-the-visual-transformer-vit-model-1ce4cc05ca8d?source=collection_archive---------5-----------------------#2023-08-15](https://towardsdatascience.com/a-deep-dive-into-the-code-of-the-visual-transformer-vit-model-1ce4cc05ca8d?source=collection_archive---------5-----------------------#2023-08-15)

## 解析 HuggingFace ViT 实现

[](https://medium.com/@alexml0123?source=post_page-----1ce4cc05ca8d--------------------------------)[![Alexey Kravets](../Images/3b31f9b3c73c6c7ca709f845e6f70023.png)](https://medium.com/@alexml0123?source=post_page-----1ce4cc05ca8d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1ce4cc05ca8d--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1ce4cc05ca8d--------------------------------) [Alexey Kravets](https://medium.com/@alexml0123?source=post_page-----1ce4cc05ca8d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcf3e4a05b535&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-deep-dive-into-the-code-of-the-visual-transformer-vit-model-1ce4cc05ca8d&user=Alexey+Kravets&userId=cf3e4a05b535&source=post_page-cf3e4a05b535----1ce4cc05ca8d---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----1ce4cc05ca8d--------------------------------) ·14分钟阅读·2023年8月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1ce4cc05ca8d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-deep-dive-into-the-code-of-the-visual-transformer-vit-model-1ce4cc05ca8d&user=Alexey+Kravets&userId=cf3e4a05b535&source=-----1ce4cc05ca8d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1ce4cc05ca8d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-deep-dive-into-the-code-of-the-visual-transformer-vit-model-1ce4cc05ca8d&source=-----1ce4cc05ca8d---------------------bookmark_footer-----------)

Vision Transformer（ViT）在计算机视觉的发展中标志着一个显著的里程碑。ViT挑战了传统观点，即图像最好通过卷积层进行处理，证明了基于序列的注意力机制能够有效捕捉图像中的复杂模式、上下文和语义。通过将图像拆分为可管理的图块并利用自注意力，ViT同时捕捉局部和全局关系，使其在图像分类、物体检测等多种视觉任务中表现优异。本文将深入剖析ViT在分类任务中的工作原理。

![](../Images/ac2ea1bf46319ca84c69d7fe2d300fa5.png)

[https://unsplash.com/photos/aVvZJC0ynBQ](https://unsplash.com/photos/aVvZJC0ynBQ)

# 介绍

ViT的核心思想是将图像视作一系列固定大小的图块，这些图块会被展平并转换为1D向量。这些图块随后由变换器编码器处理，使模型能够捕捉整个图像的全局上下文和依赖关系。通过将图像划分为图块，ViT有效降低了处理大图像的计算复杂性，同时保留了建模复杂空间交互的能力。
