# 介绍 Python 的魔术方法

> 原文：[`towardsdatascience.com/introducing-pythons-magic-methods-f443ed913703?source=collection_archive---------3-----------------------#2023-05-24`](https://towardsdatascience.com/introducing-pythons-magic-methods-f443ed913703?source=collection_archive---------3-----------------------#2023-05-24)

## PYTHON | 编程

## 一份关于利用魔术方法提高编程能力的实用指南

[](https://david-farrugia.medium.com/?source=post_page-----f443ed913703--------------------------------)![David Farrugia](https://david-farrugia.medium.com/?source=post_page-----f443ed913703--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f443ed913703--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f443ed913703--------------------------------) [David Farrugia](https://david-farrugia.medium.com/?source=post_page-----f443ed913703--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3916826092a6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroducing-pythons-magic-methods-f443ed913703&user=David+Farrugia&userId=3916826092a6&source=post_page-3916826092a6----f443ed913703---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f443ed913703--------------------------------) · 7 分钟阅读 · 2023 年 5 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff443ed913703&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroducing-pythons-magic-methods-f443ed913703&user=David+Farrugia&userId=3916826092a6&source=-----f443ed913703---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff443ed913703&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroducing-pythons-magic-methods-f443ed913703&source=-----f443ed913703---------------------bookmark_footer-----------)![](img/e1d07678085006aa654f8b8a1494a25c.png)

照片由 [Matt Palmer](https://unsplash.com/@mattpalmer?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Python 是一门极好的编程语言，正如 [GitHub](https://octoverse.github.com/2022/top-programming-languages) 所发现的，它也是 2022 年第二受欢迎的编程语言。Python 最具吸引力的优势之一是其强大的社区。似乎 Python 具有任何你可能需要的包。

Python 具有许多不为人知的酷炫功能。如果你有兴趣了解更多，可以随时查看我之前关于这个主题的文章。

[](/5-awesome-python-hidden-features-a0172e0bd98e?source=post_page-----f443ed913703--------------------------------) ## 5 个令人惊叹的 Python 隐藏特性 — 第一部分

### 使用这些酷炫的隐藏 Python 特性将你的编程技能提升到一个新水平。

towardsdatascience.com

在 Python 编程广阔而动态的世界中，存在一组初学者常常忽视但在语言生态系统中具有重要意义的独特函数。这些就是魔法方法（也称为 dunder 函数）。

魔法方法是 Python 中一组预定义的方法，提供了特殊的语法特性。它们可以通过其前后都有双下划线的特征来轻松识别…
