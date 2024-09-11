# META的Hiera：减少复杂性以提高准确性

> 原文：[https://towardsdatascience.com/metas-hiera-reduce-complexity-to-increase-accuracy-30f7a147ad0b?source=collection_archive---------4-----------------------#2023-06-21](https://towardsdatascience.com/metas-hiera-reduce-complexity-to-increase-accuracy-30f7a147ad0b?source=collection_archive---------4-----------------------#2023-06-21)

## | 人工智能 | 计算机视觉 | VITs |
## | --- | --- | --- |

## 简单性使得人工智能能够达到令人难以置信的性能和惊人的速度

[](https://salvatore-raieli.medium.com/?source=post_page-----30f7a147ad0b--------------------------------)[![Salvatore Raieli](../Images/6bb4520e2df40d20283e7283141b5e06.png)](https://salvatore-raieli.medium.com/?source=post_page-----30f7a147ad0b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----30f7a147ad0b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----30f7a147ad0b--------------------------------) [Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----30f7a147ad0b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff1a08d9452cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmetas-hiera-reduce-complexity-to-increase-accuracy-30f7a147ad0b&user=Salvatore+Raieli&userId=f1a08d9452cd&source=post_page-f1a08d9452cd----30f7a147ad0b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----30f7a147ad0b--------------------------------) · 12 分钟阅读 · 2023年6月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F30f7a147ad0b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmetas-hiera-reduce-complexity-to-increase-accuracy-30f7a147ad0b&user=Salvatore+Raieli&userId=f1a08d9452cd&source=-----30f7a147ad0b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F30f7a147ad0b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmetas-hiera-reduce-complexity-to-increase-accuracy-30f7a147ad0b&source=-----30f7a147ad0b---------------------bookmark_footer-----------)![](../Images/b2ab2597bedfa9804d3e3e8da0d02bb6.png)

图片由 [Alexander Redl](https://unsplash.com/@alexanderredl?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

[卷积网络](https://en.wikipedia.org/wiki/Convolutional_neural_network)在计算机视觉领域已经主导了二十多年。随着[变压器](https://en.wikipedia.org/wiki/Transformer_(machine_learning_model))的到来，人们曾认为它们将会被淘汰。**然而，许多从业者仍然在项目中使用基于卷积的模型。这是为什么呢？**

本文尝试回答以下问题：

+   什么是视觉变换器（Vision Transformers）？

+   它们的局限性是什么？

+   我们能否尝试克服这些问题？

+   为什么以及META Hiera是如何取得成功的？

# 视觉变换器：一张图像值多少个词？
