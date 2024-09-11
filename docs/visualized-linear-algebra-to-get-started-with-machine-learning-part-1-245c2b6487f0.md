# 视觉化线性代数以入门机器学习：第1部分

> 原文：[https://towardsdatascience.com/visualized-linear-algebra-to-get-started-with-machine-learning-part-1-245c2b6487f0?source=collection_archive---------9-----------------------#2023-02-22](https://towardsdatascience.com/visualized-linear-algebra-to-get-started-with-machine-learning-part-1-245c2b6487f0?source=collection_archive---------9-----------------------#2023-02-22)

![](../Images/a3f469428d09c518cc165b4bb3c75a2c.png)

图片由[Michael Dziedzic](https://unsplash.com/@lazycreekimages?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 掌握线性代数的基本要素，从简单且直观的基础概念解释开始

[](https://medium.com/@marcellopoliti?source=post_page-----245c2b6487f0--------------------------------)[![Marcello Politi](../Images/484e44571bd2e75acfe5fef3146ab3c2.png)](https://medium.com/@marcellopoliti?source=post_page-----245c2b6487f0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----245c2b6487f0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----245c2b6487f0--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----245c2b6487f0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7390355d40fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualized-linear-algebra-to-get-started-with-machine-learning-part-1-245c2b6487f0&user=Marcello+Politi&userId=7390355d40fe&source=post_page-7390355d40fe----245c2b6487f0---------------------post_header-----------) 发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----245c2b6487f0--------------------------------) · 11分钟阅读 · 2023年2月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F245c2b6487f0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualized-linear-algebra-to-get-started-with-machine-learning-part-1-245c2b6487f0&user=Marcello+Politi&userId=7390355d40fe&source=-----245c2b6487f0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F245c2b6487f0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualized-linear-algebra-to-get-started-with-machine-learning-part-1-245c2b6487f0&source=-----245c2b6487f0---------------------bookmark_footer-----------)

通常，当人们想要开始机器学习的旅程时，面临的主要困难之一就是需要理解数学概念。如果你没有坚实的线性代数、统计学、概率论、优化理论或其他相关学科的基础，这可能会很困难。🤔💭🔢✖️🧮

在本文中，我想从**对基本线性代数概念的直观解释**开始，这些概念在深入机器学习领域之前是必不可少的。显然，本文并不是要详尽无遗地讲述这一主题，关于这个主题还有很多内容需要了解，但也许这可以作为处理这个主题的第一步！

+   介绍

+   什么是向量？

+   简单的向量运算

+   投影

+   基础、向量空间和线性独立性

+   矩阵与方程求解

## 介绍

**为什么线性代数对数据科学很重要？**
