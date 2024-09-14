# 异常值检测与 Scikit-Learn 和 Matplotlib: 实用指南

> 原文：[`towardsdatascience.com/outlier-detection-with-scikit-learn-and-matplotlib-a-practical-guide-382d1411b8ec`](https://towardsdatascience.com/outlier-detection-with-scikit-learn-and-matplotlib-a-practical-guide-382d1411b8ec)

## 了解可视化、算法和统计如何帮助你识别机器学习任务中的异常值。

[](https://medium.com/@riccardo.andreoni?source=post_page-----382d1411b8ec--------------------------------)![Riccardo Andreoni](https://medium.com/@riccardo.andreoni?source=post_page-----382d1411b8ec--------------------------------)[](https://towardsdatascience.com/?source=post_page-----382d1411b8ec--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----382d1411b8ec--------------------------------) [Riccardo Andreoni](https://medium.com/@riccardo.andreoni?source=post_page-----382d1411b8ec--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----382d1411b8ec--------------------------------) ·阅读时间 10 分钟·2023 年 10 月 27 日

--

![](img/b52c5234e67397f7a6f6010da838a994.png)

气球与异常值有什么关系？在引言中找到答案。图片来源: [pixabay.com](https://pixabay.com/illustrations/balloons-spring-nature-watercolor-1615032/)。

想象一个房间里充满了**五彩斑斓的气球**，每个气球象征数据集中一个数据点。由于它们的特征不同，气球浮在不同的高度。现在，想象一些**充氦的气球**意外地飞得比其他气球高得多。正如这些特殊的气球打破了房间的均匀性，异常值也会打乱数据集中的模式。

回到纯统计学的角度，**异常值**被定义为异常现象，或者更准确地说，是明显偏离数据集其余部分的数据点。

考虑一个**机器学习算法**，它根据患者数据来诊断疾病。在这个实际的例子中，异常值可能是实验室结果或生理参数中的极端高值。虽然这些异常值可能源于**数据收集错误**、**测量不准确**或真正的**稀有事件**，它们的存在可能导致算法做出错误的诊断。

这就是为什么我们，机器学习或数据科学从业者，必须始终**小心处理异常值**。

在这篇简短的文章中，我将讨论几种有效识别和删除数据中的异常值的方法。

其中之一是 [**支持向量机**](https://en.wikipedia.org/wiki/Support_vector_machine)，我在这篇文章中探讨了这个方法。

[](/support-vector-machine-with-scikit-learn-a-friendly-introduction-a2969f2ff00d?source=post_page-----382d1411b8ec--------------------------------) ## 支持向量机与 Scikit-Learn：友好的介绍

### 每个数据科学家都应该在工具箱中有 SVM。通过实际操作来掌握这一多功能模型…

towardsdatascience.com

# 什么是异常值？

异常值是数据集中**不具代表性的数据点**，或者更准确地说，是那些与其他数据点显著偏离的数据点。尽管其定义简单，检测这些异常并非总是直接的，但首先，让我们回答以下基本问题。

> 为什么我们要检测数据集中的异常值？

对这个问题有两个答案。检测异常值的**第一个原因**是这些异常值可能隐藏数据中的有意义模式，并扭曲机器学习算法的学习过程。例如，在一个基于特征的[房价数据集](https://www.kaggle.com/datasets/yasserh/housing-prices-dataset)中，一个小而位置差的公寓的异常高价格，可能是异常值，导致偏差预测。

**其次**，广泛的数据科学应用的唯一目标就是检测异常值。在这些情况下，异常检测不仅仅是数据准备任务，而是应用的整个范围。例如，[金融中的欺诈检测](https://complyadvantage.com/insights/what-is-fraud-detection/#:~:text=Fraud%20detection%20refers%20to%20the,laundering%20(AML)%20compliance%20processes.)：算法的目标是识别不寻常的交易模式，指示欺诈活动。

![](img/99f230d3264b77bf8a15c7ab46929fa2.png)

图片由作者提供。

对于这份入门指南，我将介绍几种异常检测方法，这些方法分为以下三大类：

+   **图形方法**：通过**数据可视化**来检测异常值。

+   **统计方法**：通过**统计分析**和概率分布来检测异常值。

+   **算法方法**：通过**机器学习模型**来检测异常值。

![](img/abe4ed6799313c67c954005df0d804c0.png)

图片由作者提供。

# **图形方法**

异常检测的图形方法利用了**人脑**的卓越**模式识别能力**。它使用散点图、箱线图和热图等可视化工具，提供数据的叙述，并允许数据科学家发现**模式中的不规则性**。

## 散点图

在一个[散点图](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.scatter.html)中，异常值将表现为**明显偏离主要聚类**的点。

在生成合成数据后，我将使用[Matplotlib Pyplot](https://matplotlib.org/3.5.3/api/_as_gen/matplotlib.pyplot.html)库在[Python](https://www.python.org/)中创建散点图。

```py
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Generate synthetic data with outliers
np.random.seed(42)
normal_data = np.random.normal(loc=50, scale=10, size=100)
outliers = np.random.normal(loc=15, scale=5, size=10)

# Combine normal data with outliers
data = np.concatenate((normal_data, outliers))

# Visualize data using a scatter plot
fig, ax = plt.subplots(figsize=(8, 6))
ax.scatter(range_1, normal_data, color=sns.color_palette("hls",24)[7], alpha=.9, label='Normal Data')
ax.scatter(range_2, outliers, color=sns.color_palette("hls",24)[0], alpha=.9, label='Outliers')
ax.set_xlabel('Index')
ax.set_ylabel('Value')
ax.set_title('Scatter Plot')
ax.spines['top'].set_visible(False)
ax.spines['bottom'].set_visible(False)
ax.spines['right'].set_visible(False)
ax.spines['left'].set_visible(False)
ax.xaxis.set_ticks_position('none') 
ax.yaxis.set_ticks_position('none')
plt.legend()
plt.show()
```

结果图将突出显示一个数据集，其值与其他数据显著不同：

![](img/1d9d85d7ecffc96e10a635fb3b100664.png)

图片由作者提供。

## 箱线图

使用相同的数据，我们可以显示一个[箱线图](https://matplotlib.org/3.1.1/api/_as_gen/matplotlib.pyplot.boxplot.html)，其中离群点显示为**箱线图“须”之外的点**。

[Python](https://www.python.org/)代码如下。

```py
# Visualize data using a box plot
fig, ax = plt.subplots(figsize=(8, 6))
b_plot = ax.boxplot(data, vert=True, patch_artist=True, notch=True)
ax.set_ylabel('Value')
ax.set_title('Box Plot')
ax.spines['top'].set_visible(False)
ax.spines['bottom'].set_visible(False)
ax.spines['right'].set_visible(False)
ax.spines['left'].set_visible(False)
ax.xaxis.set_ticks_position('none') 
ax.yaxis.set_ticks_position('none')
ax.xaxis.set_ticks([])
# Color the box
for box in b_plot['boxes']:
    box.set_facecolor(sns.color_palette("hls",24)[12])
```

![](img/83f22edd9fd52523f395e0719e593cdb.png)

图片由作者提供。

## 小提琴图

除了箱线图，[小提琴图](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.violinplot.html)不仅展示了数据的分布，还显示了其**概率密度**。

![](img/c663f6960ced0a7486794e0be04e1f32.png)

图片由作者提供。

在这种情况下，离群点表现为超出数据主体的细小部分。一般来说，如果我注意到小提琴图的某些部分远远超出其他部分，那些很可能是离群点。

你可以用以下代码生成相同的图，或对其进行个性化调整。

```py
# Visualize data using a violin plot
fig, ax = plt.subplots(figsize=(8, 6))
v_plot = ax.violinplot(data, vert=True, showmedians=True, showextrema=False)
ax.set_ylabel('Value')
ax.set_title('Violin Plot')
ax.spines['top'].set_visible(False)
ax.spines['bottom'].set_visible(False)
ax.spines['right'].set_visible(False)
ax.spines['left'].set_visible(False)
ax.xaxis.set_ticks_position('none') 
ax.yaxis.set_ticks_position('none')
ax.xaxis.set_ticks([])
# Color the violin
for pc in v_plot['bodies']:
    pc.set_facecolor(sns.color_palette("hls",24)[7])
    pc.set_edgecolor('black')
    pc.set_alpha(.8)
```

# 统计方法

虽然图形方法确实更易理解，但也有局限性。主要问题是它们提供的**是定性的而非定量的**结果。因此，散点图、箱线图和分布图有助于**有效沟通**，但为了进行一致的分析，我们必须依赖**数学严谨性**和**统计指标**。

统计工具如[Z 分数](https://www.khanacademy.org/math/statistics-probability/modeling-distributions-of-data/z-scores/a/z-scores-review#:~:text=A%20z%2Dscore%20measures%20exactly,z%20%3D%20x%20%E2%88%92%20%CE%BC%20%CF%83%20%E2%80%8D)和[四分位距](https://en.wikipedia.org/wiki/Interquartile_range) (IQR)利用统计参数评估数据点。它们使数据科学家能够通过测量数据点偏离预期统计分布的程度，系统地识别离群点。

考虑**Z 分数**，它测量数据点距离均值的标准差数。Z 分数超过某个阈值的数据点可以被标记为离群点。通常，Z 分数高于 2 或 3 表示离群点。

```py
import numpy as np

# Generate a random dataset with outliers (100 normal points and 10 outliers)
np.random.seed(42)
data = np.concatenate((np.random.normal(loc=50, scale=10, size=100), 
                        np.random.normal(loc=110, scale=20, size=10)))

# Calculate mean and standard deviation
mean_data = np.mean(data)
std_dev = np.std(data)

# Set Z-score threshold (typically 2 or 3)
z_score_threshold = 2

# Identify outliers using Z-score
outliers = [value for value in data if (value - mean_data) / std_dev > z_score_threshold]
```

类似地，**IQR**依赖于数据分布的第一和第三四分位数之间的范围。任何显著超出此范围的数据点都会被识别为离群点。由于有时第一和第三四分位数之间的范围可能过于严格，我们可以进行参数调整，例如考虑第一和第九分位数之间的范围。

从数学上讲，超出范围*Q1–1.5*IQR*和*Q3+1.5*IQR*的数据点通常被归类为离群点。

另外，[**Tukey 的围栏**](https://community.ibm.com/community/user/ai-datascience/blogs/moloy-de1/2021/03/23/points-to-ponder)方法是一种基于 IQR 范围的参数方法。它将所有落在*Q1-k*IQR*和*Q3+k*IQR*范围之外的数据点视为异常值，其中*k*是一个常数。通常，*k*的取值在 1.5 到 3 之间。

```py
import numpy as np

# Generate a random dataset with outliers (100 normal points and 10 outliers)
np.random.seed(42)
data = np.concatenate((np.random.normal(loc=50, scale=10, size=100), 
                        np.random.normal(loc=110, scale=20, size=10)))

# Calculate Q1 and Q3
Q1 = np.percentile(data, 25)
Q3 = np.percentile(data, 75)

# Calculate IQR
IQR = Q3 - Q1

# Set lower and upper bounds for outliers
lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

# Identify outliers using IQR method
outliers = [value for value in data if value < lower_bound or value > upper_bound]
```

在同一数据集中，以图形化方式展示这三种异常值检测方法的区别非常有趣。

![](img/8a3e6003d7a957317df378e86c02fc3b.png)

作者提供的图片。

你可以看到，在这种情况下，原始的 IQR 方法过于严格。

# 算法方法

最后，算法方法利用了**机器学习算法**的力量，克服了简单统计方法的局限性。

存在多种异常值检测模型，包括[**孤立森林**](http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.IsolationForest.html)、[**局部异常因子**](http://scikit-learn.org/stable/modules/generated/sklearn.neighbors.LocalOutlierFactor.html)和[**一类支持向量机**](http://scikit-learn.org/stable/modules/generated/sklearn.svm.OneClassSVM.html)。这些模型提供了可靠的技术来辨别复杂多维数据集中的异常值。与传统统计方法不同，这些算法更擅长理解数据的复杂模式和定义更复杂的决策边界。

## **孤立森林**

我介绍的第一个异常值检测算法是[孤立森林](http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.IsolationForest.html)，因为它可能是我在日常任务中使用**最频繁**的机器学习模型。

**孤立森林**依赖于更著名的随机森林的原理，以及整体上的**集成学习技术**。如果你不熟悉随机森林或集成学习，我建议你参考这个简单的指南。

[](/ensemble-learning-with-scikit-learn-a-friendly-introduction-5dd64650de6c?source=post_page-----382d1411b8ec--------------------------------) ## 使用 Scikit-Learn 的集成学习：友好的介绍

### 像 XGBoost 或随机森林这样的集成学习算法是 Kaggle 竞赛中的顶级表现模型之一…

[towardsdatascience.com

孤立森林的核心思想基于这样一种观察：异常值由于其稀有性，通常需要在树结构中**较少的步骤就能被孤立**。因此，孤立森林构建了一个**决策树的集成**，由于其稀疏特性，更快速地孤立异常值。通过测量这些树中数据点的平均路径长度，孤立森林有效地为每个数据点量化异常值分数。

实现 Python 中的 Isolation Forest 非常简单，这要归功于[Scikit-Learn](https://scikit-learn.org/)（sklearn）库。

```py
from sklearn.ensemble import IsolationForest
import numpy as np

# Generate synthetic data with outliers
np.random.seed(42)
normal_data = np.random.normal(loc=50, scale=10, size=10000)
outliers = np.random.normal(loc=20, scale=5, size=1000)
data = np.concatenate((normal_data, outliers)).reshape(-1, 1)

np.random.shuffle(data)

# Apply Isolation Forest for outlier detection
clf = IsolationForest(contamination=0.1, random_state=42)
clf.fit(data)

# Predict outliers
outlier_preds = clf.predict(data)
outliers_indices = np.where(outlier_preds == -1) 
```

## 局部异常因子（LOF）

基于这样一个观点：异常点通常在特征空间中比其* k *最近邻更孤立，[**局部异常因子**](http://scikit-learn.org/stable/modules/generated/sklearn.neighbors.LocalOutlierFactor.html)（LOF）评估每个数据点的局部邻域，计算其相对于邻居的密度。

异常值通常显示出显著低于其邻居的局部密度，这使得它们通过 LOF 算法被检测出来。

[Scikit-Learn](https://scikit-learn.org/)（sklearn）提供了一个方便的工具来用 Python 实现 LOF。

```py
from sklearn.neighbors import LocalOutlierFactor
import numpy as np

# Generate synthetic data with outliers
np.random.seed(42)
normal_data = np.random.normal(loc=50, scale=10, size=10000)
outliers = np.random.normal(loc=20, scale=5, size=1000)
data = np.concatenate((normal_data, outliers)).reshape(-1, 1)

np.random.shuffle(data)

# Apply Local Outlier Factor (LOF) for outlier detection
clf = LocalOutlierFactor(n_neighbors=20, contamination=0.1)
outlier_preds = clf.fit_predict(data)

# Identify outlier indices
outliers_indices = np.where(outlier_preds == -1)
```

应用 LOF 算法时需要调整的参数包括用于密度估计的邻居数量以及污染系数。最后一个参数表示期望的异常值比例。

# 结论

我展示了三种不同的异常检测方法，每种方法都有**优点和局限性**。

**图形化方法**，如散点图和箱线图，毫无疑问是最直观的方法，适用于初步的数据探索。然而，它可能在处理高维数据时遇到困难，并且缺乏数值精度，仅仅是一个定性工具。

图形化方法的这一缺点被**统计方法**的数值稳健性所弥补。像 Z-score 这样的统计方法提供了数据异常值的精确度量，并探索了更复杂的数据关系。统计方法的局限在于数据常常假设为正态分布，这导致在处理偏斜数据时遇到一些困难。

最后，**机器学习**算法如 Isolation Forest 是前沿方法，因为它们在理论上比图形化和统计方法更强大。它们在理解复杂的数据空间中表现出色，其中模式难以发现。这些优点伴随着参数调整的限制。

这个介绍是一个很好的起点，但它仅仅触及了异常检测领域的表面。对于那些有兴趣深入这个领域的人，我提供了一些有趣且富有洞察力的资源列表。

如果你喜欢这个故事，可以考虑关注我，以便了解我即将推出的项目和文章！

这是我过去的一些项目：

[](/social-network-analysis-with-networkx-a-gentle-introduction-6123eddced3?source=post_page-----382d1411b8ec--------------------------------) ## 使用 NetworkX 进行社交网络分析：温和的介绍

### 了解像 Facebook 和 LinkedIn 这样的公司如何从网络中提取洞察

towardsdatascience.com [](/advanced-dimensionality-reduction-models-made-simple-639fca351528?source=post_page-----382d1411b8ec--------------------------------) ## 高级降维模型简明介绍

### 学习如何高效地应用先进的降维方法，并提升你的机器学习…

towardsdatascience.com [](/ensemble-learning-with-scikit-learn-a-friendly-introduction-5dd64650de6c?source=post_page-----382d1411b8ec--------------------------------) ## 使用 Scikit-Learn 进行集成学习：友好的介绍

### 类似 XGBoost 或随机森林的集成学习算法是 Kaggle 竞赛中的顶尖模型之一…

towardsdatascience.com

# *参考文献*

+   [Hawkins, D. (1980). 异常值的识别。Chapman and Hall。](https://link.springer.com/book/10.1007/978-94-015-3994-4)

+   [Chandola, V., Banerjee, A., & Kumar, V. (2009). 异常检测：综述。ACM 计算调查（CSUR），41(3)，1–58。](https://dl.acm.org/doi/10.1145/1541880.1541882)

+   [Tukey, J. W. (1977). 探索性数据分析。Addison-Wesley。](https://onlinelibrary.wiley.com/doi/10.1002/bimj.4710230408)

+   [Rousseeuw, P. J., & Leroy, A. M. (1987). 鲁棒回归和异常值检测。Wiley。](https://onlinelibrary.wiley.com/doi/book/10.1002/0471725382)

+   [Hoaglin, D. C., Mosteller, F., & Tukey, J. W. (1983). 理解鲁棒和探索性数据分析。Wiley。](https://www.wiley.com/en-us/Understanding+Robust+and+Exploratory+Data+Analysis-p-9780471384915)

+   [Liu, F. T., Ting, K. M., & Zhou, Z. H. (2008). 隔离森林。在 2008 年第八届 IEEE 国际数据挖掘会议论文集中（第 413–422 页）。](https://ieeexplore.ieee.org/document/4781136)

+   [Breunig, M. M., Kriegel, H. P., Ng, R. T., & Sander, J. (2000). LOF：基于密度的局部异常值识别。在 ACM SIGMOD 国际数据管理会议论文集中（第 93–104 页）。](https://www.dbs.ifi.lmu.de/Publikationen/Papers/LOF.pdf)

+   [用 Scikit-Learn, Keras 和 TensorFlow 进行动手机器学习，第 2 版 — Aurélien Géron](https://www.oreilly.com/library/view/hands-on-machine-learning/9781492032632/)ù

+   [Python 数据科学手册 数据处理的基本工具](https://jakevdp.github.io/PythonDataScienceHandbook/)
