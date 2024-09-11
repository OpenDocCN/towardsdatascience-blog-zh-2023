# 使用 Python 获取位置的温度数据

> 原文：[`towardsdatascience.com/get-temperature-data-by-location-with-python-52ed872dd621?source=collection_archive---------7-----------------------#2023-05-18`](https://towardsdatascience.com/get-temperature-data-by-location-with-python-52ed872dd621?source=collection_archive---------7-----------------------#2023-05-18)

## 通过经纬度检索历史温度数据。

[](https://gustavorsantos.medium.com/?source=post_page-----52ed872dd621--------------------------------)![Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----52ed872dd621--------------------------------)[](https://towardsdatascience.com/?source=post_page-----52ed872dd621--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----52ed872dd621--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----52ed872dd621--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4429d99b1245&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fget-temperature-data-by-location-with-python-52ed872dd621&user=Gustavo+Santos&userId=4429d99b1245&source=post_page-4429d99b1245----52ed872dd621---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----52ed872dd621--------------------------------) ·3 min read·2023 年 5 月 18 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F52ed872dd621&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fget-temperature-data-by-location-with-python-52ed872dd621&user=Gustavo+Santos&userId=4429d99b1245&source=-----52ed872dd621---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F52ed872dd621&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fget-temperature-data-by-location-with-python-52ed872dd621&source=-----52ed872dd621---------------------bookmark_footer-----------)![](img/0a6e05461c10ccab94bd840c64e8f53b.png)

图片来源于 [Tom Barrett](https://unsplash.com/es/@wistomsin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/hgGplX3PFBg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

我在进行一些项目时，需要根据位置查找平均温度，以便将其作为模型的一个可能变量。经过一段时间的研究，我发现了一个在 Python 中非常棒且易于使用的包，名为`meteostat`。

这个包有一些有趣的方法，其中一个正是我需要的：如果我输入纬度和经度，它会检索那个地方的历史温度。此外，你还可以按时间范围查询。所以，你可以获取仅一年或多年的数据。

另一个酷炫的功能是你可以要求数据按日或按月提供。太棒了！

让我们看看代码是如何工作的。

# 代码

首先，让我们用`pip install meteostat`安装 meteostat。接下来，我们可以导入所需的包。

```py
# Import packages
import pandas as pd
from datetime import datetime
import matplotlib.pyplot as plt
from meteostat import Point, Monthly
```

如果你访问[文档](https://dev.meteostat.net/python/#installation)，你会看到这段*入门*代码，这基本上就是让它工作的所需内容。我们设置了一个时间范围——我使用了 2022 年。然后我们创建一个…
