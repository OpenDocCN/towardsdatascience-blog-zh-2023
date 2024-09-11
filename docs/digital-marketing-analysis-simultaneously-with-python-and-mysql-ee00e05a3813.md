# 使用Python和MySQL进行数字营销分析

> 原文：[https://towardsdatascience.com/digital-marketing-analysis-simultaneously-with-python-and-mysql-ee00e05a3813?source=collection_archive---------0-----------------------#2023-03-19](https://towardsdatascience.com/digital-marketing-analysis-simultaneously-with-python-and-mysql-ee00e05a3813?source=collection_archive---------0-----------------------#2023-03-19)

## 一项数字营销分析练习，包含在SQL和Python环境中的逐步代码解释

[](https://goncaloggomes.medium.com/?source=post_page-----ee00e05a3813--------------------------------)[![Gonçalo Guimarães Gomes](../Images/3a789a30b0611cef47b6fa7707104b61.png)](https://goncaloggomes.medium.com/?source=post_page-----ee00e05a3813--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ee00e05a3813--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ee00e05a3813--------------------------------) [Gonçalo Guimarães Gomes](https://goncaloggomes.medium.com/?source=post_page-----ee00e05a3813--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Faa25b2c403cc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdigital-marketing-analysis-simultaneously-with-python-and-mysql-ee00e05a3813&user=Gon%C3%A7alo+Guimar%C3%A3es+Gomes&userId=aa25b2c403cc&source=post_page-aa25b2c403cc----ee00e05a3813---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ee00e05a3813--------------------------------) ·15分钟阅读·2023年3月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fee00e05a3813&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdigital-marketing-analysis-simultaneously-with-python-and-mysql-ee00e05a3813&user=Gon%C3%A7alo+Guimar%C3%A3es+Gomes&userId=aa25b2c403cc&source=-----ee00e05a3813---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fee00e05a3813&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdigital-marketing-analysis-simultaneously-with-python-and-mysql-ee00e05a3813&source=-----ee00e05a3813---------------------bookmark_footer-----------)![](../Images/612f64f25069d37873df15d948cd7435.png)

[Zdeněk Macháček](https://unsplash.com/es/@zmachacek?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上的照片

# 介绍

在这段简短的旅程中，我们将探索一个包含基本网站营销指标的数据集，如‘用户’，‘会话’和‘跳出率’，时间跨度为五个月。

本次设置的目的，不是集中在理解网站性能上，而是获得一些基础但仍然有用的知识，以回答一系列必备的运营营销问题。

我们将专注于两个强大且常用的数字工具，探索两种方式，最终引导我们得到相同的结果。

一方面，我们将通过一些多样化的查询来探索**MySQL Workbench**的语法，同时对于每个问题，我们还将使用图形和视觉资源探索**Python**的语法。这两个环境将分别标记为 # MySQL 和 # Python。每个问题都将包含对两种代码的注释和解释，以便更深入地理解。

## # MySQL

```py
-- displaying dataset (case_sql.csv)
SELECT * FROM case_sql;
```
