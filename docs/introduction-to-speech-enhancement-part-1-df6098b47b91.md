# 语音增强简介：第一部分 — 概念与任务定义

> 原文：[`towardsdatascience.com/introduction-to-speech-enhancement-part-1-df6098b47b91`](https://towardsdatascience.com/introduction-to-speech-enhancement-part-1-df6098b47b91)

## 对改善退化语音质量或抑制噪声的概念、方法和算法的介绍

[](https://medium.com/@mattiadigangi?source=post_page-----df6098b47b91--------------------------------)![Mattia Di Gangi](https://medium.com/@mattiadigangi?source=post_page-----df6098b47b91--------------------------------)[](https://towardsdatascience.com/?source=post_page-----df6098b47b91--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----df6098b47b91--------------------------------) [Mattia Di Gangi](https://medium.com/@mattiadigangi?source=post_page-----df6098b47b91--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----df6098b47b91--------------------------------) ·阅读时间 6 分钟·2023 年 1 月 17 日

--

![](img/fb50f60823ea39daa9134c875473e489.png)

图片由 [Wan San Yip](https://unsplash.com/ja/@wansan_99?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本文是系列文章的一部分：

+   **语音增强简介：第一部分 — 概念与任务定义**

+   [语音增强简介：第二部分 — 信号表示](https://medium.com/towards-data-science/introduction-to-speech-enhancement-part-2-signal-representation-ab1deca2fa74)

语音增强是一套方法和技术，旨在通过语音音频信号处理技术提高语音质量，包括清除助听器中的噪声，或从嘈杂的信道/环境中恢复难以理解的语音信号 [[Wikipedia](https://en.wikipedia.org/wiki/Speech_enhancement)]。它有许多实际应用，包括从噪声中清除语音信号或从嘈杂的信道/环境中恢复难以理解的语音信号。

它类似于但不同于**语音分离**[1]，即将一个音频信号算法性地分离成两个不同的通道，分别用于语音和背景。可以应用语音增强来获取语音信号，然后应用额外的算法来计算**残差信号**，或原始信号与增强语音之间的*差异*。

不同的语音增强方法被应用于减少不同[噪声模型](https://aishack.in/tutorials/noise-models-1/)（例如，平稳与非平稳）的影响。虽然这个研究领域并不新鲜，但近年来深度学习方法蓬勃发展，提升了在最具挑战性的非平稳噪声场景中的语音增强质量，即使在低信噪比（SNR）下也是如此。

## 术语与背景

**信号：** 观测一个变化且可测量的物理量。音频、视频、图像，都属于这个定义。

**噪声模型：** 这是描述噪声信号潜在**随机**过程的数学模型。由于该过程是随机的，通常用概率分布来描述。

**（非）平稳过程：** 假设信号由一个潜在的过程生成。它可以是确定性的或随机的。由于我们对确定性过程了解一切，因此兴趣在于随机过程。如果一个随机过程的无条件联合分布**不**依赖于时间，则称其为*平稳的*。换句话说，给定一个过程的模型，事件 **X** 在时间 **t** 发生的概率不依赖于 **t** 本身。通过扩展，由平稳过程生成的信号也称为平稳。在音频领域，当信号的频率或谱内容随时间变化时，信号就是平稳的。这意味着，对于语音增强，真正的挑战来自于非平稳噪声，因为它更不可预测且更难与语音区分，而人声也同样是非平稳的：我们声音信号中的频率一直在变化。

# 实验设置

与大多数实证研究领域类似，在语音增强中，我们需要数据集来训练我们的模型和评估它们，还需要评估结果的指标，以及运行算法的硬件和软件工具。

作为硬件要求，我们绝对需要一台配备有（至少）一个 NVIDIA GPU 的现代计算机进行训练，而现代多核 CPU 在推断过程中也可能足够，尽管预计会有较高的 CPU 使用率。

在软件方面，我们需要一个使用如 Tensorflow、Pytorch 或 Jax 派生库的代码库。上面链接的 FullSubnet+ 存储库可以是一个很好的起点。

然后，我们谈到数据。测量质量和实现监督学习的最简单方法是拥有一个网络应该重建的参考信号。因此，训练集通常通过收集干净的语音信号和噪声信号来构建。然后，通过将它们添加不同级别的信噪比（SNR）来将这两者结合起来。这样，在训练过程中，可以生成大量的语音/噪声组合，网络必须学习这些组合以获得增强的语音信号。研究社区通过共享任务来推动语音增强，因此可以从这些来源找到公开数据集。每年举办的[Deep Noise Suppression (DNS) Challenge](https://github.com/microsoft/DNS-Challenge)就在 Interspeech 上展示。在相关的代码库中，您可以找到提供的数据集链接以及其他资源。

评估通常通过手动和自动评估来进行。两者之间的权衡已被多次提及，但可能值得重复一下。手动评估由人工执行：它既昂贵又缓慢，因为需要找到、指导和支付人力，当然他们还需要在评估之前听取音频。自动指标则快速且便宜，因此在开发周期中可以重复使用，但它们只能捕捉评估的某些方面，并且在某些情况下可能不够可靠。

在语音增强的情况下，通常使用四种主要的自动评估指标：语音质量的感知评估（PESQ）、对数似然比（LLR）、倒谱距离（CD）和加权谱斜率距离（WSS）。所有这些指标都计算清晰语音和增强语音之间的距离。PESQ 比较两个信号样本之间的质量，产生一个在-0.5 到 4.5 之间的分数，分数越高越好。其他三个指标计算两个谱之间不同属性的距离：共振峰、对数谱距离或谱斜率的加权距离。当需要时，可以增加人工评估，以评估感知质量或统计增强信号中的可识别单词。语音识别可以用来识别单词，但这时我们需要处理它自身的错误。

关于信号表示，有些语音增强模型在时间域中工作，而其他模型则在频域中工作。频域是通过对信号应用[傅里叶变换](https://www.thefouriertransform.com/)获得的，是对信号的*非时间性*表示，提供了关于其频率成分的信息。在实践中，频域表示比时间域表示更适用于语音增强，所有最先进的方法都使用它。

## 示例

让我们展示一个例子，以便对这个主题有听觉上的理解。我们从一个包含响亮音乐的视频广告开始，这可能会使语音难以听清。

以创作共享许可证发布

我们首先使用著名的[ffmpeg](https://ffmpeg.org/index.html)工具提取音频轨道：

[## original_audio.wav](https://drive.google.com/file/d/1Nu2_OxkkIdIOsrT_e_E910t1bi5cxgXb/preview?source=post_page-----df6098b47b91--------------------------------)

然后，我们应用 FullSubNet+算法[2]，这是一种基于深度学习的方法，具有[开源代码](https://github.com/hit-thusz-RookieCJ/FullSubNet-plus)和[预训练模型](https://github.com/hit-thusz-RookieCJ/FullSubNet-plus#readme)可用。按照仓库中的说明，很容易复现这一结果：

[## enhanced_audio.wav](https://drive.google.com/file/d/1-IRilAqcVQ8qZn2fTg66a8ZS-i_DgqKf/preview?source=post_page-----df6098b47b91--------------------------------)

最后，为了满足你的好奇心，展示从原始信号中增强语音得到的残余音频：

[## residual_audio.wav](https://drive.google.com/file/d/1ZHSn9LtAQYUWPzQDxi_cAzDIYrALoTio/preview?source=post_page-----df6098b47b91--------------------------------)

*enhanced_audio.wav*中的语音质量显著提高，并且音乐的音量根据时间被完全去除或大幅降低。结果显示了现代语音增强的有效性，但也表明结果并不完美，该领域还有更多研究在进行。此外，通过运行代码，你会发现这一操作在计算上相当昂贵。拥有 GPU 会使过程更快，但在保持或改善质量的同时减少计算复杂性是一个重要的研究目标。

# 结论

本系列的第一部分到此为止！我们讨论了语音增强的理念和原因，并提供了一些关于数字信号的背景信息。

在本系列的第二部分，我们将详细介绍一个公开的最先进语音增强模型。

与此同时，我鼓励你阅读文章中的链接资源，如果你对这一主题感兴趣，可以查看数字信号处理的教学资料。其中一本免费提供的书籍是[Think DSP](https://github.com/AllenDowney/ThinkDSP)。你可以在线阅读或下载 pdf，当然也可以购买纸质版以支持作者（我不会从中获得任何收益）。

感谢你一直阅读，希望在下一部分中见到你！

**语音增强简介：第二部分 — 信号表示**

[](/automatic-evaluation-of-synthesized-speech-354f7a2a20d7?source=post_page-----df6098b47b91--------------------------------) ## 自动评价合成语音

### 了解深度学习和预训练模型如何用于自动语音评估的指南

towardsdatascience.com [](/data-processing-automation-with-inotifywait-663aba0c560a?source=post_page-----df6098b47b91--------------------------------) ## 使用 inotifywait 进行数据处理自动化

### 在拥有生产就绪的 MLOps 平台之前如何进行自动化

towardsdatascience.com [](/3-common-bug-sources-and-how-to-avoid-them-182f9974d2ab?source=post_page-----df6098b47b91--------------------------------) ## 3 种常见的错误来源及如何避免它们

### 某些编码模式更容易隐藏错误。编写高质量代码并了解大脑的工作原理可以帮助…

towardsdatascience.com

# 参考文献

[1] Bahmaninezhad, Fahimeh 等. “语音分离的统一框架。” *arXiv 预印本 arXiv:1912.07814* (2019)。

[2] 陈军等. “FullSubNet+: 基于通道注意力的 FullSubNet 与复杂谱图用于语音增强。” *ICASSP 2022–2022 IEEE 国际声学、语音与信号处理会议（ICASSP）*。IEEE，2022。 [`arxiv.org/abs/2203.12188`](https://arxiv.org/abs/2203.12188)

# Medium 会员

你喜欢我的写作并考虑订阅 Medium 会员以无限访问文章吗？

如果你通过此链接订阅，你将支持我而不会增加额外费用 [`medium.com/@mattiadigangi/membership`](https://medium.com/@mattiadigangi/membership)
