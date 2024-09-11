# 从头开始使用Numpy构建卷积神经网络

> 原文：[https://towardsdatascience.com/build-a-convolutional-neural-network-from-scratch-using-numpy-139cbbf3c45e?source=collection_archive---------6-----------------------#2023-11-23](https://towardsdatascience.com/build-a-convolutional-neural-network-from-scratch-using-numpy-139cbbf3c45e?source=collection_archive---------6-----------------------#2023-11-23)

## 通过从头开始自己动手构建一个卷积神经网络（CNN），掌握计算机视觉

[](https://medium.com/@riccardo.andreoni?source=post_page-----139cbbf3c45e--------------------------------)[![Riccardo Andreoni](../Images/5e22581e419639b373019a809d6e65c1.png)](https://medium.com/@riccardo.andreoni?source=post_page-----139cbbf3c45e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----139cbbf3c45e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----139cbbf3c45e--------------------------------) [Riccardo Andreoni](https://medium.com/@riccardo.andreoni?source=post_page-----139cbbf3c45e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76784541161c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-convolutional-neural-network-from-scratch-using-numpy-139cbbf3c45e&user=Riccardo+Andreoni&userId=76784541161c&source=post_page-76784541161c----139cbbf3c45e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----139cbbf3c45e--------------------------------) ·8分钟阅读·2023年11月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F139cbbf3c45e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-convolutional-neural-network-from-scratch-using-numpy-139cbbf3c45e&user=Riccardo+Andreoni&userId=76784541161c&source=-----139cbbf3c45e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F139cbbf3c45e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-convolutional-neural-network-from-scratch-using-numpy-139cbbf3c45e&source=-----139cbbf3c45e---------------------bookmark_footer-----------)![](../Images/e4cdce3bc89847e967f252d44f3c1394.png)

这些彩色的窗户让我想起了CNN的层和它们的过滤器。图片来源：[unsplash.com](https://unsplash.com/photos/photo-of-green-leafed-plants-inside-building-CuEvrPd3NYc)。

由于**计算机视觉**应用现在无处不在，对于每一个数据科学从业者来说，**理解其工作原理**和**熟悉它们**是至关重要的。

在[这篇文章](/building-a-deep-neural-network-from-scratch-using-numpy-4f28a1df157a)中，我构建了一个**深度神经网络**，没有依赖像Tensorflow、Pytorch和Keras这样的现代流行深度学习库。我使用它对手写数字图像进行了分类。虽然实现的结果没有达到最先进的水平，但仍然令人满意。现在，我想迈出**进一步的一步**，仅使用Python库[**Numpy**](https://numpy.org/)来开发一个[**卷积神经网络**](https://en.wikipedia.org/wiki/Convolutional_neural_network)（CNN）。

像上述提到的Python深度学习库是**极其强大的工具**。然而，其缺点是它们掩盖了数据科学从业者对神经网络低级工作原理的理解。尤其是CNN，它们的**过程相较于传统的全连接网络更不直观**。解决这个问题的唯一方法是亲自动手实现CNN：这就是这个任务的动机。
