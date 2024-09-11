# SHAP：用 Python 解释任何机器学习模型

> 原文：[`towardsdatascience.com/shap-explain-any-machine-learning-model-in-python-72f0bea35f7c?source=collection_archive---------2-----------------------#2023-01-11`](https://towardsdatascience.com/shap-explain-any-machine-learning-model-in-python-72f0bea35f7c?source=collection_archive---------2-----------------------#2023-01-11)

![](img/4b8cc3946e79898437b4c129844a6099.png)

图片由 [Priscilla Du Preez](https://unsplash.com/@priscilladupreez?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 你的 SHAP、TreeSHAP 和 DeepSHAP 综合指南

[](https://louis-chan.medium.com/?source=post_page-----72f0bea35f7c--------------------------------)![Louis Chan](https://louis-chan.medium.com/?source=post_page-----72f0bea35f7c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----72f0bea35f7c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----72f0bea35f7c--------------------------------) [Louis Chan](https://louis-chan.medium.com/?source=post_page-----72f0bea35f7c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6d585e26760a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshap-explain-any-machine-learning-model-in-python-72f0bea35f7c&user=Louis+Chan&userId=6d585e26760a&source=post_page-6d585e26760a----72f0bea35f7c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----72f0bea35f7c--------------------------------) ·13 分钟阅读·2023 年 1 月 11 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F72f0bea35f7c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshap-explain-any-machine-learning-model-in-python-72f0bea35f7c&user=Louis+Chan&userId=6d585e26760a&source=-----72f0bea35f7c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F72f0bea35f7c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshap-explain-any-machine-learning-model-in-python-72f0bea35f7c&source=-----72f0bea35f7c---------------------bookmark_footer-----------)

## 动机

故事时间！

想象一下，你已经训练了一个机器学习模型来预测抵押贷款申请者的违约风险。一切都很顺利，性能也非常出色。但这个模型是如何工作的呢？模型是如何得出预测值的？

我们站在那里说，模型考虑了多个变量，且多维关系和模式过于复杂，无法用简单的语言解释。

这就是模型解释性可能发挥作用的地方。在那些可以解析机器学习模型的算法中，SHAP 是领域内更为中立的参与者之一。在这篇博客中，我们将深入探讨以下内容：

+   什么是 Shapley 值？

+   如何计算它们？

+   如何在 Python 中使用它？

+   SHAP 如何支持局部和全局解释性？

+   SHAP 库中有哪些可视化功能？

+   SHAP 的常见变体是如何工作的？— TreeSHAP 和 DeepSHAP

+   LIME 与 SHAP 的比较如何？

# Shapley 值
