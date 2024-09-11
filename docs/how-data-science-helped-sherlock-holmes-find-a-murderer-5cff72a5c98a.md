# 数据科学如何帮助福尔摩斯找到凶手

> 使用图论、约束编程和混合整数线性规划解决“谁杀了登斯莫尔公爵”这一谜案

## [](https://medium.com/@bernardovf?source=post_page-----5cff72a5c98a--------------------------------)![伯纳多·富塔多](https://medium.com/@bernardovf?source=post_page-----5cff72a5c98a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5cff72a5c98a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5cff72a5c98a--------------------------------) [伯纳多·富塔多](https://medium.com/@bernardovf?source=post_page-----5cff72a5c98a--------------------------------)

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5cff72a5c98a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-data-science-helped-sherlock-holmes-find-a-murderer-5cff72a5c98a&source=-----5cff72a5c98a---------------------bookmark_footer-----------)![](img/dec485cff44fa5de03d7ae2f2233fda4.png)

原文：[`towardsdatascience.com/how-data-science-helped-sherlock-holmes-find-a-murderer-5cff72a5c98a?source=collection_archive---------5-----------------------#2023-04-28`](https://towardsdatascience.com/how-data-science-helped-sherlock-holmes-find-a-murderer-5cff72a5c98a?source=collection_archive---------5-----------------------#2023-04-28)

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F220a6eda5891&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-data-science-helped-sherlock-holmes-find-a-murderer-5cff72a5c98a&user=Bernardo+Furtado&userId=220a6eda5891&source=post_page-220a6eda5891----5cff72a5c98a---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----5cff72a5c98a--------------------------------) ·14 分钟阅读·2023 年 4 月 28 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5cff72a5c98a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-data-science-helped-sherlock-holmes-find-a-murderer-5cff72a5c98a&user=Bernardo+Furtado&userId=220a6eda5891&source=-----5cff72a5c98a---------------------clap_footer-----------)!

照片由[沃洛迪米尔·赫里申科](https://unsplash.com/@lunarts?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，来源于[Unsplash](https://unsplash.com/photos/ZT9gjcJog6U?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

库存管理、投资组合优化、机器调度、车辆路径规划等许多现实生活中的问题都是数据科学和分析技术可以应用的极好例子。这些都是我们在大学学习的最早的问题之一，也不足为奇。然而，我对可以被表示为抽象实体称为*模型*的众多其他问题非常着迷。这些模型可以是图形、时间表或一组数学方程，并且可以利用适当的技术进行求解。一旦掌握了建模技能，将这些技术应用到供应链问题和简单的谜题中就变得非常简单。

在本文中，我将展示如何使用***图论***、***约束编程***和***混合整数线性规划***来解决丹斯莫尔公爵谋杀案的悬忧。

书籍“*谁杀死了丹斯莫尔公爵*”（[Claude Berge](https://example.org/who_killed_the_duke_of_densmore)著）是一部悬疑小说，重点是识别谁在怀特岛的丹斯莫尔城堡放置了一颗炸弹，导致公爵去世。尽管已经过去十年，这个案件仍然没有被解决…
