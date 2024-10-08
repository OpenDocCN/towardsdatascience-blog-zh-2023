# 使用 Python 的 GANs 实践生成式 AI：DCGAN

> 原文：[`towardsdatascience.com/hands-on-generative-ai-with-gans-using-python-dcgan-6077f0067ac3`](https://towardsdatascience.com/hands-on-generative-ai-with-gans-using-python-dcgan-6077f0067ac3)

![](img/a663051d751ad0481e8cc825db37cbd7.png)

[Vinicius "amnx" Amano](https://unsplash.com/@viniciusamano?utm_source=medium&utm_medium=referral) 拍摄的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 使用卷积层在 PyTorch 中改进合成图像生成

[](https://medium.com/@marcellopoliti?source=post_page-----6077f0067ac3--------------------------------)![Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----6077f0067ac3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6077f0067ac3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6077f0067ac3--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----6077f0067ac3--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6077f0067ac3--------------------------------) ·5 分钟阅读·2023 年 4 月 4 日

--

## 介绍

在我的[上一篇文章](https://medium.com/towards-data-science/hands-on-generative-ai-with-gans-using-python-image-generation-9a62e591c7c6)中，我们已经看到了如何**使用 GANs 生成** MNIST 数据集类型的图像。我们取得了不错的结果，并成功实现了我们的目标。然而，这两个网络 G（生成器）和 D（判别器）主要由密集层组成。此时，你应该知道，通常在处理图像时我们使用 CNNs（卷积神经网络），因为它们使用卷积层。所以，让我们看看如何通过使用这些类型的层来**改进我们的 GANs**。使用**卷积层的 GANs 称为 DCGANs**。

## 什么是转置反卷积？

通常当我们使用 CNNs 时，我们习惯于使用卷积层。不过，在这种情况下，我们还需要“反向”操作，即转置反卷积，有时也称为反卷积。

**这个操作使我们能够对特征空间进行上采样**。例如，如果我们有一个 5x5 的网格表示的图像，我们可以将这个网格“放大”到 28x28。

原则上，你做的事情很简单，你**在初始特征图的元素内部填充零以扩大它，然后应用正常的卷积操作，使用特定的核大小、步幅和填充**。

例如，假设我们想将 5x5 的特征空间转换为 8x8。首先，通过插入零，我们创建一个 9x9 的特征空间，然后通过应用 2x2 的滤波器将其再次缩小到 8x8。让我们看一个图形示例。

![](img/535139b951ccdb3db31ae8af4dd2b93f.png)

转置卷积（图像由作者提供）

在这个网络中，我们还将使用批量归一化层，它们有助于解决内部协方差偏移问题。简而言之，它们的作用是在每一层之前对每个批次进行归一化，以便在训练过程中数据的分布没有变化。

## 生成器架构

生成器将由一系列转置卷积层构成，这些层将初始的随机向量 z 转换为我们想要生成的图像的正确尺寸，这里是 28x28。另一方面，特征图的深度将变得越来越小，不像卷积层那样。

![](img/fbb92b40136d3758154fc8afaa7c6474.png)

生成器架构（作者提供的图像）

## 判别器架构

另一方面，判别器是一个经典的 CNN 网络，负责对图像进行分类。因此，我们将有一系列卷积层，直到我们得到一个单一的数字，即输入是实际的还是假的概率。

![](img/606ce895f0fbd8d31fbe815fa531d85c.png)

判别器架构（作者提供的图像）

## 让我们开始编码吧！

我将使用[Deepnote](https://deepnote.com/)，但如果你愿意，你可以使用 Google Colab。

首先，检查你的硬件是否有可用的 GPU。

如果你在使用 Google Colab，你需要挂载你的驱动器。让我们也导入必要的库。

```py
from google.colab import drive
drive.mount('/content/drive/')
```

现在我们定义创建生成器 G 网络的函数，如我们之前所描述的。

要定义判别器 D，我们使用 Python 类，因为我们需要 forward 方法的输出。

现在我们终于可以实例化我们的 G 和 D 网络了。让我们还打印模型以查看层的摘要。

像往常一样，如果我们想进行网络训练，我们需要定义成本函数和优化器。

输入向量 z 是一个随机向量，取自某种分布，在我们的情况下可以是均匀分布或正态分布。

现在让我们定义判别器 D 的训练函数。正如我们在前一篇文章中所做的那样，**D 必须在真实图像和假图像上进行训练**。真实图像直接取自 MNIST 数据集，而对于假图像，我们会动态生成一个输入 z，将其传递给生成器 G，并获取 G 的输出。我们可以自己创建标签，知道真实图像的标签全为一，而假图像的标签全为零。**最终损失将是真实图像损失和假图像损失的总和**。

生成器以判别器的输出作为输入，因为它必须查看 D 是否识别出图像是假的还是实际的。并根据此计算其损失。

我们已经准备好导入数据集，这将使我们能够进行网络训练。使用 PyTorch 导入 MNIST 数据集非常容易，因为它已经实现了相关方法。

现在我们有了数据集，我们可以实例化数据加载器。

由于在训练结束时，我们希望了解图像生成如何随着时间的推移而改进，我们创建了一个函数，允许我们在每个周期生成和保存这些图像。

最后，我们准备开始训练。选择周期数，为了获得良好的结果，应该设定在大约 100 个周期。我只进行了 10 个周期，所以我将得到一个*“更丑”*的输出。

训练需要 100 个周期，大约需要一个小时，当然，这很大程度上取决于你所拥有的硬件。

让我们绘制结果，看看网络是否学会了如何生成这些合成图像。

![](img/7a8316c1412116c55fe95136c30337cc.png)

合成图像（作者提供的图像）

# 最终思考

在本文中，我们不仅使用了简单的 GAN 网络，还包括了在处理图像时非常有效的卷积操作，从而创建了所谓的 DCGAN。为了生成这些合成图像，我们构建了两个网络，一个生成器 G 和一个判别器 D，它们进行对抗游戏。如果这篇文章对你有帮助，关注我以获取我即将发布的关于生成网络的文章！ [😉](https://emojipedia.org/it/apple/ios-15.4/faccina-che-fa-l-occhiolino/)

# 结尾

*马切洛·波利提*

[领英](https://www.linkedin.com/in/marcello-politi/)，[推特](https://twitter.com/_March08_)，[网站](https://marcello-politi.super.site/)
