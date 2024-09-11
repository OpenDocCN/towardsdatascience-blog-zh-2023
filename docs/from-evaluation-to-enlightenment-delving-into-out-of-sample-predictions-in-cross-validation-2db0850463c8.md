# 从评估到启蒙：深入了解交叉验证中的样本外预测

> 原文：[https://towardsdatascience.com/from-evaluation-to-enlightenment-delving-into-out-of-sample-predictions-in-cross-validation-2db0850463c8?source=collection_archive---------7-----------------------#2023-06-28](https://towardsdatascience.com/from-evaluation-to-enlightenment-delving-into-out-of-sample-predictions-in-cross-validation-2db0850463c8?source=collection_archive---------7-----------------------#2023-06-28)

## 通过折叠外预测揭示洞察力并克服局限性。

[](https://medium.com/@ning.jia?source=post_page-----2db0850463c8--------------------------------)[![宁佳](../Images/46382d350365292176be9b59ebf3061f.png)](https://medium.com/@ning.jia?source=post_page-----2db0850463c8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2db0850463c8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2db0850463c8--------------------------------) [宁佳](https://medium.com/@ning.jia?source=post_page-----2db0850463c8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F731ca1da0bdd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-evaluation-to-enlightenment-delving-into-out-of-sample-predictions-in-cross-validation-2db0850463c8&user=Ning+Jia&userId=731ca1da0bdd&source=post_page-731ca1da0bdd----2db0850463c8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2db0850463c8--------------------------------) ·6分钟阅读·2023年6月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2db0850463c8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-evaluation-to-enlightenment-delving-into-out-of-sample-predictions-in-cross-validation-2db0850463c8&user=Ning+Jia&userId=731ca1da0bdd&source=-----2db0850463c8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2db0850463c8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-evaluation-to-enlightenment-delving-into-out-of-sample-predictions-in-cross-validation-2db0850463c8&source=-----2db0850463c8---------------------bookmark_footer-----------)

理解交叉验证并将其应用于实际工作是每个数据科学家必须掌握的技能。虽然交叉验证的主要目的是评估模型性能和调整超参数，但它还提供了额外的输出，值得关注。通过获取并组合每个折叠的预测，我们可以生成整个训练集的模型预测，通常称为**样本外或折叠外预测**。

不应忽视这些预测，因为它们包含了有关建模方法和数据集本身的宝贵信息。通过彻底探究这些预测，你可能会揭示出诸如模型为什么没有按预期工作、如何改进特征工程，以及数据中是否存在固有限制等问题的答案。

一般的方法很简单明了：调查模型高置信度但出错的样本。在这篇文章中，我将展示这些预测如何在三个实际项目中对我有所帮助。

## 发现数据限制

我参与了一个预测性维护项目，目标是提前预测车辆故障。我探索的一种方法是训练一个二分类器……
