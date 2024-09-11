# 使用（仅）SQL 拟合回归模型的快速而粗糙的方法

> 原文：[https://towardsdatascience.com/linear-regression-in-sql-62eaf4861290?source=collection_archive---------14-----------------------#2023-04-13](https://towardsdatascience.com/linear-regression-in-sql-62eaf4861290?source=collection_archive---------14-----------------------#2023-04-13)

## 你不总是需要 Python 或 R 来拟合模型——Postgresql 已经涵盖了基础知识

[](https://thuwarakesh.medium.com/?source=post_page-----62eaf4861290--------------------------------)[![Thuwarakesh Murallie](../Images/44f1a14a899426592bbd8c7f73ce169d.png)](https://thuwarakesh.medium.com/?source=post_page-----62eaf4861290--------------------------------)[](https://towardsdatascience.com/?source=post_page-----62eaf4861290--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----62eaf4861290--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----62eaf4861290--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F93ce19993bef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-regression-in-sql-62eaf4861290&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=post_page-93ce19993bef----62eaf4861290---------------------post_header-----------) 发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----62eaf4861290--------------------------------) ·4 min read·2023年4月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F62eaf4861290&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-regression-in-sql-62eaf4861290&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=-----62eaf4861290---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F62eaf4861290&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-regression-in-sql-62eaf4861290&source=-----62eaf4861290---------------------bookmark_footer-----------)![](../Images/3e40d3bde4f1c8617942b444355401a4.png)

图片由[Michael Dziedzic](https://unsplash.com/@lazycreekimages?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上。

SQL 程序员很难适应任何机器学习模型。

除非他们具备 Python 或 R 知识，否则其他人将会做这件事。虽然 Python 和 scikit-learn 通常是我进行机器学习的首选工具，但值得注意的是 SQL 也能进行一些快速而粗糙的模型拟合。

回归模型是几乎每个人都需要的常见模型。我记得在高中物理中第一次使用它。

在这种情况下，如果你的数据在 Postgres 表中，你无需离开 SQL 环境即可拟合这些简单的模型。

[](/3-important-sql-optimization-technique-d6da3e9c8442?source=post_page-----62eaf4861290--------------------------------) [## 3个可以即时提升查询速度的SQL优化技巧

### 在完全切换到不同的数据模型之前，可以尝试的一些简单技巧

towardsdatascience.com](/3-important-sql-optimization-technique-d6da3e9c8442?source=post_page-----62eaf4861290--------------------------------)

以下是我们如何实现这一点。

## SQL中的回归建模

Postgres拥有**内置**的工具来处理回归模型。你无需安装或激活任何特殊模块。
