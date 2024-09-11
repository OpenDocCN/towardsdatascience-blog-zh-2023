# 适用于机器学习工程师的Python设计模式：建造者

> 原文：[https://towardsdatascience.com/design-patterns-with-python-for-machine-learning-engineers-builder-45b8e749f134?source=collection_archive---------4-----------------------#2023-10-12](https://towardsdatascience.com/design-patterns-with-python-for-machine-learning-engineers-builder-45b8e749f134?source=collection_archive---------4-----------------------#2023-10-12)

![](../Images/99232beba3fbbac79e36336d6ca78b6d.png)

图片由 [Anton Maksimov 5642.su](https://unsplash.com/@juvnsky?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 了解如何使用建造者设计模式来增强你的代码

[](https://medium.com/@marcellopoliti?source=post_page-----45b8e749f134--------------------------------)[![Marcello Politi](../Images/484e44571bd2e75acfe5fef3146ab3c2.png)](https://medium.com/@marcellopoliti?source=post_page-----45b8e749f134--------------------------------)[](https://towardsdatascience.com/?source=post_page-----45b8e749f134--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----45b8e749f134--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----45b8e749f134--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7390355d40fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdesign-patterns-with-python-for-machine-learning-engineers-builder-45b8e749f134&user=Marcello+Politi&userId=7390355d40fe&source=post_page-7390355d40fe----45b8e749f134---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----45b8e749f134--------------------------------) ·6分钟阅读·2023年10月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F45b8e749f134&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdesign-patterns-with-python-for-machine-learning-engineers-builder-45b8e749f134&user=Marcello+Politi&userId=7390355d40fe&source=-----45b8e749f134---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F45b8e749f134&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdesign-patterns-with-python-for-machine-learning-engineers-builder-45b8e749f134&source=-----45b8e749f134---------------------bookmark_footer-----------)

# 介绍

对于从事AI开发的人来说，编写干净、可重用的代码是一项必备技能。因此，今天我将介绍如何使用 [Deepnote](https://deepnote.com/) 在Python中实现另一种设计模式。

无论你在深度学习、统计还是其他领域多么出色，**如果你的代码不干净且不易重用，你将永远无法开发出具有重大影响的东西**。这就是为什么我认为数据科学家具备软件工程技能非常重要的原因。[设计模式](https://en.wikipedia.org/wiki/Software_design_pattern)是所有编写代码的人都应该了解的内容。今天我们要讨论的模式叫做Builder。

## 什么是设计模式？

**设计模式只是针对重复问题的一种通用设计解决方案**。与其一遍又一遍地解决相同的问题，不如思考一个可以每次遇到相同问题时使用的解决方案，这些解决方案已经被发现了！有人幸运地已经想到让我们的生活变得更轻松！ :)

设计模式有多种类型。但主要有3种：
