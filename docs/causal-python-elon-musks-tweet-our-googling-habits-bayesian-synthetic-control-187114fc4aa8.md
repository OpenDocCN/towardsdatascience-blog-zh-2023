# 因果 Python — 埃隆·马斯克的推文、我们的搜索习惯和贝叶斯合成控制

> 原文：[https://towardsdatascience.com/causal-python-elon-musks-tweet-our-googling-habits-bayesian-synthetic-control-187114fc4aa8?source=collection_archive---------0-----------------------#2023-01-08](https://towardsdatascience.com/causal-python-elon-musks-tweet-our-googling-habits-bayesian-synthetic-control-187114fc4aa8?source=collection_archive---------0-----------------------#2023-01-08)

## 使用贝叶斯方法的合成控制量化推文的影响（使用 CausalPy）

[](https://aleksander-molak.medium.com/?source=post_page-----187114fc4aa8--------------------------------)[![Aleksander Molak](../Images/7fca5018f6ce88297fae31cef1fe0e6c.png)](https://aleksander-molak.medium.com/?source=post_page-----187114fc4aa8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----187114fc4aa8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----187114fc4aa8--------------------------------) [Aleksander Molak](https://aleksander-molak.medium.com/?source=post_page-----187114fc4aa8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff390f1bdd353&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcausal-python-elon-musks-tweet-our-googling-habits-bayesian-synthetic-control-187114fc4aa8&user=Aleksander+Molak&userId=f390f1bdd353&source=post_page-f390f1bdd353----187114fc4aa8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----187114fc4aa8--------------------------------) ·11分钟阅读·2023年1月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F187114fc4aa8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcausal-python-elon-musks-tweet-our-googling-habits-bayesian-synthetic-control-187114fc4aa8&user=Aleksander+Molak&userId=f390f1bdd353&source=-----187114fc4aa8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F187114fc4aa8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcausal-python-elon-musks-tweet-our-googling-habits-bayesian-synthetic-control-187114fc4aa8&source=-----187114fc4aa8---------------------bookmark_footer-----------)![](../Images/aeed401fdb4cd97fd78b62ea9d006cd6.png)

图片由 [Tolga Aslantürk](https://www.pexels.com/@tolgaaslanturk/) 提供，来源于 [Pexels](https://www.pexels.com/photo/a-bird-flying-in-the-sky-11017085/)

2022年10月，为旧金山的Twitter总部带来了许多新事物（还有一个水槽）。特斯拉和SpaceX的CEO埃隆·马斯克于10月27日成为公司的新老板和CEO。

一些观众热烈欢迎这一变化，而其他人则保持怀疑态度。

一天后，即10月28日，马斯克发推文“*鸟儿被解放了*”。

推特的影响力有多大？

一起来看看吧！

![](../Images/a6ce1b8901be7f13324a66b0f23244aa.png)

图片由 [劳拉·坦克雷迪](https://www.pexels.com/@laura-tancredi/) 提供，来源于 [Pexels](https://www.pexels.com/photo/curving-shaped-fragment-of-modern-building-7078717/)。

# 目标

在这篇博客文章中，我们将使用 [**CausalPy**](https://causalpy.readthedocs.io/en/latest/) —— 一个来自 [PyMC 开发者](https://medium.com/u/7c6b7b6803cd?source=post_page-----187114fc4aa8--------------------------------) ([https://www.pymc-labs.io](https://www.pymc-labs.io)) 的全新 Python 因果包来估计马斯克推特对我们搜索行为的影响，利用一种名为 **合成控制** 的强大因果技术。我们将讨论该方法的基本原理，逐步实现，并分析我们方法的潜在问题，同时提供相关资源的链接。
