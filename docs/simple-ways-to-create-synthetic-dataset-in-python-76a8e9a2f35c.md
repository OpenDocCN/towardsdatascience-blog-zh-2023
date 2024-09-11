# 在 Python 中创建合成数据集的简单方法

> 原文：[`towardsdatascience.com/simple-ways-to-create-synthetic-dataset-in-python-76a8e9a2f35c?source=collection_archive---------15-----------------------#2023-01-12`](https://towardsdatascience.com/simple-ways-to-create-synthetic-dataset-in-python-76a8e9a2f35c?source=collection_archive---------15-----------------------#2023-01-12)

## 数据科学基础

## 初学者创建模拟表格数据指南

[](https://zluvsand.medium.com/?source=post_page-----76a8e9a2f35c--------------------------------)![Zolzaya Luvsandorj](https://zluvsand.medium.com/?source=post_page-----76a8e9a2f35c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----76a8e9a2f35c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----76a8e9a2f35c--------------------------------) [Zolzaya Luvsandorj](https://zluvsand.medium.com/?source=post_page-----76a8e9a2f35c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5bca2b935223&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimple-ways-to-create-synthetic-dataset-in-python-76a8e9a2f35c&user=Zolzaya+Luvsandorj&userId=5bca2b935223&source=post_page-5bca2b935223----76a8e9a2f35c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----76a8e9a2f35c--------------------------------) ·7 min read·2023 年 1 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F76a8e9a2f35c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimple-ways-to-create-synthetic-dataset-in-python-76a8e9a2f35c&user=Zolzaya+Luvsandorj&userId=5bca2b935223&source=-----76a8e9a2f35c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F76a8e9a2f35c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimple-ways-to-create-synthetic-dataset-in-python-76a8e9a2f35c&source=-----76a8e9a2f35c---------------------bookmark_footer-----------)

在开发代码时，有时我们需要一个虚拟数据集。例如，我们希望共享代码及其基础数据，但现实中的数据集是机密的，不适合共享。一种选择是找到并使用合适的 玩具数据集或公开数据集。另一种选择是创建一个适合你使用场景的合成数据集。在这篇文章中，我们将探讨几种在 Python 中创建合成数据集的简单方法。

![](img/9298cc1e32a2bf82fd4e570d2c3cb6c6.png)

照片由[Jackie Tsang](https://unsplash.com/es/@jickii?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 🔧 设置

我们将从加载必要的库开始：

```py
import numpy as np
import pandas as pd
from faker import Faker
from scipy.stats import skewnorm
from datetime import datetime
from sklearn.datasets import (make_regression, make_classification, 
                              make_multilabel_classification, 
                              make_blobs)
from sklearn.model_selection import train_test_split
from sklearn.ensemble import (RandomForestClassifier,
                              RandomForestRegressor)
from sklearn.multioutput import MultiOutputClassifier
from sklearn.cluster import KMeans
from sklearn.metrics import mean_squared_error, roc_auc_score
import matplotlib.pyplot as plt
import…
```
