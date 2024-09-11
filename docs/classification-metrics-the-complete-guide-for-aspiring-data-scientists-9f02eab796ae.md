# 分类指标：有志数据科学家的完整指南

> 原文：[`towardsdatascience.com/classification-metrics-the-complete-guide-for-aspiring-data-scientists-9f02eab796ae?source=collection_archive---------9-----------------------#2023-05-15`](https://towardsdatascience.com/classification-metrics-the-complete-guide-for-aspiring-data-scientists-9f02eab796ae?source=collection_archive---------9-----------------------#2023-05-15)

## 掌握机器学习中分类指标的唯一指南

[](https://federicotrotta.medium.com/?source=post_page-----9f02eab796ae--------------------------------)![Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----9f02eab796ae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9f02eab796ae--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9f02eab796ae--------------------------------) [Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----9f02eab796ae--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F654cd4bbe899&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclassification-metrics-the-complete-guide-for-aspiring-data-scientists-9f02eab796ae&user=Federico+Trotta&userId=654cd4bbe899&source=post_page-654cd4bbe899----9f02eab796ae---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9f02eab796ae--------------------------------) · 26 分钟阅读 · 2023 年 5 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9f02eab796ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclassification-metrics-the-complete-guide-for-aspiring-data-scientists-9f02eab796ae&user=Federico+Trotta&userId=654cd4bbe899&source=-----9f02eab796ae---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9f02eab796ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclassification-metrics-the-complete-guide-for-aspiring-data-scientists-9f02eab796ae&source=-----9f02eab796ae---------------------bookmark_footer-----------)![](img/0c0d34355b4d7101042a7934ccc24a1e.png)

图片来自 [UliSchu](https://pixabay.com/it/users/ulischu-1993560/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1205171) 在 [Pixabay](https://pixabay.com/it//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1205171)

监督学习可以分为两类问题：分类和回归。本文旨在成为分类度量的**权威指南**：所以如果你是一个有抱负的数据科学家，或者你是一个初级数据科学家，你绝对需要阅读这篇文章。

首先，你可能还会喜欢阅读我关于掌握回归问题所需了解的 5 个度量的指南：

## 掌握回归分析的艺术：每个数据科学家都应该知道的 5 个关键度量

### 关于回归分析中使用的度量的所有知识的**权威指南**

towardsdatascience.com

其次，让我通过目录告诉你这里会找到什么：

```py
**Table of Contents:**

What is a classification problem?
Dealing with class imbalance
What a classification algorithm actually does
Accuracy
Precision and recall
F1-score
The confusion matrix
Sensitivity and specificity
Log loss (cross-entropy)
Categorical crossentropy
AUC/ROC curve
Precision-recall curve
BONUS: KDE and learning curves
```
