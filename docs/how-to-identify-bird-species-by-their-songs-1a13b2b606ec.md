# 如何通过鸟鸣声识别鸟类物种？

> 原文：[`towardsdatascience.com/how-to-identify-bird-species-by-their-songs-1a13b2b606ec`](https://towardsdatascience.com/how-to-identify-bird-species-by-their-songs-1a13b2b606ec)

## 应用机器学习于声音的起点

[](https://medium.com/@jacky.kaub?source=post_page-----1a13b2b606ec--------------------------------)![Jacky Kaub](https://medium.com/@jacky.kaub?source=post_page-----1a13b2b606ec--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1a13b2b606ec--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1a13b2b606ec--------------------------------) [Jacky Kaub](https://medium.com/@jacky.kaub?source=post_page-----1a13b2b606ec--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----1a13b2b606ec--------------------------------) ·10 分钟阅读·2023 年 5 月 22 日

--

![](img/ece658a10d750c0d4ee2d4b907341888.png)

图片由[Jan Meeus](https://unsplash.com/@janmeeus?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

你知道声音可以被转化为图像，供标准卷积网络在机器学习中识别声音是什么吗？

我们将讨论你需要的工具，以快速启动新的声音分类任务，并提供一个整体视图，展示所有内容如何组合在一起。

## 关于本文使用的数据

为了说明声音分类，我们将使用从[`xeno-canto.org/`](https://xeno-canto.org/)录制的鸟鸣声音。Xeno-canto 是一个由社区驱动的全球鸟鸣声音合集，包括大量鸟类录音数据库。

在使用他们的数据之前，你需要知道每个录音可能有不同的许可证，对于这篇文章，我选择了 CC BY 4.0 许可证下的记录。

# 关于声音和信号处理的基础知识

声音是物体振动时产生的空气压力波的结果。这些压力波从源头传递到我们这里。耳朵随后处理两个参数：压力波的频率和振幅。

耳朵接收到的压力波的频率被转化为音调：频率越高，音调越高。

波的振幅则负责声音的强度，较大的振幅会产生更大的噪音。

## 通过一个示例

下面是一个由[Camille Vacher](https://xeno-canto.org/contributor/LTIHWHLJQI)录制的[鸟鸣](https://xeno-canto.org/710407)示例，视为空气压力随时间变化的信号。我们称之为在“**时间域**”中可视化波形，因为我们观察压力波随时间的演变。信号已被截断，只保留了鸟类的第一个“pew”。

![](img/ccadef47e87fb06d59af5e319e37cb87.png)

[鸟鸣声](https://xeno-canto.org/710407) 由 [Camille Vacher](https://xeno-canto.org/contributor/LTIHWHLJQI) 录制（[**创意共享署名 4.0**](https://creativecommons.org/licenses/by/4.0/)），转录为气压时间序列，作者插图

作为参考，这就是我们实际听到的声音：

[鸟鸣声](https://xeno-canto.org/710407) 由 [Camille Vacher](https://xeno-canto.org/contributor/LTIHWHLJQI) 录制（[**创意共享署名 4.0**](https://creativecommons.org/licenses/by/4.0/)），[Mark S Jobling 的插图](https://commons.wikimedia.org/wiki/Category:Cettia_cetti?uselang=fr#/media/File:37-090505-cettis-warbler-at-Kalloni-east-river.jpg)（维基百科公共， [CC BY 3.0](https://creativecommons.org/licenses/by/3.0)）

对于人类而言，声音的不同特征，以及我们希望鸟鸣分类器识别的内容是：

+   呼叫的持续时间（一些鸟类发出较长的声音，而另一些则发出较短的声音）

+   音调及其在呼叫中的演变

+   幅度随时间的潜在演变

+   呼叫频率/幅度随时间演变的一些特征

让我们深入探索上述鸟鸣的特征。

## 呼叫频率的演变

如果我们准确地放大到歌曲的开始部分，可以观察到时间序列状态的变化，这突出了鸟鸣声开始时的主要频率。

![](img/249d3929b51eea446e5e6fb692c14036.png)

[鸟鸣声](https://xeno-canto.org/710407) 由 [Camille Vacher](https://xeno-canto.org/contributor/LTIHWHLJQI) 录制（[**创意共享署名 4.0**](https://creativecommons.org/licenses/by/4.0/)），转录为气压时间序列，作者插图

如果我们稍微深入信号中，可以看到主频率的差异以及信号幅度的差异。我们来看一下两个不同时间戳的信号的 5 个振荡周期。要计算信号的振荡频率，我们只需查看一个完整振荡之间的时间，并取倒数。我们在这里对 5 个周期进行平均，以获得稍微更稳定的测量值。

在 t=0.0918s 时，振荡频率大约为 1/[(0.0922–0.0913)/5] ~ 5.55kHz。

![](img/6be1d3e0d02ad245ead4994086ef49ec.png)

[鸟鸣声](https://xeno-canto.org/710407) 由 [Camille Vacher](https://xeno-canto.org/contributor/LTIHWHLJQI) 录制（[**创意共享署名 4.0**](https://creativecommons.org/licenses/by/4.0/)），转录为气压时间序列，作者插图

另一方面，在 0.229s 左右，频率较低：1/[(0.2295–0.2284)/5] ~ 4.55kHz。我们还可以注意到，第二个信号的幅度约为之前信号的 4 倍。

![](img/044084e3c1782f13af91f993a2f0010e.png)

[鸟鸣](https://xeno-canto.org/710407)由 [Camille Vacher](https://xeno-canto.org/contributor/LTIHWHLJQI) 录制 ([**知识共享署名 4.0**](https://creativecommons.org/licenses/by/4.0/))，转录为气压时间序列，作者插图

## 那么声音的音色呢？

我们上面观察到的频率不足以完全表征鸟鸣。想象两个乐器演奏相同的音符：气压信号的主要频率可以相同（相同的音符，因此相同的音高），但你的耳朵仍然可以区分这两个乐器。

到目前为止，我们将声音视为纯单频信号。实际上，声音由无限多个信号组成，这些信号相互叠加，并为声音赋予特定的音色，这也被我们的脑袋用来区分不同的声音。这些其他频率通常被称为“谐波”，并且从我们的“时间域”表示中很难区分。

仍然可以识别出一个线索：如果我们的鸟鸣是一个完全单调的正弦波，我们应该不会看到幅度的变化，如下图所示：

![](img/3688ff71426075f25fec4ba9edfbda0c.png)

单频周期信号的示例 (f(t) = A * sin(2 pi f t) )

如果我们在前面的时间序列中添加另一个不同频率的信号，我们将观察到幅度的调制，同时保持主要频率：

![](img/ad22b5e5b1fbb062063aac30f4753ba0.png)

将不同频率的周期信号相加：(f(t) = A1 * sin(2 pi f1 t) + A2 * sin(2 pi f2 t) )，作者插图

负责调制时间域幅度的频率是基础的，因为它们给声音带来了“色彩”。回到我们的例子，这些调制可以在我们以适当的细节观察时看到：

![](img/74f798762629569b0009eefa99134340.png)

[鸟鸣](https://xeno-canto.org/710407)由 [Camille Vacher](https://xeno-canto.org/contributor/LTIHWHLJQI) 录制 ([**知识共享署名 4.0**](https://creativecommons.org/licenses/by/4.0/))，转录为气压时间序列，作者插图

# 从声音到图像

## 傅里叶变换

**傅里叶变换**是一个数学工具，它帮助我们将复杂的信号分解成一系列简单的正弦波和余弦波，每个波都有特定的频率和幅度。以下是傅里叶变换的简化版本：

![](img/16c099f1969a64d014bc7fc5c3dab707.png)

傅里叶变换的简化版本，忽略相位信息

简而言之，傅里叶变换只是告诉我们任何时间序列都可以分解为具有不同幅度的基本正弦/余弦波的连续和（公式的积分）。这正是我们所寻找的（因为声音和频率谱是紧密相关的）

这种方法允许我们“分解”信号并识别每个频率成分，从而更全面地理解整体声音。

## 两个余弦信号混合的傅里叶变换

为了更清楚地说明，让我们考虑之前的例子，其中我们组合了两个不同频率的周期信号。正如我们观察到的，输出信号是一个幅度调制的信号。

![](img/72c7afb0ca218cad31595a3a67dcc552.png)

加上不同频率的周期信号，作者插图

傅里叶变换的思想就是识别和表示我们组合信号的不同成分，这些成分随后可以在图表上表示出每个主要余弦波的幅度和频率。

![](img/b7d6fcb46368393146e4432ad1c3e14a.png)

原始信号被分解成主要的余弦波。它们的幅度和频率显示在一个图表中，作者插图

结果图显示了复杂信号的主要成分及其频率和幅度，称为谱图，它包含了组成我们信号的“特征”。

![](img/07e5d3039606e5936d8850e043166474.png)

我们编造的例子的谱图

这个图表告诉我们的是，我们编造的周期信号由两个“主要”余弦信号组成：

+   一个频率为 2250Hz、幅度为 0.5 的余弦波

+   一个频率为 5500Hz、幅度为 1 的余弦波

这个图表足以完全描述我们的主要周期信号：我们从“时间域”跳到了“频率域”。

## 将概念扩展到实际信号

然而，我们的鸟鸣声要复杂得多，它可以混合无数个频率，每个频率都对鸟鸣声的独特音色做出贡献，频率组成也可以随着时间而变化。

我们将傅里叶变换应用于整个信号，而是仅在一个足够小的尺度上应用，以便信号“足够规律”，但又足够长以包含足够的振荡周期。

例如，让我们回到 t=0.0918s 和 t=0.229s 的信号，并查看谱图。得到的傅里叶变换这次是连续的，但在某些频率处有峰值，这些峰值与本文前一章的计算结果相匹配。

![](img/5d022311e319c1d2bc20458ff90cb8d5.png)

时间域（左）和谱图（右）（t=0.0918s），[鸟鸣声](https://xeno-canto.org/710407)由[Camille Vacher](https://xeno-canto.org/contributor/LTIHWHLJQI)录制（[**知识共享署名 4.0**](https://creativecommons.org/licenses/by/4.0/)），以气压时间序列记录，作者插图

![](img/fdd174be049b083c7acb74a79ce4d9d3.png)

时间域（左）和频谱图（右）（t=0.229s），[鸟鸣声](https://xeno-canto.org/710407)由[Camille Vacher](https://xeno-canto.org/contributor/LTIHWHLJQI)录制（[**创作共用署名 4.0**](https://creativecommons.org/licenses/by/4.0/)），转录为空气压力时间序列，作者插图

其次，我们可以更详细地确定每个信号部分的组成。特别是，我们看到第二个片段由多个频率峰值构成，从和声的角度来看更为“丰富”，为我们提供了关于之前提到的“色彩”的新信息。

对信号的一个子部分应用傅里叶变换，如上所述，通常被称为**短时傅里叶变换** **（STFT）**，这是一个强大的工具，特别适用于局部描述声音并跟踪其随时间的变化。

## 从 STFT 到谱图

现在我们有了一个工具，可以用来识别时间信号片段中的不同主要成分（幅度/频率）。我们现在可以将这种方法应用于整个信号，使用滑动窗口提取声音随时间的特征。请注意，我们将不再以散点图的形式展示谱图，而是使用热图表示，频率轴垂直显示，每个像素表示该频率的强度。

![](img/1b65b1df0c0870704d533a7b9e0fe6c7.png)

从时间信号到 STFT 热图，作者插图

利用这种表示方法，我们现在可以水平堆叠使用滚动窗口计算的整个信号的 STFT，并通过图像可视化频谱随时间的演变。生成的图像称为**谱图**。

![](img/805930c598e14e939c1d21b976c37822.png)

时间域中的鸟鸣声及其关联的谱图，[鸟鸣声](https://xeno-canto.org/710407)由[Camille Vacher](https://xeno-canto.org/contributor/LTIHWHLJQI)录制（[**创作共用署名 4.0**](https://creativecommons.org/licenses/by/4.0/)），转录为空气压力时间序列，作者插图

在上述谱图中，每一列像素代表了一个给定时间戳中心的小片段信号的 STFT。

有许多类型的谱图具有不同的尺度，以及应用时间-频率变换的不同超参数。其中包括对数频率功率谱图、常数 Q 变换（CQT）、梅尔谱图等……每种都有其独特之处，但都基于提取频率特征并以（时间 x 频率热图）的形式表示，这种表示可以被解读为图像。

## 一些示例

声谱图的优点在于它将声音的所有重要特征浓缩为一幅简单的图像。分析这幅图像可以告诉你声音在时间上的幅度、音调和颜色变化，这正是我们（或机器学习/深度学习算法）识别声音发射者所需的。

让我们看一下几个持续 5 秒的声音及其相关的声谱图。

[第一个样本](https://xeno-canto.org/794236)是由[**Benoît Van Hecke**](https://xeno-canto.org/contributor/IIGGEELDDD)录制的欧洲林雀。

[欧洲林雀的叫声](https://xeno-canto.org/794236)，由[**Benoît Van Hecke**](https://xeno-canto.org/contributor/IIGGEELDDD)提供，图像来自[Sébastien FAILLON](https://www.flickr.com/people/128090976@N08)（[维基媒体共享资源](https://commons.wikimedia.org/wiki/File:Pinson_des_arbres_femelle_(19215641796).jpg)）。

![](img/41fdf550e9ed4ec96c8ebe34d95efce3.png)

[欧洲林雀的叫声](https://xeno-canto.org/794236)在时间域中的图像，以及相关的声谱图，由[**Benoît Van Hecke**](https://xeno-canto.org/contributor/IIGGEELDDD)提供。

[第二个录音](https://xeno-canto.org/542842)是由[**Benoît Van Hecke**](https://xeno-canto.org/contributor/IIGGEELDDD)录制的欧洲知更鸟。

[欧洲知更鸟的叫声](https://xeno-canto.org/542842)，由[**Benoît Van Hecke**](https://xeno-canto.org/contributor/IIGGEELDDD)提供，图像由[Sharp Photography, sharpphotography](http://www.sharpphotography.co.uk/)（[维基媒体共享资源](https://commons.wikimedia.org/wiki/File:Pinson_des_arbres_femelle_(19215641796).jpg)）。

![](img/8a9f10f8bcf062d542f05625bcdbfd5e.png)

[欧洲知更鸟的叫声](https://xeno-canto.org/542842)在时间域中的图像，以及相关的声谱图，由[**Benoît Van Hecke**](https://xeno-canto.org/contributor/IIGGEELDDD)提供。

最后一条是由[**Benoît Van Hecke**](https://xeno-canto.org/contributor/IIGGEELDDD)录制的 5 秒[画眉](https://xeno-canto.org/629038)。

[画眉的叫声](https://xeno-canto.org/629038)，由[**Benoît Van Hecke**](https://xeno-canto.org/contributor/IIGGEELDDD)提供，图像由 Tony Wills 提供（[维基媒体共享资源](https://commons.wikimedia.org/wiki/File:Pinson_des_arbres_femelle_(19215641796).jpg)）。

![](img/e5f218af5479a5d7e97933751dfe68e4.png)

[画眉的叫声](https://xeno-canto.org/629038)，在时间域中，以及相关的声谱图，由[**Benoît Van Hecke**](https://xeno-canto.org/contributor/IIGGEELDDD)提供。

## 为什么声谱图+ CNN 在声音分类中比 LSTM 更有效。

如果你以前从未涉足声音分类系统，你可能会考虑使用像 LSTM 这样的递归神经网络直接从声音时间序列中提取相关特征。

这将是一个糟糕的主意，因为即使这些模型设计用于提取时间依赖性，它们在提取频率特征方面并不高效，而正如我们所见，频率特征对声音识别任务至关重要。LSTM 本质上也是计算开销大且低效的（因为数据是按顺序处理的）。这意味着在给定时间内需要处理的数据远远少于标准卷积神经网络（CNN）。

另一方面，将时间序列数据转换为声谱图，这实际上是频率信息随时间变化的可视化表示，允许我们使用为图像数据设计的卷积神经网络（CNN），这些网络在捕捉空间模式方面非常有效。在声谱图的情况下，这些空间模式对应于随时间变化的频率模式。这一步可以看作是一个自然的“特征工程”步骤，保证了声音分类任务的更高效率。

# 结论与进一步挑战

在这篇文章中，我们探讨了用于从声音中提取所有相关特征并将时间序列转换为图像的机制。

我们只覆盖了初步的预处理步骤，但即使声谱图是一种强大的变换工具，它也不是万能的。仍然存在若干需要解决的挑战。

主要挑战之一是弱标注数据的问题。在许多情况下，录音中感兴趣声音的确切时间未知，标签仅表示录音中某处存在声音。

另一个挑战是录音中的背景噪声。可以使用降噪或滤波等技术来减轻这个问题，但这些技术也有可能扭曲鸟鸣声，从而丢失重要信息。其他的替代方法包括特定于声音分类的数据增强技术，如“Mix-up”（一种通过线性组合原始样本创建新声音的方法）或添加新的背景噪声。

最后，录音的长度也可能成为一个问题，尤其是与弱标注问题相结合时。虽然有些声音仅持续几秒钟，但其他声音可能持续几分钟。这种变化可能使模型输入的标准化变得困难，并且也可能影响模型的性能，因为从较短的录音中准确识别物种可能更加困难。

尽管面临这些挑战，使用声谱图和卷积神经网络（CNN）提供了一个有前景的声音识别方法，我希望这一探索能够为那些开始新机器学习声音分类项目的人提供一个宝贵的起点。凭借正确的工具和理解，我们可以应对这些挑战，并解锁音频数据的巨大潜力。
