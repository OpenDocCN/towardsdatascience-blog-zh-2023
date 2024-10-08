# 使用量子计算机寻找暗物质

> 原文：[`towardsdatascience.com/finding-dark-matter-using-a-quantum-computer-a99f4bff4685`](https://towardsdatascience.com/finding-dark-matter-using-a-quantum-computer-a99f4bff4685)

## QML — 量子机器学习在高能和粒子物理的有趣应用

[](https://prashantmdgl9.medium.com/?source=post_page-----a99f4bff4685--------------------------------)![Prashant Mudgal](https://prashantmdgl9.medium.com/?source=post_page-----a99f4bff4685--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a99f4bff4685--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a99f4bff4685--------------------------------) [Prashant Mudgal](https://prashantmdgl9.medium.com/?source=post_page-----a99f4bff4685--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a99f4bff4685--------------------------------) ·阅读时间 9 分钟·2023 年 11 月 3 日

--

# 背景

今年的八月，致力于 IBM 全球量子暑期学校，在那里我不仅在压缩的时间表和紧凑的日程中学到了基础知识，还学到了一些量子计算的应用。在经过 4 周艰苦的学习后获得的 [徽章](https://www.credly.com/go/ZlukKqHe) 本身就是一段 "*量子体验*"，因为你以为你了解自己在做什么，但同时你对发生了什么一无所知。这个月的课程从量子电路基础转到变分算法的速度很快，几乎没有时间来“做自己的研究”和亲自参与应用部分。

![](img/5ec7d8ce72d318eaaaf6c7221fe5e741.png)

照片由 [Dynamic Wang](https://unsplash.com/@dynamicwang?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

就应用而言，量子化学、量子模拟和一些非常复杂的建模任务都符合可以用量子计算机解决的问题的标准。话虽如此，还有另一个正在蓬勃发展的领域，受到用户和研究人员的极大关注，那就是量子机器学习——简称 QML。

我认为 QML 应该是传统机器学习的合理继承者，于是我开始了这项工作。现在，我希望能找到一个不会因为数据量庞大、复杂模式难以识别而让机器学习算法无法直接解决的问题，但又是我可以在我那台简陋的机器上编程解决的问题。我没有再去寻找其他领域，我们的老朋友物理学正好适合，它在其怀抱中隐藏着一系列复杂但有趣的问题，而且从事这些问题听起来在智力上也很酷。

就这样。

# 问题陈述

我决定处理在与大型强子对撞机（CERN）相关的 OPERA 实验（振荡项目与乳胶追踪装置）下研究的**暗物质分类**问题。

## 问题陈述

简而言之，我们将训练一个分类器来区分信号和噪声。信号是暗物质的存在，而噪声意味着没有信号或完全是其他东西。

很简单！

## 直觉

让我们稍微详细讲解一下实验的背景，以便形成一些直觉。

暗物质是一种神秘且尚未被探测到的物质形式，它不会与电磁辐射（如光）发生相互作用。人们认为它占据了宇宙总质量的近 80%。之所以称之为“暗物质”，是因为它无法通过望远镜或其他探测电磁辐射的仪器直接观察到。

## 为什么寻找暗物质是一个挑战？

这很具挑战性，因为我们不知道自己在寻找什么。

1.  **隐形**：暗物质不会与光相互作用，这就是为什么我们真正不知道自己在寻找什么，虽然有许多理论，但仍没有共识。

1.  **背景噪声**：设计用于探测暗物质的实验必须应对各种背景噪声，这些噪声可能会模拟暗物质的预期信号。区分实际的暗物质相互作用和这些背景信号是一个重大挑战。暗物质与常规物质的相互作用非常微弱，使得在实验中探测和区分背景噪声变得困难。

1.  **多种可能性**：暗物质的理论候选者有很多，这需要不同的探测方法。科学家们正在探索这些可能性，这增加了寻找暗物质的复杂性。

## OPERA 实验中到底发生了什么？

OPERA 位于意大利的格兰萨索国家实验室。这是一个中微子物理实验，主要关注中微子振荡的研究。它并不是专门用于寻找暗物质的。

![](img/f1237659642f7c75d0eb7dffc112212a.png)

作者提供的图像

当一个假定的暗物质粒子（我们正在寻找的）与铅原子核碰撞时，原子核会以电子束的形式发射电子，这些电子束在屏幕上被探测到。这就是我们要寻找的信号。然而，当一个中微子与铅原子核碰撞时，它也会产生电子，并以相同的方式产生电磁冲击，这会将信号与噪声混淆。我们正试图区分这个信号和噪声。

## 我们的计划

基本上，我们需要筛选数据并区分信号和噪声，这可以通过传统的机器学习方法完成，但仍然是一个艰巨的任务。在一个拥有 1000 万次碰撞的数据集中，只有大约 1 万次会对应信号。这种数据集中的不平衡和稀疏使问题变得偏斜且困难。因为我们喜欢挑战，我们会加一个小难度，使用量子机器学习算法而非传统算法（对以连词开头的句子表示歉意）。

## 数据

LHC 网站上有大量数据集[在这里](https://opendata.cern.ch/search?page=1&size=20&type=Dataset)可供使用；本实验使用的数据集可以在[这里](https://www.kaggle.com/datasets/prashantmudgal/dark-matter-classification-opera-experiement-lhc)找到。

**许可证**: 数据集根据[**CC0**](https://archive.is/o/ZXMIM/https://creativecommons.org/share-your-work/public-domain/cc0) **(CC Zero)** 发布，这是*创意共享公共领域奉献许可证* ([`opendata.cern.ch/record/16541`](https://opendata.cern.ch/record/16541))

代码可以在我的[GitHub 仓库](https://github.com/Prashantmdgl9/Miscellaneous_Scripts/tree/main/Dark%20Matter)中找到。

数据包括两个 h5 文件，open30.h5 和 test.h5；h5 是层次数据格式，用于以压缩方式存储和组织大量数据。

数据包含以下变量：

1.  X — 基础轨道的 X 坐标

1.  Y — 基础轨道的 Y 坐标

1.  Z — 基础轨道的 Z 坐标

1.  TX — 从原点投影到 X 轴的角度

1.  TY — 从原点投影到 Y 轴的角度

1.  信号 — 一个二元变量，1 表示信号，0 表示噪声

## 库

IBM 的量子库 — [qiskit 0.44.3](https://github.com/Qiskit/qiskit)

```py
import qiskit.tools.jupyter

%qiskit_version_table
%qiskit_copyright
```

![](img/2808245a4551602b2903b872dccce4c9.png)

图片由作者提供

## 关于变分量子算法的说明

量子算法设计用于在量子计算机上运行，但目前我们处于 NISQ — 噪声中等规模量子计算机时代，这使得结果的重现变得困难。当前的量子计算机非常容易受到噪声的影响，甚至微小的热力学条件变化或其他电路问题都会对结果产生很大影响。我们想应用的逻辑门因为噪声会变成其他东西。这是不可取的。

聪明的研究人员开发了所谓的变分算法，它们利用经典计算机和量子计算机来提高速度和准确性。

实质上，所有算法都使用某种形式的优化和参数调整，变分算法所做的是利用量子计算机来近似成本函数，然后在经典计算机上计算成本函数的新参数值，再用新值在量子计算机上运行。这样，计算就分布在经典计算机和量子计算机之间，加速了过程。

我们将在这里使用变分量子分类器，因为任务是分类信号和噪声。有关 IBM VQC 的更多信息，请访问：[`learn.qiskit.org/course/machine-learning/variational-classification`](https://learn.qiskit.org/course/machine-learning/variational-classification)

# 建模

我们先来看看数据和变量

![](img/6f564ace336bac9aa23bbd2d69977d8e.png)

图片来源于作者

让我们查看配对图，以了解变量之间是否存在相关性。

![](img/29639672d792273d93a82f184e36e9fa.png)

图片来源于作者

好吧，存在一个模式，但相当复杂！

在完成常规的样板工作，包括采样、缩放和训练测试拆分后，我们已经准备好进行量子模型训练。

在继续之前，让我们运行支持向量分类算法，这样我们就有了传统机器学习的基准。

```py
from sklearn.svm import SVC

svc = SVC()
model_classical = svc.fit(train_features, train_labels) 
```

![](img/1115cd381771ac8191b85399c4d4623c.png)

图片来源于作者

在测试数据上 70%的准确率并不算太好，但我这里没有进行太多的特征工程。一旦我进行特征工程，它会有所改善。

现在，轮到量子计算机了。

问题被以门和电路的形式进行表述，这些门和电路将量子比特（量子位）输入到量子计算机中。

我们没有丢弃任何特征——TX、TY、X、Y、Z；我们会使用所有这些特征，因此我们电路中使用的量子比特数量将是 5。

```py
 num_features = features.shape[1] #5

feature_map = ZZFeatureMap(feature_dimension=num_features, reps=1)
feature_map.decompose().draw(output="mpl", fold=20)
```

![](img/6e1512dc553c61a910ad7dafa855b841.png)

图片来源于作者

电路的样子就是这样。它输入 5 个量子比特，应用了 Hadamard 和 P 门。Hadamard 门将量子比特的基态从|0>变为|+>，从|1>变为| — >，而 P 门则使得单量子比特绕 Z 轴旋转。

形成电路后的下一步是 Ansatz，这是量子世界中相当常见的术语；它在德语中意为“方法”，但在物理学和数学中指的是一种经过教育的猜测。

所以，我们需要做的就是对参数进行经过教育的猜测——这将创建一个量子态，并在量子计算机上执行。执行值将与期望值进行比较，根据偏差程度，优化器将调整参数或 ansatz，直到我们达到一个足够好或满意的值。

```py
from qiskit.circuit.library import RealAmplitudes

ansatz = RealAmplitudes(num_qubits=num_features, reps=3)
ansatz.decompose().draw(output="mpl", fold=20)
```

![](img/1e586bbbd183d1e1058f370f9eb1b3e5.png)

图片来源于作者

你会看到应用了很多 R 门。本质上，所有量子比特只是绕其轴旋转以获得一些任意的起始值，这就是 ansatz。

现在，让我们在训练数据上拟合 VQC。

```py
optimizer = COBYLA(maxiter=100)

vqc = VQC(
    sampler=sampler,
    feature_map=feature_map,
    ansatz=ansatz,
    optimizer=optimizer,
    callback=callback_graph,
)

vqc.fit(train_features, train_labels)
```

回调图非常酷。就像实时获取你行为的结果一样。

![](img/d123534bf544b702a40bde0248c474fc.png)

图片由作者提供

好的，下行趋势是有希望的，这意味着它正在学习，但只在测试数据上进行拟合才能告诉我们是否存在过拟合。

```py
train_score_q4 = vqc.score(train_features, train_labels)
test_score_q4 = vqc.score(test_features, test_labels)

print("Quantum VQC on the training dataset:",train_score_q4)
print("Quantum VQC on the test dataset:", test_score_q4)
```

![](img/03372b5567e2a4e78afbba776024ebef.png)

图片由作者提供

相较于传统的机器学习，略微更好。我在本地机器上使用模拟器运行了它。也许在真实的量子计算机上运行时效果会更好（没有什么阻止我们，只是需要排队，并且由于服务器负载的增加，简单的任务如加法需要几个小时才能完成，这并不是因为计算慢，而是因为越来越多的人排队使用计算时间）或者花更多时间进行特征工程。

# 结束语

话虽如此，我很确定目前传统的机器学习算法会优于量子算法，特别是在分类任务中，因为大量研究和资源已经投入到使其稳健和复杂化的工作中。一旦量子机器学习经历这样的升级，它将成为一场公平的竞争。

> 这并不意味着量子机器学习会取代机器学习，这远非事实，相反，量子计算是针对那些经典计算机无法在多项式时间内解决或甚至无法近似的问题。机器学习会有它的地位，量子机器学习也会在更大的框架中占有一席之地。

本文并不旨在展示机器学习或量子机器学习的威力，而只是展示两者之间的相似性——它们在执行上有所不同但在本质上类似，两者都依赖于特征工程和超参数的选择。

在结束之前，看看信号中是否存在某种聚类将会很有趣。我们已经将信号与噪声分开，我们能否对其进行聚类，看看是否形成了暗物质粒子的电磁喷发？

```py
kmeans = KMeans(n_clusters=5).fit(train)
clustering_labels = kmeans.labels_
X_train = train.sample(frac=0.05)

clusters['cluster'] = clustering_labels

fig = plt.figure(figsize = (20,20))
ax.scatter(X_train.X, X_train.Y, X_train.Z, c=X_train.cluster)
plt.show()
```

![](img/192a2b53a3a0646e1f2f7320f57c1b6d.png)

图片由作者提供

> 不错！它们虽然不具备独特性，但也不糟糕。可以看到一些模式。簇的垂直特性是由于质量和碰撞后的轨迹角度。 

有趣！

> 从嘈杂的数据中，我们提取了可能成为暗物质粒子相互作用候选者的最佳轨迹。

我希望这篇小文章能鼓励你迈出量子跃迁。欢迎通过 Twitter 或邮件联系我；像往常一样，我愿意接受批评和建议，这些能帮助我成长和学习。

*PS：再一次，代码在我的 GitHub 仓库中* [*这里*](https://github.com/Prashantmdgl9/Miscellaneous_Scripts/tree/main/Dark%20Matter)*。*
