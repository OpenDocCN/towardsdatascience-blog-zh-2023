# 推荐系统中的双塔网络和负采样

> 原文：[https://towardsdatascience.com/two-tower-networks-and-negative-sampling-in-recommender-systems-fdc88411601b?source=collection_archive---------0-----------------------#2023-11-24](https://towardsdatascience.com/two-tower-networks-and-negative-sampling-in-recommender-systems-fdc88411601b?source=collection_archive---------0-----------------------#2023-11-24)

## 了解推动高级推荐引擎的关键元素

[](https://roizner.medium.com/?source=post_page-----fdc88411601b--------------------------------)[![Michael Roizner](../Images/bcb68ee626ea57234b62e512ed4b383b.png)](https://roizner.medium.com/?source=post_page-----fdc88411601b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fdc88411601b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fdc88411601b--------------------------------) [Michael Roizner](https://roizner.medium.com/?source=post_page-----fdc88411601b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1bee5af37d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwo-tower-networks-and-negative-sampling-in-recommender-systems-fdc88411601b&user=Michael+Roizner&userId=1bee5af37d8&source=post_page-1bee5af37d8----fdc88411601b---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fdc88411601b--------------------------------) ·7分钟阅读·2023年11月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffdc88411601b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwo-tower-networks-and-negative-sampling-in-recommender-systems-fdc88411601b&user=Michael+Roizner&userId=1bee5af37d8&source=-----fdc88411601b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffdc88411601b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwo-tower-networks-and-negative-sampling-in-recommender-systems-fdc88411601b&source=-----fdc88411601b---------------------bookmark_footer-----------)

目前推荐系统中最重要的模型之一是双塔神经网络。它们的结构如下：神经网络的一个部分（塔）处理关于查询（用户、上下文）的所有信息，而另一个塔处理关于对象的信息。这些塔的输出是嵌入，然后将这些嵌入相乘（点积或余弦，如我们已经在[这里](https://roizner.medium.com/dot-product-or-cosine-55cd5b22a87c)讨论过）。双塔网络应用于推荐的最早提及之一可以在关于 YouTube 的一篇非常好的[论文](https://dl.acm.org/doi/10.1145/2959100.2959190)中找到。顺便提一下，我现在会将这篇文章称为经典且最适合进入推荐领域的文章。

![](../Images/4ba8e24dde42029e01b58a473e90365f.png)

来自论文 [YouTube 推荐的深度神经网络](https://dl.acm.org/doi/10.1145/2959100.2959190)

这种网络的特点是什么？它们与矩阵分解非常相似，实际上矩阵分解是一种特殊情况，仅以 *user_id* 和 *item_id* 作为输入。然而，如果我们将它们与任意网络进行比较，限制晚期交叉（不允许来自不同塔的输入在最后阶段之前融合）使得双塔网络在应用中极为高效。为了为单个用户构建推荐，我们只需要计算一次查询塔，然后将该嵌入与通常预先计算的文档嵌入相乘。这个过程非常快速。此外，这些预先计算的文档嵌入可以组织成一个 [ANN](https://en.wikipedia.org/wiki/Nearest_neighbor_search) 索引（例如，[HNSW](https://github.com/nmslib/hnswlib)），以便快速找到好的候选项，而无需遍历整个数据库。

[](/similarity-search-part-4-hierarchical-navigable-small-world-hnsw-2aad4fe87d37?source=post_page-----fdc88411601b--------------------------------) [## 相似性搜索，第 4 部分：分层可导航小世界（HNSW）

### 分层可导航小世界（HNSW）是一种用于近似最近邻搜索的最先进算法...

towardsdatascience.com](/similarity-search-part-4-hierarchical-navigable-small-world-hnsw-2aad4fe87d37?source=post_page-----fdc88411601b--------------------------------)

我们可以通过以某种规律异步计算用户部分而不是对每个查询进行计算来实现更高的效率。然而，这意味着需要牺牲对实时历史和上下文的考虑。

塔本身可以相当复杂。例如，在用户部分，我们可以使用自注意力机制处理历史记录，从而实现个性化的变换器。但引入晚期交叉限制的代价是什么？自然，它影响质量。在相同的注意力机制中，我们不能使用当前希望推荐的项目。理想情况下，我们希望关注用户历史中的相似项目。因此，具有早期交叉的网络通常在排序的后期阶段使用，当只剩下几十个或几百个候选时，而具有晚期交叉（双塔）的网络则在早期阶段和候选生成中使用。

*（然而，有一个纯理论的观点认为，任何合理的文档排名都可以通过足够维度的嵌入来编码。此外，NLP中的解码器实际上也是基于相同的原理，只是对每个标记重新计算查询塔。）*

# 损失函数和负样本采样

一个特别关注的点是用于训练双塔网络的损失函数。原则上，它们可以使用任何损失函数进行训练，针对不同的结果，甚至对不同的头部使用多个不同的损失函数（每个塔中有不同的嵌入）。然而，一个有趣的变体是使用批量内负样本上的softmax损失进行训练。对于数据集中每个查询-文档对，其他在同一小批次中的文档被用作softmax损失中的负样本。这种方法是一种高效的困难负样本挖掘形式。

但考虑这种损失函数的概率解释是很重要的，而这并不总是被很好地理解。在训练好的网络中，

![](../Images/714c36ba7b5f07a6f592cab38b6e9156.png)

得分的指数与给定查询的文档的先验概率成比例，而不是与特定于查询的PMI（[点对点互信息](https://en.wikipedia.org/wiki/Pointwise_mutual_information)）成比例。更受欢迎的文档不一定会被这种模型更频繁地推荐，因为在训练过程中，它们作为负样本的出现频率相对较高。使用得分作为特征可能是有益的，但对于最终的排序和候选生成，这可能导致非常具体但质量较差的文档。

谷歌在一篇[论文](https://research.google/pubs/pub48840/)中建议通过训练中的logQ校正来应对这个问题。而我们则通常在应用阶段处理这个问题，而不是训练阶段，通过简单地乘以文档的先验概率P(*d*)。然而，我们从未比较过这些方法，这确实是一个有趣的比较。

# 隐式正则化：连接ALS与现代神经网络

有一种协同过滤算法叫做隐式ALS（IALS）。我已经[提到过它](https://roizner.medium.com/leveraging-external-embeddings-in-recommender-systems-a-practical-guide-e3768cc574e1)。在神经网络时代之前，它无疑是最受欢迎的算法之一。其显著特点是有效的‘挖掘’负样本：所有没有互动历史的用户-对象对都被视为负样本（虽然权重低于实际互动）。此外，与实际挖掘不同，这些负样本没有被采样，而是在每次迭代中全部使用。这种方法被称为隐式正则化。

这怎么可能呢？考虑到合理的任务规模（用户和对象数量），应该有那么多负样本，甚至列出它们所需的时间都比整个训练过程还长。算法的美妙之处在于，通过使用MSE损失和最小二乘法，可以在每次完整迭代之前分别为所有用户和所有对象预先计算某些元素，这足以进行隐式正则化。这样，算法避免了二次大小。（有关更多细节，请参阅[我当时最喜欢的论文之一](https://dl.acm.org/doi/10.1145/1864708.1864726)）。

几年前，我考虑过是否可以将这个隐式正则化的奇妙想法与更先进的双塔神经网络技术结合起来。这是一个复杂的问题，因为有随机优化而不是全批处理，并且对回退到MSE损失（至少对于整个任务；对于正则化来说可能还好）有顾虑，因为这往往会产生较差的结果。

我思考了很久，最终想出了一个解决方案！有几周的时间，我兴奋不已，热切期待我们如何用这个方案替代批量负样本。

然后，当然（如同在这种情况下经常发生的那样），我在一篇论文中读到一切早在三年前就已经被想到过了。再次，它是[谷歌](https://arxiv.org/abs/1807.07187)。后来，在那篇[关于logQ校正的论文](https://research.google/pubs/pub48840/)中，他们展示了softmax损失与批量负样本的组合比隐式正则化效果更好。

就这样，我们能够节省时间而没有测试这个想法🙂

# 我们真的需要负样本采样用于推荐模型吗？

毕竟，我们有真实的推荐印象实例，如果用户没有与这些实例互动，这些可以作为强负样本使用。（这不考虑推荐服务尚未启动且没有印象的情况。）

这个问题的答案并不那么简单；它取决于我们打算如何应用训练好的模型：是用于最终排序、候选生成，还是仅仅作为输入到另一个模型的特征。

当我们仅在实际展示上训练模型时，会发生什么？会出现相当强的选择偏差，模型只学会在特定上下文中区分那些文档。对于未展示的文档（或者更准确地说，查询-文档对），模型的表现会差很多：它可能会对一些文档进行过度预测，对其他文档进行低估。当然，这种效果可以通过在排名中应用探索来缓解，但通常这只是部分解决方案。

如果候选生成器以这种方式训练，它可能会针对一个查询生成大量文档，这些文档在这样的上下文中它从未见过，并且其预测被高估。在这些文档中，常常会有完全无用的内容。如果最终排序模型足够好，它会过滤掉这些文档，并不会展示给用户。然而，我们仍然不必要地浪费候选配额在这些文档上（而且可能根本没有合适的文档）。因此，候选生成器应以一种理解大部分文档库质量较差并且不应被推荐（提名为候选）的方式进行训练。负采样是一个好的方法。

在这方面，最终排序模型与候选生成非常相似，但有一个重要的区别：它们从错误中学习。当模型通过对某些文档的预测过高而出错时，这些文档会展示给用户，并可能被纳入下一个训练数据集。我们可以在这个新数据集上重新训练模型，并再次推出给用户。新的假阳性会出现。数据集收集和重新训练过程可以重复，从而形成一种主动学习。实际上，只需几次重新训练迭代即可使过程收敛，并使模型停止推荐无用内容。当然，必须权衡随机推荐的危害，有时值得采取额外的预防措施。但总体而言，这里不需要负采样。相反，它可能会损害探索，使系统停留在局部最优。

如果模型用于将特征作为输入传递给另一个模型，那么相同的逻辑适用，但对随机候选文档的预测过高的伤害更不显著，因为其他特征可以帮助调整最终预测。（如果文档甚至没有进入候选列表，我们不会为其计算特征。）

曾经我们直接测试发现，作为特征的标准ALS比IALS表现更好，但不应用于候选生成。

总结而言，我们的探索强调了双塔网络在排序中的有效性，研究了损失函数和负采样在模型准确性中的重要性，通过隐式正则化弥合了与经典协同过滤的差距，并讨论了负采样在推荐系统中的核心作用。此次讨论突显了推荐系统技术的不断演变的复杂性和精密性。
