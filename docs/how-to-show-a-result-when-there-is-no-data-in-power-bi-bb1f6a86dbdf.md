# 如何在 Power BI 中显示没有数据的结果

> 原文：[https://towardsdatascience.com/how-to-show-a-result-when-there-is-no-data-in-power-bi-bb1f6a86dbdf?source=collection_archive---------4-----------------------#2023-01-05](https://towardsdatascience.com/how-to-show-a-result-when-there-is-no-data-in-power-bi-bb1f6a86dbdf?source=collection_archive---------4-----------------------#2023-01-05)

## *这个标题似乎有些违反直觉。当没有数据时，我为什么还要要求结果？继续了解一下客户的请求以及我如何解决它。*

[](https://medium.com/@salvatorecagliari?source=post_page-----bb1f6a86dbdf--------------------------------)[![Salvatore Cagliari](../Images/a24b0cefab6e707cfee06cde9e857559.png)](https://medium.com/@salvatorecagliari?source=post_page-----bb1f6a86dbdf--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bb1f6a86dbdf--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bb1f6a86dbdf--------------------------------) [Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----bb1f6a86dbdf--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39cccb39e92a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-show-a-result-when-there-is-no-data-in-power-bi-bb1f6a86dbdf&user=Salvatore+Cagliari&userId=39cccb39e92a&source=post_page-39cccb39e92a----bb1f6a86dbdf---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb1f6a86dbdf--------------------------------) · 9分钟阅读 · 2023年1月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbb1f6a86dbdf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-show-a-result-when-there-is-no-data-in-power-bi-bb1f6a86dbdf&user=Salvatore+Cagliari&userId=39cccb39e92a&source=-----bb1f6a86dbdf---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbb1f6a86dbdf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-show-a-result-when-there-is-no-data-in-power-bi-bb1f6a86dbdf&source=-----bb1f6a86dbdf---------------------bookmark_footer-----------)![](../Images/0a11a590227f8428e4480b4174093434.png)

照片由 [Emily Morter](https://unsplash.com/@emilymorter?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

我的一个客户有一个数据集，其中包含任务、子任务以及每个任务的度量。

此外，每个任务都有一个状态。

这些状态是：

+   完成

+   进行中

+   待处理

+   已取消

+   失败

将数据导入 Power BI 后，结果如下所示：

![](../Images/a52ffabefcdfcfaf9d20e6c543bbcdb5.png)

图 1 — 第一个结果（图自作者）

但有时，特定状态下没有任务或子任务。

在这种情况下，结果可能如下所示：

![](../Images/5d5290eff3f4371c087e9e5270f87c45.png)

图 2 — 缺失状态的结果（图由作者提供）
