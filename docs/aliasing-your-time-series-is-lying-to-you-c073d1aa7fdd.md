# 混叠：你的时间序列在对你撒谎

> 原文：[`towardsdatascience.com/aliasing-your-time-series-is-lying-to-you-c073d1aa7fdd`](https://towardsdatascience.com/aliasing-your-time-series-is-lying-to-you-c073d1aa7fdd)

## 对信号混叠的直观介绍，使用 Python 实现

[](https://harrisonfhoffman.medium.com/?source=post_page-----c073d1aa7fdd--------------------------------)![哈里森·霍夫曼](https://harrisonfhoffman.medium.com/?source=post_page-----c073d1aa7fdd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c073d1aa7fdd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c073d1aa7fdd--------------------------------) [哈里森·霍夫曼](https://harrisonfhoffman.medium.com/?source=post_page-----c073d1aa7fdd--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c073d1aa7fdd--------------------------------) ·阅读时间：12 分钟·2023 年 7 月 12 日

--

![](img/63aeedd4da3a06e394880eb32ae045ff.png)

混叠风扇。图像由作者提供。

时间序列数据无处不在，且信息丰富。金融市场、工业过程、传感器读数、健康监测、网络流量和经济指标等应用场景中，时间序列分析和信号处理都是必要的。

随着深度学习和其他时间序列预测技术的崛起，一些时间序列的基本属性逐渐被忽视。在开始任何时间序列项目之前，我们必须问自己：“我们能相信这些数据吗？”

本文将探讨离散时间序列中的一个病态特性——混叠。任何关注时间序列的频率或季节性分析的人都必须对混叠及其对最终结果的影响有清晰的认识。我们将“时间序列”和“信号”这两个术语交替使用。请享受阅读！

# 一个激励性的例子

为了理解什么是混叠以及它可能造成的误导，我们先从一个经典示例开始。我们将尝试回答关于一个基础振荡信号的问题。如果你对混叠不熟悉，答案可能会让你震惊。

## 问题

考虑以下在一秒钟内绘制的时间序列。每个点代表从信号中采样的一个样本，线条是通过样本的线性插值，帮助我们可视化信号。

![](img/09b7597b57312bb526f98592e46b1baa.png)

一个在一秒钟内采样的振荡信号样本。图像由作者提供。

此外，假设我们采样的基础信号是 [连续的](https://en.wikipedia.org/wiki/Continuous_or_discrete_variable)。这意味着，在任何时刻 t，都可以测量信号的值。由于计算和内存限制，我们选择有限的时间点来采样信号。

我们需要回答的问题是：

> 底层信号有多少个峰值？

换句话说，信号的[频率](https://en.wikipedia.org/wiki/Frequency)是多少？在继续阅读答案之前，认真思考这个问题。你会如何回答它？

也许直观的第一步是计算等于 1 的点数。通过这样做，你可能会说信号在一秒钟内有 10 个峰值。也就是说，信号的[基本频率](https://en.wikipedia.org/wiki/Fundamental_frequency)是 10 赫兹（Hz），即每秒 10 次重复出现。

## 答案

> 没有更多的信息，我们不可能知道底层信号有多少个峰值。

你没看错。如果我们只有这个示例中的数据，就不可能百分之百确定底层信号的基本频率。幸运的是，我们知道这些数据是如何生成的以及真实的频率内容应该是什么。

不管你信不信，底层信号在一秒内有 90 个峰值。也就是说，基本频率不是 10 Hz，而是 90 Hz。这意味着，在一秒钟的时间里，信号会振荡 90 次。下面是信号的方程：

![](img/85cdffc1c801c1efd80ee2f12f29e9d2.png)

底层信号的方程。图像作者提供。

这个信号是一个频率为 90 Hz 的纯余弦信号，上面的图表在一秒内从这个信号的*离散时间*点进行采样。如果你像我一样仍然难以接受这一点，可以考虑生成示例的 Python 代码：

```py
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
sns.set()

SAMPLING_RATE = 100 # The sampling rate in Hz
NUM_SECONDS = 1 # The duration of the signal in seconds
FS = 90 # The fundamental frequency of the signal in Hz

# Create the array of time samples
t = np.arange(0, NUM_SECONDS, 1 / SAMPLING_RATE)

# Create the arrat of signal values
signal_values = np.cos(2*np.pi*FS*t)

# Plot the signal
fig, ax = plt.subplots(figsize=(10, 6))
ax.plot(t, signal_values)
ax.scatter(t, signal_values)
ax.set_xlabel('Time (s)')
ax.set_ylabel('Signal Value')
ax.set_title('A Signal Sampled Over 1 Second')
ax.legend()
plt.show()
```

这段代码绘制了纯 90 Hz 余弦信号的样本，正如方程所示。那么为什么我们在原始图中只看到 10 个峰值呢？答案在于`SAMPLING_RATE`参数。

我们每秒对信号进行 100 次采样，即 100 Hz。时间数组` t = np.arange(0, NUM_SECONDS, 1 / SAMPLING_RATE)`生成从 0 到 1 的 100 个时间点，我们在这些点上对余弦信号进行采样。时间数组如下：

```py
>>> NUM_SECONDS = 1
>>> SAMPLING_RATE = 100
>>> t = np.arange(0, NUM_SECONDS, 1 / SAMPLING_RATE)
>>> t
array([0\.  , 0.01, 0.02, 0.03, 0.04, 0.05, 0.06, 0.07, 0.08, 0.09, 0.1 ,
       0.11, 0.12, 0.13, 0.14, 0.15, 0.16, 0.17, 0.18, 0.19, 0.2 , 0.21,
       0.22, 0.23, 0.24, 0.25, 0.26, 0.27, 0.28, 0.29, 0.3 , 0.31, 0.32,
       0.33, 0.34, 0.35, 0.36, 0.37, 0.38, 0.39, 0.4 , 0.41, 0.42, 0.43,
       0.44, 0.45, 0.46, 0.47, 0.48, 0.49, 0.5 , 0.51, 0.52, 0.53, 0.54,
       0.55, 0.56, 0.57, 0.58, 0.59, 0.6 , 0.61, 0.62, 0.63, 0.64, 0.65,
       0.66, 0.67, 0.68, 0.69, 0.7 , 0.71, 0.72, 0.73, 0.74, 0.75, 0.76,
       0.77, 0.78, 0.79, 0.8 , 0.81, 0.82, 0.83, 0.84, 0.85, 0.86, 0.87,
       0.88, 0.89, 0.9 , 0.91, 0.92, 0.93, 0.94, 0.95, 0.96, 0.97, 0.98,
       0.99])
```

数组中有 100 个时间样本，因为我们以 100 Hz 的频率对信号进行采样，持续 1 秒。问题是**100 Hz 的采样率不够高**，无法捕捉到 90 Hz 振荡的信号——这种现象称为**混叠**。我们可以通过以下图表看到混叠现象：

![](img/6e02497b62f7780389aeeee03af947a4.png)

混叠信号。图像作者提供。

在这个图中，我们放大了信号的前 0.25 秒，以更好地查看混叠现象。橙色线表示底层信号——一个 90 Hz 的余弦函数。蓝色点表示以 100 Hz 采样，即每 0.01 秒采样一次。这些样本显然不能很好地近似信号。每秒采样 100 次，或者在 0.25 秒内采样 25 次，不足以表征 90 Hz 的余弦信号。

要捕捉到更高频率的信息，我们必须提高系统的采样率：

![](img/437c47de827bdf02388b7140b1d9e904.png)

系统采样率的增加。图片由作者提供。

增加采样率导致信号的更好近似可能不会令人惊讶。然而，这一现象的含义深远。这个例子引发了两个关键问题：

+   我们需要多频繁地采样信号才能充分近似其行为？

+   我们可以检测或防止混叠现象吗？

我们将在本文中尝试在较高层次上解决这些问题。

# 奈奎斯特-香农采样定理

了解什么是混叠及其对时间序列数据的灾难性影响后，我们需要知道应该以多频繁的采样频率来保留感兴趣的特征。[奈奎斯特-香农采样定理](https://www.youtube.com/watch?v=FcXZ28BX-xE)为我们提供了这方面的见解。

奈奎斯特-香农采样定理有几个变种，但我们感兴趣的说法大致如下：

> 如果一个连续信号中的最高频率是 F 单位/时间，你可以通过至少以 2F 单位/时间的频率采样来捕捉信号中的所有信息。

这个简单却强大的定理为我们提供了正确从[带限信号](https://en.wikipedia.org/wiki/Bandlimiting)中采样所需的一切。这意味着奈奎斯特定理假设底层信号中有有限数量的频率。

一如既往，示例将帮助我们更好地理解奈奎斯特定理及其含义。

## 示例——一个纯余弦信号

让我们回到以 100 Hz 采样的 90 Hz 余弦信号。这是代码：

```py
SAMPLING_RATE = 100 # The sampling rate in Hz
NUM_SECONDS = 1 # The duration of the time series
FS = 90 # The fundamental frequency of the signal

# Create the array of time samples
t = np.arange(0, NUM_SECONDS, 1 / SAMPLING_RATE)

# Create the array of signal values
signal_values = np.cos(2*np.pi*FS*t)

# Plot the signal
fig, ax = plt.subplots(figsize=(10, 6))
ax.plot(t, signal_values)
ax.scatter(t, signal_values)
ax.set_xlabel('Time (s)')
ax.set_ylabel('Signal Value')
ax.set_title(f'$cos(2 \pi {FS}t$) Sampled at {SAMPLING_RATE} Hz')
ax.legend()
plt.show()
```

以及在 1 秒钟内绘制的采样信号：

![](img/6b5379bb697c68b4c7cd33f0fc036c01.png)

一个混叠信号。图片由作者提供。

接下来，我们将通过计算采样信号的快速[傅里叶变换](https://www.thefouriertransform.com/) (FFT)来分析其[频域](https://en.wikipedia.org/wiki/Frequency_domain#:~:text=In%20mathematics%2C%20physics%2C%20electronics%2C,to%20frequency%2C%20rather%20than%20time.)。FFT 为我们提供了信号频率内容的更清晰图像，这将加深我们对混叠的理解。这是计算采样信号 FFT 的代码：

```py
from scipy.fft import fft, fftfreq

# Compute the FFT of the sampled signal
power = np.abs(fft(signal_values))
freqs = fftfreq(n=len(power), d=1/SAMPLING_RATE)

# Plot the FFT with a stem plot
fig, ax = plt.subplots(figsize=(10, 6))
ax.stem(freqs, np.abs(power))
ax.set_xlabel('Frequency (Hz)')
ax.set_ylabel('Power')
ax.set_title(f'The FFT of $cos(2 \pi {FS}t)$ Sampled at {SAMPLING_RATE} Hz')
```

以及对应的 FFT 图：

![](img/0c241b92b2b1604137f26e2d5b8fbe1a.png)

混叠信号的 FFT。图片由作者提供。

这个图展示了信号中每个频率的功率或强度。在这个例子中，采样信号中只有一个频率，这通过-10 Hz 和 10 Hz 处的尖峰表示。出现[负频率](https://www.khanacademy.org/science/electrical-engineering/ee-circuit-analysis-topic/ee-ac-analysis/v/ee-negative-frequency#:~:text=Real%20sinusoidal%20signals%20have%20only,one%20with%20a%20negative%20exponent.) 是因为这是一个实值信号，而 FFT 是一个复值变换。对于这个例子，你只需要知道负轴是正轴的镜像。

在查看这个 FFT 时，我们应当注意两个重要观察点。首先是所有频率内容都集中在正负 10 Hz，即便底层信号是 90 Hz 的余弦波。第二个观察点是频率轴的绝对值仅达到 50 Hz，这只是 100 Hz 采样率的一半。从中我们可以得出以下结论：

1.  由于奈奎斯特定理的影响，FFT 只能检测到采样信号中低于采样率一半的频率。

1.  如果底层信号中存在高于采样率一半的频率，**真实频率将会混叠成一个虚假的较低频率**。

为了进一步巩固这个概念，请注意底层信号频率增加时采样信号及其 FFT 的变化：

![](img/04ed328062c595b5b788ed0d99d776be.png)

以 100 Hz 从更高频率的余弦波进行采样。图片由作者提供。

在这个动画中，我们观察到 FFT 可以检测到信号中逐渐增加的频率，直到奈奎斯特频率 50 Hz。一旦底层频率超过 50 Hz，信号开始在相反方向上出现混叠，FFT 检测到的频率变得越来越低。特别是，超过 50 Hz 的频率会预测性地映射回采样信号中 0 到 500 Hz 之间的频率。

对于这个例子，如果我们想检测 90 Hz 的余弦波，我们需要将采样率提高到 180 Hz 以上，以满足奈奎斯特定理。以下是一个动画，展示了增加采样率对采样信号及其 FFT 的影响：

![](img/1f1c5745fefa441ebe3ec18036872d3b.png)

增加采样率。图片由作者提供。

随着采样率的增加，FFT 的频域增加到采样率的一半（奈奎斯特频率）的正负范围内。一旦采样率超过 180 Hz，FFT 中的主频保持在正负 90 Hz，没有混叠现象。

> **附加信息：** 你可能注意到 FFT 在某些采样率下有超过两个非零值。这是一种被称为 [谱泄漏](https://dspillustrations.com/pages/posts/misc/spectral-leakage-zero-padding-and-frequency-resolution.html) 的现象，当采样信号不是完全周期性的时就会发生这种情况。实际操作中有一些解决方法，但这通常是采样的一个限制。

## 实际应用

在这个例子之后，你可能仍在想混叠和奈奎斯特定理是否在你的时间序列项目中相关。虽然这取决于应用和项目要求，但以下是一些一般性的指导原则：

1.  **传感器数据：** 在收集来自传感器的数据时，如温度传感器、压力传感器或加速度计，采样率需要仔细选择以避免混叠。如果传感器输出包含高频成分，采样率不足可能会导致混叠。

1.  **音频和音乐处理：** 在数字音频处理过程中，当采样[模拟](https://saving.em.keysight.com/en/knowledge/glossary/oscilloscopes/what-is-an-analog-signal-meaning-definition#:~:text=at%20set%20intervals.-,What%20is%20an%20analog%20electrical%20signal%3F,all%20examples%20of%20analog%20signals.)音频信号时可能会发生混叠。如果采样率不足以捕捉音频信号的整个频率范围，高频成分可能会折叠回到可听范围内，导致不必要的伪影和失真。

1.  **视频处理：** 视频信号也需要以足够高的采样率进行采样以避免混叠。如果采样率过低，视频信号中的高频成分可能会导致混叠，造成视觉伪影和图像质量下降。

1.  **金融时间序列分析：** 在金融市场中，高频交易涉及在非常短的时间间隔内捕捉市场数据。如果采样率不足，混叠可能会扭曲数据中的潜在模式，从而导致错误的交易决策和财务分析。这是分析接近交易级别的高频分辨率金融时间序列的一个动机，而不是将数据聚合到任意时间区间。

> 在任何时间序列项目中，成功的关键之一是对数据和领域有深入的了解。将数据聚合到较低的频率是否有意义，还是会消除过多有价值的信息？始终检查你的假设，并确保你在正确的分辨率下分析时间序列。

# 克服混叠

现在我们了解了混叠的定义和发生情况，我们应该问自己如何对抗它。多年的时间序列分析和信号处理研究已经投入到混叠问题中，我们不会在这里深入探讨解决方案。相反，这里有三种常见的方法来处理实际中的混叠问题。

## 增加采样率

对抗混叠的最有效方法是提高采样率。然而，这种方法会带来更高的计算成本和数据存储需求。简而言之，如果你怀疑有混叠问题，并且有条件的话，增加采样率是最佳选择。

## 抗混叠滤波器

[抗混叠](https://en.wikipedia.org/wiki/Anti-aliasing_filter)滤波器是信号处理系统中的关键组件。这些滤波器旨在衰减或消除高于奈奎斯特频率的频率成分，防止它们混叠并破坏信号。通过在采样或降采样信号之前应用抗混叠滤波器，我们可以去除不需要的高频成分，确保信号的清晰和准确表示。常用的抗混叠滤波器设计包括巴特沃斯滤波器、切比雪夫滤波器和椭圆滤波器，每种滤波器都有不同的性能与复杂性权衡。

## 压缩感知

[压缩感知](https://www.youtube.com/watch?v=SbU1pahbbkc)是一种相对较新的技术，能够利用有限数量的非均匀样本在远低于奈奎斯特率的情况下重建稀疏信号。这种方法利用信号在特定领域（如小波或傅里叶分析）中的稀疏性或可压缩性，促进了精确的信号恢复，同时避免了别名效应。

虽然压缩感知可能并不适用于所有场景，但当其适用时效果显著。特别是，压缩感知在解决图像压缩挑战方面取得了显著成功，因为图像本身具有固有的可压缩性。

# 最终思考

别名效应是离散时间序列的一个基本限制，它可能会误导我们并导致数据解释错误。本文通过一个简单的振荡信号示例提供了对别名效应的直观介绍。我们了解到，当采样率不足以准确捕捉潜在信号的频率内容时，别名效应就会发生。

奈奎斯特-香农采样定理被引入作为确定避免别名所需最小采样率的指导原则。通过以信号中最高频率的两倍进行采样，我们可以保留原始信号中的信息。

对各种领域中的别名效应的实际影响进行了讨论，例如传感器数据、音频和音乐处理、视频处理以及金融时间序列分析。在每种情况下，选择合适的采样率对于避免别名伪影和失真至关重要。

为了应对别名效应，提出了三种常见的方法：提高采样率、使用抗别名滤波器以及采用压缩感知技术。每种方法都有其优点和考虑因素，具体取决于应用和资源限制。

理解别名效应及其影响对任何处理时间序列数据的人来说都是至关重要的。通过了解别名效应并采用适当的技术，我们可以确保数据分析的准确性和可靠性，从而做出更明智的决策和洞察。

*成为会员:* [*https://harrisonfhoffman.medium.com/membership*](https://harrisonfhoffman.medium.com/membership)

# 参考文献

> [`en.wikipedia.org/wiki/Fundamental_frequency`](https://en.wikipedia.org/wiki/Fundamental_frequency)
> 
> [`en.wikipedia.org/wiki/Frequency`](https://en.wikipedia.org/wiki/Frequency)
> 
> [`en.wikipedia.org/wiki/Anti-aliasing_filter`](https://en.wikipedia.org/wiki/Anti-aliasing_filter)
> 
> [`www.youtube.com/watch?v=SbU1pahbbkc`](https://www.youtube.com/watch?v=SbU1pahbbkc)
> 
> [`medium.com/r?url=https%3A%2F%2Ftowardsdatascience.com%2Ffinancial-machine-learning-part-0-bars-745897d4e4ba`](https://medium.com/r?url=https%3A%2F%2Ftowardsdatascience.com%2Ffinancial-machine-learning-part-0-bars-745897d4e4ba)
> 
> [`www.youtube.com/watch?v=FcXZ28BX-xE`](https://www.youtube.com/watch?v=FcXZ28BX-xE)
