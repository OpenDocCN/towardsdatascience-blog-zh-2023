# 如何评估学习排序模型

> 原文：[`towardsdatascience.com/how-to-evaluate-learning-to-rank-models-d12cadb99d47`](https://towardsdatascience.com/how-to-evaluate-learning-to-rank-models-d12cadb99d47)

## 评估 LTR 模型的实用指南

[](https://ransakaravihara.medium.com/?source=post_page-----d12cadb99d47--------------------------------)![Ransaka Ravihara](https://ransakaravihara.medium.com/?source=post_page-----d12cadb99d47--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d12cadb99d47--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d12cadb99d47--------------------------------) [Ransaka Ravihara](https://ransakaravihara.medium.com/?source=post_page-----d12cadb99d47--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d12cadb99d47--------------------------------) ·4 分钟阅读·2023 年 1 月 17 日

--

![](img/d1829e4f1740ac9601d2d32d5ae7284a.png)

图片来源 [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我之前的文章解释了三种主要的方法来解决学习排序问题。在本文中，我们将重点讨论如何评估 LTR 模型。让我们开始吧。

我们有几个选项来评估 LTR 模型。然而，这些选项与我们正在使用的方法有所不同。如果目标是为每个文档分配二元相关性评分，我们应使用 ***二元相关性*** 指标。如果目标是为每个文档在连续尺度上设置相关性评分，我们应使用 ***分级相关性***。让我们讨论三种广泛使用的评估矩阵类型。

**均值平均精度 (MAP)**

计算排名结果的 MAP 可能很棘手且常常令人困惑。让我们一步步地查看下面的图示。

![](img/e91450e54b5fb8ce6bba079cc716a8f3.png)

MAP 步骤 | 图片来源作者

MAP 有一些潜在的缺点，

1.  它不考虑检索到的项的排名，只考虑相关文档的存在或不存在。

1.  对于相关性不是二元的 数据集，这可能不合适，因为它没有考虑项的相关性程度。

我们稍后会通过例子进一步探讨这个问题。

**均值倒排排名 (MRR)**

一个直接的评估指标。

![](img/29fea9071752be09b7cfdfd7aa3c45bf.png)

MRR 指标 | 图片来源作者

MRR 的一个潜在缺点是它只考虑给定查询的第一个相关文档。

**归一化折扣累积增益 (NDCG)**

这是大多数用例中最常见和理想的评估指标。它考虑了结果的相关性和位置。它可以为高度相关的结果分配更高的分数，并出现在列表的顶部。

![](img/8c8cf8d290a11ba3acb7a5d6c2885da4.png)

NDCG 步骤 | 作者提供的图像

如果考虑 DCG 公式，当文档的相关性高时，分子会增加；当文档的位置增加时，分母会增加。总的来说，当高度相关的项排名靠前时，DCG 值会更高。

## 手动评估 LTR 模型

假设我们有两个查询，*q1*和*q2*，我们的 LTR 模型为每个查询分别返回了下面两个文档列表。（*注意文档的顺序*）

*q1* → *(d1, d2, d3)* q2 → *(d4, d5, d6)*

此外，根据实际标签，我们注意到只有*d1, d5*和*d6*对于给定的两个查询是相关的。

让我们计算上面讨论的每一个评估指标。

+   **计算*MAP***

这里使用的公式是，

![](img/96a2aa0c373da03f19b8e297530cbac2.png)

作者提供的图像

让我们代入与查询*q1*相关的值，

![](img/3d2b20330a7b85d7c7af92e18bc3ac9f.png)

作者提供的图像

同样适用于*q2*。

![](img/1fe017ecc0fc5dfe8db431390bd0744b.png)

作者提供的图像

最后，我们可以得到下列文档-查询对的 MAP 值。

![](img/7af6bbb4c16a350a1903e3ff688c29bd.png)

作者提供的图像

请注意，如果你改变*d5*和*d6*的顺序，你仍然会得到相同的 MAP 值，因为这不会考虑排名。

+   **计算*MAR***

我们可以如上所示计算*MAR*。这不是一个复杂的公式。

![](img/0fc58bbf41b06552e3d44fc06988747e.png)

作者提供的图像

+   **计算 NDCG**（对于查询*q2*）

这里使用的公式是，

![](img/10764363c636d0beaed93db018338e11.png)

作者提供的图像

*步骤 01：计算 DCG(q2)*

![](img/837d72ba272d3a2a0c0aaef59d08336a.png)

作者提供的图像

*步骤 02：计算 IDCG(q2)*

![](img/afba39e1cad015505e1a67e78aeaf1f8.png)

作者提供的图像

*步骤 03：计算 NDCG(q2)*

![](img/5df6115890f710ea9f22c418c4c576fb.png)

作者提供的图像

*系列下一篇：* *如何使用 Python 实现学习排序模型*

[](/how-to-implement-learning-to-rank-model-using-python-569cd9c49b08?source=post_page-----d12cadb99d47--------------------------------) ## 如何使用 Python 实现学习排序模型

### 使用 Python 和 LightGBM 实现 lambdarank 算法的逐步指南

towardsdatascience.com

## 结论

还有其他用于评估 LTR 模型的评估指标。这里，我已经解释了广泛使用的指标。但是，选择合适的评估指标往往比较棘手。因为它严重依赖于数据集（主要是目标标签）和我们所解决的问题。

感谢阅读！
