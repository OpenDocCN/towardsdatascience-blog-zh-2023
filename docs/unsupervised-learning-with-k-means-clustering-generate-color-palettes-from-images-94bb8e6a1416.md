# 使用 K 均值聚类进行无监督学习：从图像生成颜色调色板

> 原文：[`towardsdatascience.com/unsupervised-learning-with-k-means-clustering-generate-color-palettes-from-images-94bb8e6a1416?source=collection_archive---------15-----------------------#2023-04-14`](https://towardsdatascience.com/unsupervised-learning-with-k-means-clustering-generate-color-palettes-from-images-94bb8e6a1416?source=collection_archive---------15-----------------------#2023-04-14)

## 关于无监督机器学习和 K 均值算法的全面指南，其中包括一个通过颜色对图像像素进行分组的聚类用例演示

[](https://nroy0110.medium.com/?source=post_page-----94bb8e6a1416--------------------------------)![Nabanita Roy](https://nroy0110.medium.com/?source=post_page-----94bb8e6a1416--------------------------------)[](https://towardsdatascience.com/?source=post_page-----94bb8e6a1416--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----94bb8e6a1416--------------------------------) [Nabanita Roy](https://nroy0110.medium.com/?source=post_page-----94bb8e6a1416--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd36a8b28c928&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funsupervised-learning-with-k-means-clustering-generate-color-palettes-from-images-94bb8e6a1416&user=Nabanita+Roy&userId=d36a8b28c928&source=post_page-d36a8b28c928----94bb8e6a1416---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----94bb8e6a1416--------------------------------) ·13 分钟阅读·2023 年 4 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F94bb8e6a1416&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funsupervised-learning-with-k-means-clustering-generate-color-palettes-from-images-94bb8e6a1416&user=Nabanita+Roy&userId=d36a8b28c928&source=-----94bb8e6a1416---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F94bb8e6a1416&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funsupervised-learning-with-k-means-clustering-generate-color-palettes-from-images-94bb8e6a1416&source=-----94bb8e6a1416---------------------bookmark_footer-----------)![](img/cf06c65cce12f976b12ba19ea8fc1473.png)

照片由[Billy Huynh](https://unsplash.com/@billy_huy?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

无监督学习是一种方法，通过它可以发现数据中的潜在模式，而不需要向机器学习算法提供额外的信息（或标签/目标）。在这篇文章中，我记录了一个我最近在阅读一些图像处理文章时发现的非常有趣的 K-Means 聚类算法应用，以及机器学习中的无监督聚类方法的介绍。

**本文的关键要点：**

1️⃣ 无监督机器学习：介绍、分类及应用

2️⃣ KMeans 聚类的全面理解

3️⃣ 使用 Scikit Learn Python 库逐步应用 K-Means 聚类，从给定图像生成颜色调色板

4️⃣ 使用 Pillow、Requests 和 Numpy 读取和处理图像

# 让我们首先深入了解无监督学习
