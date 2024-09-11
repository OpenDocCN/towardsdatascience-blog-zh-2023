# CLIP — 直观且详尽的解释

> 原文：[https://towardsdatascience.com/clip-intuitively-and-exhaustively-explained-1d02c07dbf40?source=collection_archive---------2-----------------------#2023-10-20](https://towardsdatascience.com/clip-intuitively-and-exhaustively-explained-1d02c07dbf40?source=collection_archive---------2-----------------------#2023-10-20)

## 为通用机器学习任务创建强大的图像和语言表示。

[](https://medium.com/@danielwarfield1?source=post_page-----1d02c07dbf40--------------------------------)[![Daniel Warfield](../Images/c1c8b4dd514f6813e08e401401324bca.png)](https://medium.com/@danielwarfield1?source=post_page-----1d02c07dbf40--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1d02c07dbf40--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1d02c07dbf40--------------------------------) [Daniel Warfield](https://medium.com/@danielwarfield1?source=post_page-----1d02c07dbf40--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbdc4072cbfdc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclip-intuitively-and-exhaustively-explained-1d02c07dbf40&user=Daniel+Warfield&userId=bdc4072cbfdc&source=post_page-bdc4072cbfdc----1d02c07dbf40---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1d02c07dbf40--------------------------------) ·17分钟阅读·2023年10月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1d02c07dbf40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclip-intuitively-and-exhaustively-explained-1d02c07dbf40&user=Daniel+Warfield&userId=bdc4072cbfdc&source=-----1d02c07dbf40---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1d02c07dbf40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclip-intuitively-and-exhaustively-explained-1d02c07dbf40&source=-----1d02c07dbf40---------------------bookmark_footer-----------)![](../Images/bb84737af3ff56dd6f70ef16d6a1e9ce.png)

“对比模式”由 Daniel Warfield 使用 MidJourney 制作。所有图像由作者提供，除非另有说明。

在这篇文章中，你将了解“对比语言-图像预训练”（CLIP），这是一种创建视觉和语言表示的方法，效果如此出色，以至于可以用来制作高度特定且高效的分类器，而无需任何训练数据。我们将讨论其理论，CLIP如何不同于传统方法，然后逐步介绍其架构。

![](../Images/b518de3e7bc648a9fde1be23489a9594.png)

CLIP 在从未直接训练过的分类任务中预测高度特定的标签。[来源](https://arxiv.org/pdf/2103.00020.pdf)

**这对谁有用？** 对计算机视觉、自然语言处理（NLP）或多模态建模感兴趣的任何人。

**这篇文章的难度如何？** 这篇文章对初学者数据科学家应该是易于理解的。部分后续章节稍显高级（特别是当我们深入探讨损失函数时）。

**前提条件：** 需要具备一些计算机视觉和自然语言处理的基础知识。

# 典型的图像分类器

在训练模型以检测图像是猫还是狗时，常见的方法是向模型展示一系列图像…
