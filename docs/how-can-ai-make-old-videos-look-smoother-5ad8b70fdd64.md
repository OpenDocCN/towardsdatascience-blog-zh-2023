# 人工智能如何使旧视频看起来更流畅？

> 原文：[`towardsdatascience.com/how-can-ai-make-old-videos-look-smoother-5ad8b70fdd64`](https://towardsdatascience.com/how-can-ai-make-old-videos-look-smoother-5ad8b70fdd64)

## 视频帧插值技术已经取得了长足的进展，而人工智能正在将其引向一些奇怪的方向。

[](https://mikhailklassen.medium.com/?source=post_page-----5ad8b70fdd64--------------------------------)![Mikhail Klassen](https://mikhailklassen.medium.com/?source=post_page-----5ad8b70fdd64--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5ad8b70fdd64--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5ad8b70fdd64--------------------------------) [Mikhail Klassen](https://mikhailklassen.medium.com/?source=post_page-----5ad8b70fdd64--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5ad8b70fdd64--------------------------------) ·阅读时长 7 分钟·2023 年 1 月 10 日

--

那是在 2015 年，我正在为与我的博士生导师委员会的会议创建一些数据可视化。这是在我研究生阶段接近尾声的时候，我运行了一些超级计算机模拟，模拟了一个在星际气体云中形成的大质量恒星。

我认为展示这些模拟结果的最佳方式是创建各种数据动画。我的分析代码处理了数吉字节的模拟数据，并在特定时间间隔生成了图像文件。因此，我使用 `ffmpeg` 将它们拼接成动画：

![](img/36d672e41b3f0e33b72b15a2cb91d823.png)

这是一个展示原恒星盘演化过程的天体物理学模拟视频。作者自制。请参阅 [Klassen et al. (2016)](https://iopscience.iop.org/article/10.3847/0004-637X/823/1/28/meta)。

我一直希望能拥有更高的帧率，以便获得更流畅、更美观的动画，但我受限于模拟中获得的数据。

有时我会花几个小时尝试生成中间帧，以使我的影片在编译时*看起来*更流畅。那时我的尝试笨拙且最终被放弃，但今天已经有许多技术效果非常好。事实上，大多数现代电视都包括（默认开启的）选项，以人工增强视频的帧率。

这项技术的正式名称是**视频帧插值**（VFI），结果发现这一领域已有相当多的研究。

# 什么是视频帧插值？

视频画面通常是由单独的静态帧组装而成的。如果帧率较低，视频可能会显得“跳跃”。为了让视频看起来更流畅，已经开发了各种技术来人工插入原始帧之间的中间帧。

在下面的示例中，一个短动漫剪辑的帧率通过 AI 技术提升了 8 倍。结果是视频更加平滑。

让我们深入探讨这些技术。我们将从一些不需要人工智能的方法开始，最后介绍一些最先进的方法。

作为我们的源材料，我们将使用由电影制片人[Pat Whelen](https://www.pexels.com/@pat-whelen-2913248/)拍摄的 6 秒钟火车机车剪辑，上传到免费的股票照片和视频网站[Pexels](https://www.pexels.com/video/wheels-and-connecting-rods-of-a-moving-train-5708282/)。

![](img/cdcd1f7c859b4108d98f26d0fdb9d5ae.png)

原始 23 fps 的火车机车剪辑。感谢：[Pat Whelen, Pexels](https://www.pexels.com/video/wheels-and-connecting-rods-of-a-moving-train-5708282/)。免费使用。

原始视频以 23 帧每秒的高分辨率录制。一般来说，我们的目标是在插值过程中将帧率翻倍。

# 帧插值技术

## 帧平均

添加更多帧的最简单方法之一是将两个连续的帧混合，创建这两帧的平均值。这很容易做到。我使用了`ffmpeg`来执行插值，使用了`minterpolate`滤镜：

```py
ffmpeg \
  -i input/train.mp4 \
  -crf 10 \
  -vf "minterpolate=fps=46:mi_mode=blend" \
  output/train_blended.mp4
```

有关滤镜设置的更多细节可以在[这里找到](https://ffmpeg.org/ffmpeg-filters.html#minterpolate)。结果如下，转换为动画 gif。

![](img/6081dc36e68a7e4548fcd8201c819ecb.png)

源视频帧率通过视频帧混合技术得到了提升。

如果仔细观察，你可以从结果视频中看到，任何运动的地方都会出现伪影。如果物体在帧之间移动过多，那么混合的中间帧会非常明显地显示物体出现在两个位置，并且在运动的地方呈半透明状。

为了克服这个问题，研究人员开发了运动估计技术，以提高插值的质量。

## 运动估计

为了创建看起来不仅仅是两个其他帧模糊平均值的中间帧，开发了运动补偿帧插值。这种方法通过识别连续帧中的物体，从类似的视觉特征计算这些物体的运动路径。这样就创建了一个向插值算法可以利用的向量“运动场”。

![](img/704d3387f1cb81bf74bf1002e0f5d3f7.png)

来自[Choi & Ko (2010)](https://www.researchgate.net/publication/224145279_Hierarchical_motion_estimation_algorithm_using_reliable_motion_adoption)的图示，显示了覆盖在视频帧上的运动场。

多年来，多个运动估计算法已被实现到`ffmpeg`中，我们将使用一种称为“重叠块运动补偿”（OBMC）的算法。结果如下：

![](img/0cee5b25f83f344bdb122cc98829d9ba.png)

使用名为重叠块运动补偿的运动插值技术增强了源视频的帧率。

尽管生成的视频质量要好得多，但视频中仍可能出现微小的伪影。为了实现上述视频，我再次依赖于`ffmpeg`中的`minterpolate`滤镜，但使用了一些不同的设置：

```py
ffmpeg \
  -i input/train.mp4 \
  -crf 10 \
  -vf "minterpolate=fps=46:mi_mode=mci:mc_mode=obmc:me_mode=bidir" \
  output/train_obmc.mp4
```

## 使用人工智能的帧插值

计算机视觉领域正在通过新人工智能技术的应用迅速发展。深度神经网络可以执行高度准确的图像分割、物体检测甚至 3D 深度估计。

之前的技术依赖于估计帧之间物体的运动。这些物体是通过视觉特征检测的。卷积神经网络（CNNs）在从图像中提取视觉特征方面表现出色，因此在这里应用它们似乎是合理的。

接下来，如果我们能更多地了解图像所表示的世界的 3D 特性，那么我们可以更好地估计图像中不同部分的运动，从而生成更准确的中间帧。

这里的真正改变者是增加了深度估计。一种训练有素的神经网络可以预测图像中各部分的距离。

下方的动画展示了一篇名为“[深入探讨自监督单目深度估计](https://arxiv.org/abs/1806.01260)”（ICCV 2019, [代码](https://github.com/nianticlabs/monodepth2)）的论文中的结果。深度神经网络准确地逐帧估计图像中哪些部分代表了较近的物体与较远的物体。

这种技术的良好效果并不令人感到意外。这是我们的大脑本能地做的事情，即使是在闭上一只眼睛去除视差信息时也是如此。使用双眼可以进一步提高准确性。

![](img/18dc85e2e2a0d3cfa861590a7793ff1a.png)

深度感知视频帧插值技术，如 2019 年由 Bao 等人提出的[DAIN 算法](https://arxiv.org/abs/1904.00830)，比以前的方法表现更好，因为它们可以考虑遮挡情况，即当物体互相经过时。

最近，黄等人（ECCV 2022）提出的“[实时中间流估计](https://arxiv.org/abs/2011.06294)”（RIFE）技术取得了卓越的基准成绩，同时运行速度也快得多。这可能代表了目前的最先进技术。

![](img/774419b7400a15f0ad68f7c20ee8a91b.png)

来自 RIFE 论文的一个示例视频片段，展示了 2 张输入图像的 16 倍插值：[`github.com/megvii-research/ECCV2022-RIFE`](https://github.com/megvii-research/ECCV2022-RIFE)

![](img/4ae318fc18734ebd7074fe11cd537cd1.png)

RIFE 对相同输入进行的深度估计。

让我们通过将 RIFE 算法应用于纽约市场景来总结我们的练习。我将[GitHub 仓库](https://github.com/megvii-research/ECCV2022-RIFE)克隆到我的笔记本电脑上，下载了 repo 的 ReadMe 文件中描述的预训练模型参数，并对`requirements.txt`文件进行了几处调整。我在没有 GPU 帮助的情况下在笔记本电脑上运行了代码。处理 6 秒的视频素材大约花费了 15 分钟。以下是结果：

![](img/ed1785fa4a2dec0692e6858fce783966.png)

使用新颖的深度 CNN 技术进行视频插值，“实时中间流估计”（RIFE）。

在 RIFE 创建的高分辨率 mp4 视频中效果更佳。对于本文，我将该视频转换成了动画 gif。

# 最终想法

这个领域发展非常迅速。训练于互联网上数十亿图像的图像生成 AI 已经能够从一系列输入图像或基于文本的提示中生成高度一致的、新颖的图像。

其他技术将深度估计方法扩展到从多个摄像机角度创建点云，然后使用称为神经渲染的技术合成新的人工视角。举例来说，[Rückert 等（2021）](https://arxiv.org/abs/2110.06635)及其神经渲染方法：

![](img/d5649da822126e79774534e8c2ec56e8.png)

[Rückert 等（2021）](https://arxiv.org/abs/2110.06635)中的神经渲染。左侧的图像是从一系列真实图像（右下角）生成的。

随着这一领域的持续发展，我们可以期待中间帧的准确性更高，伪影更少，即使更多的帧本质上是由非常大的预训练人工神经网络“梦到”或幻觉出来的。相对较少的输入帧即可在它们之间产生高度逼真、流畅的过渡。

这打开了很多创造性的可能性，比如恢复旧档案视频、为静态图像制作动画，以及从少量二维照片中创建虚拟现实世界。

这也可能导致世界上充斥着大量合成图像，使得区分什么是实际真相变得困难。同时，期待你的视频看起来非常流畅。

![](img/c3c4a41ab7c9954339fd12974668fbfe.png)

我的原始研究数据可视化（见上文），经过了 RIFE 算法处理。

如果你喜欢阅读这样的故事并希望支持我作为作者，可以考虑注册成为 Medium 会员。每月 $5，你可以访问我和其他成千上万名作家的所有作品。如果你通过[我的链接](https://mikhailklassen.medium.com/membership)注册，我将获得一小笔佣金，而你无需额外付费。

[](https://mikhailklassen.medium.com/membership?source=post_page-----5ad8b70fdd64--------------------------------) [## 通过我的推荐链接加入 Medium — Mikhail Klassen

### 阅读米哈伊尔·克拉森（Mikhail Klassen）以及在 Medium 上的其他数千名作家的每一个故事。你的会员费直接支持…

[mikhailklassen.medium.com](https://mikhailklassen.medium.com/membership?source=post_page-----5ad8b70fdd64--------------------------------)
