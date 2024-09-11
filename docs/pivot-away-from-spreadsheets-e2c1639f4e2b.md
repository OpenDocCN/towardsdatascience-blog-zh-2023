# 远离电子表格的透视表

> 原文：[https://towardsdatascience.com/pivot-away-from-spreadsheets-e2c1639f4e2b?source=collection_archive---------11-----------------------#2023-04-13](https://towardsdatascience.com/pivot-away-from-spreadsheets-e2c1639f4e2b?source=collection_archive---------11-----------------------#2023-04-13)

## Excel 不是唯一的数据透视表工具

[](https://bradley-stephen-shaw.medium.com/?source=post_page-----e2c1639f4e2b--------------------------------)[![Bradley Stephen Shaw](../Images/b3ef5e6e292083ff0f8523ec5ffe89f0.png)](https://bradley-stephen-shaw.medium.com/?source=post_page-----e2c1639f4e2b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e2c1639f4e2b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e2c1639f4e2b--------------------------------) [Bradley Stephen Shaw](https://bradley-stephen-shaw.medium.com/?source=post_page-----e2c1639f4e2b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc5cd0a58b5ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpivot-away-from-spreadsheets-e2c1639f4e2b&user=Bradley+Stephen+Shaw&userId=c5cd0a58b5ae&source=post_page-c5cd0a58b5ae----e2c1639f4e2b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e2c1639f4e2b--------------------------------) · 8 分钟阅读 · 2023年4月13日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe2c1639f4e2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpivot-away-from-spreadsheets-e2c1639f4e2b&user=Bradley+Stephen+Shaw&userId=c5cd0a58b5ae&source=-----e2c1639f4e2b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe2c1639f4e2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpivot-away-from-spreadsheets-e2c1639f4e2b&source=-----e2c1639f4e2b---------------------bookmark_footer-----------)![](../Images/25b9c5f30107ac892acc25c53fe0b677.png)

由 [DDP](https://unsplash.com/fr/@moino007?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 拍摄的照片

“但是数据透视表！”：许多 Excel 爱好者在听我赞美`pandas`（以及不可避免的建议他们忘记使用电子表格进行数据分析）时的常见反应。

认为数据透视表是电子表格的唯一属性，可能会阻碍许多人完全转向使用 Python 进行数据分析。

当然，这根本不是真的——正如我们今天将深入探讨的内容。

1.  `pandas`中的基本数据透视表

1.  在数据透视表中使用定制函数和计算字段

让我们开始吧——我们将从模拟一些数据开始。

# 数据

我们将从模拟一些数据开始。这里的工作量不大，我们将模拟关于两种不同产品的销售数据：

```py
import pandas as pd
import numpy as np

# product A
d_a = pd.DataFrame(
    {
        'Month':pd.date_range(
            start = pd.to_datetime('01-01-2012',format = '%d-%m-%Y'),
            end = pd.to_datetime('31-12-2022',format = '%d-%m-%Y'),
            freq = 'MS',

        ),
        'Quotes':np.random.randint(
            low = 1_000_000,
            high = 2_500_000,
            size = 132…
```
