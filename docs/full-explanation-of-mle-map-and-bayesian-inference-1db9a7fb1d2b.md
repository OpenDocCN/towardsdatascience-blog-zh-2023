# MLE、MAP 和贝叶斯推断的完整解释

> 原文：[`towardsdatascience.com/full-explanation-of-mle-map-and-bayesian-inference-1db9a7fb1d2b?source=collection_archive---------10-----------------------#2023-03-06`](https://towardsdatascience.com/full-explanation-of-mle-map-and-bayesian-inference-1db9a7fb1d2b?source=collection_archive---------10-----------------------#2023-03-06)

## 介绍最大似然估计、最大后验估计和贝叶斯推断

[](https://medium.com/@hrmnmichaels?source=post_page-----1db9a7fb1d2b--------------------------------)![Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----1db9a7fb1d2b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1db9a7fb1d2b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1db9a7fb1d2b--------------------------------) [Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----1db9a7fb1d2b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff2daf6260cca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffull-explanation-of-mle-map-and-bayesian-inference-1db9a7fb1d2b&user=Oliver+S&userId=f2daf6260cca&source=post_page-f2daf6260cca----1db9a7fb1d2b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1db9a7fb1d2b--------------------------------) · 13 分钟阅读 · 2023 年 3 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1db9a7fb1d2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffull-explanation-of-mle-map-and-bayesian-inference-1db9a7fb1d2b&user=Oliver+S&userId=f2daf6260cca&source=-----1db9a7fb1d2b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1db9a7fb1d2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffull-explanation-of-mle-map-and-bayesian-inference-1db9a7fb1d2b&source=-----1db9a7fb1d2b---------------------bookmark_footer-----------)![](img/ed3fd52dd4249d2f6376d0b4e133f357.png)

图片由 [fabio](https://unsplash.com/@fabioha?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/oyXis2kALVg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在这篇文章中，我们将介绍 *MLE（最大似然估计）*、*MAP（最大后验估计）* 和 *贝叶斯推断*——这些概念在统计学、数据科学和机器学习等领域中都是基础的。我们将通过对一个不公平硬币抛掷的例子来解释每种方法，进行解析和数值（对贝叶斯推断）推导，并展示它们的区别。

我们将学习到 MLE 最大化似然性——即选择最大化观察数据似然性的参数。MAP 增加了先验知识，给参数引入先验信息——从而将纯粹的频率学概念与贝叶斯概念连接起来（link）。贝叶斯推断最终能给我们提供最多的信息，但也是最难执行的：它涉及对给定数据的参数进行完整后验分布建模——而之前的方法仅提供点估计。

让我们通过形式化这些描述来设定舞台：从本质上讲，对于任何学习问题，我们都希望找到一个模型/参数，以尽可能好地描述观察数据。我们可以通过…完整描述和解决这个问题。
