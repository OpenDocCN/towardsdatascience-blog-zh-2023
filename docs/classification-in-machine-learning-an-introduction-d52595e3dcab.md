# 机器学习中的分类：入门

> 原文：[https://towardsdatascience.com/classification-in-machine-learning-an-introduction-d52595e3dcab?source=collection_archive---------10-----------------------#2023-02-24](https://towardsdatascience.com/classification-in-machine-learning-an-introduction-d52595e3dcab?source=collection_archive---------10-----------------------#2023-02-24)

## 了解机器学习中的分类，包括它是什么、如何使用以及一些分类算法的示例

[](https://zoumanakeita.medium.com/?source=post_page-----d52595e3dcab--------------------------------)[![Zoumana Keita](../Images/34a15c1d03687816dbdbc065f5719f80.png)](https://zoumanakeita.medium.com/?source=post_page-----d52595e3dcab--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d52595e3dcab--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d52595e3dcab--------------------------------) [Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----d52595e3dcab--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe6ae785a30d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclassification-in-machine-learning-an-introduction-d52595e3dcab&user=Zoumana+Keita&userId=e6ae785a30d&source=post_page-e6ae785a30d----d52595e3dcab---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----d52595e3dcab--------------------------------) · 11分钟阅读·2023年2月24日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd52595e3dcab&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclassification-in-machine-learning-an-introduction-d52595e3dcab&source=-----d52595e3dcab---------------------bookmark_footer-----------)![](../Images/c16208ec7dde75291c85a0ea05165cfa.png)

图片由[Markus Winkler](https://unsplash.com/@markuswinkler)拍摄，来自[Unsplash](https://unsplash.com/photos/f57lx37DCM4)

# 介绍

目前，许多行业都在处理各种类型的大型数据集。手动处理所有这些信息可能既费时又可能在长期内没有实际价值。许多策略，从简单的自动化到机器学习技术，正在被应用以获得更好的投资回报。这篇概念性博客将深入探讨其中一个重要概念：机器学习中的分类。

我们将首先定义机器学习中的分类是什么，然后阐明机器学习中的两种学习者及分类和回归之间的区别。接下来，我们将介绍一些分类可以应用的实际场景。之后，我们将介绍所有不同类型的分类，并深入探讨一些分类算法的示例。最后，我们将提供一些算法实现的实践机会。

# 机器学习中的分类是什么？
