# 一个逐步指南，帮助实现稳健的机器学习分类

> 原文：[https://towardsdatascience.com/a-step-by-step-guide-to-robust-ml-classification-5ce83592eb1d?source=collection_archive---------7-----------------------#2023-03-03](https://towardsdatascience.com/a-step-by-step-guide-to-robust-ml-classification-5ce83592eb1d?source=collection_archive---------7-----------------------#2023-03-03)

## 如何避免常见的陷阱，并深入挖掘我们的模型

[](https://ryancburke8.medium.com/?source=post_page-----5ce83592eb1d--------------------------------)[![Ryan Burke](../Images/4a6c1ac506da2456406afe46bae8e732.png)](https://ryancburke8.medium.com/?source=post_page-----5ce83592eb1d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5ce83592eb1d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5ce83592eb1d--------------------------------) [Ryan Burke](https://ryancburke8.medium.com/?source=post_page-----5ce83592eb1d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F93ea55f01411&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-step-by-step-guide-to-robust-ml-classification-5ce83592eb1d&user=Ryan+Burke&userId=93ea55f01411&source=post_page-93ea55f01411----5ce83592eb1d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5ce83592eb1d--------------------------------) ·17分钟阅读·2023年3月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5ce83592eb1d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-step-by-step-guide-to-robust-ml-classification-5ce83592eb1d&user=Ryan+Burke&userId=93ea55f01411&source=-----5ce83592eb1d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5ce83592eb1d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-step-by-step-guide-to-robust-ml-classification-5ce83592eb1d&source=-----5ce83592eb1d---------------------bookmark_footer-----------)![](../Images/f1856f0dfbd79cecda67d8bfcb71783b.png)

照片由 [Luca Bravo](https://unsplash.com/@lucabravo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/wallpapers/nature/forest?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在之前的文章中，我主要关注于介绍我发现有趣的单个算法。在这里，我将详细讲解一个完整的机器学习分类项目。目标是讨论一些机器学习项目中常见的陷阱，并向读者描述如何避免这些陷阱。我还将展示如何通过分析模型错误来进一步挖掘，获得通常被忽视的重要见解。

如果你想查看完整的笔记本，请点击 [这里](https://github.com/ryancburke/forest_cover)。

# 库

以下是我用于今天分析的库列表。这些库包括了标准的数据科学工具包以及必要的sklearn库。

```py
import sys
import os
import pandas as pd
import numpy as np

import matplotlib.pyplot as plt
import seaborn as sns
from IPython.display import display
%matplotlib inline

import plotly.offline as py
import plotly.graph_objs as go
import plotly.tools as tls
py.init_notebook_mode(connected=True)

import warnings
warnings.filterwarnings('ignore')

from pandas import set_option
from pandas.plotting import scatter_matrix
from sklearn.preprocessing import StandardScaler, MinMaxScaler, QuantileTransformer, RobustScaler…
```
