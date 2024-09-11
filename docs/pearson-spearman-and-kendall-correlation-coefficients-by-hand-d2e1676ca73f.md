# 皮尔逊、斯皮尔曼和肯德尔相关系数的手动计算

> 原文：[https://towardsdatascience.com/pearson-spearman-and-kendall-correlation-coefficients-by-hand-d2e1676ca73f?source=collection_archive---------6-----------------------#2023-09-05](https://towardsdatascience.com/pearson-spearman-and-kendall-correlation-coefficients-by-hand-d2e1676ca73f?source=collection_archive---------6-----------------------#2023-09-05)

## 了解如何手动计算皮尔逊、斯皮尔曼和肯德尔相关系数，以评估两个变量之间的关系

[](https://antoinesoetewey.medium.com/?source=post_page-----d2e1676ca73f--------------------------------)[![Antoine Soetewey](../Images/51d7837d18ff15a62cac2343a485e35d.png)](https://antoinesoetewey.medium.com/?source=post_page-----d2e1676ca73f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d2e1676ca73f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d2e1676ca73f--------------------------------) [Antoine Soetewey](https://antoinesoetewey.medium.com/?source=post_page-----d2e1676ca73f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fca32a96e6dc7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpearson-spearman-and-kendall-correlation-coefficients-by-hand-d2e1676ca73f&user=Antoine+Soetewey&userId=ca32a96e6dc7&source=post_page-ca32a96e6dc7----d2e1676ca73f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d2e1676ca73f--------------------------------) ·15 分钟阅读·2023年9月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd2e1676ca73f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpearson-spearman-and-kendall-correlation-coefficients-by-hand-d2e1676ca73f&user=Antoine+Soetewey&userId=ca32a96e6dc7&source=-----d2e1676ca73f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd2e1676ca73f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpearson-spearman-and-kendall-correlation-coefficients-by-hand-d2e1676ca73f&source=-----d2e1676ca73f---------------------bookmark_footer-----------)![](../Images/84af2666955f203411e8972ba8d5e235.png)

图片由 [S O C I A L . C U T](https://unsplash.com/@socialcut?utm_source=medium&utm_medium=referral) 提供

# 介绍

在统计学中，相关系数用于评估两个变量之间的关系。

在之前的帖子中，我们展示了如何在[R中计算相关系数并进行相关性测试](https://statsandr.com/blog/correlation-coefficient-and-correlation-test-in-r/)。在这篇帖子中，我们展示了如何手动计算皮尔逊、斯皮尔曼和肯德尔相关系数，以及在两种不同的情境下（即，有并列情况和没有并列情况）进行计算。

# 数据

为了展示有并列情况和没有并列情况的方法，我们考虑了两个不同的数据集，一个有并列情况，另一个没有并列情况。

# 有并列情况

对于带有并列情况的情景示例，假设我们有一个大小为5的样本：

![](../Images/d2c80572dfc1f0b580dbc83eff69a83e.png)

作者提供的表格
