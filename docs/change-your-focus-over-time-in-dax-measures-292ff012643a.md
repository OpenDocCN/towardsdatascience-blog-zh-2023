# 随着时间的推移在 DAX 度量中改变你的关注点

> 原文：[https://towardsdatascience.com/change-your-focus-over-time-in-dax-measures-292ff012643a?source=collection_archive---------9-----------------------#2023-05-19](https://towardsdatascience.com/change-your-focus-over-time-in-dax-measures-292ff012643a?source=collection_archive---------9-----------------------#2023-05-19)

## *如何确定在广告产品时你的投资是否值得*

[](https://medium.com/@salvatorecagliari?source=post_page-----292ff012643a--------------------------------)[![Salvatore Cagliari](../Images/a24b0cefab6e707cfee06cde9e857559.png)](https://medium.com/@salvatorecagliari?source=post_page-----292ff012643a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----292ff012643a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----292ff012643a--------------------------------) [Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----292ff012643a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39cccb39e92a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchange-your-focus-over-time-in-dax-measures-292ff012643a&user=Salvatore+Cagliari&userId=39cccb39e92a&source=post_page-39cccb39e92a----292ff012643a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----292ff012643a--------------------------------) ·7分钟阅读·2023年5月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F292ff012643a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchange-your-focus-over-time-in-dax-measures-292ff012643a&user=Salvatore+Cagliari&userId=39cccb39e92a&source=-----292ff012643a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F292ff012643a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchange-your-focus-over-time-in-dax-measures-292ff012643a&source=-----292ff012643a---------------------bookmark_footer-----------)![](../Images/183d6605f8a88c4d2475b366fc77db29.png)

图片来源：[David Travis](https://unsplash.com/@dtravisphd?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

你如何衡量广告活动的成功？

特别是当你想在时间的推移中宣传不同品牌时？

我的一位客户曾向我提出了类似的问题。

他想分析他品牌的销售情况，并将其与广告品牌进行比较，以了解投资是否值得。

挑战在于更改数据模型，以使该分析成为可能，而不会破坏现有的报告和分析。

# 关注选择的品牌

第一步是制作一个随时间变化的广告品牌表格。

像这样：

![](../Images/c184a414bb34f2b1425bf078828a4c38.png)

图 1 — 每月关注品牌的表格（图由作者提供）

在此表中，你将找到几乎每个月的一个或多个品牌。

现在我可以扩展我的数据模型。

在我的 Contoso 数据模型中的起始点如下（摘自…）
