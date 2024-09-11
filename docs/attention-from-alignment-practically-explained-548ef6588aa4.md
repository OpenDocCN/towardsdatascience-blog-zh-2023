# 从对齐中学到的注意力机制，实用解读

> 原文：[`towardsdatascience.com/attention-from-alignment-practically-explained-548ef6588aa4?source=collection_archive---------2-----------------------#2023-07-19`](https://towardsdatascience.com/attention-from-alignment-practically-explained-548ef6588aa4?source=collection_archive---------2-----------------------#2023-07-19)

## 学习重要的内容，忽略不相关的部分。

[](https://medium.com/@danielwarfield1?source=post_page-----548ef6588aa4--------------------------------)![Daniel Warfield](https://medium.com/@danielwarfield1?source=post_page-----548ef6588aa4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----548ef6588aa4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----548ef6588aa4--------------------------------) [Daniel Warfield](https://medium.com/@danielwarfield1?source=post_page-----548ef6588aa4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbdc4072cbfdc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fattention-from-alignment-practically-explained-548ef6588aa4&user=Daniel+Warfield&userId=bdc4072cbfdc&source=post_page-bdc4072cbfdc----548ef6588aa4---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----548ef6588aa4--------------------------------) ·11 分钟阅读·2023 年 7 月 19 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F548ef6588aa4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fattention-from-alignment-practically-explained-548ef6588aa4&user=Daniel+Warfield&userId=bdc4072cbfdc&source=-----548ef6588aa4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F548ef6588aa4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fattention-from-alignment-practically-explained-548ef6588aa4&source=-----548ef6588aa4---------------------bookmark_footer-----------)![](img/fddcbc3132d55c17e2ede623de6dbc4b.png)

图片由 [Armand Khoury](https://unsplash.com/@armand_khoury?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如 2017 年的开创性论文[Attention Is All You Need](https://arxiv.org/pdf/1706.03762.pdf)所推广的，注意力机制可以说是当前机器学习领域最重要的架构趋势。最初用于序列到序列建模的注意力机制，现已扩展到几乎所有机器学习的子领域。

本文将描述一种特定的注意力机制，这种机制在变压器风格的注意力机制之前就已存在。我们将讨论它的工作原理以及它的实用性。我们还会回顾一些文献和一个在 PyTorch 中实现这种注意力机制的教程。通过阅读本文，你将对注意力机制作为一种通用概念有更深入的了解，这对探索更前沿的应用非常有用。

# 注意力的理由

注意力机制最初是在[通过共同学习对齐和翻译的神经机器翻译](https://arxiv.org/pdf/1409.0473v7.pdf)(2014)中推广的，这也是本文的指导参考。该论文采用了一个用于英法翻译的编码器-解码器架构。
