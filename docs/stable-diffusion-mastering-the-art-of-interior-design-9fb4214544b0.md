# 稳定扩散：掌握室内设计的艺术

> 原文：[`towardsdatascience.com/stable-diffusion-mastering-the-art-of-interior-design-9fb4214544b0`](https://towardsdatascience.com/stable-diffusion-mastering-the-art-of-interior-design-9fb4214544b0)

## 深入探索稳定扩散及其室内设计的修复变体

[](https://medium.com/@rjguedes?source=post_page-----9fb4214544b0--------------------------------)![Rafael Guedes](https://medium.com/@rjguedes?source=post_page-----9fb4214544b0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9fb4214544b0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9fb4214544b0--------------------------------) [Rafael Guedes](https://medium.com/@rjguedes?source=post_page-----9fb4214544b0--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----9fb4214544b0--------------------------------) ·阅读时间 9 分钟·2023 年 12 月 18 日

--

在我们生活的这个快节奏的世界中以及经历了疫情后，我们中的许多人意识到，拥有一个舒适的家作为逃避现实的避风港是无价的，值得追求的目标。

无论你是寻找斯堪的纳维亚风格、极简风格，还是华丽风格来装饰你的家，很难想象每件物品如何在充满不同件数和颜色的空间中相互配合。因此，我们通常寻求专业帮助，以创建那些令人惊叹的 3D 图像，帮助我们理解未来的家会是什么样子。

然而，这些 3D 图像费用昂贵，如果我们的初步构思不如预期好，获取新图像将需要时间和更多的金钱，这些在当今社会都很稀缺。

在这篇文章中，我探讨了稳定扩散模型，首先简要解释了它是什么、如何训练以及需要什么来适应修复。最后，我以对我未来家的 3D 图像的应用结束文章，我将厨房岛和橱柜的颜色和材料更换为不同的。

![](img/2a2fbf68e14d0e4d69be676ac27fb9c3.png)

图 1：室内设计 ([source](https://unsplash.com/photos/brown-wooden-framed-yellow-padded-chair-_HqHX3LBN18))

一如既往，代码可在[Github](https://github.com/rjguedes8/stable_diffusion)上找到。

# 稳定扩散

## 这是什么？

稳定扩散[1]是 CompVis Group 于 2022 年发布的生成性 AI 模型，可以根据文本和图像提示生成逼真的图像。它主要设计用于生成受文本描述影响的图像，但也可以用于其他任务，如修复或视频创建。

它的成功源于 **感知图像压缩** 步骤，该步骤将高维图像转换为更小的潜在空间。这种压缩使得在资源有限的机器上使用该模型成为可能，使每个人都能使用，这在之前的最先进模型中是不可能的。

![](img/79fe033603707036fea343f9ec147dc1.png)

图 2：Stable Diffusion 架构 ([source](https://arxiv.org/pdf/2112.10752.pdf))

## 它是如何学习的？

Stable Diffusion 是一个 **潜在扩散模型** (LDM)，具有三个主要组件 (**变分自编码器** (VAE) [2]、**U-Net** [3] 和一个可选的 **文本编码器**)，它学习如何在提示（文本或其他图像）的条件下去噪图像，以创建新图像。

Stable Diffusion 的训练过程有 5 个主要步骤：

1. **感知图像压缩** 步骤包括一个 **编码器**，它接收一个尺寸为 *512x512x3* 的图像，并将其编码为一个尺寸为 *64x64x4* 的更小的潜在空间 *Z*。为了更好地保留图像的细节（例如，人脸中的眼睛），潜在空间 *Z* 使用低权重的 Kullback-Leibler 项进行正则化，以使其零中心并获得小方差。

![](img/f30de379460e03b36399513a035cca84.png)

图 3：感知图像压缩过程，其中编码器将 512x512x3 的图像转换为 64x64x4 的潜在空间（图像由作者制作）。

2\. **扩散过程** 负责逐步向潜在空间 *Z* 添加高斯噪声，直到所有剩下的是随机噪声，生成一个新的潜在空间 ***Zt.*** ***t*** 是扩散过程发生的次数，以实现完全的噪声潜在空间。

这一步骤很重要，因为 Stable Diffusion 必须学习如何从噪声恢复到原始图像，正如我们将在下一步中看到的那样。

![](img/08685adba6bacd6db70f947657a72950.png)

图 4：扩散过程，其中高斯噪声逐渐添加到潜在空间中（图像由作者制作）

3\. **去噪** **过程** 训练了一个 U-Net 架构来估计潜在空间 ***Zt*** 中的噪声量，以便减去它并恢复 *Z*。这个过程通过逐渐去噪 *Zt* 来恢复原始的潜在空间 *Z*，基本上是扩散过程的逆过程。

![](img/5778395079858775325368a96391d2aa.png)

图 5：去噪过程，其中 U-Net 预测潜在空间中的噪声并将其去除，直到完全恢复原始潜在空间（图像由作者制作）

4\. 在 **去噪** **过程** 中，可以将提示，通常是文本和/或其他图像，连接到潜在空间 *Zt*。这种连接将条件化去噪过程，从而允许创建新图像。作者在 U-Net 的骨干网络中添加了交叉注意机制来处理这些提示，因为它们对学习各种输入类型的基于注意的模型是有效的。

当涉及到文本时，模型使用训练好的文本编码器**CLIP** [4]，该编码器将提示编码为一个 768 维的向量，然后将其与*Zt*连接，并作为输入传递给 U-Net。

正如我们在图 6 中所见，我们将文本提示*“移除灯”*与*Zt*进行连接，这使得 Diffusion Process 对*Zt*进行调整，去除了原始*Zt*中靠近椅子的灯。

![](img/4b95fd7017e1a2147e6be92de15bf4f1.png)

图 6：使用文本提示来去除原始图像中的灯光的去噪过程（图片由作者制作）

5\. 最终，**解码器**接收去噪后的潜在空间*Z*作为输入，并学习如何估计用于将图像编码到更小潜在空间中的每个组件的方差。在估计方差后，解码器可以生成与原始图像相同尺寸的新图像。

![](img/fa1d17e8e78ea64e86b60bc59ec9046e.png)

图 7：解码器恢复了没有灯的原始图像，尺寸为 512x512x3（图片由作者制作）

# Stable Diffusion 的修补变体

修补任务是用新内容填补图像中被遮蔽的区域，无论是因为我们想要修复图像还是因为我们想要替换一些不希望出现的内容。

Stable Diffusion 可以被训练生成基于图像、文本提示和掩膜的新图像。这种模型已经在 HuggingFace 🤗 上可用，名称为*runwayml/stable-diffusion-inpainting*。

为了训练 Stable Diffusion 进行修补，我们需要经历上述部分提到的相同步骤，但输入数据稍有不同。在这种情况下，除了原始图像和文本，我们还需要一个掩膜（另一张图像）。为此，U-Net 需要适应以接收额外的掩膜输入通道。

在训练过程中，掩膜下的区域保持不变，只对其进行编码到潜在空间，而掩膜区域则经历整个编码、扩散和去噪过程。这样，Stable Diffusion 模型知道哪些区域应该保持不变，哪些区域应该发生变化（图 8 展示了这一过程）。

![](img/a22262eba205e0bec3179cef1acab740.png)

图 8：修补扩散 Stable 模型的训练过程（图片由作者制作）

在图 9 中，我们展示了在自己的使用案例中进行修补所需的示例。我们提供原始图像以及我们希望更改的掩膜和包含我们想看到更改的文本提示，Stable Diffusion 生成新图像。在下一节中，我们将看到如何实际操作。

![](img/134c22d473a948035e00308ece16a734.png)

图 9：训练 Stable Diffusion 进行图像修补的过程（图片由作者制作）

# 使用 Stable Diffusion 的室内设计

在本节中，我将讲解如何在室内设计中使用 Stable Diffusion 进行修补场景的操作。

当我们购买一栋新房或仍在建设中的公寓时，通常可以获取它的 3D 图像。基于这些图像，我们可以要求更改颜色或材料，以使其符合我们的口味。

然而，很难想象我们请求的更改是否适合房子的其他部分，而请求新的 3D 可能会很昂贵且耗时。因此，我们可以使用 Stable Diffusion 快速迭代，并了解如果应用我们想要的更改，事物会是什么样子。

为此，我们可以使用 Python 和 HuggingFace 🤗 来构建我们自己的 Stable Diffusion 室内设计师！

我们首先导入库：

```py
import PIL
import torch
import ipyplot
from diffusers import StableDiffusionInpaintPipeline
```

然后，我们加载 HuggingFace 🤗 提供的 Stable Diffusion Inpainting 模型：

```py
# load model
pipe = StableDiffusionInpaintPipeline.from_pretrained("runwayml/stable-diffusion-inpainting", torch_dtype=torch.float16).to("cuda")
```

在加载了模型后，我们加载原始图像和我们想要更改的遮罩：

+   遮罩的白色部分是将要更改的部分，而黑色部分是将保持不变的部分。

+   遮罩是手动创建的，但我们也可以使用分割模型来创建它。

```py
init_image = PIL.Image.open("<YOUR IMAGE>.jpg")
mask_image = PIL.Image.open("<YOUR MASK>.png")

ipyplot.plot_images([init_image, mask_image], labels=["original", "mask"], max_images=30, img_width=500)
```

![](img/b3f7c5c57a1d1d2bb992b2409a31eb42.png)

图 10：厨房的 3D 图像（由作者购买）和一个遮罩，用于更改岛台和黑色墙壁（图像由作者制作）

在加载了图像和遮罩后，现在是创建提示以将图像生成条件设置为我们想要的内容并生成新图像的时候了。

+   在这种情况下，我想将黑色岛台和黑色墙壁替换为大理石岛台和大理石墙壁。

```py
# define prompt
prompt = "white calacatta marble island kitchen and white calacatta marble wall"
guidance_scale = 15 # weight of the prompt
generator = torch.Generator(device="cuda").manual_seed(0) # change the seed to get different results

# generate 10 images
images = pipe(prompt=prompt, image=init_image, mask_image=mask_image, num_images_per_prompt=10, guidance_scale=guidance_scale, generator=generator).images

# show results
print(prompt)
ipyplot.plot_images([init_image] + images, labels=["original"] + ["generated"] * len(images), max_images=30, img_width=500)
```

![](img/09742ef89e90a9391e0f55615bcffa7d.png)

图 11：原始图像与我最喜欢的生成图像的比较（它们的尺寸不同，因为模型是用 512x512 图像训练的）

结果看起来不错，但我还想将木制厨房橱柜替换为白色的，所以让我们重新进行这个过程：

1.  加载最后生成的图像和一个新的遮罩：

```py
# load new image and mask
init_image = PIL.Image.open("IMG2.jpg") mask_image = PIL.Image.open("mask3.png")
ipyplot.plot_images([init_image, mask_image], labels=["original", "mask"], max_images=30, img_width=500)
```

![](img/c99eebffd0a561b6c60a0e14d261b659.png)

图 12：之前生成的图像和新的遮罩（图像由作者制作）

2. 创建新的提示并生成新图像：

```py
# define prompt
prompt = "white kitchen cabinet"
guidance_scale = 15 # weight of the prompt
generator = torch.Generator(device="cuda").manual_seed(0) # change the seed to get different results

# generate 10 images
images = pipe(prompt=prompt, image=init_image, mask_image=mask_image, num_images_per_prompt=10, guidance_scale=guidance_scale, generator=generator).images

# show results
print(prompt)
ipyplot.plot_images([init_image] + images, labels=["original"] + ["generated"] * len(images), max_images=30, img_width=500)
```

![](img/0b7e996ee6af90b86a437dc19e4ed3d0.png)

图 13：带有白色厨房橱柜的生成图像（图像由作者制作）

结果看起来非常好，我希望你注意到 Stable Diffusion 能够重现的细节，例如厨房水龙头或橱柜上的光线反射。尽管左侧的反射没有对齐，但令人惊讶的是它如何考虑了光线。

# 结论

AI 不仅对拥有大量数据的组织有用，它还可以应用于我们能想到的任何事物！

在本文中，我们探索了 Stable Diffusion 的一种非传统用途，但这是一个存在了几十年的传统工作。通过几行代码，我们能够为每个提示生成尽可能多的不同图像，这给了我们很多选择的可能性。

然而，与其他事物一样，稳定扩散模型，特别是我们使用的模型，也有其自身的局限性，例如在光线反射中或在图 14 中，其中一把椅子位于厨房岛内，并未实现完美的真实感。

尽管如此，人工智能的未来仍然光明，其中一些局限性可以通过在我们自己的数据和使用案例中进行微调来克服。

![](img/cc6bc6cb87942b845703843d69171247.png)

图 14：生成的图像存在一些质量问题（图像由作者制作）

**保持联系：** [LinkedIn](https://www.linkedin.com/in/rafaelguedes97/)， [Medium](https://medium.com/@rjguedes97)

# 参考文献

[1] Robin Rombach, Andreas Blattmann, Dominik Lorenz, Patrick Esser, Björn Ommer. 高分辨率图像合成与潜在扩散模型。arXiv:2001.08210, 2022

[2] Diederik P. Kingma, Max Welling. 变分自编码器简介。arXiv:1906.02691, 2019

[3] Olaf Ronneberger, Philipp Fischer, Thomas Brox. U-Net: 用于生物医学图像分割的卷积网络。arXiv:1505.04597, 2015

[4] Alec Radford, Jong Wook Kim, Chris Hallacy, Aditya Ramesh, Gabriel Goh, Sandhini Agarwal, Girish Sastry, Amanda Askell, Pamela Mishkin, Jack Clark, Gretchen Krueger, Ilya Sutskever. 从自然语言监督中学习可转移的视觉模型。arXiv:1911.02116, 2021
