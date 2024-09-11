# 7个示例掌握Python Pandas中的分类数据操作

> 原文：[https://towardsdatascience.com/7-examples-to-master-categorical-data-operations-with-python-pandas-51cdcb0228ba?source=collection_archive---------3-----------------------#2023-11-09](https://towardsdatascience.com/7-examples-to-master-categorical-data-operations-with-python-pandas-51cdcb0228ba?source=collection_archive---------3-----------------------#2023-11-09)

## 在处理低基数类别特征时使用类别数据类型

[](https://sonery.medium.com/?source=post_page-----51cdcb0228ba--------------------------------)[![Soner Yıldırım](../Images/c589572e9d1ee176cd4f5a0008173f1b.png)](https://sonery.medium.com/?source=post_page-----51cdcb0228ba--------------------------------)[](https://towardsdatascience.com/?source=post_page-----51cdcb0228ba--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----51cdcb0228ba--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----51cdcb0228ba--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-examples-to-master-categorical-data-operations-with-python-pandas-51cdcb0228ba&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----51cdcb0228ba---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----51cdcb0228ba--------------------------------) ·8 分钟阅读·2023年11月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F51cdcb0228ba&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-examples-to-master-categorical-data-operations-with-python-pandas-51cdcb0228ba&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=-----51cdcb0228ba---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F51cdcb0228ba&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-examples-to-master-categorical-data-operations-with-python-pandas-51cdcb0228ba&source=-----51cdcb0228ba---------------------bookmark_footer-----------)![](../Images/a0de95c1f991d735d8dc687318a5f381.png)

（图片由作者创建）

分类变量可以从有限的固定值中取值。以下是一些分类变量的示例：

+   英语水平指标（A1、A2、B1、B2、C1、C2）

+   人的血型（A、B、AB、0）

+   种族和性别等人口统计信息

+   教育水平

Pandas 提供了一种专用的分类变量数据类型（`category` 或 `CategoricalDtype`）。虽然这些数据也可以存储为`object`或`string`数据类型，但使用`category`数据类型有几个优点。我们将深入了解这些优点，但首先，让我们从如何处理分类数据开始。

当我们创建包含文本数据的 Series 或 DataFrame 时，其数据类型默认为`object`。要使用`category`数据类型，我们需要显式地定义它。

```py
import pandas as pd

# create Series
blood_type = pd.Series(["A", "B", "AB", "0"])

print(blood_type)
# output
0     A
1     B
2    AB
3     0
dtype: object

# create Series with category data type
blood_type = pd.Series(["A", "B", "AB", "0"]…
```
