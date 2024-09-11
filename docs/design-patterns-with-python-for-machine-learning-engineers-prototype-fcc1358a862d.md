# 《面向机器学习工程师的 Python 设计模式：原型模式》

> 原文：[https://towardsdatascience.com/design-patterns-with-python-for-machine-learning-engineers-prototype-fcc1358a862d?source=collection_archive---------2-----------------------#2023-12-05](https://towardsdatascience.com/design-patterns-with-python-for-machine-learning-engineers-prototype-fcc1358a862d?source=collection_archive---------2-----------------------#2023-12-05)

![](../Images/3f4030d397da6e9736107739793eda2b.png)

图片由 [Robert Katzki](https://unsplash.com/@ro_ka?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 学习如何使用原型设计模式来提升你的代码

[](https://medium.com/@marcellopoliti?source=post_page-----fcc1358a862d--------------------------------)[![Marcello Politi](../Images/484e44571bd2e75acfe5fef3146ab3c2.png)](https://medium.com/@marcellopoliti?source=post_page-----fcc1358a862d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fcc1358a862d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fcc1358a862d--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----fcc1358a862d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7390355d40fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdesign-patterns-with-python-for-machine-learning-engineers-prototype-fcc1358a862d&user=Marcello+Politi&userId=7390355d40fe&source=post_page-7390355d40fe----fcc1358a862d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fcc1358a862d--------------------------------) ·6 min read·2023年12月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffcc1358a862d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdesign-patterns-with-python-for-machine-learning-engineers-prototype-fcc1358a862d&user=Marcello+Politi&userId=7390355d40fe&source=-----fcc1358a862d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffcc1358a862d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdesign-patterns-with-python-for-machine-learning-engineers-prototype-fcc1358a862d&source=-----fcc1358a862d---------------------bookmark_footer-----------)

## 介绍

这不是我第一次写关于设计模式的博客。在我最近的帖子中，我收到了有关这个话题的积极反馈，因为显然**在 Python 世界中使用设计模式并不常见**。我认为人们应该学习这些模式，以增强和改进他们的代码。此外，如今的 AI 软件 heavily 基于 Python，因此我认为这些教程对所有从事 AI 工作的人都很有用。我将我的代码运行在[Deepnote](https://deepnote.com/)上：这是一个基于云的笔记本，非常适合协作数据科学项目。

## 什么是设计模式？

设计模式为在软件设计中经常出现的问题提供了明确定义的解决方案。这些模式提供了可重用的解决方案，而不是一次次解决同样的问题，从而加速了整个开发过程。

设计模式本质上提供了一个经过验证的蓝图，以最佳方式解决特定问题，从而使我们的生活更轻松。

设计模式有多种类型，通常分为三组：
