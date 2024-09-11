# 广义线性模型简介

> 原文：[https://towardsdatascience.com/breaking-down-generalized-linear-models-d9212526e51d?source=collection_archive---------8-----------------------#2023-07-10](https://towardsdatascience.com/breaking-down-generalized-linear-models-d9212526e51d?source=collection_archive---------8-----------------------#2023-07-10)

## 拓展你的建模技能超越线性回归

[](https://medium.com/@egorhowell?source=post_page-----d9212526e51d--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----d9212526e51d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d9212526e51d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d9212526e51d--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----d9212526e51d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbreaking-down-generalized-linear-models-d9212526e51d&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----d9212526e51d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d9212526e51d--------------------------------) ·6 分钟阅读·2023年7月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd9212526e51d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbreaking-down-generalized-linear-models-d9212526e51d&user=Egor+Howell&userId=1cac491223b2&source=-----d9212526e51d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd9212526e51d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbreaking-down-generalized-linear-models-d9212526e51d&source=-----d9212526e51d---------------------bookmark_footer-----------)![](../Images/d2ac6a93faf461a938eb43bc346402ef.png)

图片由 [Roman Mager](https://unsplash.com/@roman_lazygeek?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

[**线性回归**](https://en.wikipedia.org/wiki/Linear_regression) 迄今为止是我们在数据科学中学习的最常见算法。每个从业者都听说过并使用过它。然而，对于某些问题，它并不适用，我们需要对其进行“泛化”。这时，[**广义线性模型（GLMs）**](https://en.wikipedia.org/wiki/Generalized_linear_model) 便能提供更大的灵活性，成为数据科学家必知的重要工具。

# 什么是广义线性模型（GLM）？

正如我们上面所说的，广义线性模型（GLM）‘推广’了普通线性回归，但我们*真正*的意思是什么？

让我们考虑一个更简单的线性回归模型：

其中 ***β*** 是系数，***x*** 是解释变量，***ε*** 是[**正态分布的**](https://en.wikipedia.org/wiki/Normal_distribution)误差。

假设我们想建模一个保险公司在一个小时内接到多少次索赔电话。线性回归是否适合这个问题？

> 不！

原因如下：

+   线性回归假设误差是正态分布的，而正态分布可以取负值。然而，我们不能得到负的索赔电话。

+   第二点是正态…
