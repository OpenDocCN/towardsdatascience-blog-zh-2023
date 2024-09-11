# 非线性维度缩减、核主成分分析（kPCA）和多维缩放——使用 Python 的简单教程

> 原文：[`towardsdatascience.com/nonlinear-dimension-reduction-kernel-pca-kpca-and-multidimensional-scaling-an-easy-tutorial-63429ee9d0ae?source=collection_archive---------4-----------------------#2023-12-11`](https://towardsdatascience.com/nonlinear-dimension-reduction-kernel-pca-kpca-and-multidimensional-scaling-an-easy-tutorial-63429ee9d0ae?source=collection_archive---------4-----------------------#2023-12-11)

## 如何在不破坏的情况下扁平化你的瑞士卷数据!!

[](https://medium.com/@biman.pph?source=post_page-----63429ee9d0ae--------------------------------)![Biman Chakraborty](https://medium.com/@biman.pph?source=post_page-----63429ee9d0ae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----63429ee9d0ae--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----63429ee9d0ae--------------------------------) [Biman Chakraborty](https://medium.com/@biman.pph?source=post_page-----63429ee9d0ae--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb194a768b666&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnonlinear-dimension-reduction-kernel-pca-kpca-and-multidimensional-scaling-an-easy-tutorial-63429ee9d0ae&user=Biman+Chakraborty&userId=b194a768b666&source=post_page-b194a768b666----63429ee9d0ae---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----63429ee9d0ae--------------------------------) ·11 分钟阅读·2023 年 12 月 11 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F63429ee9d0ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnonlinear-dimension-reduction-kernel-pca-kpca-and-multidimensional-scaling-an-easy-tutorial-63429ee9d0ae&user=Biman+Chakraborty&userId=b194a768b666&source=-----63429ee9d0ae---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F63429ee9d0ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnonlinear-dimension-reduction-kernel-pca-kpca-and-multidimensional-scaling-an-easy-tutorial-63429ee9d0ae&source=-----63429ee9d0ae---------------------bookmark_footer-----------)![](img/3d4589e1299bc55b786feb6867522a9f.png)

瑞士卷数据（作者提供的图像）

在我的文章[**主成分分析（PCA）— 用 Python 轻松入门**](https://medium.com/@biman.pph/principal-component-analysis-pca-an-easy-tutorial-with-python-c623b583cf29)中，我讨论了如何使用 PCA 来减少数据的维度，同时尽可能保留点对点之间的距离。我用 MNIST 手写数据集举例说明了如何将数据的维度从 784 降至 35，并且仍然能够使用高度准确的监督学习技术。

在这篇文章中，我们从一个简单的三维**瑞士卷**数据的例子开始，数据的真实流形维度为 2，我们将从 PCA 开始。

# 示例：瑞士卷数据集

图 1 显示了使用`sklearn`库生成的模拟瑞士卷数据，共有𝑛=2000 个点。散点图显示不同颜色的点位于螺旋的不同部分。

```py
#Load the libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

#Generate the Swiss Roll Dataset
from sklearn.datasets import make_swiss_roll

np.random.seed(42)
n_samples = 2000
X, t =…
```
