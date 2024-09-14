# 使用（仅）SQL 拟合回归模型的快速而粗糙的方法

> 原文：[`towardsdatascience.com/linear-regression-in-sql-62eaf4861290`](https://towardsdatascience.com/linear-regression-in-sql-62eaf4861290)

## 你并不总是需要 Python 或 R 来拟合模型——Postgresql 已经涵盖了基础内容

[](https://thuwarakesh.medium.com/?source=post_page-----62eaf4861290--------------------------------)![Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----62eaf4861290--------------------------------)[](https://towardsdatascience.com/?source=post_page-----62eaf4861290--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----62eaf4861290--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----62eaf4861290--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----62eaf4861290--------------------------------) ·阅读时间 4 分钟·2023 年 4 月 13 日

--

![](img/3e40d3bde4f1c8617942b444355401a4.png)

照片由[迈克尔·德泽迪克](https://unsplash.com/@lazycreekimages?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

SQL 程序员很少会拟合任何机器学习模型。

除非他们具备 Python 或 R 的知识，否则其他人将会做这件事。虽然 Python 和 scikit-learn 通常是我进行机器学习的首选工具，但值得注意的是，SQL 也能进行一些快速而粗糙的模型拟合。

回归模型是几乎每个人都需要的常见模型。我记得在高中物理课上第一次使用它。

在这种情况下，如果你的数据存储在 Postgres 表中，你不必离开 SQL 环境就能拟合这些简单的模型。

[](/3-important-sql-optimization-technique-d6da3e9c8442?source=post_page-----62eaf4861290--------------------------------) ## 3 SQL 优化技巧，可以瞬间提升查询速度

### 在切换到完全不同的数据模型之前，可以尝试一些简单的技巧

towardsdatascience.com

下面是我们如何做的。

## SQL 中的回归建模

Postgres 具有**内置**的回归模型工具。你无需安装或激活任何特别的模块。

我们可以快速拟合线性回归模型，并利用它们进行预测。

线性回归模型是关于找到一个线性方程来概括数据集。因此，我们只需要找到这条线的截距和斜率。

`regr_slope` 和 `regr_intercept` 函数帮助我们完成这个任务。

假设我们有一个包含降雨量和温度列的表格。我们需要利用降雨量的信息来预测温度列中缺失的值。

这是一个 SELECT 语句，它从天气表中检索降雨量和温度值，其中缺失的温度值通过使用`regr_slope`和`regr_intercept`函数进行预测：

```py
SELECT 
  rainfall, 
  CASE 
    WHEN temperature IS NULL 
    THEN regr_slope(temperature, rainfall) * rainfall 
         + regr_intercept(temperature, rainfall) 
    ELSE temperature 
  END AS temperature
FROM 
  weather;
```

我们使用`CASE`语句来检查温度值是否缺失（即，NULL）。如果缺失，我们使用`regr_slope`和`regr_intercept`函数基于相应的降雨量预测温度值。否则，我们使用原始的温度值。

  ## 这 5 种 SQL 技巧涵盖了约 80%的实际项目

### 加速你的 SQL 学习曲线。

[towardsdatascience.com

## 持久化预测结果

如果我想永久填充表中的缺失值，我可以使用上述代码的稍微修改版本。你可以为预测创建一个物化视图，或将预测插入到一个不同的表中。

```py
-- Populate the table with predicted temperature values
INSERT INTO predicted_temperature (rainfall, temperature)
SELECT 
  t1.rainfall, 
  CASE 
    WHEN t1.temperature IS NULL 
    THEN regr_slope(t2.temperature, t2.rainfall) * t1.rainfall 
         + regr_intercept(t2.temperature, t2.rainfall) 
    ELSE t1.temperature 
  END AS temperature
FROM 
  weather t1
  LEFT JOIN weather t2 ON t1.rainfall = t2.rainfall
WHERE 
  t1.temperature IS NULL;
```

这里是相同的操作，但创建一个物化视图。物化视图只是保存在数据库中的查询及其来自最新运行的结果。

```py
CREATE MATERIALIZED VIEW predicted_temperature_mv AS
SELECT 
  rainfall, 
  CASE 
    WHEN temperature IS NULL 
    THEN regr_slope(temperature, rainfall) * rainfall 
         + regr_intercept(temperature, rainfall) 
    ELSE temperature 
  END AS temperature
FROM 
  weather;
```

  ## Python 到 SQL — 我现在可以以 20 倍的速度加载数据

### 上传大量数据的好、坏、丑的方式

[towardsdatascience.com

## 在 SQL 中拟合回归模型的限制

任何曾经做过回归模型的人都可以证明，上述示例仍需完善。拟合一个正确的模型比我们在这里讨论的要复杂。

尤其在实际情况中，我们应考虑多个自变量来预测因变量。有时我们需要在拟合回归模型时使用多项式阶数，而不仅仅是线性模型。我们可能还需要同时使用这两者。

Postgres 的回归工具无法处理如此复杂的建模。**我们只能构建一个具有一个自变量的线性回归模型。**

  ## SQL on Pandas — 我新的 10 倍速度最爱。

### 将两者的优点结合起来

[towardsdatascience.com

## 结论

线性回归模型可能是用于预测连续数据最常用的模型。数据科学家通常将其作为更复杂的机器学习建模的起点。

尽管我们需要像 Python 这样的编程语言来处理更复杂的机器学习任务，但简单任务如线性回归可以在 SQL 中完成。

我希望这篇文章中讨论的这个小技巧能帮助你在日常工作中。

> 感谢阅读，朋友！如果你喜欢我的文章，欢迎通过[**LinkedIn**](https://www.linkedin.com/in/thuwarakesh/)、[**Twitter**](https://twitter.com/Thuwarakesh)和[**Medium**](https://thuwarakesh.medium.com/)与我保持联系。
> 
> 还不是 Medium 会员？请使用这个链接来[**成为会员**](https://thuwarakesh.medium.com/membership)，因为在不增加你额外费用的情况下，我可以获得一小部分佣金。
