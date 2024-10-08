# 语音增强介绍：第二部分 — 信号表示

> 原文：[`towardsdatascience.com/introduction-to-speech-enhancement-part-2-signal-representation-ab1deca2fa74`](https://towardsdatascience.com/introduction-to-speech-enhancement-part-2-signal-representation-ab1deca2fa74)

## 让我们深入了解信号表示、傅里叶变换、谱和谐波。

[](https://medium.com/@mattiadigangi?source=post_page-----ab1deca2fa74--------------------------------)![Mattia Di Gangi](https://medium.com/@mattiadigangi?source=post_page-----ab1deca2fa74--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ab1deca2fa74--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ab1deca2fa74--------------------------------) [Mattia Di Gangi](https://medium.com/@mattiadigangi?source=post_page-----ab1deca2fa74--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ab1deca2fa74--------------------------------) ·阅读时间 10 分钟·2023 年 1 月 31 日

--

![](img/089f61fe7f5fa79d6f139a1233f7e860.png)

图片由 [Richard Horvath](https://unsplash.com/@orwhat?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本文是系列的一部分：

+   [语音增强介绍：第一部分 — 概念与任务定义](https://medium.com/towards-data-science/introduction-to-speech-enhancement-part-1-df6098b47b91)

+   **语音增强介绍：第二部分 — 信号表示**

## 介绍

在深入语音增强的讨论之前，我们需要明确一些关于数字信号处理的概念。在本系列的前一章中，我们介绍了一些概念并添加了进一步阅读的链接。

在第二部分中，我们将探讨数字信号的表示方式、如何更改表示方式，以及傅里叶变换为何如此重要。代码和图示将帮助你理解并进一步探索这些例子。

## 信号基础

本文的主要目标是理解什么是信号，它如何表示，以及傅里叶变换的重要性，这是一种经过时间考验的根据需求改变表示方式的方法。

音频信号通常表示为随时间变化的振幅。由于我们讨论的是**数字**信号处理，记录的音频由在规则间隔收集的许多**样本**组成。通常测量的是**采样频率**而不是样本之间的间隔长度。**采样频率**或**采样率**是每秒钟的样本数量。秒的倒数（*1/s*）称为赫兹（Hz）。如果我们在一秒钟内有 10 个样本，则采样率为 10Hz。如果我们有 1000 个样本，则为 1 kHz（千赫兹）或 1000 Hz，依此类推，使用常见的科学前缀。

![](img/fcd4dd22f27b62d80ed90d6dc5484469.png)

振幅为 1.0，频率为 10 Hz，采样率为 10 kHz 的正弦信号

与频率为 1Hz 的正弦信号进行比较。它看起来比之前的信号“更宽”。我们可以想象调整频率就像拉伸弹簧一样，

![](img/afdbdb8a4139e9f0617a61ac10c95cb4.png)

振幅为 1.0，频率为 1 Hz，采样率为 10 kHz 的正弦信号

采样率影响信号的感知质量，因为采样率不足会导致降级，如下图所示。

![](img/2065ca8368fbe0de938a5d9deb39a8b9.png)

振幅为 1.0，频率为 10 Hz，采样率为 100 Hz 的正弦信号

在一些极其不足的采样率情况下，记录的信号可能完全不像原始的模拟信号：

![](img/bf60dc3ca6241c5dc3ffd6d21a11c4a3.png)

相同信号的采样率为 10 Hz。样本均匀分布，并对应于函数的零值。请注意 y=0.0 处的小水平蓝线

这些基本示例向我们展示了选择足够高采样率的重要性。对于人类语音，采样率从 8kHz 开始，这是电话的采样率，当需要高精度时，可以轻松达到 44.1 kHz。上述绘制的信号可以通过以下代码重现。

```py
import matplotlib.pyplot as plt
import numpy as np

def sin_signal(amp, freq, phase, sr):
    """Generate a sin signal with the given characteristics"""
    x = np.arange(0, 2*np.pi, 1./sr)  # 2*pi is a full period for a sinusoid
    return amp*np.sin(freq*2*np.pi*x - phase)  # full formula for a sin-shaped signal

def plot_signal(s):
  x_ax = np.linspace(0, 1, len(s))
  plt.plot(x_ax, s)

# Change these values to modify the signal
amp = 1.0    # signal maximum amplitude
freq = 10    # sin frequency
phase = 0.0  # signal phase
sr = 10000   # sampling rate

sig = sin_signal(amp, freq, phase, sr)
plot_signal(sig)
```

到目前为止，我们观察到改变频率可以使信号在水平轴上“变窄”或“变宽”，而采样率会影响其精度。我们还可以通过修改振幅在垂直轴上拉伸或压缩它：

![](img/3353f69db721220df5f5b6a9bd2368e1.png)

振幅为 3.0，频率为 10 Hz，采样率为 10 kHz 的正弦信号

![](img/8c1462c2bf2a141a49e8f1509ce870ee.png)

振幅为 0.5，频率为 10 Hz，采样率为 10 kHz 的正弦信号

这两个信号看起来可能相同，但快速查看垂直轴标签会发现，第一个信号的范围是从 -3.0 到 3.0，而第二个信号的范围是从 -0.5 到 0.5。最大（绝对）值与我们选择的振幅值相匹配。

我们需要理解的最后一个入门概念是信号的**相位**。它可以被认为是相对于参考起始时间的**时间移位**，并以弧度表示。让我们通过观察一个可视化的例子来理解它。之前展示的正弦信号的相位为 0，其函数可以表示为 sin(10 * 2π*t*)，其中 10 是频率，2π是正弦函数的一个完整**周期**。周期是函数重复自身的间隔。现在，我们可以添加一个 π 弧度的相位，得到以下图像：

![](img/c6a9efaf0a6acc00a14c119a0537ecb7.png)

频率为 10 Hz，采样率为 10 Khz，相位为π的正弦波

我们可以看到，它就像是信号“向左移动”了 π 弧度的持续时间。我们也可以通过减去 π 的相位将其向右移动，但这样做不会显著显示（因为向左或向右移动 π，函数的样子是一样的），所以我们将其向右移动 π/2：

![](img/7e7e3f5ef3890f71b9b158d284717d7d.png)

频率为 10 Hz，采样率为 10 Khz，相位为-π/2 的正弦波

我们之所以说“添加”和“减去”，是因为公式是 sin(*f* * 2π*t* + *θ*)，其中 *f* 是频率，*θ* 是相位。

现在我们引入了一些概念，可以回到傅里叶变换及其如何帮助我们更好地理解信号。

## 傅里叶变换

本节的目标是为读者提供傅里叶变换的实用概念。关于傅里叶变换理论的资源很多，世界上不需要新的。建议感兴趣的读者跟随链接并查找更多资源。

傅里叶变换的基本假设是任何**平稳信号**都可以分解为不同频率的正弦函数的和，每个函数具有各自的振幅和相位。一个以正弦信号之和表示的信号被称为“频域”，而我们之前看到的则是在“时域”。

对信号应用傅里叶变换的结果，也称为*分析*，是一系列复数系数，每个系数对应于一个特定的频率。为什么是复数系数？我们可以将任何[复数](https://en.wikipedia.org/wiki/Complex_number) *z* 用极坐标表示为 *z = |z|e^(jθ)*，其中 *|z|* 是振幅，*θ* 是角度（或相位）。振幅和相位正是完全描述已知频率的正弦信号所需的全部信息。

傅里叶变换有不同类型，但最常见的是[离散傅里叶变换](https://en.wikipedia.org/wiki/Discrete_Fourier_transform#cite_ref-Strang_1-0)（DFT）及其逆变换，即离散傅里叶逆变换（IDFT），这是我们处理离散信号记录所需的变换。

离散信号 x(n) 长度为 N 的离散傅里叶变换（DFT）定义为：

X(k) = Σ x(n) * e^(-j*2*π*k*n/N) 对于 k = 0,1,2,…,N-1

其中 X(k) 是离散傅里叶变换（DFT）的复数系数，k 是频率索引，n 是时间索引，N 是信号的长度，j 是虚数单位。

IDFT 是 DFT 的逆变换，它允许我们从 DFT 系数中恢复原始信号。

x(n) = 1/N * Σ X(k) * e^(j*2*π*k*n/N) 对于 n = 0,1,2,…,N-1

DFT 系数可以用来分析信号的频率内容，而逆离散傅里叶变换（IDFT）可以用来从频率分量合成信号。DFT 和 IDFT 被广泛应用于超出音频处理的许多领域，如图像和视频处理、通信系统和深度学习。

DFT 的一个重要属性是**线性，即**，在变换之前对信号进行的加法和乘法操作与对变换值进行的加法和乘法操作结果相同，反之亦然，因为逆离散傅里叶变换（IDFT）具有相同的属性。这使我们能够在频域中修改信号，我们可以修改各个分量，并且对时域信号的结果可以轻松预测。

## 信号的频谱表示

信号的频谱表示是将信号的频率内容在频域中表示的一种方法。

DFT 系数的幅度表示信号的幅度谱，幅度谱代表信号中每个频率分量的强度。幅度谱通常以分贝（dB）表示，分贝是对数尺度，使得解释不同频率分量的相对强度变得更容易。

![](img/f5fecedc1d6fc139c255d34facb9e2a2.png)

频率为 440Hz，幅度为 1.0，初相位为 0.0 的正弦波的频谱。唯一的非零值出现在频率 440 Hz 处。

相位图更难以解释。

![](img/d070a7dfaabc0dbffddd38dac3e4c1c6.png)

相同正弦波的相位谱

这两个图可以通过以下代码生成：

```py
import matplotlib.pyplot as plt
import numpy as np

freq = 440
duration = 1.0
sr = 10000
n = round(duration * sr)
x = np.arange(0, n) / sr
a = np.sin(freq*2*np.pi*x)
f = np.fft.rfft(a)
freqs = np.fft.rfftfreq(n, 1./sr)
plt.plot(freqs, np.abs(f))
plt.plot(freqs, np.angle(f))
```

在图中，我们有复数函数来获取幅度和角度。

现在让我们看一个低通滤波器的示例，该滤波器减少高频的影响。我们通过将不同频率的正弦波相加来生成一个信号，然后对频率高于 1000 的信号幅度除以 2。

```py
import matplotlib.pyplot as plt
import numpy as np

freqs = np.arange(10, 5000, 100)
duration = 1.0
sr = 10000
n = round(duration * sr)
x = np.arange(0, n) / sr
signal = sum(np.sin(freq*2*np.pi*x) for freq in freqs)
plt.plot(x, signal)
```

![](img/17b70c262a04a1c15ce626c9a3b4e58c.png)

相同幅度的正弦波的和

频谱：

![](img/d0383063c475311dbfd3458f69d19587.png)

上述信号的频谱。所有非零频率的贡献相同。

现在我们按如下方式应用低通滤波器：

```py
low_pass = np.array(f)
low_pass[freqs > 1000] = low_pass[freqs > 1000] / 2
plt.plot(freqs, np.abs(low_pass))
```

获取可预测的频谱

![](img/a2559193809d9595e33f96ffc8a47f28.png)

应用低通滤波器后的频谱

请注意，这仅仅意味着感兴趣的频率的幅度已被除以二。由于我们仅使用正弦波，因此映射是直接的，但如果我们从不同的信号开始，情况也是如此。

最后，我们可以恢复修改后的信号。

```py
sig_ = np.fft.irfft(low_pass)
plt.plot(x, sig_)
```

得到如下信号

![](img/773c3785d477bfbe5f17c953f9ea10fe.png)

从 irfft 重建的信号

这与起始信号相似，但缩小了近乎两倍。

## 谐波

现在让我们观察一个比正弦波更复杂的信号，例如一个三角形状的信号：

![](img/477976189445aa5dfe10667396f3a77b.png)

频率为 10、幅度为 1、相位为 0 的三角形信号

我们可以用以下代码生成它：

```py
import matplotlib.pyplot as plt
import numpy as np

freq = 10
sr = 10000
ts = np.linspace(0, 1, sr)
x = freq * ts
frac, _ = np.modf(x)
y = (np.abs(frac - 0.5) - 0.25) * 4
plt.plot(x, y)
```

基本思想是我们只取 xs 的小数部分，它们具有恒定的增量，然后取与 0.5 的差的绝对值。这就形成了三角形状。然后，我们有一个偏置和缩放，将信号从[0, 0.5]转换到[-1, 1]。

现在让我们看看这个信号的幅度谱。我们将频率更改为更高的值，例如 440 Hz，以便于可视化：

![](img/38d7965e6a7003cd03ad634a8b96d98e.png)

频率为 440 Hz 的三角形信号的频谱

与正弦信号不同，这个信号有多个非零值。如果你认为它们的分布中存在某种模式，那么你是对的！一个三角形状的信号由一个相对强的正弦信号组成，频率相同，称为**基频**，在这种情况下是 440Hz。其他频率称为谐波，是基频的**倍数**。图中分辨率不高，但这些谐波的频率为：1320 Hz、2200 Hz、3080 Hz 等，覆盖了**偶数**倍数。最后一个是三角形信号的特性，而一般情况下，谐波可以是基频的所有倍数。基频很重要，因为通常情况下，信号的感知**音高**取决于基频，即使它不是最显著的，即最高的系数。

此外，你还可以看到更多（非常小的）非零系数。这是数字信号处理中的一个伪影，称为**混叠**。在对信号进行数字表示的采样时，我们丢失了两个点之间发生的情况的信息。对于低频来说这不是问题，因为我们有足够的样本来进行插值，但对于高频来说，这会变成一个明显的问题。

在上面的图中，我们可以看到频率高达 5000 Hz，但第 7 个谐波会是 5720 Hz。采样的效果是造成频率的“[折叠](https://en.wikipedia.org/wiki/Nyquist_frequency)”，也就是在 5000 之后我们开始向回计数。然后，第 7 个谐波被检测为 4280：5000 — (5720–5000)。混叠频率继续向回，直到达到 0，然后再折回到另一个方向。

采样率越高，混叠效应越低，信号越清晰。

# 结论

在这篇文章中，我们进入了信号表示和傅里叶变换的世界。理解基本术语及其在信号中的意义对于继续本系列的内容非常重要。

文章中充满了图表和代码，以帮助读者以实用的方式理解这些概念，并让读者轻松地进行示例操作。

在本系列的下一部分中，我们将通过探索混响开始进行语音增强。

感谢您读到这里，敬请关注下一部分！

[](/python-polymorphism-with-class-discovery-28908ac6456f?source=post_page-----ab1deca2fa74--------------------------------) ## Python 多态与注册 | Python 模式

### 学习一种模式以在扩展 Python 代码功能时隔离包。

[towardsdatascience.com [](/tips-for-reading-and-writing-an-ml-research-paper-a505863055cf?source=post_page-----ab1deca2fa74--------------------------------) ## 阅读和撰写 ML 研究论文的技巧

### 通过数十次同伴评审所获得的经验教训

[towardsdatascience.com [](/pick-your-deep-learning-tool-d01fcfb86845?source=post_page-----ab1deca2fa74--------------------------------) ## 选择您的深度学习工具

### 为什么您的工具可能取决于您组织的团队结构

[towardsdatascience.com

# 深入阅读

[Think DSP](https://github.com/AllenDowney/ThinkDSP) 第二章详细讲解了本文的概念。

# Medium 会员

您是否喜欢我的写作，并考虑订阅 Medium 会员以获得无限访问权限？

如果通过此链接订阅，您将支持我，而您无需额外支付费用 [`medium.com/@mattiadigangi/membership`](https://medium.com/@mattiadigangi/membership)
