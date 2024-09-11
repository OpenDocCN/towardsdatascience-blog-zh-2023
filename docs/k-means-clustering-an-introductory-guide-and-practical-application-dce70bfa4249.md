# K-means 聚类：入门指南及实际应用

> 原文：[https://towardsdatascience.com/k-means-clustering-an-introductory-guide-and-practical-application-dce70bfa4249?source=collection_archive---------9-----------------------#2023-01-23](https://towardsdatascience.com/k-means-clustering-an-introductory-guide-and-practical-application-dce70bfa4249?source=collection_archive---------9-----------------------#2023-01-23)

[](https://medium.com/@kurt.klingensmith?source=post_page-----dce70bfa4249--------------------------------)[![Kurt Klingensmith](../Images/2249e99f12d10f81598c754b1aaf76cc.png)](https://medium.com/@kurt.klingensmith?source=post_page-----dce70bfa4249--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dce70bfa4249--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----dce70bfa4249--------------------------------) [Kurt Klingensmith](https://medium.com/@kurt.klingensmith?source=post_page-----dce70bfa4249--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbaf16815de65&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fk-means-clustering-an-introductory-guide-and-practical-application-dce70bfa4249&user=Kurt+Klingensmith&userId=baf16815de65&source=post_page-baf16815de65----dce70bfa4249---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dce70bfa4249--------------------------------) ·10 分钟阅读·2023 年 1 月 23 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdce70bfa4249&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fk-means-clustering-an-introductory-guide-and-practical-application-dce70bfa4249&user=Kurt+Klingensmith&userId=baf16815de65&source=-----dce70bfa4249---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdce70bfa4249&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fk-means-clustering-an-introductory-guide-and-practical-application-dce70bfa4249&source=-----dce70bfa4249---------------------bookmark_footer-----------)![](../Images/cbfe2141e10479a0bee80aa89d178bef.png)

不同发动机类型、尺寸和重量的汽车。作者摄影。

使用如 K-means 这样的聚类算法是机器学习中最常见的入门点之一。K-means 聚类是一种无监督的机器学习技术，它将相似的数据分组或聚集到一起。特定聚类中的数据在聚类内的观察值之间具有更高的相似度，而与聚类外的观察值相比则相对较低。

K均值中的K代表用户定义的*k*簇数量。K均值聚类通过尝试在数据中找到最佳的簇中心位置来工作，确保簇内的数据距离给定中心比其他中心更近。理想情况下，生成的簇在每个唯一簇内最大化数据之间的相似性。

请注意，存在各种聚类方法；本文将重点介绍最流行的技术之一：K均值。

## **本指南包括两个部分：**

1.  使用生成数据的K均值聚类简介。

1.  对汽车数据集应用K均值聚类。

## 代码：

所有代码[可以在这里的github页面上获取](https://github.com/kurtklingensmith/KMeansClustering)**。** 随时下载笔记本（点击CODE和Download Zip）并与本文一起运行！

# 1\. K均值聚类简介

对于本指南，我们将使用[scikit-learn](https://scikit-learn.org/stable/)库[1]：

```py
from sklearn.cluster import KMeans
from sklearn import preprocessing
from sklearn.datasets import make_blobs
```

为了演示K均值聚类，我们首先需要数据。方便的是，sklearn库包括了[生成数据块](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.make_blobs.html)的功能[2]。代码相当简单：

```py
# Generate sample data:
X, y = make_blobs(n_samples=150, 
                  centers=3, 
                  cluster_std=.45, 
                  random_state = 0)
```

make_blobs()函数的参数允许用户指定中心的数量（这可能与潜在的簇中心相关）以及“数据块”的混乱程度（cluster_std，调整簇的标准差）。上述代码生成了数据块；下面的代码将其转换为数据框和plotly散点图：

```py
# Import required libraries:
import plotly.express as px
import pandas as pd

# Convert to dataframe:
dfBlobs = pd.DataFrame(X, columns = ['X','Y'])

# Plot data:
plot = px.scatter(dfBlobs, x="X", y="Y")
plot.update_layout(   
    title={'text':"Randomly Generated Data",
           'xanchor':'center',
           'yanchor':'top',
           'x':0.5})
plot.show()
```

这是输出结果：

![](../Images/906ac1b0ddc8eb615f621159644161ae.png)

作者截图。

由于在make_blobs()函数中选择了较低的“cluster_std”值，生成的图形有三个非常明确的数据块，这应该很容易被K均值聚类算法处理。

## 多少个簇？

K均值中的K是簇的数量，由用户定义。对于给定的数据集，通常存在一个最佳的簇数量。在上面生成的数据中，可能是三个。

要数学地确定最佳的簇数量，可以使用“肘部法则”。该方法计算不同*k*值的簇内平方和（WCSS），一般较低的值更好。WCSS表示每个数据点到簇中心的平方距离之和。肘部法则将WCSS绘制为添加额外簇的结果；最终，随着新簇的增加，WCSS的下降变得缓慢，出现一个“肘部”，这揭示了最佳簇数量。

以下代码为上述数据生成一个肘部图：

```py
# Determine optimal number of clusters:
wcss = []

for k in range(1, 11):
    kmeans = KMeans(n_clusters=k, max_iter=5000, random_state=42)
    kmeans.fit(dfBlobs)
    wcss.append(kmeans.inertia_)

# Prepare data for visualization:
wcss = pd.DataFrame(wcss, columns = ['Value'])
wcss.index += 1
```

绘制后，这将产生：

```py
# Plot the elbow curve:
plot = px.line(wcss, y = "Value")
plot.update_layout(   
    title={'text':"Within Cluster Sum of Squares or 'Elbow Chart'",
           'xanchor':'center',
           'yanchor':'top',
           'x':0.5},
    xaxis_title = 'Clusters',
    yaxis_title = 'WCSS')
plot.show()
```

![](../Images/d7298ba26f86ccc1fb44e0c890840eca.png)

作者截图。

注意WCSS的图形在3个聚类时有一个明显的“肘部”。这意味着3是最佳聚类选择，因为WCSS值在增加到三个聚类时急剧下降。超过3个聚类只会带来WCSS减少的微小增益。因此，最佳的聚类值是*k* = 3。

## 生成聚类

下一步是运行K均值聚类算法。在下面的代码中，`kmeans = KMeans(3)`是输入*k*值的地方：

```py
# Cluster the data:
kmeans = KMeans(3)
clusters = kmeans.fit_predict(dfBlobs)

# Add the cluster labels to the dataframe:
labels = pd.DataFrame({'Cluster':clusters})
labeledDF = pd.concat((dfBlobs, labels), axis = 1)
```

结果是一个带有“Cluster”列的标记数据框：

![](../Images/d3ea0a5f10d0373005e1e87b05b5e8a6.png)

作者截图。

绘制结果如下：

```py
# Generate plot:

# Change Cluster column to strings for cluster visualization:
labeledDF["Cluster"] = labeledDF["Cluster"].astype(str)

# Generate plot:
plot = px.scatter(labeledDF, x="X", y="Y", color="Cluster")
plot.update_layout(
    title={'text': "Clustered Data",
           'xanchor': 'center',
           'yanchor': 'top',
           'x': 0.5})
plot.show()
```

![](../Images/bf6d46d3c7bc52287ee247dc92bd5372.png)

作者截图。

K均值方法已成功将数据分成三个不同的聚类。现在让我们看看更现实的数据会发生什么。

# 2\. 汽车数据中的K均值聚类

Python库Seaborn提供了各种数据集，包括来自石油危机时代汽车的燃油效率数据集。为了帮助学习聚类，我们将过滤数据集，选择8缸和4缸发动机的汽车。这代表了当时通常可用的最大和最小发动机。

幸运的是，这可能是一个真实世界分析场景的示例。在1970年代，燃油价格的快速上涨使得8缸车变得不那么受欢迎；因此，4缸车变得越来越普遍，但4缸和8缸发动机的汽车在燃油消耗方面的实际表现如何呢？

具体而言，我们将探讨4缸和8缸汽车在*重量*和*燃油效率*（以每加仑多少英里（MPG）衡量）方面的表现。

准备数据非常简单：

```py
import seaborn as sns

# Load in the data - Seaborn's mpg data:
df = sns.load_dataset('mpg')

# Filter for 4 and 8 cylinder cars:
df = df[(df['cylinders'] == 4) | (df['cylinders'] == 8)]
df = df.reset_index(drop=True)

# Display dataframe head:
df.head(3)
```

数据集如下所示：

![](../Images/2997f7cac78fb3d4fe037099ef3bc3eb.png)

作者截图。

可视化汽车的重量和MPG如下：

```py
# Plot the mpg and weight data:

plot = px.scatter(df, x='weight', y='mpg',
                  hover_data=['name', 'model_year'])
plot.update_layout(
    title={'text': "Vehicle Fuel Efficiency",
           'xanchor': 'center',
           'yanchor': 'top',
           'x': 0.5})
plot.show()
```

![](../Images/8b65254a0c286a353220d23e4eaaaa10.png)

作者截图。

## 数据缩放

注意x轴表示重量，而y轴表示MPG。增加10磅的重量不如增加10 MPG重要。这可能会影响聚类结果。

有多种选择来解决这个问题；Jeff Hale [有一篇很好的文章链接在这里](/scale-standardize-or-normalize-with-scikit-learn-6ccc7d176a02)，提供了有关缩放、标准化和归一化的各种方法和使用案例的技术概述[1]。对于这个练习，我们将使用Sci-Kit Learn的StandardScaler()函数。

```py
# Create DF copy for standardizing:
dfCluster = df.copy()

# Set the scaler:
scaler = preprocessing.StandardScaler()

# Normalize the two variables of interest:
dfCluster[['weight', 'mpg']] = scaler.fit_transform(dfCluster[['weight', 'mpg']])

# Create dataframe for clustering:
dfCluster = dfCluster[['weight', 'mpg']]

# View dataframe head:
dfCluster.head(3)
```

结果如下：

![](../Images/17fd6413a6a9755569920c8ce52f6c9f.png)

这个两列数据框现在可以通过肘部法则处理。

## 多少个聚类？

如第1节所述，我们使用相同的代码遵循肘部法则，以返回以下图表：

```py
# Determine optimal number of clusters:

wcss = []

for k in range(1, 11):
    kmeans = KMeans(n_clusters=k, max_iter=5000, random_state=42)
    kmeans.fit(dfCluster)
    wcss.append(kmeans.inertia_)

# Prepare data for visualization:
wcss = pd.DataFrame(wcss, columns=['Value'])
wcss.index += 1

# Plot the elbow curve:
plot = px.line(wcss)
plot.update_layout(
    title={'text': "Within Cluster Sum of Squares or 'Elbow Chart'",
           'xanchor': 'center',
           'yanchor': 'top',
           'x': 0.5},
    xaxis_title='Clusters',
    yaxis_title='WCSS')
plot.update_layout(showlegend=False)
plot.show()
```

![](../Images/a230b8f3b371d374be369d31af942c31.png)

作者截图。

两个聚类似乎是肘部。然而，从2到3个聚类的跳跃不像第1节数据块示例中的那样平缓。

**注意：** 肘部法则可能不会始终提供最清晰的结果，因此需要尝试肘部区域中几个不同的*k*值。虽然我们选择了2个簇，但尝试3个簇也可能值得。有关该主题的更高级分析，Satyam Kumar在这篇[《数据科学中的轮廓法》](https://github.com/kurtklingensmith/KMeansClustering)中提供了一种替代的肘部法则方法。

## 生成簇

生成簇的代码与之前的示例类似：

```py
# Cluster the data:
kmeans = KMeans(2)
clusters = kmeans.fit_predict(dfCluster)

# Add the cluster labels to the dataframe:
labels = pd.DataFrame({'Cluster': clusters})
labeledDF = pd.concat((df, labels), axis=1)
labeledDF['Cluster'] = labeledDF['Cluster'].astype(str)
```

这使数据框回到了我们开始的地方，但新增了一列——标识每个观测值的簇：

![](../Images/06bbcac8532cf712f93c47d08da6431b.png)

作者截图。

绘制这些数据揭示了簇：

```py
# Generate plot:

plot = px.scatter(labeledDF, x="weight", y="mpg", color="Cluster",
                  hover_data=['name', 'model_year', 'cylinders'])
plot.update_yaxes(categoryorder='category ascending')
plot.update_layout(
    title={'text': "Clustered Data",
           'xanchor': 'center',
           'yanchor': 'top',
           'x': 0.5})
plot.show()
```

![](../Images/24c319cf86fe9a5cd87dccc76d8d1808.png)

作者截图。

## 进一步分析

请记住，K均值聚类试图根据数据的相似性对数据进行分组，这些相似性由与簇质心的距离决定。将簇值添加到原始数据框中可以对原始数据集进行额外的分析。例如，上述簇可视化显示了大约3000磅和20 MPG之间的簇分裂。

额外的可视化可能会提供更多见解。考虑以下条形图（[代码见链接的Git页面](https://github.com/kurtklingensmith/KMeansClustering)）：

![](../Images/6cb232be802e906e802415e355cda643.png)

作者截图。

首先，K均值聚类仅根据重量和MPG将车辆分类，结果几乎完美地与气缸数对齐。一个簇是100%4缸车，而另一个几乎全是8缸车。

其次，主要是8缸的簇中有四辆4缸车。尽管这些车的发动机较小，但它们的MPG表现接近于大型8缸车。簇零中的4缸车都接近3000磅重量，同时MPG表现为20或更低。

最终，似乎有些8缸车的MPG得分接近4缸车。另一张图表显示这些例子被认为是异常值（[代码见Git页面](https://github.com/kurtklingensmith/KMeansClustering)）：

![](../Images/d92f0ba49a072dec3bfad8c9b7968a7a.png)

作者截图。

这是聚类如何帮助理解数据并指导后续分析和数据探索的一个例子。工程师可能会发现分析为什么某些4缸车在效率上表现较差并与8缸车聚类在一起是有意义的。

**欲进行额外练习和学习：** [访问链接中的完整笔记本](https://github.com/kurtklingensmith/KMeansClustering)，重新加载 seaborn MPG 数据集，但不要过滤 4 和 8 缸的汽车。你会发现肘部法揭示了 2 与 3 聚类之间的模糊分界——这两者都值得尝试。或者，尝试 [使用其他 seaborn 数据集](/seaborn-essentials-for-data-visualization-in-python-291aa117583b) 或 MPG 数据集中的其他特征 [5]。

**K-means 聚类是处理这些数据的最佳技术吗？**

记住，对于有 blobs 的示例，K-means 肘部法有一个非常明确的最佳点，结果聚类分析也很容易识别出不同的 blobs。K-means 通常在数据更加球形的情况下表现更好，就像数据 blobs 的情况一样。其他方法可能在复杂数据中效果更佳，甚至在本示例中使用的汽车数据中。有关一些替代聚类方法的进一步阅读，Victor Roman 在这篇 Towards Data Science 文章中提供了很好的 [概述](/unsupervised-machine-learning-clustering-analysis-d40f2b34ae7e) [6]。

# 结论

K-means 聚类是一个强大的机器学习工具，可以识别数据中的相似性。这种技术可以提供见解或增强对数据的理解，从而指导进一步的分析问题并改善数据可视化。本文及代码提供了 K-means 聚类的指南，但还有其他聚类技术可用，根据分析的数据类型，有些技术可能更为合适。即使 K-means 不是给定数据的最合适方法，K-means 聚类仍然是一个值得了解的优秀方法，也是熟悉机器学习的良好起点。此外，K-means 聚类还可以作为其他聚类方法的基准，尽管它可能不是给定数据集的理想聚类算法，但仍可能证明有用。

## 参考文献：

[1] Scikit learn, [scikit-learn: machine learning in Python](https://scikit-learn.org/stable/) (2023)。

[2] Scikit learn, [sklearn.datasets.make_blobs](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.make_blobs.html) (2023)。

[3] J. Hale, [使用 Scikit-Learn 进行缩放、标准化或归一化](/scale-standardize-or-normalize-with-scikit-learn-6ccc7d176a02) (2019), Towards Data Science。

[4] S. Kumar, [轮廓法——比肘部法更好的寻找最佳聚类方法](/silhouette-method-better-than-elbow-method-to-find-optimal-clusters-378d62ff6891) (2020), Towards Data Science。

[5] M. Alam, [Python 中的数据可视化必备 Seaborn](/seaborn-essentials-for-data-visualization-in-python-291aa117583b) (2020), Towards Data Science。

[6] V. Roman, [机器学习：聚类分析](/unsupervised-machine-learning-clustering-analysis-d40f2b34ae7e) (2019), Towards Data Science。
