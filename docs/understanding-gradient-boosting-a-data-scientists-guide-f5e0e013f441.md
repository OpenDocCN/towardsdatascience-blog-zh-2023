# 理解梯度提升：数据科学家的指南

> 原文：[`towardsdatascience.com/understanding-gradient-boosting-a-data-scientists-guide-f5e0e013f441?source=collection_archive---------3-----------------------#2023-02-07`](https://towardsdatascience.com/understanding-gradient-boosting-a-data-scientists-guide-f5e0e013f441?source=collection_archive---------3-----------------------#2023-02-07)

![](img/ddead02c0234cca8db16b03529386445.png)

图片来源：Midjourney

[](https://louis-chan.medium.com/?source=post_page-----f5e0e013f441--------------------------------)![Louis Chan](https://louis-chan.medium.com/?source=post_page-----f5e0e013f441--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f5e0e013f441--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f5e0e013f441--------------------------------) [Louis Chan](https://louis-chan.medium.com/?source=post_page-----f5e0e013f441--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6d585e26760a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-gradient-boosting-a-data-scientists-guide-f5e0e013f441&user=Louis+Chan&userId=6d585e26760a&source=post_page-6d585e26760a----f5e0e013f441---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f5e0e013f441--------------------------------) ·10 min 阅读·2023 年 2 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff5e0e013f441&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-gradient-boosting-a-data-scientists-guide-f5e0e013f441&user=Louis+Chan&userId=6d585e26760a&source=-----f5e0e013f441---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff5e0e013f441&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-gradient-boosting-a-data-scientists-guide-f5e0e013f441&source=-----f5e0e013f441---------------------bookmark_footer-----------)

梯度提升机（GBM）是机器学习和数据科学领域最重要的进展之一，它使我们这些从业者能够利用模型集成来解决许多特定领域的问题。虽然这个工具在像`scikit-learn`和`xgboost`这样的 Python 包中广泛可用，但作为数据科学家，我们应该始终研究模型的理论和数学，而不是仅仅将其作为黑箱使用。在这篇博客中，我们将深入探讨以下几个方面：

+   GBM 的不同支持概念

+   分步图解 GBM 的重建

+   优势与劣势

# 梯度提升的基础知识

## 1\. 弱学习器和集成学习

弱学习者和集成学习是使梯度提升有效的两个关键概念。弱学习者是指那些仅比随机猜测稍微好一点的模型。将多个弱学习者结合起来，它们可以形成一个强大的集成模型，以做出准确的预测。

> 太啰嗦，太复杂了

好吧，想象一下我们和另外两个朋友一起玩 10,000 片的拼图。（他们一定是非常好的朋友才会愿意参与这个活动）我们每个人负责拼接其中的一部分……
