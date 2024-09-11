# 生成用于数据分析的虚假数据

> 原文：[`towardsdatascience.com/generating-fake-data-for-data-analytics-19cd5ed82a1?source=collection_archive---------7-----------------------#2023-03-14`](https://towardsdatascience.com/generating-fake-data-for-data-analytics-19cd5ed82a1?source=collection_archive---------7-----------------------#2023-03-14)

## 如果你没有真实数据，就得造假数据！

[](https://weimenglee.medium.com/?source=post_page-----19cd5ed82a1--------------------------------)![魏盟·李](https://weimenglee.medium.com/?source=post_page-----19cd5ed82a1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----19cd5ed82a1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----19cd5ed82a1--------------------------------) [魏盟·李](https://weimenglee.medium.com/?source=post_page-----19cd5ed82a1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6599e1e08a48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerating-fake-data-for-data-analytics-19cd5ed82a1&user=Wei-Meng+Lee&userId=6599e1e08a48&source=post_page-6599e1e08a48----19cd5ed82a1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----19cd5ed82a1--------------------------------) ·8 分钟阅读·2023 年 3 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F19cd5ed82a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerating-fake-data-for-data-analytics-19cd5ed82a1&user=Wei-Meng+Lee&userId=6599e1e08a48&source=-----19cd5ed82a1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F19cd5ed82a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerating-fake-data-for-data-analytics-19cd5ed82a1&source=-----19cd5ed82a1---------------------bookmark_footer-----------)![](img/50a67f9bb4a4499679e187efc9c4e02f.png)

图片由 [Leif Christoph Gottwald](https://unsplash.com/@project2204?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在数据分析的世界里，获取一个好的数据集至关重要。在现实世界中，你可能会接触到很多未清理的数据，这些数据通常需要花费一些时间进行清理。如果你没有所需的数据，并且想快速制作一个概念验证的演示怎么办？在这种情况下，你通常需要自己编造数据，同时你的数据还需要具备一定的现实感。那么你怎么做？是辛苦地手动编造数据，还是有一种自动化的方法？

在这篇文章中，我将展示一些很酷的方法来伪造你的数据，并让它们看起来真实！

# 生成名字

要生成一些假名字，你可以使用`names`包。要使用它，你需要首先安装它：

`!pip install names`

现在你可以使用包中的各种函数生成性别特定的名字：

```py
import names

display(names.get_full_name('male'))
display(names.get_first_name())
display(names.get_last_name())

display(names.get_full_name('female'))
display(names.get_first_name())
display(names.get_last_name())
```
