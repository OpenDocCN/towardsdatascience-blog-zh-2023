# 傅里叶变换，实用的 Python 实现

> 原文：[https://towardsdatascience.com/fourier-transform-the-practical-python-implementation-acdd32f1b96a?source=collection_archive---------0-----------------------#2023-02-27](https://towardsdatascience.com/fourier-transform-the-practical-python-implementation-acdd32f1b96a?source=collection_archive---------0-----------------------#2023-02-27)

## 真实世界信号的实际应用

[](https://medium.com/@omar.ok1998?source=post_page-----acdd32f1b96a--------------------------------)[![Omar Alkousa](../Images/7598618abe8e8fa89f1d8a4bfc21f014.png)](https://medium.com/@omar.ok1998?source=post_page-----acdd32f1b96a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----acdd32f1b96a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----acdd32f1b96a--------------------------------) [Omar Alkousa](https://medium.com/@omar.ok1998?source=post_page-----acdd32f1b96a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff8302b9534b5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffourier-transform-the-practical-python-implementation-acdd32f1b96a&user=Omar+Alkousa&userId=f8302b9534b5&source=post_page-f8302b9534b5----acdd32f1b96a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----acdd32f1b96a--------------------------------) ·10分钟阅读·2023年2月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Facdd32f1b96a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffourier-transform-the-practical-python-implementation-acdd32f1b96a&user=Omar+Alkousa&userId=f8302b9534b5&source=-----acdd32f1b96a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Facdd32f1b96a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffourier-transform-the-practical-python-implementation-acdd32f1b96a&source=-----acdd32f1b96a---------------------bookmark_footer-----------)

傅里叶变换是信号处理和时间序列分析中最著名的工具之一。快速傅里叶变换（FFT）是傅里叶变换在数字信号上的实际应用。FFT被认为是20世纪对科学和工程影响最大的前10个算法之一 [**[1]**](https://doi.ieeecomputersociety.org/10.1109/MCISE.2000.814652)。

![](../Images/ac064d5315ad701a5a208c5466249a07.png)

图片由 [Edz Norton](https://unsplash.com/@edznorton?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇文章中，讨论了FFT的实际应用，即如何使用它来表示信号数据的频率域（谱），并使用 Plotly 绘制谱图，以提供更多互动性，并更好地理解谱中的特征。到文章末尾，我们将构建一个类来分析信号。提供了一个 ECG 信号及其谱作为最终示例。

![](../Images/10097e595e8555e1baedbda84a9ba92a.png)

使用我们类 Fourier 的表示 [作者提供的图片]

# 介绍

傅里叶变换（FT）将信号的时间域与其频率域联系起来，其中频率域包含构成信号的正弦波（幅度、频率、相位）的信息。由于FT是一个连续变换，离散傅里叶变换（DFT）在数字世界中成为适用的变换，它以离散格式的样本集合形式保存信号信息，其中采样定理是离散化和信号的严格规则。具有 N 个样本的信号（xn）的DFT 由以下方程给出 [**[2]**](https://pythonnumericalmethods.berkeley.edu/notebooks/chapter24.02-Discrete-Fourier-Transform.html)：

![](../Images/aadc5aa1519714b6ab1f6b92d4cab792.png)

DFT 方程 [[2]](https://pythonnumericalmethods.berkeley.edu/notebooks/chapter24.02-Discrete-Fourier-Transform.html)

**其中：**

+   N: 样本数量

+   n: 当前样本

+   k: 当前频率，其中 k∈[0,N−1]

+   xn: 样本 n 的正弦值

+   Xk: 包含幅度和相位信息的DFT

DFT（Xk）的输出是一个复杂数数组，包含频率成分的信息 [**[2]**](https://pythonnumericalmethods.berkeley.edu/notebooks/chapter24.02-Discrete-Fourier-Transform.html)。直接应用DFT到信号上需要进行复杂的计算。幸运的是，已经开发了快速傅里叶变换（FFT） [**[3]**](https://www.ams.org/journals/mcom/1965-19-090/S0025-5718-1965-0178586-1/) 来提供DFT的更快实现。FFT利用了DFT输出的对称性。我们将不会进一步讨论FFT的工作原理，因为它类似于DFT的标准实际应用。如果你需要更多细节，请参见 [**[3]**](https://www.ams.org/journals/mcom/1965-19-090/S0025-5718-1965-0178586-1/)。

# 代码示例：

我们将简单地开始理解我们在这篇文章中使用的每种方法的输入和输出。首先，我们将导入所需的包。Numpy 用于处理矩阵和计算。我们从 scipy.fft 模块（fft、rfft、fftfreq、rfftfreq）中导入帮助我们进行傅里叶分析相关计算的方法。最后，Plotly 和 matplotlib 用于可视化。

## 让我们开始吧…

```py
# Import the required packages
import numpy as np
from scipy.fft import fft, rfft
from scipy.fft import fftfreq, rfftfreq
import plotly.graph_objs as go
from plotly.subplots import make_subplots
import matplotlib.pyplot as plt
%matplotlib inline
```

我们需要信号来测试我们的代码。正弦波非常适合我们的示例。在接下来的代码中，我们使用一个名为Signal的类生成正弦信号，你可以在以下[**GitHub gist**](https://gist.github.com/OmarAlkousa/4bd0bacb0ff976be4105777965854e06)中找到现成的。我们将使用这个类生成的信号包含三个正弦波（1、10、20）Hz，幅度分别为（3、1、0.5）。采样率将是200，信号的持续时间为2秒。

```py
# Generate the three signals using Signal class and its method sine()
signal_1hz = Signal(amplitude=3, frequency=1, sampling_rate=200, duration=2)
sine_1hz = signal_1hz.sine()
signal_20hz = Signal(amplitude=1, frequency=20, sampling_rate=200, duration=2)
sine_20hz = signal_20hz.sine()
signal_10hz = Signal(amplitude=0.5, frequency=10, sampling_rate=200, duration=2)
sine_10hz = signal_10hz.sine()

# Sum the three signals to output the signal we want to analyze
signal = sine_1hz + sine_20hz + sine_10hz

# Plot the signal
plt.plot(signal_1hz.time_axis, signal, 'b')
plt.xlabel('Time [sec]')
plt.ylabel('Amplitude')
plt.title('Sum of three signals')
plt.show()
```

![](../Images/5f8605af3acfa389493fe5d21456cf2b.png)

我们接下来要处理的信号。[作者提供的图片]

该信号的傅里叶变换可以使用scipy包中的(fft)计算，如下所示 [**[4]**](https://docs.scipy.org/doc/scipy/reference/generated/scipy.fft.fft.html#scipy.fft.fft)：

```py
# Apply the FFT on the signal
fourier = fft(signal)

# Plot the result (the spectrum |Xk|)
plt.plot(np.abs(fourier))
plt.show()
```

![](../Images/47cc5e61eeb983ed96bb616413cb0735.png)

信号的FFT输出。[作者提供的图片]

上面的图应表示信号的频谱。注意，x轴是样本数量（而不是频率分量），y轴应表示正弦波的幅度。为了获得实际的频谱幅度，我们必须将(fft)的输出除以N/2（样本数量）。

```py
# Calculate N/2 to normalize the FFT output
N = len(signal)
normalize = N/2

# Plot the normalized FFT (|Xk|)/(N/2)
plt.plot(np.abs(fourier)/normalize)
plt.ylabel('Amplitude')
plt.xlabel('Samples')
plt.title('Normalized FFT Spectrum')
plt.show()
```

![](../Images/fceffbd48be98e5b2cb3b863c21efca4.png)

正常化后的FFT输出。[作者提供的图片]

要获取频率分量（x轴），可以使用scipy包中的(fftfreq)。该方法需要样本数量（N）和采样率作为输入参数。它返回一个包含N个频率分量的频率轴 [**[5]**](https://docs.scipy.org/doc/scipy/reference/generated/scipy.fft.fftfreq.html#scipy.fft.fftfreq)。

```py
# Get the frequency components of the spectrum
sampling_rate = 200.0 # It's used as a sample spacing
frequency_axis = fftfreq(N, d=1.0/sampling_rate)
norm_amplitude = np.abs(fourier)/normalize
# Plot the results
plt.plot(frequency_axis, norm_amplitude)
plt.xlabel('Frequency[Hz]')
plt.ylabel('Amplitude')
plt.title('Spectrum')
plt.show()
```

![](../Images/3de9718aead7454cf588ecd925e3bec7.png)

实际幅度和频率轴的频谱。[作者提供的图片]

为了理解上一段代码发生了什么，让我们仅绘制频率轴：

```py
# Plot the frequency axis for more explanation
plt.plot(frequency_axis)
plt.ylabel('Frequency[Hz]')
plt.title('Frequency Axis')
plt.show()
```

![](../Images/4a1f28f529929a8023f4fee1dbca156c.png)

频率轴。[作者提供的图片]

注意频率数组从零开始。然后，它以(d)的步长逐步增加，直到达到最大值（100Hz）。之后，它从负最大频率（-100Hz）开始，逐步增加回到正频率。可以从信号中获取信息的最大频率（100Hz）是采样率的一半，这符合采样定理 [**[2]**](https://pythonnumericalmethods.berkeley.edu/notebooks/chapter24.02-Discrete-Fourier-Transform.html)。

由于实值信号频谱的对称性，我们只关注频谱的前半部分 [**[2]**](https://pythonnumericalmethods.berkeley.edu/notebooks/chapter24.02-Discrete-Fourier-Transform.html)。Scipy包提供了处理实值信号傅里叶变换的方法，其中利用了频谱的对称性质。这些方法包括（rfft [**[6]**](https://docs.scipy.org/doc/scipy/reference/generated/scipy.fft.rfft.html#scipy.fft.rfft)，rfftfreq [**[7]**](https://docs.scipy.org/doc/scipy/reference/generated/scipy.fft.rfftfreq.html#scipy.fft.rfftfreq)）。这些方法分别对应（fft，fftfreq）。通过比较相同信号上（fft）和（rfft）方法的时间执行，你会发现（rfft）稍微快一点。处理实值信号时，通常情况下，使用（rfft）是最佳选择。

```py
# Calculate the time execution of (fft)
print('Execution time of fft function:')
%timeit fft(signal)
# Calculate the time execution of (rfft)
print('\nExecution time of rfft function:')
%timeit rfft(signal)
```

```py
Execution time of fft function:
13.5 µs ± 8.3 µs per loop (mean ± std. dev. of 7 runs, 100000 loops each)

Execution time of rfft function:
12.3 µs ± 3.55 µs per loop (mean ± std. dev. of 7 runs, 100000 loops each)
```

为总结我们关于缩放幅度和生成具有对称性质的实值信号频谱频率轴的讨论，下面的代码表示频谱的最终形式（右侧频率上的实际幅度）。

```py
# Plot the actual spectrum of the signal
plt.plot(rfftfreq(N, d=1/sampling_rate), 2*np.abs(rfft(signal))/N)
plt.title('Spectrum')
plt.xlabel('Frequency[Hz]')
plt.ylabel('Amplitude')
plt.show()
```

![](../Images/8f5189fd03a3d1690ed5f3723dc81893.png)

信号的实际频谱。[作者提供的图片]

下面的图帮助你理解和记住如何获取频率轴和构成频谱的正弦波的实际幅度。

![](../Images/b0d48cdb9d2dc51adb7eb7e4c91328d6.png)

花点时间阅读每一行代码，它为你提供了信号的实际频谱。[作者提供的图片]

# 最终代码

现在我们已经理解了在傅里叶分析中使用的每种方法的输入和输出，让我们进行最终的代码编写。我们将构建一个类（Fourier），使我们使用傅里叶变换更方便和更易于使用。我们需要的类应该计算信号数据的DFT并直观地可视化数据。确保阅读该类的文档以理解其使用方法。如果你不熟悉Python中的类及其构建方法，请参阅之前的 [**帖子**](https://medium.com/towards-data-science/use-classes-for-generating-signals-6694d22e9a80)，了解如何构建类以生成信号。

```py
# Building a class Fourier for better use of Fourier Analysis.
class Fourier:
  """
  Apply the Discrete Fourier Transform (DFT) on the signal using the Fast Fourier 
  Transform (FFT) from the scipy package.

  Example:
    fourier = Fourier(signal, sampling_rate=2000.0)
  """

  def __init__(self, signal, sampling_rate):
    """
    Initialize the Fourier class.

    Args:
        signal (np.ndarray): The samples of the signal
        sampling_rate (float): The sampling per second of the signal

    Additional parameters,which are required to generate Fourier calculations, are
    calculated and defined to be initialized here too:
        time_step (float): 1.0/sampling_rate
        time_axis (np.ndarray): Generate the time axis from the duration and
                              the time_step of the signal. The time axis is
                              for better representation of the signal.
        duration (float): The duration of the signal in seconds.
        frequencies (numpy.ndarray): The frequency axis to generate the spectrum.
        fourier (numpy.ndarray): The DFT using rfft from the scipy package.
    """
    self.signal = signal
    self.sampling_rate = sampling_rate
    self.time_step = 1.0/self.sampling_rate
    self.duration = len(self.signal)/self.sampling_rate
    self.time_axis = np.arange(0, self.duration, self.time_step)
    self.frequencies = rfftfreq(len(self.signal), d = self.time_step)
    self.fourier = rfft(self.signal)

  # Generate the actual amplitudes of the spectrum
  def amplitude(self):
    """
    Method of Fourier

    Returns:
        numpy.ndarray of the actual amplitudes of the sinusoids.
    """
    return 2*np.abs(self.fourier)/len(self.signal)

  # Generate the phase information from the output of rfft  
  def phase(self, degree = False):
    """
    Method of Fourier

    Args:
        degree: To choose the type of phase representation (Radian, Degree).
                By default, it's in radian. 

    Returns:
        numpy.ndarray of the phase information of the Fourier output.
    """
    return np.angle(self.fourier, deg = degree)

  # Plot the spectrum
  def plot_spectrum(self, interactive=False):
    """
    Plot the Spectrum (Frequency Domain) of the signal either using the matplotlib
    package, or plot it interactive using the plotly package.

    Args:
        interactive: To choose if you want the plot interactive (True), or not
        (False). The default is the spectrum non-interactive.

    Retruns:
        A plot of the spectrum.
    """
    # When the argument interactive is set to True:
    if interactive:
      self.trace = go.Line(x=self.frequencies, y=self.amplitude())
      self.data = [self.trace]
      self.layout = go.Layout(title=dict(text='Spectrum',
                                         x=0.5,
                                         xanchor='center',
                                         yanchor='top',
                                         font=dict(size=25, family='Arial, bold')),
                              xaxis=dict(title='Frequency[Hz]'),
                              yaxis=dict(title='Amplitude'))
      self.fig = go.Figure(data=self.data, layout=self.layout)
      return self.fig.show()
    # When the argument interactive is set to False:
    else:
      plt.figure(figsize = (10,6))
      plt.plot(self.frequencies, self.amplitude())
      plt.title('Spectrum')
      plt.ylabel('Amplitude')
      plt.xlabel('Frequency[Hz]')

  # Plot the Signal and the Spectrum interactively
  def plot_time_frequency(self, t_ylabel="Amplitude", f_ylabel="Amplitude",
                          t_title="Signal (Time Domain)",
                          f_title="Spectrum (Frequency Domain)"):
    """
    Plot the Signal in Time Domain and Frequency Domain using plotly.

    Args:
        t_ylabel (String): Label of the y-axis in Time-Domain
        f_ylabel (String): Label of the y-axis in Frequency-Domain
        t_title (String): Title of the Time-Domain plot
        f_title (String): Title of the Frequency-Domain plot 

    Returns:
        Two figures: the first is the time-domain, and the second is the
                     frequency-domain.
    """
    # The Signal (Time-Domain)
    self.time_trace = go.Line(x=self.time_axis, y=self.signal)
    self.time_domain = [self.time_trace]
    self.layout = go.Layout(title=dict(text=t_title,
                                       x=0.5,
                                       xanchor='center',
                                       yanchor='top',
                                       font=dict(size=25, family='Arial, bold')),
                            xaxis=dict(title='Time[sec]'),
                            yaxis=dict(title=t_ylabel),
                            width=1000,
                            height=400)
    fig = go.Figure(data=self.time_domain, layout=self.layout)
    fig.show()
    # The Spectrum (Frequency-Domain)
    self.freq_trace = go.Line(x=self.frequencies, y=self.amplitude())
    self.frequency_domain = [self.freq_trace]
    self.layout = go.Layout(title=dict(text=f_title,
                                       x=0.5,
                                       xanchor='center',
                                       yanchor='top',
                                       font=dict(size=25, family='Arial, bold')),
                            xaxis=dict(title='Frequency[Hz]'),
                            yaxis=dict(title=f_ylabel),
                            width=1000,
                            height=400)
    fig = go.Figure(data=self.frequency_domain, layout=self.layout)
    fig.show()
```

让我们在上面的信号上测试我们的类。输入参数是实值信号数据（时间域）和该信号的采样率。

```py
# Apply the DFT using the class Fourier
fourier = Fourier(signal, sampling_rate=200)
# Plot the spectrum interactively using the class Fourier
fourier.plot_spectrum(interactive=True)
```

![](../Images/b7c4928957a20befc08ae54634032c1e.png)

使用我们类（Fourier）的信号频谱。如果你应用上述代码，可以通过悬停在蓝色线条上互动获取数值。[作者提供的图片]

# 心电图的频谱

我们的最终示例将是现实世界的信号数据。我们将使用 Fourier 类绘制心电图（ECG）的时频域。该信号为心脏电活动的 5 分钟长，采样频率为 360Hz。[**[8]**](https://docs.scipy.org/doc/scipy/reference/generated/scipy.misc.electrocardiogram.html#scipy.misc.electrocardiogram)

```py
# Import the ECG signal from scipy package
from scipy.misc import electrocardiogram
# Built-in ECG signal
ecg = electrocardiogram()
# DFT using the class Fourier
ecg_spectrum = Fourier(signal = ecg, sampling_rate = 360.0)
# Plot the time-frequency domains of the ECG signal
ecg_spectrum.plot_time_frequency(t_title="ECG Signal", f_title="ECG Spectrum",
                                 t_ylabel="Amplitude[mV]")
```

![](../Images/10097e595e8555e1baedbda84a9ba92a.png)

ECG 信号及其频谱。[图片由作者提供]

上面的图像展示了我们在本帖中构建的 Fourier 类的一个示例（心电图信号及其频谱）。使用 Plotly 包使你可以轻松地悬停查看图表的值并对有趣的部分进行缩放。

# 结论

+   我们在数学上介绍了离散傅里叶变换（DFT）。

+   讨论了逐步傅里叶分析编码。我们首先介绍了快速傅里叶变换（FFT）及其在 Python 中的实现，以生成信号的谱。

+   我们介绍了标准化频谱的要求，以便获得正弦波的实际幅度。此外，我们还使用了 scipy.fft 的一个辅助函数来生成频谱的频率轴（fftfreq）。

+   我们指出了傅里叶变换的对称性质以及谱在采样频率周围的对称性。这就是我们讨论处理实值信号数据的新方法（rfft，rfftfreq）的原因。

+   我们建立了一个类来简化傅里叶变换的使用，并使用 Plotly 包交互式地生成信号的频域。

## 感谢阅读 ^_^

# 参考文献

[**[1]**](https://doi.ieeecomputersociety.org/10.1109/MCISE.2000.814652) Dongarra, J., & Sullivan, F. (2000)。十大算法的客座编辑介绍。*科学与工程计算*，2(01)，22–23。

[**[2]**](https://pythonnumericalmethods.berkeley.edu/notebooks/chapter24.02-Discrete-Fourier-Transform.html) Kong, Q., Siauw, T., & Bayen, A. (2020)。傅里叶变换。*Python 编程与数值方法：工程师和科学家的指南*（第 415–444 页）。学术出版社。

[**[3]**](https://www.ams.org/journals/mcom/1965-19-090/S0025-5718-1965-0178586-1/) Cooley, J. W., & Tukey, J. W. (1965)。复杂傅里叶级数的机器计算算法。*计算数学*，19(90)，297–301。

[**[4]**](https://docs.scipy.org/doc/scipy/reference/generated/scipy.fft.fft.html#scipy.fft.fft) Scipy 文档，API 参考，离散傅里叶变换 (scipy.fft.fft)。[访问日期：2023年2月23日]

[**[5]**](https://docs.scipy.org/doc/scipy/reference/generated/scipy.fft.fftfreq.html#scipy.fft.fftfreq) Scipy 文档，API 参考，离散傅里叶变换 (scipy.fft.fftfreq)。[访问日期：2023年2月23日]

[**[6]**](https://docs.scipy.org/doc/scipy/reference/generated/scipy.fft.rfft.html#scipy.fft.rfft) Scipy 文档，API 参考，离散傅里叶变换 (scipy.fft.rfft)。[访问日期：2023年2月23日]

[**[7]**](https://docs.scipy.org/doc/scipy/reference/generated/scipy.fft.rfftfreq.html#scipy.fft.rfftfreq) Scipy 文档，API 参考，离散傅里叶变换 (scipy.fft.rfftfreq)。 [访问于 2023年2月23日]

[**[8]**](https://docs.scipy.org/doc/scipy/reference/generated/scipy.misc.electrocardiogram.html#scipy.misc.electrocardiogram) Scipy 文档，API 参考，杂项例程 (scipy.misc.electrocardiogram)。 [访问于 2023年2月23日]
