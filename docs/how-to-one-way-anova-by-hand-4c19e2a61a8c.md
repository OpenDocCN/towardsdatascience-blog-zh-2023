# 如何操作：手动进行单因素方差分析

> 原文：[https://towardsdatascience.com/how-to-one-way-anova-by-hand-4c19e2a61a8c?source=collection_archive---------8-----------------------#2023-08-30](https://towardsdatascience.com/how-to-one-way-anova-by-hand-4c19e2a61a8c?source=collection_archive---------8-----------------------#2023-08-30)

## 学习如何手动进行单因素方差分析（ANOVA），以比较三个或更多组之间的定量测量

[](https://antoinesoetewey.medium.com/?source=post_page-----4c19e2a61a8c--------------------------------)[![Antoine Soetewey](../Images/51d7837d18ff15a62cac2343a485e35d.png)](https://antoinesoetewey.medium.com/?source=post_page-----4c19e2a61a8c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4c19e2a61a8c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4c19e2a61a8c--------------------------------) [Antoine Soetewey](https://antoinesoetewey.medium.com/?source=post_page-----4c19e2a61a8c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fca32a96e6dc7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-one-way-anova-by-hand-4c19e2a61a8c&user=Antoine+Soetewey&userId=ca32a96e6dc7&source=post_page-ca32a96e6dc7----4c19e2a61a8c---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4c19e2a61a8c--------------------------------) ·6 分钟阅读·2023年8月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4c19e2a61a8c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-one-way-anova-by-hand-4c19e2a61a8c&user=Antoine+Soetewey&userId=ca32a96e6dc7&source=-----4c19e2a61a8c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4c19e2a61a8c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-one-way-anova-by-hand-4c19e2a61a8c&source=-----4c19e2a61a8c---------------------bookmark_footer-----------)![](../Images/fe19036a2b1f92b4cab19e94d4f8f34a.png)

图片由 [Rohan Makhecha](https://unsplash.com/@rohanmakhecha?utm_source=medium&utm_medium=referral) 提供

# 介绍

方差分析（ANOVA）是一种统计检验，用于比较组之间的[定量变量](https://statsandr.com/blog/variable-types-and-examples/#quantitative)，以确定几个总体均值之间是否存在统计学上显著的差异。在实践中，它通常用于比较三组或更多组。然而，在理论上，它也可以仅用于两个组。[1](https://statsandr.com/blog/how-to-one-way-anova-by-hand/#fn1)

在之前的帖子中，我们展示了如何在 R 中进行[单因素 ANOVA](https://statsandr.com/blog/anova-in-r/)。在本帖子中，我们通过通常称为“ANOVA 表”的方法，演示了如何手动进行单因素 ANOVA。

# 数据和假设

为了说明这一方法，假设我们从12名学生中抽取一个[样本](https://statsandr.com/blog/what-is-the-difference-between-population-and-sample/)，将他们平分为三个班级（A、B 和 C），并观察他们的年龄。以下是样本：

![](../Images/4164bb0dd850bfd1ef18777059f543e7.png)

作者提供的表格

我们有兴趣比较各班级之间的[总体](https://statsandr.com/blog/what-is-the-difference-between-population-and-sample/)均值。

记住，ANOVA 的原假设是所有均值相等（即，年龄没有…
