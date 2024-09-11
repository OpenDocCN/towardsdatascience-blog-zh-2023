# 《虚假的先知：一个自制的时间序列回归模型》

> 原文：[`towardsdatascience.com/false-prophet-a-homemade-time-series-regression-model-54e296b99438?source=collection_archive---------1-----------------------#2023-10-31`](https://towardsdatascience.com/false-prophet-a-homemade-time-series-regression-model-54e296b99438?source=collection_archive---------1-----------------------#2023-10-31)

## 从 Meta 的 Prophet 中借鉴理念，构建强大的时间序列回归模型

[](https://bradley-stephen-shaw.medium.com/?source=post_page-----54e296b99438--------------------------------)![布拉德利·斯蒂芬·肖](https://bradley-stephen-shaw.medium.com/?source=post_page-----54e296b99438--------------------------------)[](https://towardsdatascience.com/?source=post_page-----54e296b99438--------------------------------)![数据科学之路](https://towardsdatascience.com/?source=post_page-----54e296b99438--------------------------------) [布拉德利·斯蒂芬·肖](https://bradley-stephen-shaw.medium.com/?source=post_page-----54e296b99438--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc5cd0a58b5ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffalse-prophet-a-homemade-time-series-regression-model-54e296b99438&user=Bradley+Stephen+Shaw&userId=c5cd0a58b5ae&source=post_page-c5cd0a58b5ae----54e296b99438---------------------post_header-----------) 发表在 [数据科学之路](https://towardsdatascience.com/?source=post_page-----54e296b99438--------------------------------) ·16 分钟阅读·2023 年 10 月 31 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F54e296b99438&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffalse-prophet-a-homemade-time-series-regression-model-54e296b99438&user=Bradley+Stephen+Shaw&userId=c5cd0a58b5ae&source=-----54e296b99438---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F54e296b99438&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffalse-prophet-a-homemade-time-series-regression-model-54e296b99438&source=-----54e296b99438---------------------bookmark_footer-----------)![](img/a3ee91d49478804f92ad7d17cb7c3c44.png)

照片由 [尼克拉斯·罗斯](https://unsplash.com/@blitzer?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇后续文章中，我继续我的使命，通过结合流行的 Prophet 包¹ 和 “用简单甚至线性模型取胜”² 的理念，构建 Frankenstein 的时间序列怪兽。

在我们回顾了自己的目标之后，我们将探讨回归模型——它是什么，以及它为何特别。

然后我们将进行超参数调优，使用时间序列交叉验证来获得一个“最佳”的模型参数化。

最后，我们将使用 SHAP 对模型进行验证，然后利用模型形式进行定制调查和手动调整。

这涉及的内容很多——让我们开始吧。

*另外：我们在之前的文章中已涵盖了数据准备和特征工程，因此这里直接进入建模部分。赶上之前的内容请看：*
