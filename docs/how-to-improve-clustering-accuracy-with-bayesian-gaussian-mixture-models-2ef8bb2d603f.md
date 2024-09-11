# 如何通过贝叶斯高斯混合模型提高聚类准确性

> 原文：[https://towardsdatascience.com/how-to-improve-clustering-accuracy-with-bayesian-gaussian-mixture-models-2ef8bb2d603f?source=collection_archive---------8-----------------------#2023-02-15](https://towardsdatascience.com/how-to-improve-clustering-accuracy-with-bayesian-gaussian-mixture-models-2ef8bb2d603f?source=collection_archive---------8-----------------------#2023-02-15)

## 聚类

## 一种用于真实世界数据的更高级的聚类技术

[](https://medium.com/@maclayton?source=post_page-----2ef8bb2d603f--------------------------------)[![Mike Clayton](../Images/2d37746b13b7d2ff1c6515893914da97.png)](https://medium.com/@maclayton?source=post_page-----2ef8bb2d603f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2ef8bb2d603f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2ef8bb2d603f--------------------------------) [Mike Clayton](https://medium.com/@maclayton?source=post_page-----2ef8bb2d603f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F51dce1c5bc03&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-improve-clustering-accuracy-with-bayesian-gaussian-mixture-models-2ef8bb2d603f&user=Mike+Clayton&userId=51dce1c5bc03&source=post_page-51dce1c5bc03----2ef8bb2d603f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2ef8bb2d603f--------------------------------) ·27分钟阅读·2023年2月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2ef8bb2d603f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-improve-clustering-accuracy-with-bayesian-gaussian-mixture-models-2ef8bb2d603f&user=Mike+Clayton&userId=51dce1c5bc03&source=-----2ef8bb2d603f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2ef8bb2d603f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-improve-clustering-accuracy-with-bayesian-gaussian-mixture-models-2ef8bb2d603f&source=-----2ef8bb2d603f---------------------bookmark_footer-----------)![](../Images/4dfa3e01cd737d3436bde521a650137a.png)

照片来源于 [Tima Miroshnichenko](https://www.pexels.com/photo/mixture-of-paint-on-palette-5034000/) 来自 [Pexels](https://www.pexels.com/)

**在现实世界中，你会发现数据往往遵循某种概率分布。具体是高斯（或正态）分布、威布尔分布、泊松分布、指数分布等，这将取决于具体的数据。**

**了解哪种分布描述了你的数据，或者最有可能描述你的数据，可以让你利用这一点，从而改善你的推断和/或预测。**

**本文将探讨如何利用数据集的潜在概率分布来改进普通K-Means聚类模型的拟合，并且能够直接从底层数据中自动选择适当的簇数。**

# 介绍

许多引人注目的机器学习/深度学习技术往往涉及***监督学习***，即数据已被标记，并且模型得到了正确的答案以供学习。训练后的模型随后会应用于未来的数据进行预测。
