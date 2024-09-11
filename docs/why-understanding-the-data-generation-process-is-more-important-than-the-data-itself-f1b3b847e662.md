# 为什么理解数据生成过程比数据本身更重要

> 原文：[`towardsdatascience.com/why-understanding-the-data-generation-process-is-more-important-than-the-data-itself-f1b3b847e662?source=collection_archive---------5-----------------------#2023-12-07`](https://towardsdatascience.com/why-understanding-the-data-generation-process-is-more-important-than-the-data-itself-f1b3b847e662?source=collection_archive---------5-----------------------#2023-12-07)

![](img/1e94220c137112adb87f108b94fa3b70.png)

图片由[Ryoji Iwata](https://unsplash.com/@ryoji__iwata?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 《为什么》 第五章和第六章，阅读系列

[](https://zzhu17.medium.com/?source=post_page-----f1b3b847e662--------------------------------)![Zijing Zhu, PhD](https://zzhu17.medium.com/?source=post_page-----f1b3b847e662--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f1b3b847e662--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f1b3b847e662--------------------------------) [Zijing Zhu, PhD](https://zzhu17.medium.com/?source=post_page-----f1b3b847e662--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7d83c09fb5d4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-understanding-the-data-generation-process-is-more-important-than-the-data-itself-f1b3b847e662&user=Zijing+Zhu%2C+PhD&userId=7d83c09fb5d4&source=post_page-7d83c09fb5d4----f1b3b847e662---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----f1b3b847e662--------------------------------) · 15 分钟阅读·2023 年 12 月 7 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff1b3b847e662&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-understanding-the-data-generation-process-is-more-important-than-the-data-itself-f1b3b847e662&source=-----f1b3b847e662---------------------bookmark_footer-----------)

在婴儿期早期，我们的大脑已经学会将相关性与因果关系联系起来，并尝试为周围发生的每件事寻找解释。如果我们后面的车长时间跟着我们做相同的转弯，我们会假设它在跟随我们，这是一种因果假设。然而，当我们从电影的情节中醒来时，我们会认为我们只是正好去往相同的目的地——这是一种混杂因素。一个共同的原因引入了两辆车运动之间的相关性。Pearl 给出的这个生动而相关的例子证明了人脑的运作方式。

那些我们无法理解合理解释的相关性呢？比如在整个群体中不相关但在住院患者中相关的两种疾病。如果你回顾一下[我上一篇文章](https://medium.com/towards-data-science/causal-diagram-confronting-the-achilles-heel-in-observational-data-a69cdb1c4818)中讨论的不同因果结构，它指出条件化碰撞器（住院患者）会产生一种解释效应，使两个不相关的变量虚假相关。换句话说，住院患者群体并不是总体群体的准确代表，从这个样本中得出的任何观察结果都不能被推广。
