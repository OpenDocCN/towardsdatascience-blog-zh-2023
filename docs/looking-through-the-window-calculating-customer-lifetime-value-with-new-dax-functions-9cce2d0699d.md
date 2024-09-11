# 透过窗口看——使用新的 DAX 函数计算客户生命周期价值

> 原文：[`towardsdatascience.com/looking-through-the-window-calculating-customer-lifetime-value-with-new-dax-functions-9cce2d0699d?source=collection_archive---------13-----------------------#2023-01-03`](https://towardsdatascience.com/looking-through-the-window-calculating-customer-lifetime-value-with-new-dax-functions-9cce2d0699d?source=collection_archive---------13-----------------------#2023-01-03)

## 新的窗口函数是 DAX 语言中最重要的增强功能之一！了解如何利用它们来计算客户生命周期价值。

[](https://datamozart.medium.com/?source=post_page-----9cce2d0699d--------------------------------)![Nikola Ilic](https://datamozart.medium.com/?source=post_page-----9cce2d0699d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9cce2d0699d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9cce2d0699d--------------------------------) [Nikola Ilic](https://datamozart.medium.com/?source=post_page-----9cce2d0699d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F64005b7daa38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flooking-through-the-window-calculating-customer-lifetime-value-with-new-dax-functions-9cce2d0699d&user=Nikola+Ilic&userId=64005b7daa38&source=post_page-64005b7daa38----9cce2d0699d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9cce2d0699d--------------------------------) ·7 分钟阅读·2023 年 1 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9cce2d0699d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flooking-through-the-window-calculating-customer-lifetime-value-with-new-dax-functions-9cce2d0699d&user=Nikola+Ilic&userId=64005b7daa38&source=-----9cce2d0699d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9cce2d0699d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flooking-through-the-window-calculating-customer-lifetime-value-with-new-dax-functions-9cce2d0699d&source=-----9cce2d0699d---------------------bookmark_footer-----------)![](img/6190e47157c687068627731da77f3602.png)

图片来源于 Pexels 上的 Pixabay

在我以前作为 SQL 专业人士的“生涯”中，我广泛使用了 T-SQL 窗口函数来处理各种分析任务。我在[这篇文章](https://data-mozart.com/island-adventures-with-t-sql-window-functions/)中描述了其中一个可能的使用案例，但实际上有数十种场景可以通过使用窗口函数快速而直观地解决。

因此，当我过渡到 Power BI 时，我感到非常惊讶（更不用说失望了），因为没有 DAX 等效于 SQL 窗口函数。好吧，我们本可以通过编写更复杂的 DAX 来解决在一定行集上执行不同计算的这些挑战——但说实话，那确实经常是一个非常痛苦的经历。

所以，当 Power BI Desktop 2022 年 12 月的更新宣布了一组全新的 DAX 函数——统称为窗口函数——它们应该实现与 SQL 窗口函数相同的目标时，我非常激动。到目前为止，有三种 DAX 窗口函数：***OFFSET***、***INDEX*** 和 ***WINDOW***。
