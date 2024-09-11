# 反向传播：逐步推导

> 原文：[https://towardsdatascience.com/backpropagation-step-by-step-derivation-99ac8fbdcc28?source=collection_archive---------2-----------------------#2023-04-10](https://towardsdatascience.com/backpropagation-step-by-step-derivation-99ac8fbdcc28?source=collection_archive---------2-----------------------#2023-04-10)

## 完整指南：用于训练神经网络的算法

[](https://medium.com/@roiyeho?source=post_page-----99ac8fbdcc28--------------------------------)[![Dr. Roi Yehoshua](../Images/905a512ffc8879069403a87dbcbeb4db.png)](https://medium.com/@roiyeho?source=post_page-----99ac8fbdcc28--------------------------------)[](https://towardsdatascience.com/?source=post_page-----99ac8fbdcc28--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----99ac8fbdcc28--------------------------------) [Dr. Roi Yehoshua](https://medium.com/@roiyeho?source=post_page-----99ac8fbdcc28--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3886620c5cf9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbackpropagation-step-by-step-derivation-99ac8fbdcc28&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=post_page-3886620c5cf9----99ac8fbdcc28---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----99ac8fbdcc28--------------------------------) ·11分钟阅读·2023年4月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F99ac8fbdcc28&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbackpropagation-step-by-step-derivation-99ac8fbdcc28&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=-----99ac8fbdcc28---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F99ac8fbdcc28&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbackpropagation-step-by-step-derivation-99ac8fbdcc28&source=-----99ac8fbdcc28---------------------bookmark_footer-----------)![](../Images/7e828233e39b8fb6f6f3bf155056673e.png)

图片由[DeepMind](https://unsplash.com/@deepmind?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/photos/8heReYC6Zt0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在[上一篇文章](https://medium.com/@roiyeho/multi-layer-perceptrons-8d76972afa2b)中，我们讨论了多层感知器（MLPs）作为第一个能够解决非线性和复杂问题的神经网络模型。

长时间以来，如何在给定的数据集上训练这些网络并不明确。虽然单层感知机具有一个简单的学习规则，保证收敛到一个解决方案，但该规则无法扩展到多层网络。人工智能界在这个问题上挣扎了30多年（在一个被称为“人工智能寒冬”的时期），直到1986年Rumelhart等人发表了开创性的论文，引入了**反向传播算法**[1]。

在本文中，我们将详细讨论反向传播算法，并逐步推导其数学公式。由于这是用于训练各种神经网络（包括我们今天所拥有的深度网络）的主要算法，我认为了解该算法的细节对任何从事神经网络工作的人都会有所帮助。

尽管你可以在许多教科书和在线资源中找到对该算法的描述，但在撰写本文时，我尽量遵循以下原则：

+   使用清晰且一致的……
