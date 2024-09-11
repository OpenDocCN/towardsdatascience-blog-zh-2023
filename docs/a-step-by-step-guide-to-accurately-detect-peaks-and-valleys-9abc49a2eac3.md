# 精确检测峰值和谷值的逐步指南。

> 原文：[https://towardsdatascience.com/a-step-by-step-guide-to-accurately-detect-peaks-and-valleys-9abc49a2eac3?source=collection_archive---------1-----------------------#2023-09-25](https://towardsdatascience.com/a-step-by-step-guide-to-accurately-detect-peaks-and-valleys-9abc49a2eac3?source=collection_archive---------1-----------------------#2023-09-25)

## 峰值检测是许多应用中的一个挑战性步骤。阅读并学习如何在 1D 向量和 2D 数组（图像）中精确检测峰值和谷值。

[](https://erdogant.medium.com/?source=post_page-----9abc49a2eac3--------------------------------)[![Erdogan Taskesen](../Images/8e62cdae0502687710d8ae4bbcd8966e.png)](https://erdogant.medium.com/?source=post_page-----9abc49a2eac3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9abc49a2eac3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9abc49a2eac3--------------------------------) [Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----9abc49a2eac3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4e636e2ef813&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-step-by-step-guide-to-accurately-detect-peaks-and-valleys-9abc49a2eac3&user=Erdogan+Taskesen&userId=4e636e2ef813&source=post_page-4e636e2ef813----9abc49a2eac3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9abc49a2eac3--------------------------------) · 13 分钟阅读 · 2023年9月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9abc49a2eac3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-step-by-step-guide-to-accurately-detect-peaks-and-valleys-9abc49a2eac3&user=Erdogan+Taskesen&userId=4e636e2ef813&source=-----9abc49a2eac3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9abc49a2eac3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-step-by-step-guide-to-accurately-detect-peaks-and-valleys-9abc49a2eac3&source=-----9abc49a2eac3---------------------bookmark_footer-----------)![](../Images/d5dafe822d8d4c83b379bcd509324bea.png)

由 [Willian Justen de Vasconcellos](https://unsplash.com/@willianjusten?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄于 [Unsplash](https://unsplash.com/photos/GZLY-0kPG3M?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我们的大脑在峰值检测方面非常出色，但在自动化过程中，眼睛看似简单的任务可能变得具有挑战性。一般而言，峰值和谷值表示（显著的）事件，如价格/成交量的突然增加或减少，或需求的急剧上升。挑战之一是峰值/谷值的定义在不同应用和领域中可能有所不同。其他挑战可能更具技术性，例如噪声信号可能导致许多假阳性，或单一阈值可能无法准确检测局部事件。*在本博客中，我将描述如何准确检测一维向量或二维数组（图像）中的峰值和谷值，而不对峰值形状做任何假设。此外，我还将演示如何处理信号中的噪声。* 分析通过***findpeaks library***进行，并提供了实际操作的示例以供实验。

# 关于峰值和谷值的简要介绍。
