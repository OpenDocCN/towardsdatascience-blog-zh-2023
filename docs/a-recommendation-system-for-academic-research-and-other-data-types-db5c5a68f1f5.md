# 一个用于学术研究（以及其他数据类型）的推荐系统！

> 原文：[`towardsdatascience.com/a-recommendation-system-for-academic-research-and-other-data-types-db5c5a68f1f5?source=collection_archive---------12-----------------------#2023-03-29`](https://towardsdatascience.com/a-recommendation-system-for-academic-research-and-other-data-types-db5c5a68f1f5?source=collection_archive---------12-----------------------#2023-03-29)

## 实现自然语言处理和图论来比较和推荐不同类型的文档

[](https://ben-mccloskey20.medium.com/?source=post_page-----db5c5a68f1f5--------------------------------)![Benjamin McCloskey](https://ben-mccloskey20.medium.com/?source=post_page-----db5c5a68f1f5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----db5c5a68f1f5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----db5c5a68f1f5--------------------------------) [Benjamin McCloskey](https://ben-mccloskey20.medium.com/?source=post_page-----db5c5a68f1f5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F503796fc1483&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-recommendation-system-for-academic-research-and-other-data-types-db5c5a68f1f5&user=Benjamin+McCloskey&userId=503796fc1483&source=post_page-503796fc1483----db5c5a68f1f5---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----db5c5a68f1f5--------------------------------) ·15 分钟阅读·2023 年 3 月 29 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdb5c5a68f1f5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-recommendation-system-for-academic-research-and-other-data-types-db5c5a68f1f5&source=-----db5c5a68f1f5---------------------bookmark_footer-----------)![](img/0dd61847b82293c61235cf84a6d1f226.png)

图片由 [Shubham Dhage](https://unsplash.com/@theshubhamdhage?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

现在许多人开发的项目通常从第一个关键步骤开始：*积极研究*。投资于其他人已经完成的工作并在其基础上进行构建，对你的项目增加价值至关重要。你不仅要从其他人得出的有力结论中学习，还要弄清楚你*不应该做什么*以确保项目的成功。

当我进行论文写作时，我开始收集各种类型的研究文件。例如，我有我阅读过的不同学术出版物的收藏，还有包含不同实验结果的 excel 表格。当我完成论文的研究时，我想知道：**有没有办法创建一个推荐系统，可以比较我档案中的所有研究，并帮助我指导下一项目？**

事实上，确实存在！

*注意：这不仅适用于你可能从各种搜索引擎收集的所有研究的存储库，也适用于*…
