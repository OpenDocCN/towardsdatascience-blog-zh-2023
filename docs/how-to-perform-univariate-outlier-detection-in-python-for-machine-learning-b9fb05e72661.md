# 如何在Python中进行单变量离群值检测以用于机器学习

> 原文：[https://towardsdatascience.com/how-to-perform-univariate-outlier-detection-in-python-for-machine-learning-b9fb05e72661?source=collection_archive---------6-----------------------#2023-02-06](https://towardsdatascience.com/how-to-perform-univariate-outlier-detection-in-python-for-machine-learning-b9fb05e72661?source=collection_archive---------6-----------------------#2023-02-06)

## 离群值检测系列 — 第2部分

[](https://ibexorigin.medium.com/?source=post_page-----b9fb05e72661--------------------------------)[![Bex T.](../Images/516496f32596e8ad56bf07f178a643c6.png)](https://ibexorigin.medium.com/?source=post_page-----b9fb05e72661--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b9fb05e72661--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b9fb05e72661--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----b9fb05e72661--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39db050c2ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-perform-univariate-outlier-detection-in-python-for-machine-learning-b9fb05e72661&user=Bex+T.&userId=39db050c2ac2&source=post_page-39db050c2ac2----b9fb05e72661---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b9fb05e72661--------------------------------) ·9分钟阅读·2023年2月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb9fb05e72661&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-perform-univariate-outlier-detection-in-python-for-machine-learning-b9fb05e72661&user=Bex+T.&userId=39db050c2ac2&source=-----b9fb05e72661---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb9fb05e72661&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-perform-univariate-outlier-detection-in-python-for-machine-learning-b9fb05e72661&source=-----b9fb05e72661---------------------bookmark_footer-----------)![](../Images/32e714fae05225ece5cf9f7517a7195a.png)

图片来自 [Alexa](https://pixabay.com/users/alexas_fotos-686414/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1744091) [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1744091)

## 介绍

在开始异常值检测之前，首先要问的问题是：“我的数据集中是否有异常值？”。虽然通常的回答是“有”，但在做出重大努力（例如使用机器学习模型）来隔离它们之前，始终建议先探查一下是否有异常值的迹象。

因此，我们将通过查看检测异常值存在的一般方法（如数据可视化）来开始异常值检测教程系列的第二部分。然后，我们将继续讨论检测单变量和多变量异常值的方法。

让我们开始吧！

[## 如何在 Python 中进行异常值检测以用于机器学习：第 1 部分](https://towardsdatascience.com/how-to-perform-outlier-detection-in-python-in-easy-steps-for-machine-learning-1-8f9a3e6c88b5?source=post_page-----b9fb05e72661--------------------------------)

### 地球是一个异常值——理论

[towardsdatascience.com](https://towardsdatascience.com/how-to-perform-outlier-detection-in-python-in-easy-steps-for-machine-learning-1-8f9a3e6c88b5?source=post_page-----b9fb05e72661--------------------------------)

## 要使用的数据集

在整个教程中，我们将使用 Diamonds 数据集。它足够大，不是玩具数据集，并且具有良好的数值和分类特征组合。

```py
import seaborn as sns

diamonds =…
```
