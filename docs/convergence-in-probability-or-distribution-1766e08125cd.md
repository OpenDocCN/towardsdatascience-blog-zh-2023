# 概率或分布收敛

> 原文：[https://towardsdatascience.com/convergence-in-probability-or-distribution-1766e08125cd?source=collection_archive---------10-----------------------#2023-09-04](https://towardsdatascience.com/convergence-in-probability-or-distribution-1766e08125cd?source=collection_archive---------10-----------------------#2023-09-04)

## 这两者之间有什么区别？

[](https://r-shuo-wang.medium.com/?source=post_page-----1766e08125cd--------------------------------)[![Shuo Wang](../Images/17a7299c0a36d9a4c0d07ebfc9d5c282.png)](https://r-shuo-wang.medium.com/?source=post_page-----1766e08125cd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1766e08125cd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1766e08125cd--------------------------------) [Shuo Wang](https://r-shuo-wang.medium.com/?source=post_page-----1766e08125cd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F693d095d5e0d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvergence-in-probability-or-distribution-1766e08125cd&user=Shuo+Wang&userId=693d095d5e0d&source=post_page-693d095d5e0d----1766e08125cd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1766e08125cd--------------------------------) ·6分钟阅读·2023年9月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1766e08125cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvergence-in-probability-or-distribution-1766e08125cd&user=Shuo+Wang&userId=693d095d5e0d&source=-----1766e08125cd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1766e08125cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvergence-in-probability-or-distribution-1766e08125cd&source=-----1766e08125cd---------------------bookmark_footer-----------)![](../Images/38ca6ca8cbc2275769040cd2e887056b.png)

图片由作者提供。

在你学习统计学的过程中，是否遇到过概率收敛和分布收敛的概念？你是否曾经思考过这些概念最初被引入的原因？如果有的话，这个故事旨在帮助你解答其中的一些问题。

## **概率收敛**

让我们从概率收敛的概念开始，因为它是较为简单易懂的概念。假设我们有一个随机变量的序列：*X1*、*X2*、…、*Xn*，当我们让n趋近于无穷大时，如果*Xn*非常接近x的概率趋近于1，我们可以得出*Xn*在概率上收敛于x的结论。

为什么以这种方式定义？这种定义背后的理由在于，无论n变得多大，`Xn`都不会精确等于*x*（常数）。我们能做的最多是指定*Xn*在概率上落在*x*周围的某个区间内的接近程度。

因此，我们的定义断言，随着n趋近于无穷大，*Xn*与*x*的差异大于ε的可能性会减少到一个无穷小的水平，*最终*趋近于零。此外，ε可以任意小。
