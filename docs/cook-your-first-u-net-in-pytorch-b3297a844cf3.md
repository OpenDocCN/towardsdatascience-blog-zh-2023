# 在 PyTorch 中制作你的第一个 U-Net

> 原文：[`towardsdatascience.com/cook-your-first-u-net-in-pytorch-b3297a844cf3`](https://towardsdatascience.com/cook-your-first-u-net-in-pytorch-b3297a844cf3)

## 一份提升你图像分割项目的魔法配方

[](https://mostafawael.medium.com/?source=post_page-----b3297a844cf3--------------------------------)![Mostafa Wael](https://mostafawael.medium.com/?source=post_page-----b3297a844cf3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b3297a844cf3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b3297a844cf3--------------------------------) [Mostafa Wael](https://mostafawael.medium.com/?source=post_page-----b3297a844cf3--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b3297a844cf3--------------------------------) ·6 分钟阅读·2023 年 5 月 12 日

--

![](img/c1de747dfac6df8728f9837b2c6bed9d.png)

照片由 [Stefan C. Asafti](https://unsplash.com/@stefanasafti?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/x5jilo3ck3o?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上提供

U-Net 是一种用于图像分析中语义分割任务的深度学习架构。它由 Olaf Ronneberger、Philipp Fischer 和 Thomas Brox 在题为 “[U-Net: Convolutional Networks for Biomedical Image Segmentation](https://papers.labml.ai/paper/2e48c3ffdc8311eba3db37f65e372566)” 的论文中提出。

它对生物医学图像分割任务特别有效，因为它能够处理任意大小的图像，并生成平滑、高质量的分割掩膜和清晰的物体边界。此后，它已广泛应用于许多其他图像分割任务，如卫星和航拍图像分析，以及自然图像分割。

在本教程中，我们将深入了解 U-Net 及其工作原理，并用 PyTorch 制作我们自己的实现配方。那么，开始吧！

# 它是如何工作的？

U-Net 架构由两部分组成：编码器和解码器。

![](img/302f2b53c15c476ff36507d93dc9f5e3.png)

[U-Net: Convolutional Networks for Biomedical Image Segmentation](https://paperswithcode.com/paper/u-net-convolutional-networks-for-biomedical)

## 编码器（收缩路径）

编码器是一系列**卷积**和**池化**层，这些层逐步**下采样**输入图像，以在多个尺度上提取特征。

在编码器中，图像的大小逐渐减少，而深度逐渐增加。这基本上意味着网络**学习了图像中的“WHAT”**信息，但**丧失了“WHERE”**信息。

## 解码器（扩展路径）

解码器由一系列 **卷积** 和 **上采样** 层组成，这些层将特征图上采样到原始输入图像的大小，同时结合来自编码器的高分辨率特征。这使得解码器能够生成与原始输入图像大小相同的分割掩膜。

> 你可以从这篇精彩的 文章 中了解更多关于上采样和转置卷积的内容。

在解码器中，图像的大小逐渐增加，而深度逐渐减少。这基本上意味着网络通过逐渐应用上采样来 **学习图像中的“WHERE”** 信息。

## 最终层

在最终层，使用 1x1 卷积将每个 64 维特征向量映射到所需的类别数。

# 我们的烹饪食谱！

我们将进行一个非常直接的实现，编码时最好将上述图片放在你面前。

## 导入

首先，从 `torch` 和 `torchvision` 包中导入必要的模块，包括用于构建神经网络的 `nn` 模块和 `torchvision.models` 提供的预训练模型。 `relu` 函数也从 `torch.nn.functional` 导入。

```py
import torch
import torch.nn as nn
from torchvision import models
from torch.nn.functional import relu
```

## UNet 类

然后，定义了一个自定义类 `UNet`，作为 `nn.Module` 的子类。 `__init__` 方法通过定义编码器和解码器部分的层来初始化 U-Net 的架构。参数 `n_class` 指定了分割任务的类别数。

```py
class UNet(nn.Module):
    def __init__(self, n_class):
        super().__init__()

        # Encoder
        # In the encoder, convolutional layers with the Conv2d function are used to extract features from the input image. 
        # Each block in the encoder consists of two convolutional layers followed by a max-pooling layer, with the exception of the last block which does not include a max-pooling layer.
        # -------
        # input: 572x572x3
        self.e11 = nn.Conv2d(3, 64, kernel_size=3, padding=1) # output: 570x570x64
        self.e12 = nn.Conv2d(64, 64, kernel_size=3, padding=1) # output: 568x568x64
        self.pool1 = nn.MaxPool2d(kernel_size=2, stride=2) # output: 284x284x64

        # input: 284x284x64
        self.e21 = nn.Conv2d(64, 128, kernel_size=3, padding=1) # output: 282x282x128
        self.e22 = nn.Conv2d(128, 128, kernel_size=3, padding=1) # output: 280x280x128
        self.pool2 = nn.MaxPool2d(kernel_size=2, stride=2) # output: 140x140x128

        # input: 140x140x128
        self.e31 = nn.Conv2d(128, 256, kernel_size=3, padding=1) # output: 138x138x256
        self.e32 = nn.Conv2d(256, 256, kernel_size=3, padding=1) # output: 136x136x256
        self.pool3 = nn.MaxPool2d(kernel_size=2, stride=2) # output: 68x68x256

        # input: 68x68x256
        self.e41 = nn.Conv2d(256, 512, kernel_size=3, padding=1) # output: 66x66x512
        self.e42 = nn.Conv2d(512, 512, kernel_size=3, padding=1) # output: 64x64x512
        self.pool4 = nn.MaxPool2d(kernel_size=2, stride=2) # output: 32x32x512

        # input: 32x32x512
        self.e51 = nn.Conv2d(512, 1024, kernel_size=3, padding=1) # output: 30x30x1024
        self.e52 = nn.Conv2d(1024, 1024, kernel_size=3, padding=1) # output: 28x28x1024

        # Decoder
        self.upconv1 = nn.ConvTranspose2d(1024, 512, kernel_size=2, stride=2)
        self.d11 = nn.Conv2d(1024, 512, kernel_size=3, padding=1)
        self.d12 = nn.Conv2d(512, 512, kernel_size=3, padding=1)

        self.upconv2 = nn.ConvTranspose2d(512, 256, kernel_size=2, stride=2)
        self.d21 = nn.Conv2d(512, 256, kernel_size=3, padding=1)
        self.d22 = nn.Conv2d(256, 256, kernel_size=3, padding=1)

        self.upconv3 = nn.ConvTranspose2d(256, 128, kernel_size=2, stride=2)
        self.d31 = nn.Conv2d(256, 128, kernel_size=3, padding=1)
        self.d32 = nn.Conv2d(128, 128, kernel_size=3, padding=1)

        self.upconv4 = nn.ConvTranspose2d(128, 64, kernel_size=2, stride=2)
        self.d41 = nn.Conv2d(128, 64, kernel_size=3, padding=1)
        self.d42 = nn.Conv2d(64, 64, kernel_size=3, padding=1)

        # Output layer
        self.outconv = nn.Conv2d(64, n_class, kernel_size=1)
```

> 在 U-Net 论文中，他们使用了 0 填充，并应用了后处理技术以恢复图像的原始大小，而在这里，我们使用 1 填充，这样最终的特征图不会被裁剪，也不需要对输出图像进行后处理。

## 前向方法

`forward` 方法指定了输入如何通过网络进行处理。输入图像首先通过编码器层提取特征。然后，解码器层用于将特征上采样到原始图像大小，同时连接相应的编码器特征图。最后，输出层使用 1x1 卷积层将特征映射到所需的输出类别数。

```py
 def forward(self, x):
        # Encoder
        xe11 = relu(self.e11(x))
        xe12 = relu(self.e12(xe11))
        xp1 = self.pool1(xe12)

        xe21 = relu(self.e21(xp1))
        xe22 = relu(self.e22(xe21))
        xp2 = self.pool2(xe22)

        xe31 = relu(self.e31(xp2))
        xe32 = relu(self.e32(xe31))
        xp3 = self.pool3(xe32)

        xe41 = relu(self.e41(xp3))
        xe42 = relu(self.e42(xe41))
        xp4 = self.pool4(xe42)

        xe51 = relu(self.e51(xp4))
        xe52 = relu(self.e52(xe51))

        # Decoder
        xu1 = self.upconv1(xe52)
        xu11 = torch.cat([xu1, xe42], dim=1)
        xd11 = relu(self.d11(xu11))
        xd12 = relu(self.d12(xd11))

        xu2 = self.upconv2(xd12)
        xu22 = torch.cat([xu2, xe32], dim=1)
        xd21 = relu(self.d21(xu22))
        xd22 = relu(self.d22(xd21))

        xu3 = self.upconv3(xd22)
        xu33 = torch.cat([xu3, xe22], dim=1)
        xd31 = relu(self.d31(xu33))
        xd32 = relu(self.d32(xd31))

        xu4 = self.upconv4(xd32)
        xu44 = torch.cat([xu4, xe12], dim=1)
        xd41 = relu(self.d41(xu44))
        xd42 = relu(self.d42(xd41))

        # Output layer
        out = self.outconv(xd42)

        return out
```

> *不要忘记点击* ***Clap*** *和* ***Follow*** *按钮，以帮助我写更多这样的文章。*

# 就这样！

恭喜你成功实现了第一个 PyTorch 中的 U-Net 模型！通过遵循这个食谱，你已经掌握了实现 U-Net 的知识，现在可以将其应用于你未来可能遇到的任何图像分割问题。然而，验证尺寸和通道数以确保兼容性是重要的。U-Net 架构是你工具库中的一个强大工具，可以应用于各种任务，包括医学成像和自动驾驶。所以，快去从互联网上获取任何图像分割数据集，并开始测试你的代码吧！

为了方便，我在[这个仓库中添加了一个简单的测试脚本](https://github.com/Mostafa-wael/U-Net-in-PyTorch/blob/main/test.py)。

该脚本生成随机图像和掩膜，并训练 U-net 模型进行图像分割。它有一个名为`generate_random_data()`的函数，创建具有几何形状（如三角形、圆形、正方形和十字形）的输入图像及其对应的掩膜。U-net 模型使用这些随机图像和掩膜进行训练。训练好的模型随后在新的随机图像上进行测试，分割结果使用`plot_img_array()`函数绘制。该脚本使用 PyTorch 来训练 U-net 模型，并使用各种函数向输入图像和掩膜中添加形状。

请考虑下载并使用以下代码片段运行测试：

```py
import test
test.run(UNet)
```

![](img/535b89fef06b03ab927b6ff48e175ed2.png)

预期的测试输出（由我提供）。

# 最后的想法

总之，U-Net 架构因其在解决各种图像分割任务中的有效性而在计算机视觉社区中变得非常流行。它独特的设计，包括一个收缩路径和一个扩展路径，使其能够同时捕捉图像的局部和全局特征，同时保留空间信息。

此外，U-Net 架构的灵活性使得修改和改进网络以适应特定需求成为可能。研究人员提出了对原始 U-Net 架构的各种修改，包括改变卷积层、引入注意力机制以及添加跳跃连接等。这些修改在各种应用中提高了性能和分割结果。

总体而言，U-Net 架构已被证明是一个可靠且多功能的图像分割任务解决方案。随着计算机视觉的不断进步，我们可能会看到 U-Net 架构的进一步创新和改进，以提升其性能，使其在解决现实世界问题时更加有效。

不要犹豫与我分享你的想法！

> *我没有获得 Medium 的补助，因为埃及不支持。如果你喜欢我的内容，请考虑* [*请我喝咖啡*](https://ko-fi.com/mostafawael#) *☕ 来鼓励我发表更多帖子。谢谢！*
