# TensorFlow-GNN：图神经网络的终极指南

> 原文：[`towardsdatascience.com/tensorflow-gnn-an-end-to-end-guide-for-graph-neural-networks-a66bfd237c8c?source=collection_archive---------3-----------------------#2023-01-16`](https://towardsdatascience.com/tensorflow-gnn-an-end-to-end-guide-for-graph-neural-networks-a66bfd237c8c?source=collection_archive---------3-----------------------#2023-01-16)

![](img/f6cf81efc4d2972bc977bddcaf88dd1e.png)

“Mapsterpiece”由 Heidi Malin 创作，经许可使用

## 教程

## 如何使用你自己的 Pandas/NetworkX 数据集进行图、节点和边的预测

[](https://michael-malin.medium.com/?source=post_page-----a66bfd237c8c--------------------------------)![Michael Malin](https://michael-malin.medium.com/?source=post_page-----a66bfd237c8c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a66bfd237c8c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a66bfd237c8c--------------------------------) [Michael Malin](https://michael-malin.medium.com/?source=post_page-----a66bfd237c8c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8225885ee2a7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftensorflow-gnn-an-end-to-end-guide-for-graph-neural-networks-a66bfd237c8c&user=Michael+Malin&userId=8225885ee2a7&source=post_page-8225885ee2a7----a66bfd237c8c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a66bfd237c8c--------------------------------) · 20 min 阅读 · 2023 年 1 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa66bfd237c8c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftensorflow-gnn-an-end-to-end-guide-for-graph-neural-networks-a66bfd237c8c&user=Michael+Malin&userId=8225885ee2a7&source=-----a66bfd237c8c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa66bfd237c8c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftensorflow-gnn-an-end-to-end-guide-for-graph-neural-networks-a66bfd237c8c&source=-----a66bfd237c8c---------------------bookmark_footer-----------)

*特别感谢来自 DeepMind 的 Alvaro Sanchez Gonzalez，以及来自 Google 的 Bryan Perozzi 和 Sami Abu-el-haija，他们协助我完成了这个教程*

> 更新于 2023 年 4 月 22 日，进行了小修复，并增加了图网络方法

图数据无处不在。图研究还处于初期阶段，图数据建模的工具才刚刚开始出现。**如果你是一位希望脱颖而出的数据科学家，现在正是最好的时机**。不幸的是，由于缺乏教程和支持，处于前沿领域可能会很困难。这个指南希望能显著减少这一痛点。

# 为什么选择 TensorFlow-GNN？

TF-GNN 是谷歌最近为使用 TensorFlow 的图神经网络发布的。虽然还有其他 GNN 库，但 TF-GNN 的建模灵活性、因分布式学习而在大规模图上的性能，以及谷歌的支持意味着它可能会成为行业标准。这个指南假设你已经了解了这个库的优点，但请参阅[这篇论文](https://arxiv.org/abs/2207.03522)以获取更多信息和性能比较。此外…
