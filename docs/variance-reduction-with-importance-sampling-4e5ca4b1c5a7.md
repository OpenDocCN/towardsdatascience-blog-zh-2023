# 方差减少与重要性抽样

> 原文：[https://towardsdatascience.com/variance-reduction-with-importance-sampling-4e5ca4b1c5a7?source=collection_archive---------17-----------------------#2023-01-23](https://towardsdatascience.com/variance-reduction-with-importance-sampling-4e5ca4b1c5a7?source=collection_archive---------17-----------------------#2023-01-23)

## 数学解释与Python实现

[](https://medium.com/@hrmnmichaels?source=post_page-----4e5ca4b1c5a7--------------------------------)[![Oliver S](../Images/b5ee0fa2d5fb115f62e2e9dfcb92afdd.png)](https://medium.com/@hrmnmichaels?source=post_page-----4e5ca4b1c5a7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4e5ca4b1c5a7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4e5ca4b1c5a7--------------------------------) [Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----4e5ca4b1c5a7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff2daf6260cca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvariance-reduction-with-importance-sampling-4e5ca4b1c5a7&user=Oliver+S&userId=f2daf6260cca&source=post_page-f2daf6260cca----4e5ca4b1c5a7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4e5ca4b1c5a7--------------------------------) ·6分钟阅读·2023年1月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4e5ca4b1c5a7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvariance-reduction-with-importance-sampling-4e5ca4b1c5a7&user=Oliver+S&userId=f2daf6260cca&source=-----4e5ca4b1c5a7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4e5ca4b1c5a7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvariance-reduction-with-importance-sampling-4e5ca4b1c5a7&source=-----4e5ca4b1c5a7---------------------bookmark_footer-----------)![](../Images/08c4a7efaed06bcd1a42a2a74b87d3d5.png)

照片由 [Edge2Edge Media](https://unsplash.com/@edge2edgemedia?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/uKlneQRwaxY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在之前的帖子中，我介绍了[不同的数值抽样技术](https://medium.com/towards-data-science/introduction-to-sampling-methods-c934b64b6b08)，其中之一是重要性抽样。在那篇帖子中，我们使用了这种技术来允许从复杂的分布中抽样，否则从这些分布中抽样是不切实际的。然而，重要性抽样还常用于另一个原因，即方差减少：即通过选择合适的提议分布，我们可以减少估计器的方差——我们将在这里讨论这个问题。

# 重要性抽样回顾

假设我们不仅想计算随机变量`X`的期望`E[X]`，而是计算该变量函数`f[X]`的期望。在连续情况下，这可以这样计算：

我们可以使用数值近似，也称为蒙特卡洛方法，通过从分布`p`中抽取`n`个随机值来逼近这个期望，然后计算样本均值：

重要性抽样的想法是使用一个简单的重新表述技巧，将期望写作

— 通过给出`f(x)p(x)/q(x)`在分布`q`上的期望！这样，我们可以通过从`q`中抽样来计算样本均值：
