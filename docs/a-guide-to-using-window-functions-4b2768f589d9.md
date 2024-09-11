# 《BigQuery中的窗口函数指南》

> 原文：[https://towardsdatascience.com/a-guide-to-using-window-functions-4b2768f589d9?source=collection_archive---------18-----------------------#2023-07-21](https://towardsdatascience.com/a-guide-to-using-window-functions-4b2768f589d9?source=collection_archive---------18-----------------------#2023-07-21)

## 在BigQuery中轻松创建运行总计、移动平均和排名。

[](https://medium.com/@thomas.ellyatt?source=post_page-----4b2768f589d9--------------------------------)[![汤姆·艾利亚特](../Images/8756acdd11fef8db9a868820251e7575.png)](https://medium.com/@thomas.ellyatt?source=post_page-----4b2768f589d9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4b2768f589d9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4b2768f589d9--------------------------------) [汤姆·艾利亚特](https://medium.com/@thomas.ellyatt?source=post_page-----4b2768f589d9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F79566ac4b1bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-using-window-functions-4b2768f589d9&user=Tom+Ellyatt&userId=79566ac4b1bb&source=post_page-79566ac4b1bb----4b2768f589d9---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----4b2768f589d9--------------------------------) · 16分钟阅读 · 2023年7月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4b2768f589d9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-using-window-functions-4b2768f589d9&user=Tom+Ellyatt&userId=79566ac4b1bb&source=-----4b2768f589d9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4b2768f589d9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-using-window-functions-4b2768f589d9&source=-----4b2768f589d9---------------------bookmark_footer-----------)![](../Images/e881dc2a741d886c4abc1ac4ba0686bf.png)

照片由[本杰明·沃罗斯](https://unsplash.com/@vorosbenisop?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

如果你曾经搜索过或偶然发现类似于‘*你需要掌握的6个SQL技能*’或‘*我希望早几年就知道的SQL概念*’的内容。窗口函数很可能会在那个列表中占有一席之地。

> 窗口函数非常棒。

本文的目标是帮助你理解这些窗口函数及其用法。一旦完成教程，我还准备了一些可以在你的项目中运行的使用案例，因为我在这些示例中使用了公共数据。

# 我们将涵盖：

+   **什么是** **窗口函数？**

+   **窗口函数的语法**——即分区、排序和框架部分

+   详细了解如何创建7天移动平均线以及其工作原理

+   你可以使用的**聚合**和**窗口函数**是什么？

+   最后，我们将通过几个用例演示如何应用窗口函数。

## 什么**是**窗口函数？
