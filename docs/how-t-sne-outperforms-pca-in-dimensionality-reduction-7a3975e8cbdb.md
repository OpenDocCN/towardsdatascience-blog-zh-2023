# t-SNE 如何在降维中优于 PCA

> 原文：[`towardsdatascience.com/how-t-sne-outperforms-pca-in-dimensionality-reduction-7a3975e8cbdb`](https://towardsdatascience.com/how-t-sne-outperforms-pca-in-dimensionality-reduction-7a3975e8cbdb)

## PCA 与 t-SNE 在低维空间中可视化高维数据的比较

[](https://rukshanpramoditha.medium.com/?source=post_page-----7a3975e8cbdb--------------------------------)![Rukshan Pramoditha](https://rukshanpramoditha.medium.com/?source=post_page-----7a3975e8cbdb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7a3975e8cbdb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7a3975e8cbdb--------------------------------) [Rukshan Pramoditha](https://rukshanpramoditha.medium.com/?source=post_page-----7a3975e8cbdb--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7a3975e8cbdb--------------------------------) ·15 分钟阅读·2023 年 5 月 23 日

--

![](img/3b7e20bb0c5cf927b95d2c4314e06247.png)

照片由 [Pat Whelen](https://unsplash.com/@patwhelen?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/Rxt252RzQlY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在机器学习中，**降维**是指减少数据集中的输入变量数量。输入变量的数量即数据集的**维度**。

降维技术主要分为两大类：*线性*和*非线性（流形）*。

在线性方法下，我们讨论了 [*主成分分析（PCA）*](https://medium.com/data-science-365/3-easy-steps-to-perform-dimensionality-reduction-using-principal-component-analysis-pca-79121998b991)，*因子分析（FA）*，*线性判别分析（LDA）* 和 *非负矩阵分解（NMF）*。

在非线性方法下，我们讨论了 *自编码器（AEs）* 和 *核 PCA*。

**t-分布随机邻域嵌入（t-SNE）** 也是一种*非线性*降维方法，用于在低维空间中可视化高维数据，以寻找数据中的重要集群或群体。

> 所有的降维技术都属于无监督机器学习的范畴，我们可以在不需要标签的情况下揭示数据中的隐藏模式和重要关系。
> 
> 因此，降维算法处理的是未标记的数据。在训练这样的算法时，**fit()**方法只需要特征矩阵**X**作为输入，而不需要标签列**y** — **来源：** 用于图像数据降维的非负矩阵分解（NMF）

```py
**What you will learn:
---------------------------------------------------** 01\. Advantages of t-SNE over PCA
02\. Disadvantages of t-SNE
03\. How t-SNE works
04\. Python implementation of t-SNE - TSNE() class
05\. Important arguments of TSNE() class
06\. KL divergence
07\. The MNIST data in tabular format
08\. Visualizing MNIST data using PCA
09\. Visualizing MNIST data using t-SNE
10\. PCA before t-SNE (Very special trick)
11\. Choosing the right value for perplexity
12\. Changing the right number of iterations
13\. Randomness of initialization
14\. PCA initialization
15\. Using a random state in random initialization

**Other dimensionality reduction methods
---------------------------------------------------**
1\. [Principal Component Analysis (PCA)](https://medium.com/data-science-365/3-easy-steps-to-perform-dimensionality-reduction-using-principal-component-analysis-pca-79121998b991)
2\. Factor Analysis (FA)
3\. Linear Discriminant Analysis (LDA)
4\. Non-Negative Matrix Factorization (NMF)
5\. Autoencoders (AEs)
6\. Kernel PCA
```

# t-SNE 相对于 PCA 的优点

t-SNE 相对于 PCA 有两个主要优点：

+   t-SNE 可以在减少数据的维度后保持数据点之间的空间关系。这意味着在原始维度中相邻的数据（具有相似特征的点）在低维中仍将保持相邻！这就是为什么 t-SNE 主要用于发现数据中的聚类。

+   t-SNE 可以处理在实际应用中非常常见的非线性数据。

> PCA 尝试通过最大化数据的方差来减少维度，而 t-SNE 则通过在高维和低维中将相似的数据点保持在一起（并将不相似的数据点分开）来实现相同的目标。

因此，t-SNE 在降维方面往往能超越 PCA。今天，你将看到对同一数据集上 t-SNE 和 PCA 的实际实现。你可以比较这两种方法的输出，并验证这一事实！

# t-SNE 的缺点

+   t-SNE 的主要缺点是它在大数据集上执行算法时需要大量计算资源。因此，当数据的维度非常高时，执行 t-SNE 是非常耗时的。为了解决这个问题，我们将讨论一个非常特别的技巧。你可以在显著减少时间的同时获得几乎相似的结果！

+   另一个缺点是，如果使用随机初始化，即使在相同的超参数值下，t-SNE 也会得到不稳定（不同）的结果。你可以在最后学习更多关于这个问题的信息。

你还将学习如何调整 Scikit-learn 的 t-SNE 算法中的一些最重要的超参数，以获得更好的结果！

所以，继续阅读吧！

# t-SNE 的工作原理

t-SNE 计算中涉及两个概率分布。因此，正如其名称所暗示的，算法本质上是随机的。

+   在高维空间中，我们使用**Gaussian (normal) distribution**将数据点之间的成对距离转换为条件概率。

+   在低维空间中，我们使用**Student’s t-distribution**将数据点之间的成对距离转换为条件概率。

KL 散度测量这两个概率分布之间的差异（不同）。这是我们在算法训练过程中尝试最小化的成本函数。

所以，t-SNE 的目标是将数据点定位到低维空间中，以便尽可能最小化两个概率分布之间的 KL 散度。

t-SNE 需要大量计算资源和时间来进行这些概率计算，尤其是在处理大型数据集时。这就是为什么 t-SNE 比 PCA 慢得多。对此，我们将讨论一个非常特别的技巧。

# t-SNE 的 Python 实现

在 Python 中，t-SNE 是通过使用 Scikit-learn 的 **TSNE()** 类来实现的。Scikit-learn 是 Python 机器学习库。

首先，你需要导入并创建 **TSNE()** 类的实例。

```py
# Import
from sklearn.manifold import TSNE

# Create an instance
TSNE_model = TSNE(n_components=2, perplexity=30.0, learning_rate='auto',
                  n_iter=1000, init='pca', random_state=None)
```

## TSNE() 类的重要参数

**TSNE()** 类中有许多参数。如果我们没有直接指定这些参数，它们在调用 **TSNE()** 函数时会取其默认值。要了解更多关于这些参数的信息，请参阅 [Scikit-learn 文档](https://scikit-learn.org/stable/modules/generated/sklearn.manifold.TSNE.html#examples-using-sklearn-manifold-tsne)。

以下列表包含了 **TSNE()** 类中最重要参数的详细解释。

+   **n_components:** 一个整数，定义嵌入空间的维度数量。默认值为 2。最常用的值是 2 和 3，分别用于在 2D 和 3D 空间中可视化数据，因为 t-SNE 主要用于数据可视化。

+   **perplexity:** 在可视化数据时考虑的最近邻居的数量。这是 TSNE() 类中最重要的参数。默认值为 30.0，但强烈建议尝试 5 到 50 之间的值，因为不同的值可能会产生显著不同的结果。你需要绘制不同困惑度值的 KL 散度，并选择合适的值。这在技术上称为 *超参数调优*。一般来说，较大的数据集需要较大的困惑度值。

+   **learning_rate:** TSNE() 函数使用随机梯度下降来最小化 KL 散度成本函数。随机梯度下降优化器需要一个合适的学习率值来执行此过程。学习率决定了优化器最小化损失函数的快慢。较大的值可能会导致模型无法收敛，而较小的值可能需要太多时间才能收敛。

+   **n_iter:** 为梯度下降设置的最大迭代次数。默认值为 1000。

+   **init:** 初始化方法。有两个选项：*“random”* 或 *“pca”*。默认是 PCA 初始化，这比随机初始化更稳定，并且在不同执行之间产生相同的结果。如果选择随机初始化，由于 TSNE 具有非凸成本函数，且 GD 优化器可能会停留在局部最小值，算法会在不同的执行中生成不同的结果。

+   **random_state：** 一个整数，用于在使用随机初始化时获取相同的结果，以避免每次运行算法时生成显著不同的结果。常用的整数有 0、1、2 和 42。了解更多信息 这里。

## TSNE() 类的重要方法

+   **fit(X):** 从特征矩阵 **X** 学习一个 TSNE 模型。这里不进行任何转换。

+   **fit_transform(X):** 从特征矩阵 **X** 学习一个 TSNE 模型，并返回 TSNE 转换后的数据。

```py
TSNE_transformed_data = TSNE_model.fit_transform(X)
```

## TSNE() 类的重要属性

+   **kl_divergence_:** 返回优化后的 Kullback-Leibler 散度。GD 优化器在训练过程中尝试最小化 KL 散度。通过设置不同的困惑度值来分析这一散度是选择正确困惑度值的好方法。稍后将详细介绍！

# PCA 的 Python 实现（可选）

本文还包括了使用 PCA 算法在低维空间中可视化 MNIST 数据。因此，虽然在这里是可选的，但讨论 PCA 算法的 Python 实现仍然是有意义的。我已经发布了详细的 PCA 文章，相关链接如下。

> **PCA 相关文章：** [使用主成分分析（PCA）进行降维的 3 个简单步骤](https://medium.com/data-science-365/3-easy-steps-to-perform-dimensionality-reduction-using-principal-component-analysis-pca-79121998b991)，[使用 Scikit-learn 进行主成分分析（PCA）](https://medium.com/data-science-365/principal-component-analysis-pca-with-scikit-learn-1e84a0c731b0)，[使用 NumPy 进行主成分分析（PCA）的深入指南](https://medium.com/data-science-365/an-in-depth-guide-to-pca-with-numpy-1fb128535b3e)。

# 表格格式的 MNIST 数据

加载 MNIST 数据集的方式有很多，这个数据集包含 70,000 张灰度手写数字图像，涵盖 10 个类别（0 到 9）。

+   **使用 Scikit-learn API：** 我们得到的形状为 (70000, 784)，这是 TSNE 和 PCA 算法所需的表格格式。数据集将被加载为 Pandas 数据框。数据集中有 70000 个观测（图像）。每个观测有 784 个特征（像素值）。图像的大小是 28 x 28。

+   **使用 Keras API：** 训练集的形状为 (60000, 28, 28)，测试集的形状为 (10000, 28, 28)。数据将被加载为三维的 NumPy 数组。这种格式不能直接用于 TSNE 和 PCA 算法。我们需要 [重新调整 MNIST 数据的形状](https://medium.com/data-science-365/acquire-understand-and-prepare-the-mnist-dataset-3d71a84e07e7#a69a)。

> **注意：** 要了解这两个 API 之间的差异，请点击 这里。

目前，我们将使用 Scikit-learn API 加载 MNIST 数据集。为了加快使用 TSNE 和 PCA 时的计算过程，我们只加载 MNIST 数据集的一部分（前 10,000 个实例）。

```py
from sklearn.datasets import fetch_openml

mnist = fetch_openml('mnist_784', version=1)
image_data = mnist['data'][0:10000]
labels = mnist['target'][0:10000]

print("Data shape:", image_data.shape)
print("Data type:", type(image_data))
print()
print("Label shape:", labels.shape)
print("Label type:", type(labels))
```

![](img/4cd875bf670b1e87a54f3312f45ca023.png)

（图片来源：作者）

我们还将像素值进行标准化，以便与 PCA 和 t-SNE 一起使用。

```py
# Normalize the pixel values
image_data = image_data.astype('float32') / 255
```

# 使用 PCA 可视化 MNIST 数据

如上面的输出所示，MNIST 数据的原始维度是 784，无法在 2D 图中绘制。因此，我们需要通过应用 PCA 将维度减少到 2。

让我们查看输出结果。

```py
from sklearn.decomposition import PCA

PCA_model = PCA(n_components=2)
PCA_transformed_data = PCA_model.fit_transform(image_data)

print("PCA transformed data shape:", PCA_transformed_data.shape)
print("PCA transformed data type:", type(PCA_transformed_data))
```

![](img/be836666523b5da3f7a99997e9ba29a5.png)

（图片来源：作者）

PCA 转换后的 MNIST 数据形状为 (10000, 2)，现在可以在 2D 图中绘制。

![](img/23073b5f97c0a7c83c4d1aaea6b48967.png)

（图片来源：作者）

显然，所有数据点都在一个簇中，我们无法看到每个类别标签的不同簇。这不是我们需要的表示方式。

让我们通过对 MNIST 数据应用 TSNE 来解决这个问题。

# 使用 t-SNE 可视化 MNIST 数据

现在，我们将在相同的数据集上应用 t-SNE。应用 t-SNE 时，我们将使用 **TSNE()** 类中的所有参数（超参数）的默认值。

```py
from sklearn.manifold import TSNE

TSNE_model = TSNE(n_components=2, perplexity=30.0)
TSNE_transformed_data = TSNE_model.fit_transform(image_data)

print("TSNE transformed data shape:", TSNE_transformed_data.shape)
print("TSNE transformed data type:", type(TSNE_transformed_data))
```

![](img/0a3afd960bfb45b76fd0a47c9a29f3c7.png)

（图片来源：作者）

TSNE 转换后的 MNIST 数据形状为 (10000, 2)，现在可以像以前一样在 2D 图中绘制。

![](img/2f7271b1ec59c6a0659d010559a211f1.png)

（图片来源：作者）

显然，数据点根据其类别标签被分隔成不同的簇。原始维度中同一类别的附近数据点在较低维度中仍将保持接近！

> t-SNE 在降维方面比 PCA 更有效，因为它在高维和低维中都能将相似的数据点保持在一起（而将不相似的数据点分开），并且在处理非线性数据时表现良好。

# PCA 之后的 t-SNE — 结合这两种方法（非常特别的技巧）

尽管 PCA 执行得非常快，但它无法在降维后保持数据点之间的空间关系。

另一方面，t-SNE 在处理较大数据集时确实很慢，但它可以在降维后保持数据点之间的空间关系。

为了加快 t-SNE 的计算过程，我们在 t-SNE 之前应用 PCA，并将两种方法结合，如下图所示。

![](img/61b75660ca3bd1ca2710b13b9bd15347.png)

**PCA 之前的 t-SNE 工作流程**（图片来源：作者）

首先，我们对 MNIST 数据应用 PCA，将维度降至 100（仅保留 100 个维度/成分/特征）。然后，我们对 PCA 转换后的 MNIST 数据应用 t-SNE。这次，t-SNE 仅看到 100 个特征而不是 784 个特征，因此不需要进行大量计算。现在，t-SNE 执行速度非常快，但仍能生成相同或更好的结果！

在 t-SNE 之前应用 PCA，你将获得以下好处。

+   PCA 去除数据中的噪声，只保留数据中最重要的特征。将 PCA 转换后的数据输入 t-SNE，你将获得更好的输出！

+   PCA 去除了输入特征之间的多重共线性。PCA 转换后的数据具有不相关的变量，这些变量被输入到 t-SNE 中。

+   正如我之前所说，PCA 显著减少了特征的数量。PCA 转换后的数据将输入 t-SNE，你将能非常快速地得到结果！

```py
PCA_model = PCA(n_components=100)
PCA_transformed_data = PCA_model.fit_transform(image_data)

TSNE_model = TSNE(n_components=2, perplexity=30.0)
PCA_TSNE_transformed_data = TSNE_model.fit_transform(PCA_transformed_data)

plt.figure(figsize=[7, 4.9])

plt.scatter(PCA_TSNE_transformed_data[:, 0], PCA_TSNE_transformed_data[:, 1], 
            c=np.array(labels).astype('int32'), s=5, cmap='tab10')

plt.title('Lower dimensional representation of MNIST data - TSNE after PCA')
plt.xlabel('1st dimension')
plt.ylabel('2nd dimension')
plt.savefig("PCA_TSNE.png")
```

![](img/c928cbb4f5e73ed9f6f8b131c18d300b.png)

(图片来源：作者)

使用所有 784 个特征运行 t-SNE 花费了 100 秒。在应用 PCA 后，只需 20 秒就可以用具有 100 个特征的 PCA 转换数据运行 t-SNE。

PCA 转换后的数据准确地表示了原始 MNIST 数据，因为前 100 个组件捕获了原始数据中约 90% 的方差。我们可以通过查看以下图表来确认这一点。因此，用 PCA 转换后的数据替代原始数据输入 t-SNE 是合理的。

```py
pca_all = PCA(n_components=784)
pca_all.fit(image_data)

plt.figure(figsize=[5, 3.5])
plt.grid()
plt.plot(np.cumsum(pca_all.explained_variance_ratio_ * 100))
plt.xlabel('Number of components')
plt.ylabel('Explained variance')
```

![](img/cf459c8709b07f79200f312ca684e15f.png)

(图片来源：作者)

# 选择合适的困惑度值

困惑度决定了在可视化数据时要考虑的最近邻居数量。它是 t-SNE 中最重要的超参数。因此，你需要正确地调整它。

一种选择是你可以绘制不同困惑度值的 KL 散度，并分析当困惑度值增加时 KL 散度的变化情况。

```py
perplexity_vals = np.arange(10, 220, 10)
KL_divergences = []

for i in perplexity_vals:
  TSNE_model = TSNE(n_components=2, perplexity=i, n_iter=500).fit(PCA_transformed_data)
  KL_divergences.append(TSNE_model.kl_divergence_)

plt.style.use("ggplot") 
plt.figure(figsize=[5, 3.5])
plt.plot(perplexity_vals, KL_divergences, marker='o', color='blue')
plt.xlabel("Perplexity values")
plt.ylabel("KL divergence")
```

![](img/3827d9ccb25d6fc552f4f980ba8a0332.png)

(图片来源：作者)

当困惑度值增加时，KL 散度持续减少！因此，我们不能仅通过分析这个图来决定困惑度的正确值。

作为第二种选择，我们需要多次运行 t-SNE 算法，使用不同的困惑度值并可视化结果。

![](img/56c410280a45f1740bf8f74efe35eb5e.png)

(图片来源：作者)

在困惑度 = 5（较低值）时，会形成清晰的簇。但簇之间的空间不足，簇内的点分离得很好。

在困惑度 = 30、50 和 100（合适值）时，簇之间的间距增加，而簇内的点集中得很好。

在困惑度 = 210 和 500（过高值）时，簇之间往往会重叠，这对我们来说是不需要的。

对于 MNIST 数据集，任何介于 30 和 50 之间的困惑度值都足够好。

> **注意：** 困惑度的最佳值高度依赖于数据的性质和数量。通常，较大的数据集需要较大的困惑度值。最好从默认值 30 开始。

# 更改合适的迭代次数

现在，我们将可视化梯度下降优化算法中使用的迭代次数的效果。t-SNE 使用随机梯度下降来最小化 KL 散度成本函数。

迭代次数决定了优化器在优化过程中进行梯度更新的次数。默认值为 1000。最小值应为 250。

迭代次数在***n_iter***参数中指定。

```py
from sklearn.manifold import TSNE

TSNE_model = TSNE(n_components=2, perplexity=30.0, n_iter=250)
```

在这个实验中，我们将尝试三种不同的值：250、500 和 1000。

**250 次迭代**

![](img/63463b84b0308fba48446629185ea641.png)

（图片来源于作者）

**500 次迭代**

![](img/36d0989f401e7372a30c5ad5be36bcc9.png)

（图片来源于作者）

**1000 次迭代**

![](img/23f0298b955d036ac5674eabbd56e505.png)

（图片来源于作者）

运行 250 次迭代后，簇尚未明显形成，因为成本函数未被完全最小化。因此，我们需要给算法更多时间以进一步优化。运行 500 次迭代后，簇的分离非常明显。一旦优化，增加迭代次数对改变簇形成几乎没有影响！它只会增加训练时间！在这种情况下，500 次迭代足以优化算法。

> **注意：** 迭代次数的选择高度依赖于数据的性质和量。

# 初始化的随机性

TSNE 有两种初始化方式：

+   **随机初始化**

```py
from sklearn.manifold import TSNE

TSNE_model = TSNE(n_components=2, perplexity=30.0, init='random')
```

+   **PCA 初始化（默认）**

```py
from sklearn.manifold import TSNE

TSNE_model = TSNE(n_components=2, perplexity=30.0, init='pca')
# or
TSNE_model = TSNE(n_components=2, perplexity=30.0) # default
```

PCA 初始化比随机初始化更稳定，并且在不同执行中产生相同的结果。

相比之下，随机初始化会在不同执行中生成不同的结果，因为**TSNE 具有非凸的成本函数**，并且 GD 优化器可能会陷入局部最小值，如下图所示。

![](img/e899f18df4def4d415788ddca9c5106e.png)

（图片来源于作者）

如果初始化发生在点**A**附近，优化器可能会停留在局部最小值，成本函数将无法完全最小化。

如果初始化发生在点**B**附近，优化器可能会停留在全局最小值，这时，TSNE 将生成与之前完全不同的输出。

成本函数中可能存在许多局部最小值。因此，不同的初始化可能会导致算法陷入局部最小值。随机初始化就是这种情况，而 PCA 初始化不会遭受这种问题！

# 在随机初始化中使用随机状态

如果你仍然想在不同的 TSNE 算法执行中获得稳定的结果，可以将***random_state***参数指定为整数，如 0、1 或 42。

```py
from sklearn.manifold import TSNE

TSNE_model = TSNE(n_components=2, perplexity=30.0, init='random',
                  random_state=42)
```

如果你运行多次，你将会得到相同的结果。然而，如果你将***random_state***指定为 0，你会得到不同于之前的结果。

```py
from sklearn.manifold import TSNE

TSNE_model = TSNE(n_components=2, perplexity=30.0, init='random',
                  random_state=0)
```

让我们看看两个输出：

**随机状态 = 42**

![](img/99b9b3e66709180e5d1afa2cc547ea80.png)

（图片来源于作者）

**随机状态 = 0**

![](img/e775ac7ed760f42084be02a0195f5501.png)

（图片来源于作者）

你将会得到完全不同的输出，取决于不同的随机状态值！

> **注意：** 指定随机状态**不会**阻止算法陷入局部最小值。

今天的文章到此结束。

**如果你有任何问题或反馈，请告诉我。**

## 你可能感兴趣的其他降维方法

+   [**主成分分析（PCA）**](https://medium.com/data-science-365/3-easy-steps-to-perform-dimensionality-reduction-using-principal-component-analysis-pca-79121998b991)

+   **因子分析（FA）**

+   **线性判别分析（LDA）**

+   **非负矩阵分解（NMF）**

+   **自编码器（AEs）**

+   **核主成分分析**

## 继续阅读（推荐）

+   [**PCA 和维度降维特别合集**](https://rukshanpramoditha.medium.com/list/pca-and-dimensionality-reduction-special-collection-146045a5acb5)

## 怎么样，来一门 AI 课程？

+   [**神经网络与深度学习课程**](https://rukshanpramoditha.medium.com/list/neural-networks-and-deep-learning-course-a2779b9c3f75)

## 支持我作为一名写作者

*希望你喜欢阅读这篇文章。如果你愿意支持我作为一名写作者，请考虑* [***注册会员***](https://rukshanpramoditha.medium.com/membership) *以获得无限访问 Medium 的权限。每月只需 $5，我将获得部分会员费用。*

[](https://rukshanpramoditha.medium.com/membership?source=post_page-----7a3975e8cbdb--------------------------------) [## 通过我的推荐链接加入 Medium - Rukshan Pramoditha

### 阅读 Rukshan Pramoditha 的每一篇故事（以及 Medium 上的其他成千上万的作者）。你的会员费直接…

[rukshanpramoditha.medium.com](https://rukshanpramoditha.medium.com/membership?source=post_page-----7a3975e8cbdb--------------------------------)

## 加入我的私人邮件列表

*不要再错过我的精彩故事。通过* [***订阅我的邮件列表***](https://rukshanpramoditha.medium.com/subscribe)*，你将直接在我发布故事时第一时间收到。*

非常感谢你的持续支持！下篇文章见。祝大家学习愉快！

## MNIST 数据集信息

+   **引用：** Deng, L., 2012\. 用于机器学习研究的手写数字图像 MNIST 数据库。**IEEE 信号处理杂志**，29(6)，第 141–142 页。

+   **来源：** [`yann.lecun.com/exdb/mnist/`](http://yann.lecun.com/exdb/mnist/)

+   **许可：** *Yann LeCun*（纽约大学 Courant Institute）和 *Corinna Cortes*（谷歌实验室，纽约）持有 MNIST 数据集的版权，该数据集采用*Creative Commons Attribution-ShareAlike 4.0 International License*（[**CC BY-SA**](https://creativecommons.org/licenses/by-sa/4.0/)）。你可以在[这里](https://rukshanpramoditha.medium.com/dataset-and-software-license-types-you-need-to-consider-d20965ca43dc#6ade)了解更多关于不同数据集许可类型的信息。

**设计及编写者：** [Rukshan Pramoditha](https://medium.com/u/f90a3bb1d400?source=post_page-----7a3975e8cbdb--------------------------------)

**2023–05–23**
