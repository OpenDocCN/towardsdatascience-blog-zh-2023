# 多层神经网络在神经退行性疾病分类中的应用

> 原文：[`towardsdatascience.com/multi-layer-neural-network-for-neurodegenerative-disorder-classification-27c0a5efc766?source=collection_archive---------8-----------------------#2023-03-03`](https://towardsdatascience.com/multi-layer-neural-network-for-neurodegenerative-disorder-classification-27c0a5efc766?source=collection_archive---------8-----------------------#2023-03-03)

![](img/b8e6cdffb84e992c90f9e59b694fb71b.png)

图片由 [Jon Tyson](https://unsplash.com/es/@jontyson?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 逐步指南：开发用于分类阿尔茨海默病磁共振图像的多层神经网络

[](https://luca-zammataro.medium.com/?source=post_page-----27c0a5efc766--------------------------------)![Luca Zammataro](https://luca-zammataro.medium.com/?source=post_page-----27c0a5efc766--------------------------------)[](https://towardsdatascience.com/?source=post_page-----27c0a5efc766--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----27c0a5efc766--------------------------------) [Luca Zammataro](https://luca-zammataro.medium.com/?source=post_page-----27c0a5efc766--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5ac72d0e7a58&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmulti-layer-neural-network-for-neurodegenerative-disorder-classification-27c0a5efc766&user=Luca+Zammataro&userId=5ac72d0e7a58&source=post_page-5ac72d0e7a58----27c0a5efc766---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----27c0a5efc766--------------------------------) ·23 分钟阅读·2023 年 3 月 3 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F27c0a5efc766&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmulti-layer-neural-network-for-neurodegenerative-disorder-classification-27c0a5efc766&source=-----27c0a5efc766---------------------bookmark_footer-----------)

在 2019 年 12 月，在*Medium*和*Toward Data Science*的支持下，我开始撰写一系列文章和 Python 教程，解释机器学习（ML）是如何工作的，以及它应该如何应用于生物医学数据。从头构建 ML 算法的想法，尤其是在没有像*TensorFlow*或*Scikit-learn*这样的框架支持下，是有意为之，并旨在解释 ML 的基础知识，这对于希望深入理解所有概念的读者特别有益。此外，这些被过度引用的框架是强大的工具，需要特别注意参数设置。因此，对其基础的深入研究可以向读者解释如何充分发挥它们的潜力。

随着时间的推移，我意识到*诊断成像*可能是生物医学领域中最能受益于人工智能帮助的领域。如果说其他方面无关紧要，那么这是一个提供了许多优化 ML 算法线索的领域。确实，诊断成像是一个医疗领域，其中变异性和定义困难与广义上需要抽象的性质碰撞…
