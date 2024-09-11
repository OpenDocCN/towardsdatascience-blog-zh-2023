# 《差分隐私的基础》

> 原文：[`towardsdatascience.com/abcs-of-differential-privacy-8dc709a3a6b3?source=collection_archive---------13-----------------------#2023-04-27`](https://towardsdatascience.com/abcs-of-differential-privacy-8dc709a3a6b3?source=collection_archive---------13-----------------------#2023-04-27)

## 掌握基础

## 理解基本定义和关键原则指南

![Essi Alizadeh](https://medium.ealizadeh.com/?source=post_page-----8dc709a3a6b3--------------------------------) ![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8dc709a3a6b3--------------------------------) [Essi Alizadeh](https://medium.ealizadeh.com/?source=post_page-----8dc709a3a6b3--------------------------------)

·

[阅读](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F35a932e89ec1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fabcs-of-differential-privacy-8dc709a3a6b3&user=Essi+Alizadeh&userId=35a932e89ec1&source=post_page-35a932e89ec1----8dc709a3a6b3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8dc709a3a6b3--------------------------------) · 8 分钟阅读 · 2023 年 4 月 27 日

--

![](img/a9c9c860a5cbeead7b567f365584a0b4.png)

作者提供的图片

差分隐私（DP）是一个严谨的数学框架，它允许分析和处理敏感数据，同时提供强有力的隐私保障。

DP 的基础是一个前提，即单个个体的加入或排除不应显著改变对整个数据集进行的任何分析或查询的结果。换句话说，当比较这两个数据集时，算法应得出可比的结果，这使得很难得知关于该个体的任何独特信息。这种保护措施可以防止私人信息泄露，同时仍能从数据中提取有用的洞见。

差分隐私最初出现在 Cynthia Dwork [1] 在微软研究院工作时所做的研究《Differential Privacy》中。

让我们来看一个示例，以更好地理解差分隐私如何帮助保护数据。

# 差分隐私如何保护数据的示例

## 示例 1

在一个研究社会阶层与健康结果之间关系的研究中，研究人员…
