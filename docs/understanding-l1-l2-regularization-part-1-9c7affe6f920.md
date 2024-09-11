# 勇敢学习机器学习：揭示 L1 和 L2 正则化（第 1 部分）

> 原文：[https://towardsdatascience.com/understanding-l1-l2-regularization-part-1-9c7affe6f920?source=collection_archive---------5-----------------------#2023-11-22](https://towardsdatascience.com/understanding-l1-l2-regularization-part-1-9c7affe6f920?source=collection_archive---------5-----------------------#2023-11-22)

## 理解 L1 和 L2 正则化的基本目的

[](https://amyma101.medium.com/?source=post_page-----9c7affe6f920--------------------------------)[![艾米·马](../Images/2edf55456a1f92724535a1441fa2bef5.png)](https://amyma101.medium.com/?source=post_page-----9c7affe6f920--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9c7affe6f920--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9c7affe6f920--------------------------------) [艾米·马](https://amyma101.medium.com/?source=post_page-----9c7affe6f920--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd6d8df787b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-l1-l2-regularization-part-1-9c7affe6f920&user=Amy+Ma&userId=d6d8df787b&source=post_page-d6d8df787b----9c7affe6f920---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9c7affe6f920--------------------------------) · 6 分钟阅读 · 2023年11月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9c7affe6f920&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-l1-l2-regularization-part-1-9c7affe6f920&user=Amy+Ma&userId=d6d8df787b&source=-----9c7affe6f920---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9c7affe6f920&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-l1-l2-regularization-part-1-9c7affe6f920&source=-----9c7affe6f920---------------------bookmark_footer-----------)![](../Images/96d31baeaebab0907c5ebf3211eec9a3.png)

图片由 [霍莉·曼达里奇](https://unsplash.com/@hollymandarich?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

欢迎来到“[勇敢学习机器学习](towardsdatascience.com/tagged/courage-to-learn-ml)”，我们将从探索 L1 和 L2 正则化开始。本系列旨在简化复杂的机器学习概念，以轻松和信息丰富的对话呈现，类似于 “[勇敢做自己](https://www.goodreads.com/book/show/43306206-the-courage-to-be-disliked)” 的风格，但聚焦于机器学习。

这些问答环节反映了我个人的学习历程，我很高兴与你分享。把它看作是一个记录我深入机器学习的博客。你的互动——点赞、评论和关注——不仅仅是支持，它们还是推动这一系列内容继续下去和我分享过程的动力。

今天的讨论不仅仅是回顾 L1 和 L2 正则化的公式和性质。我们将深入探讨为什么在机器学习中使用这些方法的核心原因。如果你希望真正理解这些概念，你来对地方了，这里有一些启发性的见解！

在这篇文章中，我们将回答以下问题：

+   什么是正则化？我们为什么需要它？

+   什么是 L1 和 L2 正则化？

+   为什么我们更倾向于使用小系数而不是大系数？大系数如何导致模型复杂度增加？

+   为什么神经网络中存在多种权重和偏置组合？

+   为什么在 L1 和 L2 正则化中偏置项不受惩罚？

# 什么是正则化？我们为什么需要它？

正则化是机器学习中的一个基础技术，旨在**防止模型过拟合**。过拟合发生在一个模型，通常是过于复杂，不仅从训练数据中的潜在模式（信号）中学习，还会捕捉并放大噪声。这会导致模型在训练数据上表现良好，但在未见过的数据上表现较差。

# **什么是 L1 和 L2 正则化？**

有多种方法可以防止过拟合。L1 和 L2 正则化主要通过在模型的损失函数上添加惩罚项来解决过拟合问题。这种惩罚项阻止模型对任何单一特征（由大系数表示）赋予过多的重要性，从而简化模型。本质上，正则化保持模型平衡，专注于真正的信号，提高其对未见数据的泛化能力。

# 等等，我们为什么要对模型中的大权重施加惩罚？大系数如何导致模型复杂度增加？

虽然有很多组合可以最小化损失函数，但并非所有的组合都对泛化效果同样良好。大系数往往会放大数据中的有用信息（信号）和不必要的噪声。这种放大使得模型对输入中的小变化非常敏感，导致它过度强调噪声。因此，模型在新数据上的表现不佳。

相反，小系数有助于模型专注于数据中更重要、更广泛的模式，从而减少对细微波动的敏感性。这种方法促进了更好的平衡，使模型能更有效地泛化。

举个例子，假设一个神经网络被训练用来预测猫的体重。如果一个模型的系数为10，而另一个模型的系数大得多为1000，那么它们对下一层的输出将大相径庭——分别为300和30000。系数较大的模型更容易做出极端预测。在30磅作为异常值（对猫来说非常不寻常！）的情况下，第二个系数较大的模型会产生明显不准确的结果。这个例子说明了调节系数的重要性，以避免对数据中的异常值做出夸张的反应。

# **你能详细解释一下为什么神经网络中有多个权重和偏置组合吗？**

想象一下在神经网络损失函数的复杂地形中导航，你的任务是找到最低点，即‘最小值’。你可能会遇到以下情况：

![](../Images/fdca2f0a2e22f36e1a31b47f951957ab.png)

照片由[Tamas Tuzes-Katai](https://unsplash.com/@tamas_tuzeskatai?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上。

+   **多重目的地的风景**：当你穿越这片风景时，你会发现它充满了各种局部最小值，就像一个具有许多凹陷和山谷的非凸地形。这是因为具有多个隐藏层的神经网络的损失函数本质上是非凸的。每个局部最小值代表了不同的权重和偏置组合，提供了多种潜在的解决方案。

+   **多条路径到达同一目的地**：网络的非线性激活函数使其能够形成复杂的模式，近似数据的实际基础函数。通过多个这些函数的层，有许多方式来表示同一真相，每种方式由一组独特的权重和偏置特征。这就是网络设计中的冗余。

+   **序列的灵活性**：想象一下改变你的旅程顺序，比如交换骑车和乘公交车的顺序，但仍然到达同一目的地。与具有两个隐藏层的神经网络相关：如果你在第一层中将权重和偏置加倍，然后在第二层中将它们减半，最终输出保持不变。（请注意，这种灵活性主要适用于具有某些线性特征的激活函数，如ReLU，而不适用于sigmoid或tanh等其他函数）。这一现象被称为神经网络中的‘尺度对称’。

# 我一直在阅读L1和L2正则化，并观察到惩罚项主要集中在权重上，而不是偏置上。但这是为什么呢？难道偏置不是也可以被惩罚的系数吗？

![](../Images/be415f749b622557c43be559af9f0c16.png)

来源 — [http://laid.delanover.com/difference-between-l1-and-l2-regularization-implementation-and-visualization-in-tensorflow/](http://laid.delanover.com/difference-between-l1-and-l2-regularization-implementation-and-visualization-in-tensorflow/)

简而言之，像 L1 和 L2 这样的正则化技术的主要目标是通过调节模型权重的大小来防止过拟合（个人认为这就是我们称其为正则化的原因）。相比之下，偏差对模型复杂性的影响相对较小，因此通常不需要对其施加惩罚。

为了更好地理解，让我们来看一下权重和偏差的作用。权重决定了模型中每个特征的重要性，影响模型的复杂性以及在高维空间中的决策边界的形状。可以把它们看作是调整模型决策过程形状的旋钮，影响模型的复杂程度。

然而，偏差具有不同的作用。它们像线性函数中的截距一样，独立于输入特征来调整模型的输出。

这里是关键点：过拟合主要发生在特征之间的复杂相互作用中，而这些相互作用主要由权重处理。为了解决这个问题，我们对权重施加惩罚，调整每个特征的重要性以及模型从中提取的信息量。这反过来会重塑模型的格局，从而改变其复杂性。

相比之下，偏差对模型复杂性的影响不大。此外，偏差可以随着权重的变化而调整，从而减少了对单独偏差惩罚的需求。

现在你已经对权重和偏差的多个集合以及偏好较小权重的原因有了了解，我们可以深入探讨。

加入我在 [系列的第二部分](https://medium.com/@yujing-ma45/courage-to-learn-ml-unraveling-l1-l2-regularization-part-2-1bb171e43b35) 中，我将揭示 L1 和 L2 正则化背后的层次，并通过拉格朗日乘子提供直观的理解（不用担心名字，这个概念很简单 😃）

到时见！

如果你喜欢这篇文章，你可以在 [LinkedIn](https://www.linkedin.com/in/amyma101/) 上找到我，随时欢迎你联系我或者提出问题和建议！
