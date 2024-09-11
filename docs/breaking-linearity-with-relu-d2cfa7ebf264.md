# 使用 ReLU 打破线性

> 原文：[`towardsdatascience.com/breaking-linearity-with-relu-d2cfa7ebf264?source=collection_archive---------14-----------------------#2023-03-01`](https://towardsdatascience.com/breaking-linearity-with-relu-d2cfa7ebf264?source=collection_archive---------14-----------------------#2023-03-01)

## 解释 ReLU 激活函数如何以及为何是非线性的

[](https://medium.com/@egorhowell?source=post_page-----d2cfa7ebf264--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----d2cfa7ebf264--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d2cfa7ebf264--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d2cfa7ebf264--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----d2cfa7ebf264--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbreaking-linearity-with-relu-d2cfa7ebf264&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----d2cfa7ebf264---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d2cfa7ebf264--------------------------------) ·4 min read·2023 年 3 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd2cfa7ebf264&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbreaking-linearity-with-relu-d2cfa7ebf264&user=Egor+Howell&userId=1cac491223b2&source=-----d2cfa7ebf264---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd2cfa7ebf264&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbreaking-linearity-with-relu-d2cfa7ebf264&source=-----d2cfa7ebf264---------------------bookmark_footer-----------)![](img/e9eb0239dd7d7e0b58720e6193479978.png)

图片由 [Alina Grubnyak](https://unsplash.com/@alinnnaaaa?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 引言

[***神经网络***](https://en.wikipedia.org/wiki/Artificial_neural_network) 和 [***深度学习***](https://en.wikipedia.org/wiki/Deep_learning) 可以说是人们转向数据科学的最热门原因之一。然而，这种兴奋感可能会导致忽视神经网络的核心概念。在这篇文章中，我想讲解神经网络的一个可能最关键的特性，我认为大多数从业者应该了解，以便完全理解其内部运作。

# 为什么我们需要激活函数？

[***激活函数***](https://en.wikipedia.org/wiki/Activation_function)在数据科学和机器学习中无处不在。它们通常指的是对神经网络中神经元的[***线性***](https://en.wikipedia.org/wiki/Linear_equation)输入应用的变换***：***

其中***f***是激活函数，***y***是输出，***b***是偏置，***w_i***和***x_i***是[***权重***](https://en.wikipedia.org/wiki/Weighting)及其对应的特征值。

那么，我们为什么需要激活函数？

简单的答案是，它们使我们能够建模复杂的模式，方式是通过使神经网络变得[***非线性***](https://en.wikipedia.org/wiki/Nonlinear_system)。如果网络中没有非线性激活函数，整个模型就变成了[***线性回归***](https://en.wikipedia.org/wiki/Linear_regression)模型！

> 非线性是对输入的变化…
