# CatBoost 回归：为我解析一下

> 原文：[https://towardsdatascience.com/catboost-regression-break-it-down-for-me-16ed8c6c1eca?source=collection_archive---------4-----------------------#2023-09-02](https://towardsdatascience.com/catboost-regression-break-it-down-for-me-16ed8c6c1eca?source=collection_archive---------4-----------------------#2023-09-02)

## 对 CatBoost 内部工作原理的全面（且图示化）解析

[](https://medium.com/@shreya.rao?source=post_page-----16ed8c6c1eca--------------------------------)[![Shreya Rao](../Images/03f13be6f5f67783d32f0798f09a4f86.png)](https://medium.com/@shreya.rao?source=post_page-----16ed8c6c1eca--------------------------------)[](https://towardsdatascience.com/?source=post_page-----16ed8c6c1eca--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----16ed8c6c1eca--------------------------------) [Shreya Rao](https://medium.com/@shreya.rao?source=post_page-----16ed8c6c1eca--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F99b63de2f2c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcatboost-regression-break-it-down-for-me-16ed8c6c1eca&user=Shreya+Rao&userId=99b63de2f2c3&source=post_page-99b63de2f2c3----16ed8c6c1eca---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----16ed8c6c1eca--------------------------------) · 14 分钟阅读 · 2023年9月2日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F16ed8c6c1eca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcatboost-regression-break-it-down-for-me-16ed8c6c1eca&user=Shreya+Rao&userId=99b63de2f2c3&source=-----16ed8c6c1eca---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F16ed8c6c1eca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcatboost-regression-break-it-down-for-me-16ed8c6c1eca&source=-----16ed8c6c1eca---------------------bookmark_footer-----------)

CatBoost，即分类增强，是一种强大的机器学习算法，擅长处理分类特征并生成准确的预测。传统上，处理分类数据相当棘手——需要进行独热编码、标签编码或其他一些可能扭曲数据固有结构的预处理技术。为了解决这个问题，CatBoost 采用了自己的内置编码系统，称为 **Ordered Target Encoding**。

让我们通过建立一个模型来预测某人如何评分书籍 *Murder, She Texted*，这个模型基于他们在 [Goodreads](https://www.goodreads.com/) 上的平均书籍评分和他们的最爱类型，以实际了解 CatBoost 是如何工作的。

我们邀请了6个人对*Murder, She Texted*进行评分，并收集了他们的其他相关信息。

![](../Images/c55b6b7914e005d3d922bd1388c2897e.png)

这是我们当前的训练数据集，我们将使用它来训练（显而易见）数据。

## 步骤 1：打乱数据集并使用**有序目标编码**对分类数据进行编码

我们对分类数据的预处理方式是CatBoost算法的核心。在这种情况下，我们只有一个分类列——*Favorite Genre*…
