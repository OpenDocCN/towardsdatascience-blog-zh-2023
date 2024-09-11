# SQL 窗口函数的解析

> 原文：[`towardsdatascience.com/anatomy-of-sql-window-functions-7256d8cf509a?source=collection_archive---------0-----------------------#2023-03-30`](https://towardsdatascience.com/anatomy-of-sql-window-functions-7256d8cf509a?source=collection_archive---------0-----------------------#2023-03-30)

## 回到基础 | SQL 初学者的基础知识

[](https://iffatm.medium.com/?source=post_page-----7256d8cf509a--------------------------------)![Iffat Malik](https://iffatm.medium.com/?source=post_page-----7256d8cf509a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7256d8cf509a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7256d8cf509a--------------------------------) [Iffat Malik](https://iffatm.medium.com/?source=post_page-----7256d8cf509a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F88491120e677&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanatomy-of-sql-window-functions-7256d8cf509a&user=Iffat+Malik&userId=88491120e677&source=post_page-88491120e677----7256d8cf509a---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7256d8cf509a--------------------------------) ·9 分钟阅读·2023 年 3 月 30 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7256d8cf509a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanatomy-of-sql-window-functions-7256d8cf509a&user=Iffat+Malik&userId=88491120e677&source=-----7256d8cf509a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7256d8cf509a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanatomy-of-sql-window-functions-7256d8cf509a&source=-----7256d8cf509a---------------------bookmark_footer-----------)![](img/cb45c6657e13eb4af98d3e3c9d64e15e.png)

图片由作者提供，创建于 [canva](https://www.canva.com)

为了理解企业数据，你需要频繁查询数据。当我说“频繁”时，我是认真的。处理不熟悉的数据时，常常会感到压力，因此花时间探索和理解数据本身是一个好习惯。掌握基本的数据检索技能固然重要，但了解分析函数以从数据中提取有用的见解就如同锦上添花，而且也很有趣！

我来自数据可视化背景，对我来说，不仅要理解数据，还要找出任何值得关注的发现，以便向更广泛的团队展示。此外，构建复杂的仪表盘往往是一个来回的过程，你需要回到数据源来核对数据，而 SQL 窗口函数始终陪伴着我在数据分析的旅程中。

尽管它们在数据分析中非常有用，但仍然存在一些混淆，人们往往对使用它们感到畏惧。在编写关于 SQL 窗口函数的详细指南时，我意识到内容变得过于描述性，但我又不想跳过任何细节，尤其是与之相关的语法和子句。理解构建模块很重要，对吧？所以，我会尝试在这篇文章中分解窗口函数的构建模块。
