# 偏差和方差之间是否总是存在权衡？

> 原文：[`towardsdatascience.com/is-there-always-a-tradeoff-between-bias-and-variance-5ca44398a552?source=collection_archive---------4-----------------------#2023-02-15`](https://towardsdatascience.com/is-there-always-a-tradeoff-between-bias-and-variance-5ca44398a552?source=collection_archive---------4-----------------------#2023-02-15)

## 不敬的解密者

## 偏差-方差权衡，第一部分，共 3 部分

[](https://kozyrkov.medium.com/?source=post_page-----5ca44398a552--------------------------------)![Cassie Kozyrkov](https://kozyrkov.medium.com/?source=post_page-----5ca44398a552--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5ca44398a552--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5ca44398a552--------------------------------) [Cassie Kozyrkov](https://kozyrkov.medium.com/?source=post_page-----5ca44398a552--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2fccb851bb5e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-there-always-a-tradeoff-between-bias-and-variance-5ca44398a552&user=Cassie+Kozyrkov&userId=2fccb851bb5e&source=post_page-2fccb851bb5e----5ca44398a552---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5ca44398a552--------------------------------) ·5 分钟阅读·2023 年 2 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5ca44398a552&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-there-always-a-tradeoff-between-bias-and-variance-5ca44398a552&user=Cassie+Kozyrkov&userId=2fccb851bb5e&source=-----5ca44398a552---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5ca44398a552&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-there-always-a-tradeoff-between-bias-and-variance-5ca44398a552&source=-----5ca44398a552---------------------bookmark_footer-----------)

你应该阅读这篇文章吗？如果你理解下一节的所有单词，那么不。如果你不关心理解它们，那也不。如果你想了解那些加粗部分的解释，那就应该。

# 偏差-方差权衡

*“偏差-方差权衡”* 是在 [ML/AI](http://bit.ly/quaesita_emperor) 语境中常听到的一个流行词。如果你是一名 [统计学家](http://bit.ly/quaesita_statistics)，你可能会认为这就是总结这个公式：

**均方误差 = 偏差² + 方差**

这不是。

好吧，它确实有一定关联，但这个短语实际上指的是一个*实际的方案*，用于选择模型的**复杂性甜点**。当你在调整**正则化** **超参数**时，它最为有用。

![](img/45a49487f48458ec80ce35b0cf1a3fd5.png)

插图由作者提供。

***注意：*** *如果你从未听说过 MSE，可能需要对一些术语进行一些帮助。当你遇到新的术语时，可以通过点击链接查看我其他文章中对这些词汇的详细介绍。*

# 理解基础知识

均方误差 ([MSE](http://bit.ly/quaesita_babymse)) 是最常用（也是最基础）的模型[损失函数](http://bit.ly/quaesita_emperorm)选择，它倾向于是……
