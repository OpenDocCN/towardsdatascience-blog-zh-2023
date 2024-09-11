# 数据分析师数据清洗指南

> 原文：[https://towardsdatascience.com/data-analyst-guide-to-data-cleaning-6409159ebf3?source=collection_archive---------6-----------------------#2023-08-04](https://towardsdatascience.com/data-analyst-guide-to-data-cleaning-6409159ebf3?source=collection_archive---------6-----------------------#2023-08-04)

## 如何处理不同类型的数据清理

[](https://madfordata.medium.com/?source=post_page-----6409159ebf3--------------------------------)[![Vicky Yu](../Images/54a32f45ebd13a18811912877f60f2f7.png)](https://madfordata.medium.com/?source=post_page-----6409159ebf3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6409159ebf3--------------------------------)[![走向数据科学](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6409159ebf3--------------------------------) [Vicky Yu](https://madfordata.medium.com/?source=post_page-----6409159ebf3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcd08464b29cc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-analyst-guide-to-data-cleaning-6409159ebf3&user=Vicky+Yu&userId=cd08464b29cc&source=post_page-cd08464b29cc----6409159ebf3---------------------post_header-----------) 发表在[走向数据科学](https://towardsdatascience.com/?source=post_page-----6409159ebf3--------------------------------) ·6 分钟阅读·2023 年 8 月 4 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6409159ebf3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-analyst-guide-to-data-cleaning-6409159ebf3&user=Vicky+Yu&userId=cd08464b29cc&source=-----6409159ebf3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6409159ebf3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-analyst-guide-to-data-cleaning-6409159ebf3&source=-----6409159ebf3---------------------bookmark_footer-----------)![](../Images/07a0be03e19ccd76543c5e5b692bc784.png)

图片由[Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=880462)的[Janeke88](https://pixabay.com/users/janeke88-975535/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=880462)提供

尽管有许多资源可供学习技术技能，但很少有资源深入探讨数据清洗——这是数据分析师所需的基本技能。您可能认为可以应用相同的规则来清理数据，但情况并非总是如此。今天，我想分享作为数据分析师多年来在如何处理各种数据以进行数据分析和报告中所学到的经验。

## 数值

数值是指那些对数据分析和报告有用的数值。一个好的经验法则是如果平均值会有用。例如，数值订单号字段的平均值是没有意义的。然而，平均收入金额是有用的。

**以数值字段存储的数字**

对于存储在数值字段中的数字，应用以下清理规则：

1.  计算***最小值***、***最大值***、***中位数***、***99百分位数***和***平均值***。如果***最小值***是负数，但数值应为零或更高，则将其替换为零（如适用）。在下面的销售数据示例中，请注意**第13行**中**$800**的***中位数***与**第12行**中**$20,560**的***平均值***之间的巨大差异。如果中位数和平均值或最大值和99百分位数之间存在较大差异，我……
