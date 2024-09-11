# 4 个 Pandas 函数用于 DataFrame 的逐元素比较

> 原文：[https://towardsdatascience.com/4-pandas-functions-for-element-wise-comparison-of-dataframes-c7e06e51d399?source=collection_archive---------3-----------------------#2023-06-10](https://towardsdatascience.com/4-pandas-functions-for-element-wise-comparison-of-dataframes-c7e06e51d399?source=collection_archive---------3-----------------------#2023-06-10)

## 通过示例进行解释。

[](https://sonery.medium.com/?source=post_page-----c7e06e51d399--------------------------------)[![Soner Yıldırım](../Images/c589572e9d1ee176cd4f5a0008173f1b.png)](https://sonery.medium.com/?source=post_page-----c7e06e51d399--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c7e06e51d399--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c7e06e51d399--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----c7e06e51d399--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-pandas-functions-for-element-wise-comparison-of-dataframes-c7e06e51d399&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----c7e06e51d399---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c7e06e51d399--------------------------------) ·4 min read·2023年6月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc7e06e51d399&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-pandas-functions-for-element-wise-comparison-of-dataframes-c7e06e51d399&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=-----c7e06e51d399---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc7e06e51d399&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-pandas-functions-for-element-wise-comparison-of-dataframes-c7e06e51d399&source=-----c7e06e51d399---------------------bookmark_footer-----------)![](../Images/79a6153d096edfc0b646b5adb84002bf.png)

照片由 [NordWood Themes](https://unsplash.com/@nordwood?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，发布在 [Unsplash](https://unsplash.com/photos/E9tFH39iRPE?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Pandas DataFrame 是带有标签的二维数据结构。

![](../Images/e7756b0352e79032b25187b5be0e23cb.png)

带有 3 行 3 列的 DataFrame（图像由作者提供）

我们有时需要对两个 DataFrame 进行逐元素比较。例如：

+   使用另一个 DataFrame 中的值更新 DataFrame 中的值。

+   比较数值并选择更大或更小的值。

在本文中，我们将学习四个不同的 Pandas 函数，这些函数可以用于此类任务。我们还将通过示例更好地理解它们之间的差异和相似之处。

*如果你想了解更多关于 Pandas 的内容，可以访问我的课程* [*掌握 Python Pandas 的 500 个练习*](https://www.udemy.com/course/500-exercises-to-master-python-pandas/learn/lecture/37842166#overview)*。*

首先让我们创建两个 DataFrame 以用于示例。

```py
import numpy as np
import pandas as pd

# create DataFrames with random integers
df1 = pd.DataFrame(np.random.randint(0, 10, size=(4, 4)), columns=list("ABCD"))
df2 = pd.DataFrame(np.random.randint(0, 10, size=(4, 4)), columns=list("ABCD"))

# add a couple of missing values
df1.iloc[2, 3] = np.nan
df1.iloc[1, 2] = np.nan
```
