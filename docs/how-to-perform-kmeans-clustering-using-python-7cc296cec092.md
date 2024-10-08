# 如何使用 Python 执行 KMeans 聚类

> 原文：[`towardsdatascience.com/how-to-perform-kmeans-clustering-using-python-7cc296cec092`](https://towardsdatascience.com/how-to-perform-kmeans-clustering-using-python-7cc296cec092)

## KMeans 聚类和 Python 实现的完整概述

[](https://zoumanakeita.medium.com/?source=post_page-----7cc296cec092--------------------------------)![Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----7cc296cec092--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7cc296cec092--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7cc296cec092--------------------------------) [Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----7cc296cec092--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7cc296cec092--------------------------------) ·阅读时间 7 分钟·2023 年 1 月 17 日

--

# 介绍

假设你是一家零售公司工作的数据科学家，你的老板要求将客户根据消费行为划分为以下组别：低价值、平均价值、中等价值或铂金客户，以便进行有针对性的市场营销和产品推荐。

*考虑到这些客户没有任何历史标签，我们如何对他们进行分类？*

这就是聚类能够发挥作用的地方。它是一种无监督机器学习技术，用于将未标记的数据分组到相似的类别或簇中。

本概念文章将更多地关注 K-means 聚类方法，这是无监督机器学习中众多技术之一。文章将首先概述 K-means 聚类的基本概念，然后带你逐步实现 Python 中的操作，使用流行的 `Scikit-learn` 库。

# 什么是 K-Means 聚类？

K-means 聚类的基本思想是将数据集划分为指定数量的簇（k），其中同一簇内的所有点彼此相似，而不同簇中的点则不同。

它的工作原理是随机将每个数据点分配到一个簇中，然后通过将数据点移动到距离它们最近的簇中心来迭代改进这些簇。这一逻辑持续进行，直到簇分配停止变化，或者达到最大迭代次数。

## K-means 聚类的关键步骤是什么？

以下是 K-means 算法的五个主要步骤：

![](img/2610f3f6f8203ae41401ad9e3af82484.png)

K-Means 聚类的五个主要步骤（图像由作者提供）

下面我们可以看到 K-means 的一个示意图，其中在第 14 次迭代时达到了收敛。

![](img/25ecacf287650a53419b0b12d8514e19.png)

*k*-均值聚类算法的收敛（[图片来源于维基百科](https://en.wikipedia.org/wiki/K-means_clustering)）

# K-means 聚类的实际应用

现在我们了解了 K-means 的工作原理，让我们看看如何在 Python 中实现它。

首先，你需要安装以下库：

+   使用 `Pandas` 加载数据框。

+   使用 `Matplotlib` 进行数据可视化。

+   使用 `Scikit-learn` 的 `Kmean` 算法。

安装可以通过 `pip`，Python 包管理器，按如下方式进行：

```py
# Scikit Learn
pip install scikit-learn

# Pandas
pip install pandas

# Matplotlib
pip install matplotlib
```

## 导入库并加载数据

现在你对 K-means 聚类算法有了理解，让我们深入探讨。我们将使用 [Mall Customer Data](https://www.kaggle.com/datasets/shwetabh123/mall-customers)，这是 Kaggle 上免费提供的数据。

对于每个客户，它包含这些基本信息：`ID`、`Gender`、`Age`、`Income` 和 `Annual Spending score`

```py
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.cluster import KMeans

# load the customer data into a DataFrame
customer_df = pd.read_csv('customer_data.csv')

# Check the first 5 rows
customer_df.head()
```

之前的 `.head()` 语句应该生成以下结果：

![](img/a00dbf8657271fa7dd5eedb591b3daaf.png)

客户数据的前 5 行（图片由作者提供）

## 探索数据

在进一步实现算法之前，让我们快速统计和可视化地了解数据。

```py
plt.scatter(customer_df["Age"], 
            customer_df["Spending Score (1-100)"])

plt.xlabel("Age")
plt.ylabel("Spending Score (1-100)")
```

![](img/32ae14bab6a9f4cfd40f6734b0d9b2a7.png)

客户年龄与花费得分的散点图（图片由作者提供）

```py
 plt.scatter(customer_df["Age"], 
            customer_df["Annual Income (k$)"])

plt.xlabel("Age")
plt.ylabel("Annual Income (k$)")
```

![](img/2fd0f59d38c739d8217cc03202549c94.png)

客户年龄与年收入的散点图（图片由作者提供）

```py
plt.scatter(customer_df["Spending Score (1-100)"], 
            customer_df["Annual Income (k$)"])

plt.xlabel("Spending Score (1-100)")
plt.ylabel("Annual Income (k$)")
```

![](img/1926663f6c7994ee5f0573bf86982e92.png)

客户花费得分与年收入的散点图（图片由作者提供）

所有这些图表有不同的结果，从而导致不同的解释。例如，第一个图表似乎提出了两个不同的客户群体，而第二个图表不明显，最后一个图表显示了五个不同的群体。这时，Kmeans 将有助于有效生成正确的群体/簇。

此外，我们从以下结果中看到数据中没有缺失值。

```py
# Check for null values
customer_df.isnull().sum()
```

![](img/fb47ca1e71039f263e87d237d118fbc0.png)

数据中没有空值（图片由作者提供）

## 获取用于聚类的相关列

并不是所有的列都与聚类相关。在这个例子中，我们将使用数值列：`Age`、`Annual Income` 和 `Spending Score`

```py
relevant_cols = ["Age", "Annual Income (k$)", "Spending Score (1-100)"]

customer_df = customer_df[relevant_cols]
```

## 数据转换

Kmeans 对数据的测量单位和尺度敏感。最好先对数据进行标准化以解决这个问题。此外，这是在实施任何机器学习模型之前的常见做法。

基本上，标准化过程是从实际值中减去任何特征的均值，然后除以该特征的标准差。

这个过程很简单，以下是用 Python 完成的步骤：

+   使用 `sklearn.preprocessing` 模块中的 `StandardScaler` 类。

+   应用 `fit()` 方法来计算特征的均值和标准差。

+   然后最终使用 `transform()` 来缩放数据。

```py
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

scaler.fit(customer_df)

scaled_data = scaler.transform(customer_df)
```

## 确定最佳的簇数量

如果我们未能确定正确的簇数量，则聚类模型将无关紧要。文献中存在多种技术。我们将考虑肘部法，它是一种启发式方法，也是寻找最佳簇数量的广泛使用方法之一。

第一个辅助函数为每个`K`值创建对应的`KMeans`模型，并保存其惯性值及实际`K`值。

第二个函数利用这些惯性值和`K`值生成最终的肘部图。

```py
def find_best_clusters(df, maximum_K):

    clusters_centers = []
    k_values = []

    for k in range(1, maximum_K):

        kmeans_model = KMeans(n_clusters = k)
        kmeans_model.fit(df)

        clusters_centers.append(kmeans_model.inertia_)
        k_values.append(k)

    return clusters_centers, k_values
```

```py
def generate_elbow_plot(clusters_centers, k_values):

    figure = plt.subplots(figsize = (12, 6))
    plt.plot(k_values, clusters_centers, 'o-', color = 'orange')
    plt.xlabel("Number of Clusters (K)")
    plt.ylabel("Cluster Inertia")
    plt.title("Elbow Plot of KMeans")
    plt.show()
```

现在，我们可以使用最大`K`值为`12`的函数对数据集进行处理，得到最终结果。

```py
clusters_centers, k_values = find_best_clusters(scaled_data, 12)

generate_elbow_plot(clusters_centers, k_values)
```

以下是最终结果。

![](img/ff7dca91c819e23166e404210d4ffc6f.png)

寻找最佳簇数量的图表（图片由作者提供）

从图中我们可以看到，随着簇的数量增加，簇的惯性减少。而且，在`K=5`之后，惯性的下降幅度最小，因此`5`可以被认为是最佳的簇数量。

## 创建最终的 KMeans 模型

一旦我们确定了最佳的簇数量，我们可以按照以下步骤将 KMeans 模型应用于该值。

```py
kmeans_model = KMeans(n_clusters = 5)

kmeans_model.fit(scaled_data)
```

我们可以通过使用`.labels_`属性来访问每个数据点所属的簇。让我们创建一个对应这些值的新列。

```py
customer_df["clusters"] = kmeans_model.labels_

customer_df.head()
```

![](img/cbb0bca74c9adba6f05e52f62c2c1a08.png)

聚类后的最终数据集（图片由作者提供）

通过查看前 5 个客户，我们可以观察到前两个和最后两个被分配到第一个簇（簇#1），而第三个客户属于第三个簇（簇#3）。

## 可视化簇

既然我们已经生成了簇，最后一步是可视化它们。

```py
plt.scatter(customer_df["Spending Score (1-100)"], 
            customer_df["Annual Income (k$)"], 
            c = customer_df["clusters"])
```

![](img/cdd8dad72510c59e60c065afe9baf18b.png)

簇可视化（图片由作者提供）

KMeans 聚类似乎生成了相当不错的结果，五个簇之间分隔良好，尽管紫色和黄色簇之间有轻微的重叠。

总体观察如下：

+   左上角的客户消费评分较低但年收入较高。可以实施一种好的营销策略来针对这些客户，使他们能够花费更多。

+   另一方面，左下角的客户年收入较低，支出也较少，这很合理，因为他们试图根据预算调整消费习惯。

+   右上角的客户与左下角的客户类似，不同之处在于他们有足够的预算来消费。

+   最终，黄色组的客户超出了他们的预算。

# 结论

恭喜，你已经学会了如何使用 Python 进行 KMeans 聚类。希望你已经掌握了高效分析未标记数据集所需的技能。

如果你喜欢阅读我的故事并希望支持我的写作，可以考虑[成为 Medium 会员](https://zoumanakeita.medium.com/membership)。通过每月$5 的承诺，你可以无限制地访问 Medium 上的故事。

随时在[我的社交网络](https://linktr.ee/zoumana)上关注我。讨论 AI、ML、数据科学、NLP 和 MLOps 相关话题总是一件愉快的事！

博客的源代码[可以在 GitHub 上找到](https://github.com/keitazoumana/Medium-Articles-Notebooks/blob/main/KMean_Clustering.ipynb)。
