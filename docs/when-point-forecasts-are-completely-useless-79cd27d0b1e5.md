# 当点预测完全无用时

> 原文：[`towardsdatascience.com/when-point-forecasts-are-completely-useless-79cd27d0b1e5?source=collection_archive---------5-----------------------#2023-01-01`](https://towardsdatascience.com/when-point-forecasts-are-completely-useless-79cd27d0b1e5?source=collection_archive---------5-----------------------#2023-01-01)

## 尽管点预测非常流行，但要注意一些不幸的陷阱

[](https://sarem-seitz.medium.com/?source=post_page-----79cd27d0b1e5--------------------------------)![Sarem Seitz](https://sarem-seitz.medium.com/?source=post_page-----79cd27d0b1e5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----79cd27d0b1e5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----79cd27d0b1e5--------------------------------) [Sarem Seitz](https://sarem-seitz.medium.com/?source=post_page-----79cd27d0b1e5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8f6d033b1a40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-point-forecasts-are-completely-useless-79cd27d0b1e5&user=Sarem+Seitz&userId=8f6d033b1a40&source=post_page-8f6d033b1a40----79cd27d0b1e5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----79cd27d0b1e5--------------------------------) ·11 分钟阅读·2023 年 1 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F79cd27d0b1e5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-point-forecasts-are-completely-useless-79cd27d0b1e5&user=Sarem+Seitz&userId=8f6d033b1a40&source=-----79cd27d0b1e5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F79cd27d0b1e5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-point-forecasts-are-completely-useless-79cd27d0b1e5&source=-----79cd27d0b1e5---------------------bookmark_footer-----------)![](img/e63f16578e099928702a3ae2905cb800.png)

图片由 [Kai Pilger](https://unsplash.com/@kaip?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/1k3vsv7iIIc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 引言

[在上一篇文章中](https://www.sarem-seitz.com/why-i-prefer-probabilistic-forecasts-hitting-time-probabilities/)，我们讨论了概率预测相对于点预测的一个优势——即处理超过时间的问题。在这篇文章中，我们将探讨点预测的另一个局限性：更高阶的统计特性。

这些概念对于有数学或统计学背景的人来说会非常熟悉。因此，没有正式培训的读者可能会从这篇文章中获益最多。

在这篇文章结束时，你将更好地了解高阶统计特性如何影响你的预测表现。特别是，我们将看到点预测在没有进一步调整的情况下可能完全失败。

# 点预测失败的两个例子

为了让你对点预测的问题有更多了解，让我们继续两个非常简单的例子。这两个时间序列都适用一个相当简单的自回归数据生成过程。

我们将生成足够的数据，以使自回归梯度提升模型变得合理。因此，我们避免使用过于僵化的模型以及……
