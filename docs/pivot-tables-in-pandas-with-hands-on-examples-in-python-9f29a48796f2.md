# Pandas中的透视表和在Python中处理多重索引数据的实际示例。

> 原文：[https://towardsdatascience.com/pivot-tables-in-pandas-with-hands-on-examples-in-python-9f29a48796f2?source=collection_archive---------2-----------------------#2023-02-09](https://towardsdatascience.com/pivot-tables-in-pandas-with-hands-on-examples-in-python-9f29a48796f2?source=collection_archive---------2-----------------------#2023-02-09)

## 学习如何对Pandas DataFrame进行透视，并获得有意义的见解。

[](https://suemnjeri.medium.com/?source=post_page-----9f29a48796f2--------------------------------)[![Susan Maina](../Images/a3ac5f206f175c711a9e7779283fc18d.png)](https://suemnjeri.medium.com/?source=post_page-----9f29a48796f2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9f29a48796f2--------------------------------)[![走向数据科学](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9f29a48796f2--------------------------------) [Susan Maina](https://suemnjeri.medium.com/?source=post_page-----9f29a48796f2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7df9dec030e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpivot-tables-in-pandas-with-hands-on-examples-in-python-9f29a48796f2&user=Susan+Maina&userId=7df9dec030e&source=post_page-7df9dec030e----9f29a48796f2---------------------post_header-----------) 发布在[《走向数据科学》](https://towardsdatascience.com/?source=post_page-----9f29a48796f2--------------------------------) ·11分钟阅读·2023年2月9日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9f29a48796f2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpivot-tables-in-pandas-with-hands-on-examples-in-python-9f29a48796f2&source=-----9f29a48796f2---------------------bookmark_footer-----------)![](../Images/58df12263c028369de91b8ed82ca9f81.png)

照片由[Елена from Pexels](https://www.pexels.com/photo/upside-down-multicolored-pencils-8850038/)拍摄。

透视表是一种数据操纵工具，它重新排列表格，有时对值进行聚合以便进行轻松的分析。

在这篇文章中，我们将讨论Pandas的pivot_table函数以及如何使用它提供的各种参数。我们将使用Kaggle的一个真实数据集来说明何时以及如何使用pivot_table函数。

透视表的优势

+   您可以按照一个或多个列对数据进行分组，然后使用各种统计数据来汇总值，如平均值、总和和计数。

+   它拥有一种易于使用的语法，直观地支持从简单到复杂的数据转换。

**透视表语法**

```py
pandas.pivot_table(data, 
                  values=None, 
                  index=None, 
                  columns=None, 
                  aggfunc='mean', 
                  fill_value=None, 
                  margins=False, 
                  dropna=True, 
                  margins_name='All', 
                  observed=False, 
                  sort=True)
```
