# PostgreSQL中的窗口函数

> 原文：[https://towardsdatascience.com/window-functions-in-postgresql-788d2ad57c6b?source=collection_archive---------7-----------------------#2023-01-06](https://towardsdatascience.com/window-functions-in-postgresql-788d2ad57c6b?source=collection_archive---------7-----------------------#2023-01-06)

## 任何数据从业者都必须了解的内容

[![Merlin Schäfer](../Images/70108782dda329ca5bbc4cfb5d650e5e.png)](https://ms101196.medium.com/?source=post_page-----788d2ad57c6b--------------------------------) [![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----788d2ad57c6b--------------------------------) [Merlin Schäfer](https://ms101196.medium.com/?source=post_page-----788d2ad57c6b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5a2c8842187b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwindow-functions-in-postgresql-788d2ad57c6b&user=Merlin+Sch%C3%A4fer&userId=5a2c8842187b&source=post_page-5a2c8842187b----788d2ad57c6b---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----788d2ad57c6b--------------------------------) ·8分钟阅读·2023年1月6日

--

![](../Images/6fb505f5f62aafc51e7e278fa8654702.png)

图片来自 [Stephen Dawson](https://unsplash.com/@dawson2406?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

你是否曾经在编写包含多个步骤或子查询的长SQL查询时遇到困难，以计算看似简单的指标？你是否想过是否“可以通过不同的方式沿表计算，以简化操作”？那么，也许是时候深入了解窗口函数了。

窗口函数是 SQL 中一种方便而强大的功能，允许你在多行上进行计算，类似于聚合函数。但不同于使用 `GROUP BY` 的聚合函数，它们不会为一组行返回单一值，而是为集合中的每一行返回一个值。让我们看一个例子：

```py
SELECT
  user_id,
  SUM(sales)
FROM transactions
GROUP BY 
  user_id;
```

```py
SELECT
SUM(sales) OVER(
            PARTITION BY user_id
)
FROM transactions;
```

![](../Images/c630683f642ebee9e71a1a2d75085451.png)

## 你应该在何时以及为什么使用窗口函数？

窗口函数的一个优势是，它们允许你处理结合了聚合和非聚合值的数据，因为这些行不会被合并在一起。这……
