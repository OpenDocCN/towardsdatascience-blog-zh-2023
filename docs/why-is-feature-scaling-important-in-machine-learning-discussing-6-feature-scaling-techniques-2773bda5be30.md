# 为什么特征缩放在机器学习中很重要？讨论 6 种特征缩放技术

> 原文：[`towardsdatascience.com/why-is-feature-scaling-important-in-machine-learning-discussing-6-feature-scaling-techniques-2773bda5be30?source=collection_archive---------8-----------------------#2023-08-15`](https://towardsdatascience.com/why-is-feature-scaling-important-in-machine-learning-discussing-6-feature-scaling-techniques-2773bda5be30?source=collection_archive---------8-----------------------#2023-08-15)

## 标准化、归一化、鲁棒缩放、均值归一化、最大绝对缩放和向量单位长度缩放

[](https://rukshanpramoditha.medium.com/?source=post_page-----2773bda5be30--------------------------------)![Rukshan Pramoditha](https://rukshanpramoditha.medium.com/?source=post_page-----2773bda5be30--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2773bda5be30--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2773bda5be30--------------------------------) [Rukshan Pramoditha](https://rukshanpramoditha.medium.com/?source=post_page-----2773bda5be30--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff90a3bb1d400&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-is-feature-scaling-important-in-machine-learning-discussing-6-feature-scaling-techniques-2773bda5be30&user=Rukshan+Pramoditha&userId=f90a3bb1d400&source=post_page-f90a3bb1d400----2773bda5be30---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----2773bda5be30--------------------------------) · 13 分钟阅读 · 2023 年 8 月 15 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2773bda5be30&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-is-feature-scaling-important-in-machine-learning-discussing-6-feature-scaling-techniques-2773bda5be30&user=Rukshan+Pramoditha&userId=f90a3bb1d400&source=-----2773bda5be30---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2773bda5be30&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-is-feature-scaling-important-in-machine-learning-discussing-6-feature-scaling-techniques-2773bda5be30&source=-----2773bda5be30---------------------bookmark_footer-----------)![](img/fd0c135d0ae75a274f8063f5a34b65b7.png)

图片由[Mediamodifier](https://unsplash.com/@mediamodifier?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，发布于[Unsplash](https://unsplash.com/photos/TuZAl7v4TCM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

许多机器学习算法需要将特征缩放到相同的尺度。

我们可以在不同场景中选择不同的特征缩放方法。它们有不同的（技术性）名称。术语 ***特征缩放*** 只是指这些方法中的任何一种。

```py
**Topics
------**
**1\. Feature scaling in different scenarios**

   a. Feature scaling in PCA
   b. Feature scaling in k-means
   c. Feature scaling in KNN and SVM
   d. Feature scaling in linear models
   e. Feature scaling in neural networks
   f. Feature scaling in the convergence
   g. Feature scaling in tree-based algorithms
   h. Feature scaling in LDA

**2\. Feature scaling methods**

   a. Standardization
   b. Min-Max Scaling (Normalization)
   c. Robust Scaling
   d. Mean Normalization
   e. Maximum Absolute Scaling
   f. Vector Unit-Length Scaling

**3\. Feature scaling and distribution of data

4\. Data leakage when feature scaling

5\. Summary of feature scaling methods**
```

# 不同场景中的特征缩放
