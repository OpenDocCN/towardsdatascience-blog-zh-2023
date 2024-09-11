# 使用 Python 进行多目标分类的实践

> 原文：[`towardsdatascience.com/hands-on-multitarget-classification-using-python-1ac439aac708?source=collection_archive---------8-----------------------#2023-01-02`](https://towardsdatascience.com/hands-on-multitarget-classification-using-python-1ac439aac708?source=collection_archive---------8-----------------------#2023-01-02)

![](img/5e4cebbae254a835625b377735638543.png)

图片来源：[Christin Hume](https://unsplash.com/@christinhumephoto?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 方法概述、评估指标和最佳实践

[](https://medium.com/@marcellopoliti?source=post_page-----1ac439aac708--------------------------------)![Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----1ac439aac708--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1ac439aac708--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1ac439aac708--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----1ac439aac708--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7390355d40fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhands-on-multitarget-classification-using-python-1ac439aac708&user=Marcello+Politi&userId=7390355d40fe&source=post_page-7390355d40fe----1ac439aac708---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1ac439aac708--------------------------------) ·9 分钟阅读·2023 年 1 月 2 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1ac439aac708&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhands-on-multitarget-classification-using-python-1ac439aac708&user=Marcello+Politi&userId=7390355d40fe&source=-----1ac439aac708---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1ac439aac708&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhands-on-multitarget-classification-using-python-1ac439aac708&source=-----1ac439aac708---------------------bookmark_footer-----------)

## 介绍

最近我在开发一种机器学习算法，该算法能够识别建筑物中的不同类型的损伤。这些损伤并不完全相同，每种损伤都有不同的原因和风险，因此我们已经识别出了大约 4 种不同类型的裂缝。该算法将被部署在一架无人机上，无人机将自动拍摄建筑物的照片，并能够判断建筑物中存在的损伤类型及其严重程度。

显然，在无人机拍摄的照片中可能会出现不同类型的损伤，因此无人机在给定一张照片时，必须能够识别照片中存在的所有不同损伤类别，而不仅仅是一个。这就是为什么我开始研究所谓的**多目标分类任务**。我写这篇文章希望它对你也有所帮助。

## 什么是多目标分类？

多目标分类是一种机器学习任务，涉及为单个样本预测多个标签。与传统的二分类或多分类不同，在传统分类中，每个样本被分配到一个单一类别，而**多目标分类**…
