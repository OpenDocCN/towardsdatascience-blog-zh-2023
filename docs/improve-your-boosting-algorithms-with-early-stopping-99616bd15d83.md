# 改进您的提升算法，使用早期停止

> 原文：[`towardsdatascience.com/improve-your-boosting-algorithms-with-early-stopping-99616bd15d83?source=collection_archive---------15-----------------------#2023-05-15`](https://towardsdatascience.com/improve-your-boosting-algorithms-with-early-stopping-99616bd15d83?source=collection_archive---------15-----------------------#2023-05-15)

## 概述与 Python 实现

[](https://medium.com/@aashishnair?source=post_page-----99616bd15d83--------------------------------)![Aashish Nair](https://medium.com/@aashishnair?source=post_page-----99616bd15d83--------------------------------)[](https://towardsdatascience.com/?source=post_page-----99616bd15d83--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----99616bd15d83--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----99616bd15d83--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3087ba81e065&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimprove-your-boosting-algorithms-with-early-stopping-99616bd15d83&user=Aashish+Nair&userId=3087ba81e065&source=post_page-3087ba81e065----99616bd15d83---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----99616bd15d83--------------------------------) ·6 分钟阅读·2023 年 5 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F99616bd15d83&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimprove-your-boosting-algorithms-with-early-stopping-99616bd15d83&user=Aashish+Nair&userId=3087ba81e065&source=-----99616bd15d83---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F99616bd15d83&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimprove-your-boosting-algorithms-with-early-stopping-99616bd15d83&source=-----99616bd15d83---------------------bookmark_footer-----------)![](img/931b4e752e9becf6f9f8aadda8a47114.png)

图片由 [Glenn Carstens-Peters](https://unsplash.com/@glenncarstenspeters?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

提升算法在数据科学领域非常流行，这也是有道理的。包含提升的模型通常表现最佳，这也是它们在学术界和工业界都很常见的原因。

话虽如此，如果这些算法没有正确配置，它们会注册次优的结果。

一个常被忽视的特性是**早期停止**。

在这里，我们将概述早期停止的基本概念以及为什么它应该被纳入你的提升算法中。

## 提升方法回顾

在讨论早期停止之前，让我们简要讨论一下提升方法。

简而言之，利用提升的算法训练一系列顺序模型，每个模型都旨在解决其前身模型所犯的错误。

提升算法遵循以下步骤：

1.  使用初始权重训练一个弱模型

1.  评估第一个模型的“错误”

1.  训练一个使用修改后的权重的新模型，以解决前一个模型的问题

1.  评估这个新模型的“错误”
