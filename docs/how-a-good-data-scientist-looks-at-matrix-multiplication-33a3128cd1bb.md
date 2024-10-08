# 一个优秀的数据科学家如何看待矩阵乘法

> 原文：[`towardsdatascience.com/how-a-good-data-scientist-looks-at-matrix-multiplication-33a3128cd1bb`](https://towardsdatascience.com/how-a-good-data-scientist-looks-at-matrix-multiplication-33a3128cd1bb)

## 四种不同的视角

[](https://sekharm.medium.com/?source=post_page-----33a3128cd1bb--------------------------------)![Sekhar M](https://sekharm.medium.com/?source=post_page-----33a3128cd1bb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----33a3128cd1bb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----33a3128cd1bb--------------------------------) [Sekhar M](https://sekharm.medium.com/?source=post_page-----33a3128cd1bb--------------------------------)

·发表于[数据科学前沿](https://towardsdatascience.com/?source=post_page-----33a3128cd1bb--------------------------------) ·阅读时间 5 分钟·2023 年 2 月 23 日

--

![](img/b93b5d8d7496fa4351392950a690bef0.png)

作者提供的图片

# 介绍：

数据科学是通过科学计算方法、过程和算法从结构化或非结构化数据中提取知识和洞察的领域。

数据——无论是结构化数据（如数据表）还是非结构化数据（如图像）——都被表示为矩阵。

这些数据在计算过程（如机器学习模型）中的处理主要涉及矩阵乘法。

因此，对矩阵乘法操作的深入理解对从事数据科学和机器学习领域的人员大有裨益。

矩阵的乘法可以从 4 个不同的角度来看待：

1.  行和列的点积

1.  列的线性组合

1.  行的线性组合

1.  排名为 1 的矩阵之和

设*A*和*B*为相乘的矩阵，表示为*AB*。设*A*和*B*的维度分别为`*m*x*p*`和`*p*x*n*`。我们知道，为了能够相乘*A*和*B*，矩阵*A*的列数应与矩阵*B*的行数匹配。

![](img/ff625eb7e862c77ae2a67ba8bf25af24.png)

为了简单起见，并且不失一般性，我们考虑以下示例维度。

![](img/afac1616370f7245df45016dbbe6ba08.png)

如我们所知，下面的*AB*是两个矩阵*A*和*B*的乘积。

![](img/0ee472144eb534a0a757fb8be193c6eb.png)

# 1\. 行和列的点积：

在矩阵*AB*中，`element-11`可以看作是矩阵*A*的`row-1`和矩阵*B*的`column-1`的点积。

![](img/9605e2d7f3ee284924ca9f9b1beadb88.png)

同样，`element-12`可以看作是矩阵*A*的`row-1`和矩阵*B*的`column-2`的点积。

![](img/2176ee5b0b299283cbd5c1e1c93ec962.png)![](img/c7551f4814f5096ca284c5345967193b.png)

矩阵乘法作为行和列的点积（图片由作者提供）

## **视角：**

*AB*中的`Element-*ij*`是*A*的`row-*i*`与*B*的`column-*j*`的点积。

# 2\. 列的线性组合：

为了形成矩阵乘法的列视角，将矩阵*AB*重新组织如下。

![](img/41fc416908f08fec8e9b3ede90d03b57.png)

*AB*的第 1 列可以看作是`*b11*`倍的*A*的第 1 列和`*b21*`倍的第 2 列的和。即，*AB*的第 1 列是*A*列的线性组合（加权和），其中组合的权重是*B*的第 1 列的元素。

同样，`column-2`的*AB*是*A*列的线性组合，其中组合的权重是*A*的`column-2`的元素。

![](img/52d59417d63eb7b1120c7f80b5afc416.png)

矩阵乘法作为列的线性组合（图片由作者提供）

## **视角：**

*AB*中的每一列是*A*的列的线性组合，其中组合的权重是*B*中对应列的元素。

# 3\. 行的线性组合：

现在，让我们从行的角度来看*AB*，将其重写如下。

![](img/589aef6bb62c2fe18e56bd4ddac3479c.png)

*AB*的第 1 行可以看作是`*a11*`倍的*B*的第 1 行和`*a12*`倍的第 2 行的和。即，*AB*的第 1 行是*B*行的线性组合（加权和），其中组合的权重是*A*的第 1 行的元素。

同样，`row-2`的*AB*是*B*行的线性组合，其中组合的权重是*A*的`row-2`的元素。

![](img/1e909822bd4919e0a251ee577daba751.png)

矩阵乘法作为行的线性组合（图片由作者提供）

## **视角：**

*AB*中的每一行是*B*行的线性组合，其中组合的权重是*A*中对应行的元素。

# 4\. 秩-1 矩阵的和：

将*AB*重写如下，可以得到两个秩-1 矩阵，每个矩阵的大小与*AB*相同。

![](img/450532316894e9dc7a869b8ed56ea3ca.png)

很明显，上述两个矩阵是秩-1 的，因为它们的行（和列）都是线性相关的，即所有其他行（列）都是某一行（列）的倍数。因此，秩为 1。

![](img/4e8c4fa19c4cb8dc936900bda39d3fc9.png)

矩阵乘法作为秩-1 矩阵的和（图片由作者提供）

## **视角：**

矩阵*AB*是*p*个秩-1 矩阵的和，每个矩阵的大小为*m*x*n*，其中第`*i_*`个矩阵（在*p*中）是将*A*的`column-*i*`与*B*的`row-*i*`相乘的结果。

# 结论：

这些不同的视角在不同的场合中都有其相关性。

例如，在[Transformer](https://medium.com/towards-data-science/into-the-transformer-5ad892e0cee)神经网络架构中的*Attention*机制中，注意力矩阵的计算可以从“行和列的点积”角度看作矩阵乘法。

更多关于注意力机制和 Transformer 的信息可以在下面的文章中找到。

[](/into-the-transformer-5ad892e0cee?source=post_page-----33a3128cd1bb--------------------------------) ## 走进变压器

### 数据流、参数和维度

towardsdatascience.com

我希望这些关于矩阵乘法的视角能够帮助读者更直观地理解机器学习和数据科学算法与模型中的数据流。
