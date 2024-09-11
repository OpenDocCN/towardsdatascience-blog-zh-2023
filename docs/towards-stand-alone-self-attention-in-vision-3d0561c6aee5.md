# 在视觉中实现独立的自注意力

> 原文：[https://towardsdatascience.com/towards-stand-alone-self-attention-in-vision-3d0561c6aee5?source=collection_archive---------8-----------------------#2023-04-28](https://towardsdatascience.com/towards-stand-alone-self-attention-in-vision-3d0561c6aee5?source=collection_archive---------8-----------------------#2023-04-28)

## *深入探讨变换器架构及其自注意力操作在视觉中的应用*

[](https://medium.com/@ju2ez?source=post_page-----3d0561c6aee5--------------------------------)[![Julian Hatzky](../Images/9f1ce9a29d215feeb5223e8fd659383e.png)](https://medium.com/@ju2ez?source=post_page-----3d0561c6aee5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3d0561c6aee5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3d0561c6aee5--------------------------------) [Julian Hatzky](https://medium.com/@ju2ez?source=post_page-----3d0561c6aee5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe24e3594d8a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-stand-alone-self-attention-in-vision-3d0561c6aee5&user=Julian+Hatzky&userId=e24e3594d8a&source=post_page-e24e3594d8a----3d0561c6aee5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3d0561c6aee5--------------------------------) ·14 分钟阅读·2023年4月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3d0561c6aee5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-stand-alone-self-attention-in-vision-3d0561c6aee5&user=Julian+Hatzky&userId=e24e3594d8a&source=-----3d0561c6aee5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3d0561c6aee5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-stand-alone-self-attention-in-vision-3d0561c6aee5&source=-----3d0561c6aee5---------------------bookmark_footer-----------)![](../Images/b58911b38409853b14032c171382c9bd.png)

图像由作者使用 [craiyon AI](https://www.craiyon.com/) 创建

虽然自注意力在 NLP 中已经广泛应用，并显著提升了最先进模型的性能（例如 [2]，[3]），但越来越多的工作正在致力于在视觉领域实现类似的结果。

尽管有一些混合方法，例如将 CNN 与注意力机制结合[4]，或对图像的块应用线性变换[5]，但由于各种原因，纯粹基于注意力的模型更难有效训练，我们将进一步探讨这些原因。

*《视觉模型中的独立自注意力》* [6] 论文引入了这样一个纯基于注意力的视觉模型的概念。在接下来的部分，我将概述论文的思想和相关的后续工作。此外，我假设你对 [transformer 的工作原理](https://peterbloem.nl/blog/transformers)已经有一定了解，并且对 [CNNs 的基础知识](https://www.youtube.com/watch?v=2hS_54kgMHs)有所掌握。对 [PyTorch](https://pytorch.org/) 的理解对于编码部分也有帮助，但这些部分也可以安全跳过。

*如果你只对代码感兴趣，可以跳过这篇文章，直接查看* [*这个注释过的 Colab 笔记本*](https://colab.research.google.com/drive/1DDjyC3d1R8jgbaP73u6-77FIlnCEN57M?usp=sharing)*。*

## 这篇论文的立场是…
