# 在 Power BI 中切换数据视角

> 原文：[https://towardsdatascience.com/changing-perspective-on-your-data-in-power-bi-4cca965f5fe7?source=collection_archive---------14-----------------------#2023-07-24](https://towardsdatascience.com/changing-perspective-on-your-data-in-power-bi-4cca965f5fe7?source=collection_archive---------14-----------------------#2023-07-24)

## *通常，我们需要的报告页面空间超过了实际可用的空间。但如果我们能在同一页面上切换数据的视角呢？让我们看看怎么做。*

[](https://medium.com/@salvatorecagliari?source=post_page-----4cca965f5fe7--------------------------------)[![Salvatore Cagliari](../Images/a24b0cefab6e707cfee06cde9e857559.png)](https://medium.com/@salvatorecagliari?source=post_page-----4cca965f5fe7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4cca965f5fe7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4cca965f5fe7--------------------------------) [Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----4cca965f5fe7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39cccb39e92a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchanging-perspective-on-your-data-in-power-bi-4cca965f5fe7&user=Salvatore+Cagliari&userId=39cccb39e92a&source=post_page-39cccb39e92a----4cca965f5fe7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4cca965f5fe7--------------------------------) ·7分钟阅读·2023年7月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4cca965f5fe7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchanging-perspective-on-your-data-in-power-bi-4cca965f5fe7&user=Salvatore+Cagliari&userId=39cccb39e92a&source=-----4cca965f5fe7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4cca965f5fe7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchanging-perspective-on-your-data-in-power-bi-4cca965f5fe7&source=-----4cca965f5fe7---------------------bookmark_footer-----------)![](../Images/d7b197ce714a31413dcf17890266e3b3.png)

图片来源：[Nathan Dumlao](https://unsplash.com/@nate_dumlao?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

想象一个包含一些卡片、列和折线图的报告页面。

在页面顶部，你会看到四个按钮：

+   实际数据

+   年初至今（YTD）

+   年末（YE）

+   过去三个月

大致如下：

![](../Images/10d4617855af42712a4f67d57f130120.png)

图1 — 实际数据的模型图（图由作者提供）

当你点击按钮 YTD 时，你的数据将会更改为显示 YTD 结果，从而改变我们结果的视角。

![](../Images/d515dfb091deea0ccbcb2121ceba3bb9.png)

图 2 — YTD 结果的模型（图由作者提供）

这就是我们想要实现的目标。

最后，我们希望根据选择更改结果的格式。
