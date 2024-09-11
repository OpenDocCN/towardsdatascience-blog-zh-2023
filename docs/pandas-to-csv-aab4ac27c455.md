# 如何将 Pandas DataFrame 写入 CSV 文件

> 原文：[`towardsdatascience.com/pandas-to-csv-aab4ac27c455?source=collection_archive---------10-----------------------#2023-01-13`](https://towardsdatascience.com/pandas-to-csv-aab4ac27c455?source=collection_archive---------10-----------------------#2023-01-13)

## 利用在将 pandas DataFrames 写入 CSV 文件时提供的所有选项

[](https://gmyrianthous.medium.com/?source=post_page-----aab4ac27c455--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----aab4ac27c455--------------------------------)[](https://towardsdatascience.com/?source=post_page-----aab4ac27c455--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----aab4ac27c455--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----aab4ac27c455--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-to-csv-aab4ac27c455&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----aab4ac27c455---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----aab4ac27c455--------------------------------) ·5 分钟阅读·2023 年 1 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Faab4ac27c455&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-to-csv-aab4ac27c455&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----aab4ac27c455---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faab4ac27c455&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-to-csv-aab4ac27c455&source=-----aab4ac27c455---------------------bookmark_footer-----------)![](img/881b8b4f1e7bf014f76b61f5f78e7900.png)

图片由 [Hello I’m Nik](https://unsplash.com/fr/@helloimnik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/l4ADb9OVqTY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在使用 pandas 进行数据处理时，一个非常常见的任务是将 DataFrame 导出到 CSV 文件中。

在本文中，我们将演示如何将 pandas DataFrame 写入 CSV 文件。此外，我们还将展示如何使用 pandas API 的各种选项来处理缺失值、日期时间格式化、压缩以及其他与在 CSV 文件中持久化 DataFrame 相关的需求。

现在让我们创建一个示例 DataFrame，我们将在本教程中使用并引用它，以演示将 pandas DataFrame 导出到 CSV 文件的几种不同选项。

```py
from datetime import datetime

import pandas as pd

df = pd.DataFrame(
  [
    (1, 'A', True, datetime(2018, 12, 27), 158, None),
    (2, 'B', False, datetime(2019, 11, 15), 10, 76.3),
    (3, 'A', True, None, 210, 78.1),
    (4, 'F', None, datetime(2022, 10, 5), 679, 54.1),
    (5, 'F', False, datetime(2021, 10, 10), 765, 21.1),
    (6, 'F', True, datetime(2021, 7, 23), 132, 4.5),
    (7, 'C', False, datetime(2017, 4, 5), 45, 5.6),
    (8, 'B', False, datetime(2016, 3, 4), 67, 6.7),
    (9, 'A', True…
```
