# 相似性搜索，第1部分：kNN与倒排文件索引

> 原文：[https://towardsdatascience.com/similarity-search-knn-inverted-file-index-7cab80cc0e79?source=collection_archive---------1-----------------------#2023-04-28](https://towardsdatascience.com/similarity-search-knn-inverted-file-index-7cab80cc0e79?source=collection_archive---------1-----------------------#2023-04-28)

## 介绍kNN的相似性搜索及其通过倒排文件的加速。

[](https://medium.com/@slavahead?source=post_page-----7cab80cc0e79--------------------------------)[![Vyacheslav Efimov](../Images/db4b02e75d257063e8e9d3f1f75d9d6d.png)](https://medium.com/@slavahead?source=post_page-----7cab80cc0e79--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7cab80cc0e79--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7cab80cc0e79--------------------------------) [Vyacheslav Efimov](https://medium.com/@slavahead?source=post_page-----7cab80cc0e79--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc8a0ca9d85d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimilarity-search-knn-inverted-file-index-7cab80cc0e79&user=Vyacheslav+Efimov&userId=c8a0ca9d85d8&source=post_page-c8a0ca9d85d8----7cab80cc0e79---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7cab80cc0e79--------------------------------) ·9 分钟阅读·2023年4月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7cab80cc0e79&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimilarity-search-knn-inverted-file-index-7cab80cc0e79&user=Vyacheslav+Efimov&userId=c8a0ca9d85d8&source=-----7cab80cc0e79---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7cab80cc0e79&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimilarity-search-knn-inverted-file-index-7cab80cc0e79&source=-----7cab80cc0e79---------------------bookmark_footer-----------)![](../Images/451ba5a7aab8a570f7a67d387bf5fc3d.png)

**相似性搜索**是一个问题，给定一个查询，目标是找到数据库中与之最相似的文档。

# 介绍

在数据科学中，**相似性搜索**常出现在自然语言处理领域、搜索引擎或推荐系统中，需要为查询检索最相关的文档或项目。通常，文档或项目以文本或图像的形式表示。然而，机器学习算法不能直接处理原始文本或图像，这就是为什么文档和项目通常被预处理并存储为**向量**的原因。

有时，向量的每个组件可以存储语义信息。在这种情况下，这些表示也称为**嵌入**。这些嵌入可以有数百维，并且其数量可以达到数百万！由于这些巨大的数字，任何信息检索系统必须能够迅速检测到相关文档。

> 在机器学习中，向量也被称为**对象**或**点**。

# 索引

为了加速搜索性能，数据集嵌入之上建立了一个特殊的数据结构。这种数据结构称为**索引**。在这一领域已有大量研究，并且发展出了许多类型的索引。在选择适用于特定任务的索引之前，有必要了解其内部操作原理，因为每种索引有不同的用途，并且各自有优缺点。

在本文中，我们将看看最简单的方法——**kNN**。基于kNN，我们将转到**倒排文件**——一种用于更可扩展搜索的索引，可以加速搜索过程数倍。

# kNN

**kNN**是相似性搜索中最简单和最原始的算法。考虑一个向量数据集和一个新的查询向量 *Q*。我们希望找到与 *Q* 最相似的前 *k* 个数据集向量。首先要考虑的是如何测量两个向量之间的相似性（距离）。实际上，有几种相似性度量可以实现这一点。下面的图中展示了其中一些。

![](../Images/4205e53775ecd0632f4afe4c159b7868.png)

相似性度量

## 训练

kNN是机器学习中为数不多的无需训练阶段的算法之一。选择合适的度量后，我们可以直接进行预测。

## 推理

对于一个新的对象，该算法会穷尽地计算与所有其他对象的距离。之后，它会找到距离最小的 *k* 个对象，并将其作为响应返回。

![](../Images/17b2fd6c8e14a87f9829ebcfaca17bd2.png)

kNN工作流程

显然，通过检查与所有数据集向量的距离，kNN可以保证100%的准确结果。然而，这种蛮力方法在时间性能上非常低效。如果一个数据集由 *n* 个具有 *m* 维度的向量组成，那么对于每个 *n* 向量，需要 *O(m)* 时间来计算与查询 *Q* 的距离，总时间复杂度为 *O(mn)*。正如我们稍后将看到的，存在更高效的方法。

此外，原始向量没有压缩机制。想象一下一个包含数十亿个对象的数据集。将所有这些对象存储在RAM中几乎是不可能的！

![](../Images/29a705c831ea8e2a323f7552749caa1b.png)

kNN性能。具有100%的准确率和没有训练阶段会导致在推理过程中进行穷举搜索以及向量的无内存压缩。注意：这种图示显示了不同算法的相对比较。根据情况和选择的超参数，性能可能会有所不同。

## 应用

kNN的应用范围有限，应该仅在以下情况之一中使用：

+   数据集的大小或嵌入维度相对较小。这一方面确保了算法仍然能够快速执行。

+   算法的要求准确度必须达到100%。在准确度方面，没有其他最近邻算法能够超越kNN。

基于指纹检测一个人的例子是需要100%准确度的问题。如果一个人犯了罪并留下了指纹，检索到的结果必须完全正确。否则，如果系统不是100%可靠，则可能会错误地将另一人定罪，这是一种非常严重的错误。

基本上，改进kNN有两种主要方法（稍后我们将讨论）：

+   缩小搜索范围。

+   降低向量的维度。

使用这两种方法之一时，我们将不会再次进行穷举搜索。这些算法被称为**近似最近邻（ANN）**，因为它们不保证100%的准确结果。

# 倒排文件索引

> **“倒排索引**（也称为**文档列表**、**文档文件**或**倒排文件**）是一个数据库索引，存储内容的映射，例如单词或数字，及其在表格、文档或文档集合中的位置” — 维基百科

在执行查询时，计算查询的哈希函数，并从哈希表中获取映射值。这些映射值中的每一个都包含自己的一组潜在候选者，然后在条件下完全检查是否为查询的最近邻。通过这样做，所有数据库向量的搜索范围被缩小。

![](../Images/51ba64fc16fa1491e565d610b242d177.png)

倒排文件索引工作流程

这种索引有不同的实现方式，具体取决于哈希函数的计算方式。我们将要研究的实现是使用**Voronoi图**（或**Dirichlet镶嵌**）的方法。

## 训练

该算法的思想是创建几个不相交的区域，每个数据集点将属于其中一个区域。每个区域都有自己的质心，指向该区域的中心。

> 有时**Voronoi区域**也被称为**单元**或**分区**。

![](../Images/48380f868bba90119ee7108e5ff65b08.png)

Voronoi图示例。白点是各自分区的中心，分区内包含一组候选者。

> Voronoi图的主要特性是，质心到其区域内任意点的距离小于该点到另一质心的距离。

## 推理

当给定一个新对象时，计算所有Voronoi分区质心的距离。然后选择距离最小的质心，并将该分区中的向量作为候选项。

![](../Images/fce8e2a887cf3367fc3eb36fa88ca718.png)

通过给定查询，我们搜索最近的质心（位于绿色区域）

最终，通过计算与候选项的距离并选择前*k*个最近的候选项，返回最终答案。

![](../Images/75d38fc65edd40656193b2f1a8040ec4.png)

在选定区域内查找最近邻居

如你所见，这种方法比之前的要快得多，因为我们不需要遍历所有数据集向量。

## 边缘问题

随着搜索速度的提高，倒排文件也有一个缺点：它不能保证找到的对象始终是最近的。

在下图中，我们可以看到这样的场景：实际的最近邻居位于红色区域，但我们仅从绿色区域选择候选项。这种情况被称为**边缘问题**。

![](../Images/a1f1b2a0a2066e904df3b7b357469704.png)

边缘问题

这种情况通常发生在查询对象靠近另一区域边界时。为了减少这种情况下的错误数量，我们可以扩大搜索范围，并基于与对象最接近的前*m*个质心选择多个区域来搜索候选项。

![](../Images/1a4ccefb2576c2eb6f79749b89d4df8e.png)

在几个区域内查找最近邻居（m = 3）

> 探索的区域越多，结果越准确，但计算所需的时间也越长。

## 应用

尽管存在边缘问题，倒排文件在实践中表现出色。它在需要在准确度和速度提升之间进行权衡时非常适合使用。

一个使用案例示例是基于内容的推荐系统。假设它根据用户过去观看的其他电影向用户推荐一部电影。数据库包含一百万部电影供选择。

+   使用kNN时，系统确实会选择对用户最相关的电影并进行推荐。然而，执行查询所需的时间非常长。

+   假设使用倒排文件索引，系统推荐第5个最相关的电影，这在现实生活中可能是这种情况。搜索时间比kNN快20倍。

从用户体验的角度来看，很难区分这两种推荐结果的质量：第1个和第5个最相关的结果都是来自百万个可能电影中的良好推荐。用户可能对这些推荐中的任何一个都感到满意。从时间的角度来看，倒排文件显然是赢家。这就是为什么在这种情况下，最好使用后者的方法。

![](../Images/2511a8d8682cf99383f3d628ccc4f4ca.png)

反向文件索引性能。在这里，我们稍微降低了准确度以在推理过程中获得更高的速度。

# Faiss实现

> [**Faiss**](https://github.com/facebookresearch/faiss)（Facebook AI 搜索相似性）是一个用C++编写的Python库，用于优化的相似性搜索。该库展示了不同类型的索引，这些索引是用来高效存储数据和执行查询的数据结构。

根据[Faiss文档](https://faiss.ai)中的信息，我们将了解索引的创建和参数化过程。

## kNN

实现kNN方法的索引在Faiss中被称为**flat**，因为它们不压缩任何信息。它们是唯一保证正确搜索结果的索引。实际上，Faiss中存在两种类型的flat索引：

+   *IndexFlatL2*。相似度计算为欧几里得距离。

+   *IndexFlatIP*。相似度计算为内积。

这两种索引在构造函数中都需要一个单一的参数**d**：数据维度。这些索引没有任何可调参数。

![](../Images/a7c95b54cdc224f68c942727d7552b88.png)

Faiss对IndexFlatL2和IndexFlatIP的实现

存储一个向量的单一分量需要4字节。因此，存储一个维度为*d*的向量需要*4 * d*字节。

![](../Images/cf6a38cc71459bd1317b705373914496.png)

## 反向文件索引

对于描述的反向文件，Faiss实现了*IndexIVFFlat*类。与kNN的情况一样，" *Flat* "一词表示没有对原始向量进行解压，它们被完全存储。

为了创建这个索引，我们首先需要传递一个量化器——一个决定数据库向量如何存储和比较的对象。

*IndexIVFFlat*有两个重要参数：

+   **nlist**：定义在训练过程中创建的区域（Voronoi单元）的数量。

+   **nprobe**：决定搜索候选区域的数量。更改nprobe参数不需要重新训练。

![](../Images/e689102761b3812784f4ca933ed7c552.png)

Faiss对IndexIVFFlat的实现

与之前的情况一样，我们需要*4 * d*字节来存储一个向量。但现在我们还需要存储有关Voronoi区域的信息，这些区域是数据集向量所属的。在Faiss实现中，这些信息每个向量占用8字节。因此，存储单个向量所需的内存为：

![](../Images/e6584546fe377cd8f1a1faef2a7b5cca.png)

# 结论

我们已经探讨了相似性搜索中的两种基础算法。实际上，朴素的 kNN 几乎不应该用于机器学习应用，因为它在扩展性方面表现不佳，除非在特定情况下。另一方面，倒排文件提供了加速搜索的良好启发式方法，其质量可以通过调整超参数来提高。从不同的角度仍然可以提升搜索性能。在本系列文章的下一部分，我们将深入探讨一种旨在压缩数据集向量的方法。

[](/similarity-search-product-quantization-b2a1a6397701?source=post_page-----7cab80cc0e79--------------------------------) [## 相似性搜索，第2部分：产品量化

### 学习一种有效压缩大数据的强大技术

towardsdatascience.com](/similarity-search-product-quantization-b2a1a6397701?source=post_page-----7cab80cc0e79--------------------------------)

# 资源

+   [倒排索引 | 维基百科](https://en.wikipedia.org/wiki/Inverted_index)

+   [Faiss 文档](https://faiss.ai)

+   [Faiss 仓库](https://github.com/facebookresearch/faiss)

+   [Faiss 索引概述](https://github.com/facebookresearch/faiss/wiki/Faiss-indexes)

+   [选择索引的指南](https://github.com/facebookresearch/faiss/wiki/Guidelines-to-choose-an-index)

*除非另有说明，否则所有图片均由作者提供。*
