# 学习机器学习的勇气：揭开 L1 和 L2 正则化的面纱（第二部分）

> 原文：[`towardsdatascience.com/courage-to-learn-ml-unraveling-l1-l2-regularization-part-2-1bb171e43b35?source=collection_archive---------6-----------------------#2023-11-25`](https://towardsdatascience.com/courage-to-learn-ml-unraveling-l1-l2-regularization-part-2-1bb171e43b35?source=collection_archive---------6-----------------------#2023-11-25)

## 解锁 L1 稀疏性的直觉与拉格朗日乘子

[](https://amyma101.medium.com/?source=post_page-----1bb171e43b35--------------------------------)![Amy Ma](https://amyma101.medium.com/?source=post_page-----1bb171e43b35--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1bb171e43b35--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1bb171e43b35--------------------------------) [艾米·马](https://amyma101.medium.com/?source=post_page-----1bb171e43b35--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd6d8df787b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcourage-to-learn-ml-unraveling-l1-l2-regularization-part-2-1bb171e43b35&user=Amy+Ma&userId=d6d8df787b&source=post_page-d6d8df787b----1bb171e43b35---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1bb171e43b35--------------------------------) ·6 分钟阅读·2023 年 11 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1bb171e43b35&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcourage-to-learn-ml-unraveling-l1-l2-regularization-part-2-1bb171e43b35&user=Amy+Ma&userId=d6d8df787b&source=-----1bb171e43b35---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1bb171e43b35&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcourage-to-learn-ml-unraveling-l1-l2-regularization-part-2-1bb171e43b35&source=-----1bb171e43b35---------------------bookmark_footer-----------)

欢迎回到《学习机器学习的勇气: 揭开 L1 和 L2 正则化的面纱》第二部分。在[我们之前的讨论](https://medium.com/@yujing-ma45/understanding-l1-l2-regularization-part-1-9c7affe6f920)中，我们探讨了较小系数的好处以及通过权重惩罚技术来实现这些系数的方法。在这次跟进中，我们的导师和学习者将**深入**探讨 L1 和 L2 正则化的领域。

如果你曾在思考这些问题，那么你来对地方了：

+   L1 和 L2 正则化命名的原因是什么？

+   我们如何解读经典的 L1 和 L2 正则化图？

+   Lagrange 乘数是什么，我们如何直观理解它们？

+   应用 Lagrange 乘数理解 L1 稀疏性。

你们的参与 — 点赞、评论和关注 — 不仅仅是提升士气，更是推动我们探索之旅的动力！所以，让我们深入探讨吧。

![](img/eb38cadf6bb97d614c0c0bc8ddd9755a.png)

照片由[Aarón Blanco Tejedor](https://unsplash.com/@the_meaning_of_love?utm_source=medium&utm_medium=referral)拍摄，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 为什么称之为 L1、L2 正则化？

名称，L1 和 L2 正则化，直接来自 Lp 范数的概念。Lp 范数表示从一个点到另一点的距离的不同计算方式…
