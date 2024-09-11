# 线性回归 — 预测机器学习建模的奥卡姆剃刀

> 原文：[https://towardsdatascience.com/linear-regression-occams-razor-of-predictive-machine-learning-modeling-f2ba5b144a2b?source=collection_archive---------16-----------------------#2023-01-03](https://towardsdatascience.com/linear-regression-occams-razor-of-predictive-machine-learning-modeling-f2ba5b144a2b?source=collection_archive---------16-----------------------#2023-01-03)

## 使用 Python 的线性回归进行机器学习建模

[![Farzad Mahmoodinobar](../Images/2d75209693b712300e6f0796bd2487d0.png)](https://medium.com/@fmnobar?source=post_page-----f2ba5b144a2b--------------------------------) [![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f2ba5b144a2b--------------------------------) [Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----f2ba5b144a2b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3c56b7d4893e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-regression-occams-razor-of-predictive-machine-learning-modeling-f2ba5b144a2b&user=Farzad+Mahmoodinobar&userId=3c56b7d4893e&source=post_page-3c56b7d4893e----f2ba5b144a2b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f2ba5b144a2b--------------------------------) ·16 分钟阅读·2023年1月3日

--

![](../Images/2494e60058d5a0eb46decef5f125cf20.png)

水晶球，来自 [DALL.E 2](https://openai.com/dall-e-2/)

你对奥卡姆剃刀（Occam’s Razor）熟悉吗？我记得在《生活大爆炸》电视系列中提到过它！奥卡姆剃刀的思想是，在其他条件相同的情况下，现象的最简单解释比复杂解释更可能是真的（即，最简单的解决方案几乎总是最好的解决方案）。我想认为机器学习中的预测建模的奥卡姆剃刀是线性回归，它几乎是最简单的建模方法，并且对于某些任务来说可能是最佳解决方案。这篇文章将介绍线性回归的基本概念和实现方法。

与我其他的帖子类似，通过练习题和答案来实现学习。根据需要，我会在问题中包含提示和解释，以便让学习过程更轻松。最后，我用来创建这个练习的笔记本也在帖子底部提供了链接，你可以下载、运行并跟随操作。

让我们开始吧！

*(除非另有说明，所有图片均由作者提供。)*
