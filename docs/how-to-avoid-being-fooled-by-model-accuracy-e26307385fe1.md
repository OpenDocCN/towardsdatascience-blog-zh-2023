# 如何避免被模型准确性愚弄

> 原文：[https://towardsdatascience.com/how-to-avoid-being-fooled-by-model-accuracy-e26307385fe1?source=collection_archive---------5-----------------------#2023-07-07](https://towardsdatascience.com/how-to-avoid-being-fooled-by-model-accuracy-e26307385fe1?source=collection_archive---------5-----------------------#2023-07-07)

## 二分类模型指标及其正确使用的视觉指南

[](https://johnadeojo.medium.com/?source=post_page-----e26307385fe1--------------------------------)[![John Adeojo](../Images/f6460fae462b055d36dce16fefcd142c.png)](https://johnadeojo.medium.com/?source=post_page-----e26307385fe1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e26307385fe1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e26307385fe1--------------------------------) [John Adeojo](https://johnadeojo.medium.com/?source=post_page-----e26307385fe1--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff933e1637e40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-avoid-being-fooled-by-model-accuracy-e26307385fe1&user=John+Adeojo&userId=f933e1637e40&source=post_page-f933e1637e40----e26307385fe1---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e26307385fe1--------------------------------) ·8 分钟阅读·Jul 7, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe26307385fe1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-avoid-being-fooled-by-model-accuracy-e26307385fe1&user=John+Adeojo&userId=f933e1637e40&source=-----e26307385fe1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe26307385fe1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-avoid-being-fooled-by-model-accuracy-e26307385fe1&source=-----e26307385fe1---------------------bookmark_footer-----------)![](../Images/752a27d99b7d07b9b21cd0e1e97debde.png)

图片由作者生成：Midjourney

# 背景 — 简单表面上看

用于评估分类模型性能的指标从数学角度来看相当直观。然而，我注意到许多建模者和数据科学家在表达这些指标时遇到困难，甚至错误地应用它们。这是一个容易犯的错误，因为这些指标表面上看起来很简单，但根据问题领域的不同，它们的影响可能是深远的。

本文作为解释常见分类模型指标的视觉指南。我们将探索定义并使用示例来突出指标被不当使用的地方。

## 关于可视化的简要说明

每个可视化包含九十个主题，代表我们可能希望分类的任何内容。蓝色主题表示负样本，而红色主题表示正样本。紫色框表示模型，它试图预测正样本。框内的内容是模型预测为正样本的部分。

明确了这些之后，让我们深入定义。
