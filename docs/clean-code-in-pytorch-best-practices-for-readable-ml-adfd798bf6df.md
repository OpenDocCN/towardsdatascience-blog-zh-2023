# 《PyTorch中的清晰代码：可读的机器学习最佳实践》

> 原文：[https://towardsdatascience.com/clean-code-in-pytorch-best-practices-for-readable-ml-adfd798bf6df?source=collection_archive---------6-----------------------#2023-03-17](https://towardsdatascience.com/clean-code-in-pytorch-best-practices-for-readable-ml-adfd798bf6df?source=collection_archive---------6-----------------------#2023-03-17)

![](../Images/bd25c3ef78df4d6a87b77701273f3c31.png)

图片由[Greyson Joralemon](https://unsplash.com/@greysonjoralemon?utm_source=medium&utm_medium=referral)提供，刊登在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 在PyTorch中编写清晰、高效且可读的代码的五个技巧

[](https://medium.com/@marcellopoliti?source=post_page-----adfd798bf6df--------------------------------)[![Marcello Politi](../Images/484e44571bd2e75acfe5fef3146ab3c2.png)](https://medium.com/@marcellopoliti?source=post_page-----adfd798bf6df--------------------------------)[](https://towardsdatascience.com/?source=post_page-----adfd798bf6df--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----adfd798bf6df--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----adfd798bf6df--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7390355d40fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclean-code-in-pytorch-best-practices-for-readable-ml-adfd798bf6df&user=Marcello+Politi&userId=7390355d40fe&source=post_page-7390355d40fe----adfd798bf6df---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----adfd798bf6df--------------------------------) · 5分钟阅读·2023年3月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fadfd798bf6df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclean-code-in-pytorch-best-practices-for-readable-ml-adfd798bf6df&user=Marcello+Politi&userId=7390355d40fe&source=-----adfd798bf6df---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fadfd798bf6df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclean-code-in-pytorch-best-practices-for-readable-ml-adfd798bf6df&source=-----adfd798bf6df---------------------bookmark_footer-----------)

## 引言

在数据科学和编程领域，编写易于阅读和维护的代码是非常重要的。你肯定也有过这样的经历：写出的代码看起来工作正常且相当清晰，但当你在一天或一周后重新阅读时，却变得难以理解。当你需要审查其他人编写的代码时，这种困难更为明显。

在人工智能中，编写清晰易懂的代码至关重要，因为我们经常需要设置多个实验，然后尝试多种模型、多组数据和大量超参数。

在这篇文章中，我与你分享了一些在使用 PyTorch 编程时我用的技巧，你可以立即应用，希望这些技巧能帮助你在作为数据科学家的工作中提高效率。

## 创建一个 DataModule 类来管理你的数据

在这个示例中，我将使用著名的 MNIST 数据集。虽然我使用的这些方法在这种情况下可能显得多余，因为各种库已经方便地支持了如此简单的……
