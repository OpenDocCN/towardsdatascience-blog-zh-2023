# 如何使用 Python 识别数据中的异常值

> 原文：[`towardsdatascience.com/how-to-identify-outliers-in-data-with-python-a9fe40235064`](https://towardsdatascience.com/how-to-identify-outliers-in-data-with-python-a9fe40235064)

## *一篇探讨数据集异常值检测技术的文章。学习如何使用数据可视化、z-分数和聚类技术来识别数据集中的异常值*

[](https://medium.com/@theDrewDag?source=post_page-----a9fe40235064--------------------------------)![Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----a9fe40235064--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a9fe40235064--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a9fe40235064--------------------------------) [Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----a9fe40235064--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a9fe40235064--------------------------------) ·阅读时间 8 分钟·2023 年 5 月 12 日

--

![](img/c9cb3fd47a2778d757dee6bf94bdaf8c.png)

图片由 [Tim Mossholder](https://unsplash.com/@timmossholder?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) 提供

Nassim Taleb 书写了如何“尾部”事件定义了世界现象的成功（或失败）的很大一部分。

> 每个人都知道预防比治疗更重要，但很少有人奖励预防行为。
> 
> N. Taleb — 《黑天鹅》

尾部事件是一种罕见事件，其概率位于分布的尾部，左侧或右侧。

![](img/9d93fce8d5248c9bf0289e3d026f1df2.png)

[`www.researchgate.net/figure/A-normal-distribution-curve-with-its-two-tails-Note-that-an-observed-result-is-likely-to_fig2_50196301`](https://www.researchgate.net/figure/A-normal-distribution-curve-with-its-two-tails-Note-that-an-observed-result-is-likely-to_fig2_50196301)

根据 Taleb 的说法，我们的生活主要集中在最可能发生的事件上。这样一来，**我们并没有为可能发生的罕见事件做好准备。**

当罕见事件发生时（尤其是负面的事件），它们会让我们感到意外，而我们通常采取的行动往往无效。

想想我们在罕见事件发生时的行为，例如 FTX 加密货币交易所的破产，或是一场强大的地震破坏了地区。对于直接涉事的人，**典型的反应是恐慌。**

**异常现象无处不在**，当我们绘制分布及其概率函数时，我们实际上获得了有用的信息，以保护自己或在这些尾部事件发生时实施策略。

**因此，有必要了解如何识别这些异常，尤其是在观察到这些异常时要做好行动准备。**

在本文中，我们将重点介绍用于识别异常值（即上述提到的异常现象）的方法和技术。特别是，我们将探讨数据可视化技术以及描述性统计和统计测试的使用。

# 异常值的定义

**异常值是数据集中显著偏离其他值的值**。这种偏离可以是数值上的，也可以是类别上的。

例如，数值异常是指我们有一个值比数据集中大多数其他值要大得多或小得多。

另一方面，类别异常值是指我们有标签称为“其他”或“未知”，这些标签占数据集中其他标签的比例要高得多。

**异常值可能由测量错误、输入错误、抄录错误或数据本身不符合数据集的正常趋势引起。**

在某些情况下，异常值可能表明数据集或产生数据的过程存在更广泛的问题，并能为开发数据收集过程的人员提供重要见解。

# 帮助我们识别数据集中异常值的技术

我们可以使用几种技术来识别数据中的异常值。这些是我们将在本文中讨论的方法。

+   **数据可视化**：这可以通过查看数据分布来识别异常，利用适合此目的的图表。

+   使用**描述性统计**，例如四分位距

+   使用**z-score**

+   使用**聚类技术**：这可以帮助识别相似数据的组，并识别任何“孤立”或“无法分类”的数据。

每种方法都有效于识别异常值，应根据我们的数据来选择。让我们逐一看看这些方法。

## 数据可视化

寻找异常的最常见技术之一是通过**探索性数据分析**，特别是通过数据可视化。

使用 Python，你可以利用像 Matplotlib 或 Seaborn 这样的库来可视化数据，以便你可以轻松发现任何异常。

例如，你可以创建直方图或箱线图来可视化数据的分布，并发现任何显著偏离均值的值。

![](img/1e7f9ab9a3b4dfbbe71e825f41632d2b.png)

图片由作者提供。

箱线图的结构可以通过这篇 Kaggle 文章来理解。

[`www.kaggle.com/discussions/general/219871`](https://www.kaggle.com/discussions/general/219871?ref=diariodiunanalista.it)

如果你想了解更多关于如何进行探索性数据分析（EDA）的内容，请阅读这篇文章 👇

[](/exploratory-data-analysis-in-python-a-step-by-step-process-d0dfa6bf94ee?source=post_page-----a9fe40235064--------------------------------) ## Python 中的探索性数据分析——逐步过程

### 什么是探索性分析，它的结构是什么，如何在 Python 中利用 Pandas 和其他数据工具应用它……

[towardsdatascience.com

## 描述性统计的使用

识别异常的另一种方法是使用描述性统计。例如，可以使用四分位距（IQR）来识别偏离均值显著的值。

四分位距（IQR）定义为数据集中第三四分位数（Q3）与第一四分位数（Q1）之间的差值。**异常值被定义为超出 IQR 范围并乘以通常为 1.5 的系数的值。**

之前讨论的箱线图只是利用这些描述性指标来识别异常的一种方法。

在 Python 中使用四分位距识别异常值的示例如下：

```py
import numpy as np

def find_outliers_IQR(data, threshold=1.5):
    # Find first and third quartiles
    Q1, Q3 = np.percentile(data, [25, 75])
    # Compute IQR (interquartile range)
    IQR = Q3 - Q1
    # Compute lower and upper bound
    lower_bound = Q1 - (threshold * IQR)
    upper_bound = Q3 + (threshold * IQR)
    # Select outliers
    outliers = [x for x in data if x < lower_bound or x > upper_bound]
    return outliers
```

这种方法计算数据集的第一和第三四分位数，然后计算 IQR 以及下限和上限。最后，将异常值识别为超出下限和上限的值。

这个实用函数可以用来识别数据集中异常值，并且可以被添加到你几乎所有项目的工具函数库中。

## 使用 z 分数

另一种发现异常的方法是通过 z 分数。**z 分数测量一个值相对于均值的标准差偏差程度**。

转换数据为 z 分数的公式如下：

![](img/b0bf40cb975c5703a529bf197f8075c5.png)

其中 *x* 是原始值，*μ* 是数据集均值，*σ* 是数据集标准差。z 分数指示原始值距离均值的标准差数量。z 分数大于 3（或小于 -3）通常被视为异常值。

这种方法在处理大数据集时特别有用，当你想以客观和可重复的方式识别异常时更是如此。

在 Python 的 Sklearn 中，z 分数的转换可以这样进行

```py
from sklearn.preprocessing import StandardScaler

def find_outliers_zscore(data, threshold=3):
    # Normalize data
    scaler = StandardScaler()
    standardized = scaler.fit_transform(data.reshape(-1, 1))
    # Select outliers
    outliers = [data[i] for i, x in enumerate(standardized) if x < -threshold or x > threshold]
    return outliers
```

## 使用聚类技术

最后，聚类技术可以用来识别任何“孤立”或“无法分类”的数据。这在处理非常大且复杂的数据集时特别有用，因为数据可视化不足以发现异常。

在这种情况下，一个选择是使用**基于密度的空间聚类应用噪声（DBSCAN）**算法，这是一种聚类算法，可以基于数据的密度识别数据组，并找到不属于任何聚类的点。这些点被视为异常值。

DBSCAN 算法可以再次通过 Python 的 sklearn 库实现。

以这个可视化数据集为例

![](img/a3939414646e9596f92a6bfff3cb01ca.png)

图片由作者提供。

DBSCAN 应用程序提供了这种可视化效果

![](img/1bdd66cb55f17572b7b35039d5c415f8.png)

图片由作者提供。

创建这些图表的代码如下

```py
import numpy as np
import matplotlib.pyplot as plt
from sklearn.cluster import DBSCAN

def generate_data_with_outliers(n_samples=100, noise=0.05, outlier_fraction=0.05, random_state=42):
    # Create random data
    X = np.concatenate([np.random.normal(0.5, 0.1, size=(n_samples//2, 2)),
                         np.random.normal(1.5, 0.1, size=(n_samples//2, 2))], axis=0)

    # Add outliers
    n_outliers = int(outlier_fraction * n_samples)
    outliers = np.random.RandomState(seed=random_state).rand(n_outliers, 2) * 3 - 1.5
    X = np.concatenate((X, outliers), axis=0)

    # Add noise to the data to resemble real-world data
    X = X + np.random.randn(n_samples + n_outliers, 2) * noise

    return X

# Genereate data
X = generate_data_with_outliers(outlier_fraction=0.2)

# Apply DBSCAN to cluster the data and find outliers
dbscan = DBSCAN(eps=0.2, min_samples=5)
dbscan.fit(X)

# Select outliers
outlier_indices = np.where(dbscan.labels_ == -1)[0]

# Visualize
plt.scatter(X[:, 0], X[:, 1], c=dbscan.labels_, cmap="viridis")
plt.scatter(X[outlier_indices, 0], X[outlier_indices, 1], c="red", label="Outliers", marker="x")
plt.xticks([])
plt.yticks([])
plt.legend()
plt.show()
```

该方法创建一个 DBSCAN 对象，使用 `eps` 和 `min_samples` 参数并将其拟合到数据上。然后将那些不属于任何簇的值标识为异常值，即标记为 -1 的值。

这只是许多可以用来识别异常的聚类技术中的一种。例如，基于深度学习的方法依赖于 *自编码器*，这是一种利用数据压缩表示来识别输入数据中独特特征的神经网络。

# 结论

在这篇文章中，我们已经看到几种用于识别数据异常值的技术。

我们讨论了数据可视化、描述性统计和 z 分数的使用，以及聚类技术。

每种技术都是有效的，应根据您分析的数据类型来选择。重要的是要记住，识别异常值可以提供重要的信息，以改进数据收集过程并根据获得的结果做出更好的决策。

**如果您想支持我的内容创作活动，请随时通过以下推荐链接加入 Medium 的会员计划**。我将从您的投资中获得一部分，您将能够无缝地访问 Medium 上关于数据科学及更多内容的丰富文章。

[](https://medium.com/@theDrewDag/membership?source=post_page-----a9fe40235064--------------------------------) [## 使用我的推荐链接加入 Medium - Andrea D'Agostino

### 作为 Medium 会员，您的会员费的一部分将会分配给您阅读的作者，同时您可以完全访问每一个故事……

medium.com](https://medium.com/@theDrewDag/membership?source=post_page-----a9fe40235064--------------------------------)

# 推荐阅读

对于有兴趣的读者，这里是我推荐的与机器学习相关的每个主题的书单。这些书籍在我看来是必读的，并对我的职业生涯产生了重大影响。

*免责声明：这些是亚马逊的关联链接。我将因推荐这些商品而从亚马逊获得少量佣金。您的体验不会改变，也不会额外收费，但这将帮助我扩展业务，并制作更多关于人工智能的内容。*

+   **机器学习入门：** [*掌握数据技能：掌握数据处理基础并提升职业生涯*](https://amzn.to/3ZzKTz6) 由 Kirill Eremenko 编著

+   **Sklearn / TensorFlow：** [*使用 Scikit-Learn、Keras 和 TensorFlow 的实用机器学习*](https://amzn.to/433F4Nm) 由 Aurelien Géron 编著

+   **NLP：** [*文本数据：机器学习与社会科学的新框架*](https://amzn.to/3zvH43j) 由 Justin Grimmer 编著

+   **Sklearn / PyTorch：** [*使用 PyTorch 和 Scikit-Learn 进行机器学习：使用 Python 开发机器学习和深度学习模型*](https://amzn.to/3Gcavve) 由 Sebastian Raschka 编著

+   **数据可视化：** [*数据讲述：商业专业人士的数据可视化指南*](https://amzn.to/3HUtGtB) 作者：科尔·克纳弗利克

# 有用的链接（由我编写）

+   **学习如何在 Python 中进行顶级的探索性数据分析**: *Python 中的探索性数据分析 — 分步过程*

+   **学习 TensorFlow 的基础知识**: [*开始使用 TensorFlow 2.0 — 深度学习简介*](https://medium.com/towards-data-science/a-comprehensive-introduction-to-tensorflows-sequential-api-and-model-for-deep-learning-c5e31aee49fa)

+   **使用 TF-IDF 在 Python 中进行文本聚类**: [*使用 TF-IDF 在 Python 中进行文本聚类*](https://medium.com/mlearning-ai/text-clustering-with-tf-idf-in-python-c94cd26a31e7)
