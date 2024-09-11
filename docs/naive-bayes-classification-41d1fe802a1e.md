# 朴素贝叶斯分类

> 原文：[https://towardsdatascience.com/naive-bayes-classification-41d1fe802a1e?source=collection_archive---------3-----------------------#2023-06-01](https://towardsdatascience.com/naive-bayes-classification-41d1fe802a1e?source=collection_archive---------3-----------------------#2023-06-01)

## 对朴素贝叶斯分类器系列的深入解释，包括在 Python 中的文本分类示例

[](https://medium.com/@roiyeho?source=post_page-----41d1fe802a1e--------------------------------)[![Roi Yehoshua 博士](../Images/905a512ffc8879069403a87dbcbeb4db.png)](https://medium.com/@roiyeho?source=post_page-----41d1fe802a1e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----41d1fe802a1e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----41d1fe802a1e--------------------------------) [Roi Yehoshua 博士](https://medium.com/@roiyeho?source=post_page-----41d1fe802a1e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3886620c5cf9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnaive-bayes-classification-41d1fe802a1e&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=post_page-3886620c5cf9----41d1fe802a1e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----41d1fe802a1e--------------------------------) ·23分钟阅读·2023年6月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F41d1fe802a1e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnaive-bayes-classification-41d1fe802a1e&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=-----41d1fe802a1e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F41d1fe802a1e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnaive-bayes-classification-41d1fe802a1e&source=-----41d1fe802a1e---------------------bookmark_footer-----------)![](../Images/6bacbaca2c4e4c42e4040132ae1c3102.png)

图片由 [Mediocre Studio](https://unsplash.com/@paucasals?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，拍摄于 [Unsplash](https://unsplash.com/photos/1Gvog1VdtDA?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

朴素贝叶斯分类器是一类基于贝叶斯定理，并对特征之间的独立性做出朴素假设的概率分类器。

这些分类器在训练和预测方面都极其快速，同时也具有很高的可扩展性和可解释性。尽管它们的假设过于简化，但在复杂的现实世界问题中通常表现良好，特别是在文本分类任务中，如垃圾邮件过滤和情感分析，其天真的假设往往能够很好地成立。

朴素贝叶斯也是最早的**生成模型**之一（早在ChatGPT之前…），它学习每个类别中输入的分布。这些模型不仅可以用于预测，还可以用于生成新样本（有关生成模型与判别模型的深入讨论，请参见[这篇文章](https://medium.com/@roiyeho/generative-vs-discriminative-models-35b81f677822)）。

在本文中，我们将深入探讨朴素贝叶斯模型及其变体，并展示如何在Scikit-Learn中使用其实现来解决文档分类任务。

# 背景：贝叶斯定理
