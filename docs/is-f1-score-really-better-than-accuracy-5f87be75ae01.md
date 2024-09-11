# F1-Score真的比准确率更好吗？

> 原文：[https://towardsdatascience.com/is-f1-score-really-better-than-accuracy-5f87be75ae01?source=collection_archive---------1-----------------------#2023-04-18](https://towardsdatascience.com/is-f1-score-really-better-than-accuracy-5f87be75ae01?source=collection_archive---------1-----------------------#2023-04-18)

## 根据不同指标，错误的代价（和正确的收益）是什么

[](https://medium.com/@mazzanti.sam?source=post_page-----5f87be75ae01--------------------------------)[![Samuele Mazzanti](../Images/432477d6418a3f79bf25dec42755d364.png)](https://medium.com/@mazzanti.sam?source=post_page-----5f87be75ae01--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5f87be75ae01--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5f87be75ae01--------------------------------) [Samuele Mazzanti](https://medium.com/@mazzanti.sam?source=post_page-----5f87be75ae01--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe16f3bb86e03&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-f1-score-really-better-than-accuracy-5f87be75ae01&user=Samuele+Mazzanti&userId=e16f3bb86e03&source=post_page-e16f3bb86e03----5f87be75ae01---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5f87be75ae01--------------------------------) ·10分钟阅读·2023年4月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5f87be75ae01&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-f1-score-really-better-than-accuracy-5f87be75ae01&user=Samuele+Mazzanti&userId=e16f3bb86e03&source=-----5f87be75ae01---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5f87be75ae01&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-f1-score-really-better-than-accuracy-5f87be75ae01&source=-----5f87be75ae01---------------------bookmark_footer-----------)![](../Images/a09fe8380a1e7a9f6979a654ee98ace5.png)

[作者图片]

如果你在谷歌上搜索“哪个指标更好，准确率还是F1-score？”你可能会找到类似的答案：

> “如果你的数据集是不平衡的，忘记准确率，选择F1-score。”

如果你寻找解释，你可能会发现对“假阴性和假阳性相关的不同成本”的模糊参考。但这些来源并没有明确说明不同指标中的这些成本是多少。

这就是我感到需要写这篇文章的原因：尝试回答以下问题。准确率中的假阴性和假阳性成本是多少？在F1-score中又是多少？是否有一个指标可以让我们为这些成本分配自定义值？

# 从混淆开始

准确率和 F1 分数都可以从所谓的**混淆矩阵**中计算得出。混淆矩阵是一个方阵，比较模型预测的标签与真实标签。

这是一个在两个标签情况下的混淆矩阵示例：0（又名负类）和 1（又名正类）。请注意，在本文中，我们将专注于二分类，但我们说的一切都可以…
