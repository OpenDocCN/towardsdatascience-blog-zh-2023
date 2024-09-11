# 贝叶斯深度学习的温和介绍

> 原文：[https://towardsdatascience.com/a-gentle-introduction-to-bayesian-deep-learning-d298c7243fd6?source=collection_archive---------1-----------------------#2023-07-26](https://towardsdatascience.com/a-gentle-introduction-to-bayesian-deep-learning-d298c7243fd6?source=collection_archive---------1-----------------------#2023-07-26)

## 欢迎来到充满活力的概率编程世界！这篇文章是对该领域的温和介绍，你只需要对深度学习和贝叶斯统计有基本了解。

[](https://medium.com/@francoisporcher?source=post_page-----d298c7243fd6--------------------------------)[![François Porcher](../Images/9ddb233f8cadbd69026bd79e2bd62dea.png)](https://medium.com/@francoisporcher?source=post_page-----d298c7243fd6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d298c7243fd6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d298c7243fd6--------------------------------) [François Porcher](https://medium.com/@francoisporcher?source=post_page-----d298c7243fd6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e8e73046f53&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-gentle-introduction-to-bayesian-deep-learning-d298c7243fd6&user=Fran%C3%A7ois+Porcher&userId=8e8e73046f53&source=post_page-8e8e73046f53----d298c7243fd6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d298c7243fd6--------------------------------) ·8分钟阅读·2023年7月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd298c7243fd6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-gentle-introduction-to-bayesian-deep-learning-d298c7243fd6&user=Fran%C3%A7ois+Porcher&userId=8e8e73046f53&source=-----d298c7243fd6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd298c7243fd6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-gentle-introduction-to-bayesian-deep-learning-d298c7243fd6&source=-----d298c7243fd6---------------------bookmark_footer-----------)

到文章结束时，你应该对这个领域及其应用有一个基本的理解，并且了解它与传统深度学习方法的不同之处。

如果你像我一样听说过贝叶斯深度学习，并且猜测它涉及贝叶斯统计，但你不完全知道它是如何应用的，那么你来对地方了。

# 传统深度学习的局限性

传统深度学习的主要限制之一是，尽管它们是非常强大的工具，**但它们并不提供不确定性的度量**。

Chat GPT 可能会以明显的自信说出错误的信息。分类器的输出概率通常未经过校准。

**不确定性估计是决策过程中的一个关键方面，** 尤其是在医疗保健、自驾车等领域。我们希望模型能够在对脑癌患者的分类非常不确定时进行估计，在这种情况下我们需要医疗专家的进一步诊断。同样，我们希望自动驾驶汽车在识别到新环境时能够减速。
