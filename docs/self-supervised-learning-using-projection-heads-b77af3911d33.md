# 使用投影头的自监督学习

> 原文：[`towardsdatascience.com/self-supervised-learning-using-projection-heads-b77af3911d33?source=collection_archive---------7-----------------------#2023-06-29`](https://towardsdatascience.com/self-supervised-learning-using-projection-heads-b77af3911d33?source=collection_archive---------7-----------------------#2023-06-29)

## 使用未标记的数据提升性能

[](https://medium.com/@danielwarfield1?source=post_page-----b77af3911d33--------------------------------)![Daniel Warfield](https://medium.com/@danielwarfield1?source=post_page-----b77af3911d33--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b77af3911d33--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b77af3911d33--------------------------------) [Daniel Warfield](https://medium.com/@danielwarfield1?source=post_page-----b77af3911d33--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbdc4072cbfdc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fself-supervised-learning-using-projection-heads-b77af3911d33&user=Daniel+Warfield&userId=bdc4072cbfdc&source=post_page-bdc4072cbfdc----b77af3911d33---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b77af3911d33--------------------------------) ·13 min 阅读·2023 年 6 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb77af3911d33&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fself-supervised-learning-using-projection-heads-b77af3911d33&user=Daniel+Warfield&userId=bdc4072cbfdc&source=-----b77af3911d33---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb77af3911d33&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fself-supervised-learning-using-projection-heads-b77af3911d33&source=-----b77af3911d33---------------------bookmark_footer-----------)![](img/289279afe6fc5596e60b7f246b366c9d.png)

“自监督学习”由 Daniel Warfield 使用 p5.js 编写

在这篇文章中，你将了解自监督学习，它如何用于提升模型性能，以及投影头在自监督学习过程中的作用。我们将讨论直观理解、一些文献，以及 PyTorch 中的计算机视觉示例。

**谁适合阅读这篇文章？** 任何拥有未标记和可增强数据的人。

**这篇文章有多高级？** 这篇文章的开头对于初学者在概念上是容易理解的，但示例更侧重于中级和高级的数据科学家。

**前提条件：** 需要对卷积网络和全连接网络有较高的理解。

**代码：** 完整代码可以在[这里](https://github.com/DanielWarfield1/MLWritingAndResearch/blob/main/sslDemo.ipynb)找到。

# 自监督与其他方法

通常，当人们想到模型时，他们会考虑两大类：监督模型和无监督模型。

+   **监督学习**是基于**标记**信息训练模型的过程。例如，当训练一个模型来预测图像中是否包含猫或狗时，首先需要策划一组图像，并标记这些图像是否包含猫或狗，然后使用[梯度下降](https://medium.com/@danielwarfield1/what-are-gradients-and-why-do-they-explode-add23264d24b)的方法来训练模型，以理解包含猫和狗的图像之间的区别。
