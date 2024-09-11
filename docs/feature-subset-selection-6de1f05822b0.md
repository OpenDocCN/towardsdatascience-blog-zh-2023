# 特征子集选择

> 原文：[https://towardsdatascience.com/feature-subset-selection-6de1f05822b0?source=collection_archive---------4-----------------------#2023-03-22](https://towardsdatascience.com/feature-subset-selection-6de1f05822b0?source=collection_archive---------4-----------------------#2023-03-22)

## 一个关于特征选择的教程和推荐策略

[](https://medium.com/@PadraigC?source=post_page-----6de1f05822b0--------------------------------)[![Pádraig Cunningham](../Images/9521fb3dc64c947edf6adf2ce7b80f0f.png)](https://medium.com/@PadraigC?source=post_page-----6de1f05822b0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6de1f05822b0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6de1f05822b0--------------------------------) [Pádraig Cunningham](https://medium.com/@PadraigC?source=post_page-----6de1f05822b0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F52562b8f71f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffeature-subset-selection-6de1f05822b0&user=P%C3%A1draig+Cunningham&userId=52562b8f71f9&source=post_page-52562b8f71f9----6de1f05822b0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6de1f05822b0--------------------------------) · 16 分钟阅读 · 2023年3月22日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6de1f05822b0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffeature-subset-selection-6de1f05822b0&user=P%C3%A1draig+Cunningham&userId=52562b8f71f9&source=-----6de1f05822b0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6de1f05822b0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffeature-subset-selection-6de1f05822b0&source=-----6de1f05822b0---------------------bookmark_footer-----------)![](../Images/eea3aed19636e0bc3cd8b85ca6a2f878.png)

图片由 [gokhan polat](https://unsplash.com/@go_pol?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/qyC7DTbWJJk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# TL;DR

特征子集选择在监督学习中非常重要，不仅因为它可以得到更好的模型，还因为它提供了深入的见解。这在当前机器学习（ML）强调可解释性的背景下尤为重要。

对于从业者来说，挑战在于有大量的特征选择方法可供选择。在这篇文章中，我简要概述了这一领域，并提出了一种在大多数情况下都有效的策略。该策略使用*Wrapper*作为最终选择过程的工具，并在必要时使用*permutation importance*作为初步筛选。

## 介绍

在数据分析中，使用多个特征描述的对象有时可以仅用这些特征的子集来描述而不会丢失信息。识别这些特征子集的过程称为特征选择、变量选择或特征子集选择，是数据分析中的一个关键过程。这篇文章简要概述了特征子集选择（FSS）方法，并提出了一种在大多数场景中都有效的策略。本文基于一篇可在[arXiv](https://arxiv.org/abs/2106.06437)上找到的教程论文[1]。那篇论文中介绍的方法的Python代码可在[Github](https://github.com/PadraigC/FeatSelTutorial)上找到。
