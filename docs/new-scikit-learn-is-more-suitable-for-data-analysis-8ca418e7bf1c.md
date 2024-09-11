# 新版 Scikit-Learn 更适合数据分析

> 原文：[https://towardsdatascience.com/new-scikit-learn-is-more-suitable-for-data-analysis-8ca418e7bf1c?source=collection_archive---------3-----------------------#2023-03-08](https://towardsdatascience.com/new-scikit-learn-is-more-suitable-for-data-analysis-8ca418e7bf1c?source=collection_archive---------3-----------------------#2023-03-08)

## Pandas 兼容性及 Scikit-Learn 版本 ≥1.2.0 中的新功能

[](https://saptashwa.medium.com/?source=post_page-----8ca418e7bf1c--------------------------------)[![Saptashwa Bhattacharyya](../Images/b01238113a1f6b91cb6fb0fbfa50303a.png)](https://saptashwa.medium.com/?source=post_page-----8ca418e7bf1c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8ca418e7bf1c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8ca418e7bf1c--------------------------------) [Saptashwa Bhattacharyya](https://saptashwa.medium.com/?source=post_page-----8ca418e7bf1c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9a3c3c477239&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnew-scikit-learn-is-more-suitable-for-data-analysis-8ca418e7bf1c&user=Saptashwa+Bhattacharyya&userId=9a3c3c477239&source=post_page-9a3c3c477239----8ca418e7bf1c---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8ca418e7bf1c--------------------------------) ·5 min read·2023年3月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8ca418e7bf1c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnew-scikit-learn-is-more-suitable-for-data-analysis-8ca418e7bf1c&user=Saptashwa+Bhattacharyya&userId=9a3c3c477239&source=-----8ca418e7bf1c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8ca418e7bf1c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnew-scikit-learn-is-more-suitable-for-data-analysis-8ca418e7bf1c&source=-----8ca418e7bf1c---------------------bookmark_footer-----------)![](../Images/9b506c4e0906db732b941fdb09a6a595.png)

新版 Sklearn 中的一些非常酷的更新！(来源: 作者的笔记)

大约在去年12月，Scikit-Learn 发布了一个主要的[稳定更新](https://scikit-learn.org/stable/auto_examples/release_highlights/plot_release_highlights_1_2_0.html)（v. 1.2.0–1），我终于有机会尝试一些突出的新功能。它现在与 Pandas 的兼容性更高，一些其他功能也将帮助我们完成回归和分类任务。下面，我将详细介绍一些新更新，并举例说明如何使用它们。让我们开始吧！

## 与 Pandas 的兼容性：

在对数据进行机器学习模型训练（如回归或神经网络）之前，应用一些数据标准化是一个常见的技术，以确保具有不同范围的特征在预测中获得相等的重要性（如果需要的话）。Scikit-Learn 提供了各种预处理 API，如 `StandardScaler`、`MaxAbsScaler` 等。随着新版本的发布，**即使在预处理后也可以保持数据框格式**，我们来看看下面：

```py
from sklearn.datasets import load_wine
from sklearn.model_selection import train_test_split
########################
X, y = load_wine(as_frame=True, return_X_y=True) 
# available from version >=0.23; as_frame
########################
X_train, X_test, y_train, y_test = train_test_split(X, y, stratify=y…
```
