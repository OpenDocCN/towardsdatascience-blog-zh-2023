# 合成数据能提升机器学习性能吗？

> 原文：[`towardsdatascience.com/can-synthetic-data-boost-machine-learning-performance-6b4041e75dda?source=collection_archive---------10-----------------------#2023-07-05`](https://towardsdatascience.com/can-synthetic-data-boost-machine-learning-performance-6b4041e75dda?source=collection_archive---------10-----------------------#2023-07-05)

## 探索合成数据提升模型在不平衡数据集上的表现的能力

[](https://johnadeojo.medium.com/?source=post_page-----6b4041e75dda--------------------------------)![John Adeojo](https://johnadeojo.medium.com/?source=post_page-----6b4041e75dda--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6b4041e75dda--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6b4041e75dda--------------------------------) [John Adeojo](https://johnadeojo.medium.com/?source=post_page-----6b4041e75dda--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff933e1637e40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-synthetic-data-boost-machine-learning-performance-6b4041e75dda&user=John+Adeojo&userId=f933e1637e40&source=post_page-f933e1637e40----6b4041e75dda---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6b4041e75dda--------------------------------) ·7 分钟阅读·2023 年 7 月 5 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6b4041e75dda&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-synthetic-data-boost-machine-learning-performance-6b4041e75dda&user=John+Adeojo&userId=f933e1637e40&source=-----6b4041e75dda---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6b4041e75dda&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-synthetic-data-boost-machine-learning-performance-6b4041e75dda&source=-----6b4041e75dda---------------------bookmark_footer-----------)![](img/71bd354c415afacf5396ba2c0956b4fc.png)

作者图像：由 Midjourney 生成

# 背景 — 不平衡数据集

不平衡分类问题在商业机器学习应用中经常出现。你可能会在流失预测、欺诈检测、医疗诊断或垃圾邮件检测中遇到它们。在所有这些场景中，我们的目标是检测属于少数类的内容，这些内容在我们的数据中可能被严重低估。为提高模型在不平衡数据集上的表现，提出了几种方法：

+   **欠采样**：通过随机欠采样多数类来实现更平衡的训练数据集。

+   **过采样**：通过随机过采样少数类来获得平衡的训练数据集。

+   **加权损失**：根据少数类为损失函数分配权重。

+   **合成数据**：使用生成式人工智能创建高保真度的少数类合成数据样本。

在本文中，我展示了如何通过在合成数据上训练模型来超越其他方法，从而提升分类器的性能。
