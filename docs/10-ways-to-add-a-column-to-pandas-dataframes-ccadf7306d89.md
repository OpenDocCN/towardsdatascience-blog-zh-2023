# 10 种向 Pandas DataFrames 添加列的方法

> 原文：[https://towardsdatascience.com/10-ways-to-add-a-column-to-pandas-dataframes-ccadf7306d89?source=collection_archive---------5-----------------------#2023-07-20](https://towardsdatascience.com/10-ways-to-add-a-column-to-pandas-dataframes-ccadf7306d89?source=collection_archive---------5-----------------------#2023-07-20)

## 我们经常需要派生或创建新列

[](https://sonery.medium.com/?source=post_page-----ccadf7306d89--------------------------------)[![Soner Yıldırım](../Images/c589572e9d1ee176cd4f5a0008173f1b.png)](https://sonery.medium.com/?source=post_page-----ccadf7306d89--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ccadf7306d89--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ccadf7306d89--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----ccadf7306d89--------------------------------)

·

[Follow](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F10-ways-to-add-a-column-to-pandas-dataframes-ccadf7306d89&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----ccadf7306d89---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ccadf7306d89--------------------------------) ·7 min read·2023年7月20日

--

![](../Images/281887f475304710966b47c2ebe1dd89.png)[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fccadf7306d89&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F10-ways-to-add-a-column-to-pandas-dataframes-ccadf7306d89&source=-----ccadf7306d89---------------------bookmark_footer-----------)

由 [Austin Chan](https://unsplash.com/@austinchan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/ukzHlkoz1IE?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

DataFrame 是一种具有标签的二维数据结构。我们经常需要在数据分析或特征工程过程中添加新列。

有很多不同的方法来添加新列。最适合你的需求的方法取决于当前的任务。

在本文中，我们将学习 10 种向 Pandas DataFrames 添加列的方法。

让我们通过使用Pandas的`DataFrame`构造函数创建一个简单的DataFrame。我们将数据作为一个Python `dictionary` 传入，其中列名作为键，行作为字典的值。

```py
import pandas as pd

# create DataFrame
df = pd.DataFrame(

    {
        "first_name": ["Jane", "John", "Max", "Emily", "Ashley"],
        "last_name": ["Doe", "Doe", "Dune", "Smith", "Fox"],
        "id": [101, 103, 143, 118, 128]
    }   
)

# display DataFrame
df
```

![](../Images/1e8e03441c0fe11d013be173a9e115ce.png)

df（作者提供的图像）

## 1\. 使用常量值

我们可以按照以下步骤添加一个常量值的新列：

```py
df.loc[:, "department"] = "engineering"

#…
```
