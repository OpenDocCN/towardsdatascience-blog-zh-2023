# 在处理机器学习模型之前要问自己的4个问题

> 原文：[https://towardsdatascience.com/4-questions-to-ask-yourself-before-working-on-a-machine-learning-model-d0f9fdc2fbb2?source=collection_archive---------20-----------------------#2023-04-11](https://towardsdatascience.com/4-questions-to-ask-yourself-before-working-on-a-machine-learning-model-d0f9fdc2fbb2?source=collection_archive---------20-----------------------#2023-04-11)

## 你真的需要使用它吗？

[](https://sonery.medium.com/?source=post_page-----d0f9fdc2fbb2--------------------------------)[![Soner Yıldırım](../Images/c589572e9d1ee176cd4f5a0008173f1b.png)](https://sonery.medium.com/?source=post_page-----d0f9fdc2fbb2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d0f9fdc2fbb2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d0f9fdc2fbb2--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----d0f9fdc2fbb2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-questions-to-ask-yourself-before-working-on-a-machine-learning-model-d0f9fdc2fbb2&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----d0f9fdc2fbb2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d0f9fdc2fbb2--------------------------------) ·4 分钟阅读·2023年4月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd0f9fdc2fbb2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-questions-to-ask-yourself-before-working-on-a-machine-learning-model-d0f9fdc2fbb2&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=-----d0f9fdc2fbb2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd0f9fdc2fbb2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-questions-to-ask-yourself-before-working-on-a-machine-learning-model-d0f9fdc2fbb2&source=-----d0f9fdc2fbb2---------------------bookmark_footer-----------)![](../Images/2acee73c46470784e1400ed88bbbe08b.png)

照片由 [Ana Municio](https://unsplash.com/es/@lamunix?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/PbzntH58GLQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

机器学习（ML）不是一根可以用来解决所有问题的魔法棒，也不能期望它能提供可靠的解决方案。

机器学习（ML）在准确性和速度上的进展使我们明显倾向于将任何问题都用ML解决方案来处理。

这种心态是危险的，可能会产生前所未有的结果。我可以向你保证，你不希望在生产环境中得到前所未有的结果。

在这篇文章中，我们将探讨在将机器学习作为解决方案之前需要解决的4个问题。

## 1\. 你是否拥有与希望进行预测的数据具有相似特征或模式的训练数据？

机器学习不是魔法。它不能在未经过训练之前产生结果。因此，任何机器学习系统的首要要求是数据。在考虑是否可以将机器学习应用于某个问题之前，我们需要确保我们可以访问数据。

但是，我们用于训练模型的数据需要具有与我们希望预测的数据相似的模式。
