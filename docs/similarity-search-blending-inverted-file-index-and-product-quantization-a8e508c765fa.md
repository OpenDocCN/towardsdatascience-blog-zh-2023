# 相似性搜索，第3部分：结合倒排文件索引和产品量化

> 原文：[https://towardsdatascience.com/similarity-search-blending-inverted-file-index-and-product-quantization-a8e508c765fa?source=collection_archive---------1-----------------------#2023-05-19](https://towardsdatascience.com/similarity-search-blending-inverted-file-index-and-product-quantization-a8e508c765fa?source=collection_archive---------1-----------------------#2023-05-19)

## 了解如何结合两种基本的相似性搜索索引，以获得两者的优势

[](https://medium.com/@slavahead?source=post_page-----a8e508c765fa--------------------------------)[![Vyacheslav Efimov](../Images/db4b02e75d257063e8e9d3f1f75d9d6d.png)](https://medium.com/@slavahead?source=post_page-----a8e508c765fa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a8e508c765fa--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a8e508c765fa--------------------------------) [Vyacheslav Efimov](https://medium.com/@slavahead?source=post_page-----a8e508c765fa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc8a0ca9d85d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimilarity-search-blending-inverted-file-index-and-product-quantization-a8e508c765fa&user=Vyacheslav+Efimov&userId=c8a0ca9d85d8&source=post_page-c8a0ca9d85d8----a8e508c765fa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a8e508c765fa--------------------------------) ·8分钟阅读·2023年5月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa8e508c765fa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimilarity-search-blending-inverted-file-index-and-product-quantization-a8e508c765fa&user=Vyacheslav+Efimov&userId=c8a0ca9d85d8&source=-----a8e508c765fa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa8e508c765fa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimilarity-search-blending-inverted-file-index-and-product-quantization-a8e508c765fa&source=-----a8e508c765fa---------------------bookmark_footer-----------)![](../Images/8768896b9afee7af342394c9d1259b58.png)

**相似性搜索** 是一个问题，其中给定一个查询的目标是找到数据库中与之最相似的文档。

# 引言

在数据科学中，相似性搜索常见于自然语言处理（NLP）领域、搜索引擎或推荐系统中，这些系统需要为查询检索出最相关的文档或项目。在海量数据中提升搜索性能的方法有很多种。

在本系列的前两部分中，我们讨论了信息检索中的两种基本算法：**倒排文件索引**和**产品量化**。这两者都优化了搜索性能，但关注的方面不同：前者加快了搜索速度，而后者则将向量压缩为更小、更节省内存的表示。

[## 相似性搜索，第一部分：kNN与倒排文件索引](https://towardsdatascience.com/similarity-search-knn-inverted-file-index-7cab80cc0e79?source=post_page-----a8e508c765fa--------------------------------)

### 相似性搜索是一个热门问题，其中给定一个查询Q，我们需要在所有文档中找到最相似的文档。

[## 相似性搜索，第二部分：产品量化](https://medium.com/@slavahead/similarity-search-product-quantization-b2a1a6397701?source=post_page-----a8e508c765fa--------------------------------)

### 在本系列文章的第一部分，我们查看了用于执行相似性搜索的kNN和倒排文件索引结构。

[medium.com](https://medium.com/@slavahead/similarity-search-product-quantization-b2a1a6397701?source=post_page-----a8e508c765fa--------------------------------)

由于这两种算法侧重于不同方面，自然会产生一个问题，即是否可以将这两种算法合并成一种新算法。

在本文中，我们将结合这两种方法的优点，以产生一种快速且节省内存的算法。供参考，大多数讨论的想法将基于这篇[论文](https://lear.inrialpes.fr/pubs/2011/JDS11/jegou_searching_with_quantization.pdf)。

在深入细节之前，有必要了解残差向量是什么，并对其有用的属性有一个简单的直观认识。我们将在设计算法时使用它们。

# 残差向量

想象一下执行了一个聚类算法，并产生了几个簇。每个簇都有一个质心和与之相关的点。**残差**是一个点（向量）与其质心之间的偏移。基本上，要找出特定向量的残差，需要从其质心中减去该向量。

如果簇是由k-means算法构建的，那么簇的质心是所有属于该簇的点的均值。因此，从任何点找出残差将等同于从中减去簇的均值。通过从属于特定簇的所有点中减去均值，这些点将围绕0中心对齐。

![](../Images/7070915590dccde2b899bc0a636574f5.png)

原始的点簇显示在左侧。然后从所有簇点中减去簇质心。结果的残差向量显示在右侧。

我们可以观察到一个有用的事实：

> 用残差替换原始向量不会改变它们之间的相对位置。

也就是说，向量之间的距离始终保持不变。我们可以简单地查看下面的两个方程。

![](../Images/46d8743cac599b38e4087207b97f6a19.png)

减去均值不会改变相对距离

第一个方程是两个向量之间的欧几里得距离公式。在第二个方程中，从两个向量中减去簇的均值。我们可以看到，均值项会被消去——整个表达式变得与第一个方程中的欧几里得距离完全相同！

> 我们通过使用L2度量（欧几里得距离）的公式证明了这一声明。重要的是要记住，这个规则可能不适用于其他度量。

因此，如果对于给定的查询，目标是找到最近的邻居，可以仅从查询中减去簇均值，然后在残差中进行正常的搜索过程。

![](../Images/db343cbb826a8da69214579119ce24eb.png)

从查询中减去均值不会改变其相对位置。

现在让我们看看下图中的另一个例子，其中两个簇的向量残差分别计算。

> *从每个簇的对应质心中减去均值将使所有数据集向量围绕0中心*。

这是一个有用的观察，将在未来使用。此外，对于给定的查询，我们可以计算到所有簇的查询残差。查询残差使我们能够计算到簇的原始残差的距离。

![](../Images/1b183b38434931323d2192408830c3cb.png)

从每个簇中减去均值后，所有点都围绕0中心。查询和查询残差与相应簇中其他点的相对位置保持不变。

# 训练

考虑到上一节中的有用观察，我们可以开始设计算法。

给定一个向量数据库，构建一个倒排文件索引，将向量集划分为*n*个Voronoi分区，从而减少推理过程中的搜索范围。

在每个Voronoi分区内，从每个向量中减去质心的坐标。结果是，所有分区中的向量变得彼此更接近，并且围绕0中心。此时，无需原始向量，因为我们存储它们的残差。

之后，对所有分区中的向量运行产品量化算法。

*重要方面*：产品量化**不会**对每个分区单独执行——那样会很低效，因为分区的数量通常很高，这将需要大量的内存来存储所有的码本。相反，**算法会对所有分区的残差同时执行**。

实际上，现在每个子空间包含来自不同 Voronoi 分区的子向量。然后，对于每个子空间，执行一个聚类算法，构建出如常规的 *k* 个簇及其中心点。

![](../Images/d790a9adfcd54f936d2deaab13c2390d.png)

训练过程

*替换向量为其残差的重要性是什么？* 如果向量没有被其残差替换，那么每个子空间将包含更多的各种子向量（因为子空间将存储来自不同不相交的 Voronoi 分区的子向量，而这些子向量可能在空间中相距很远）。现在来自不同分区的向量（残差）彼此重叠。由于现在每个子空间的变化更小，因此表示向量所需的重现值也更少。换句话说：

> 使用之前相同长度的 PQ 代码，向量可以更准确地表示，因为它们的方差更小。

# 推断

对于给定的查询，找到 Voronoi 分区的 *k* 个最近中心点。所有这些区域内的点都被视为候选点。由于原始向量在每个 Voronoi 区域中最初被其残差所替代，查询向量的残差也需要被计算。在这种情况下，查询残差需要为每个 Voronoi 分区单独计算（因为每个区域有不同的中心点）。只有来自所选 Voronoi 分区的残差才会成为候选点。

查询残差随后被拆分为子向量。与原始的产品量化算法相同，对于每个子空间，计算包含从子空间中心点到查询子向量的距离的距离矩阵 *d*。必须记住，查询残差在每个 Voronoi 分区中都是不同的。这基本上意味着距离矩阵 *d* 需要为每个查询残差单独计算。这是所需优化的计算代价。

最后，部分距离被汇总，就像在产品量化算法中之前所做的那样。

## 排序结果

在计算所有距离后，需要选择 *k* 个最近邻点。为了高效完成这一过程，作者建议使用一个 [MaxHeap](https://medium.com/@slavahead/heapify-with-heap-sort-5df23b5764c1) 数据结构。它的容量有限为 *k*，并在每一步中存储 *k* 个当前最小的距离。每当计算出一个新距离时，只有当计算出的值小于 MaxHeap 中的最大值时，该值才会被添加到 MaxHeap 中。计算完所有距离后，查询的答案已经存储在 MaxHeap 中。使用 MaxHeap 的优点是其构建时间很快，为 *O(n)*。

![](../Images/e743b5800768527c67c5281736ef73a8.png)

推断过程

# 性能

该算法利用了倒排文件索引和产品量化。根据推理过程中 Voronoi 分区的数量，相同数量的子向量到质心矩阵 *d* 需要计算并存储在内存中。这可能看起来像是一个缺点，但与整体优势相比，这是一个相当好的折衷。

![](../Images/d6b859afba6649a67a22658427d08a91.png)

该算法从倒排文件索引继承了良好的搜索速度，从产品量化继承了压缩效率。

# Faiss 实现

> [**Faiss**](https://github.com/facebookresearch/faiss)（Facebook AI 搜索相似性）是一个用 C++ 编写的 Python 库，用于优化相似性搜索。该库提供了不同类型的索引，这些数据结构用于高效地存储数据和执行查询。

根据 [Faiss 文档](https://faiss.ai) 的信息，我们将了解如何将倒排文件和产品量化索引组合在一起形成新的索引。

Faiss 在 *IndexIVFPQ* 类中实现了上述算法，该类接受以下参数：

+   **quantizer**：指定计算向量之间距离的方式。

+   **d**：数据维度。

+   **nlist**：Voronoi 分区的数量。

+   **M**：子空间的数量。

+   **nbits**：编码单个簇 ID 所需的位数。这意味着单个子空间中的簇总数将等于 *k = 2^nbits*。

此外，可以调整 **nprobe** 属性，该属性指定在推理过程中用于搜索候选项的 Voronoi 分区数量。更改此参数无需重新训练。

![](../Images/6725c5f1ff01d708620078a43c2de613.png)

Faiss 的 IndexIVFPQ 实现

存储单个向量所需的内存与原始产品量化方法相同，只是现在我们增加了 8 个字节，用于在倒排文件索引中存储关于向量的信息。

![](../Images/3c990b125612a4c97a448acbe3bd99a9.png)

# 结论

利用之前文章部分的知识，我们探讨了一个先进算法的实现，该算法实现了高效的内存压缩和加速的搜索速度。该算法在处理大量数据时广泛用于信息检索系统。

# 资源

+   [最近邻搜索的产品量化](https://lear.inrialpes.fr/pubs/2011/JDS11/jegou_searching_with_quantization.pdf)

+   [Faiss 文档](https://faiss.ai)

+   [Faiss 仓库](https://github.com/facebookresearch/faiss)

+   [Faiss 索引汇总](https://github.com/facebookresearch/faiss/wiki/Faiss-indexes)

*除非另有说明，所有图片均由作者提供。*
