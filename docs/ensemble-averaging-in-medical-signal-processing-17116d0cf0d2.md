# 医学信号处理中的集成平均

> 原文：[https://towardsdatascience.com/ensemble-averaging-in-medical-signal-processing-17116d0cf0d2?source=collection_archive---------14-----------------------#2023-05-02](https://towardsdatascience.com/ensemble-averaging-in-medical-signal-processing-17116d0cf0d2?source=collection_archive---------14-----------------------#2023-05-02)

## 简单却强大的方法

[](https://medium.com/@omar.ok1998?source=post_page-----17116d0cf0d2--------------------------------)[![Omar Alkousa](../Images/7598618abe8e8fa89f1d8a4bfc21f014.png)](https://medium.com/@omar.ok1998?source=post_page-----17116d0cf0d2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----17116d0cf0d2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----17116d0cf0d2--------------------------------) [Omar Alkousa](https://medium.com/@omar.ok1998?source=post_page-----17116d0cf0d2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff8302b9534b5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fensemble-averaging-in-medical-signal-processing-17116d0cf0d2&user=Omar+Alkousa&userId=f8302b9534b5&source=post_page-f8302b9534b5----17116d0cf0d2---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----17116d0cf0d2--------------------------------) ·6 分钟阅读·2023 年 5 月 2 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F17116d0cf0d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fensemble-averaging-in-medical-signal-processing-17116d0cf0d2&user=Omar+Alkousa&userId=f8302b9534b5&source=-----17116d0cf0d2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F17116d0cf0d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fensemble-averaging-in-medical-signal-processing-17116d0cf0d2&source=-----17116d0cf0d2---------------------bookmark_footer-----------)

作为研究人员、数据科学家/分析师，甚至在任何科学领域处理数据是不可避免的。因此，你处理的数据的可靠性至关重要，因为它决定了你工作的可靠性。

在本文中，我们将介绍集成平均方法，一种简单但强大的方法，并介绍其在信号数据上的应用。

![](../Images/48226e70172f46fd4a7b4dfb1e0acd79.png)

图片由 [Eran Menashri](https://unsplash.com/@chesnutt?utm_source=medium&utm_medium=referral) 拍摄于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

平均值是一个可以描述一组数值的单一值。它可以定义为变量的总和除以变量的数量。如果我们有一组变量 {x1, x2, …, xn}，则可以按如下方式计算平均值：

![](../Images/1a85d51eb64276c3f1ba00d74e8f36c6.png)

你可以考虑一个简单的例子来介绍集成平均的概念，即测量在测量过程中温度稳定的溶液的温度。可以使用电子传感器在十秒钟内测量三次温度。集成平均可以通过取这三次测量的平均值来完成。

从上述示例来看，集成平均方法要求能够重复你所进行的实验，并保持测量系统的相同属性 [[1](https://doi.org/10.1016/B978-0-12-809395-5.00002-3)]。这在许多实际情况中很难应用。因此，在许多情况下可能考虑的解决方案是一次性进行实验，但同时使用多个传感器 [[1](https://doi.org/10.1016/B978-0-12-809395-5.00002-3)]，以产生多个相应实验的观测值。

在信号类型的数据（时间序列）的情况下，集成平均方法是通过逐点平均信号样本来完成的 [[2](https://www.ijarcce.com/upload/2016/august-16/IJARCCE%2052.pdf)]。因此，在应用该方法之前，一个先决条件是生成的信号在时间上对齐 [[1](https://doi.org/10.1016/B978-0-12-809395-5.00002-3)]。下面是应用集成平均于信号数据的示例。

![](../Images/dcb3cc4f607fe1a064a6277f15de228c.png)

使用两个实验应用集成平均方法。注意信号的样本在时间上是对齐的。 [作者提供的图像]

# 为什么使用集成平均方法？我们为什么要重复实验？

我第一次接触到集成平均是在生物力学实验室，该实验室专门研究步态模式和分析人体运动。当我的教授指示对同一患者进行实验以获得至少三个成功的实验时，**我问为什么要三次而不是一次成功的实验？！**

答案是计算这三个实验的集成平均值，以获得具有更高可靠性且不易受到随机噪声影响的最终信号。

常识上，重复实验确保你获得的数据是感兴趣的数据。但当然，实验重复的成本和时间也需要考虑。

从信号处理的角度来看，集成平均在增强信号数据方面有许多优点。这些优点可以总结如下：

+   集成平均对于过滤信号中的随机噪声非常有用 [[2](https://www.ijarcce.com/upload/2016/august-16/IJARCCE%2052.pdf)]。

+   应用集合平均方法可以提高信噪比（SNR）。如果我们有N次重复，改进程度与N的平方根成正比 [[2](https://www.ijarcce.com/upload/2016/august-16/IJARCCE%2052.pdf)]。

# 真实世界的医学示例

集合平均方法的应用是无限的。医学领域的一个很好的例子是视觉诱发响应/潜能测试（VER或VEP测试）的理念。该测试的目的是测量对视觉刺激的响应，该响应以电信号的形式产生在大脑的视觉皮层中。该测试依赖于特定时间间隔的重复刺激 [[3](https://eyewiki.aao.org/Visual_Evoked_Potential/_Response_(VEP/VER))]。视觉刺激通过黑白棋盘图案的屏幕进行 [[3](https://eyewiki.aao.org/Visual_Evoked_Potential/_Response_(VEP/VER))]。重复刺激的信号逐点平均（集合平均）以确定VER信号 [[1](https://doi.org/10.1016/B978-0-12-809395-5.00002-3)]，请记住信号在刺激过程中是时间对齐的。以下是如何使用集合平均方法提取VER信号的示例。

我们将使用的信号数据可以通过以下[**链接**](https://github.com/Roshni1999/John-Semmlow-Signals-and-Systems-for-Bioengineers-2ed/blob/9c6efc1c68e042212cff45a5fb41902d6699a09e/Chapter1/ver.mat)（MIT许可）获取。数据包括100个以200 Hz采样率记录的电信号。

# 使用Python进行集合平均

首先，我们将导入所需的Python包：

+   **Numpy**：用于数组操作和算术计算。

+   **Matplotlib**：用于可视化VER响应的信号数据。

+   **Scipy**：我们将使用scipy.io模块读取数据文件（.MAT文件类型）。

```py
# Import the required packages
import matplotlib.pyplot as plt
import numpy as np
from scipy import io
```

现在，我们使用*scipy.io.loadmat*方法读取包含电信号的数据文件。输出是文件内容的字典。电信号可以在“ver”项中找到。

```py
# Read a Mat file using scipy.io
mat = io.loadmat('ver.mat')
# Exract the electrical signals
ver = mat['ver']
```

让我们探索一下我们正在处理的数据的大小。

```py
# The size of variable "ver"
print(ver.shape)
```

```py
(100, 500)
```

这意味着我们有100个信号，每个信号有500个样本。对这些数据应用集合平均方法将平均这100个信号，并生成一个包含500个样本的信号。

提取信号数据后，我们将定义采样率以生成信号的时间轴。

```py
# Calculate the sampling rate
sampling_rate = 200.0
# Calculate the duration of the recordings
duration = ver.shape[1]/sampling_rate
# Generate the time axis of the recordings
time_axis = np.arange(0, duration, 1/sampling_rate)
```

现在我们有了时间轴和要研究的信号，我们可以在同一时间轴上可视化一些信号，以查看单一视觉响应的样子。我们将仅可视化前10个信号。

```py
# Subplot figure with 10 rows and one column
fig, axs = plt.subplots(10, figsize=(10,5), sharex=True)

# Plot the first 10 recordings
for i in range(10):
    axs[i].plot(time_axis, ver[i,:])
    # Hide the values of y-axis
    axs[i].set_yticks([])
    # Fit the x-axis along the signal
    axs[i].set_xlim([time_axis[0], time_axis[-1]])

# Set a title and xlabel
axs[0].set_title('The first 10 Signals')
axs[9].set_xlabel('Time [Sec]')
plt.show()
```

![](../Images/3ccc10bb8adeb0f57e40cff62c49fd95.png)

数据的前10个信号。[作者提供的图像]

注意，单一信号中无法识别视觉响应。因此，我们将使用集合平均法来确定VER。

```py
# Apply the ensamble averaging method on the Recordings.
ver_signal = np.mean(ver, axis=0)
# Extract the actual VER from the Mat file
actual_ver = np.reshape(mat['actual_ver'], [500])

# Plot the ensambled EEG recordings and the actual VER
plt.plot(time_axis, ver_signal, color='green', label='Ensamble Averaging')
plt.plot(time_axis, actual_ver, color='red', label='Actual VER')
plt.xlim([time_axis[0], time_axis[-1]])
plt.xlabel('Time [sec]')
plt.ylabel('Average')
plt.title('Visual Evoked Response')
plt.legend()
plt.show()
```

![](../Images/1408db1ce22d6739a4a0480b7b593465.png)

使用集合平均法对100个信号进行VER分析。[作者提供的图像]

# 结论

+   我们介绍了集成平均法，这是一种简单但非常有用的方法，你可以用来提取可靠的数据。

+   我们指出了重复实验的原因及其如何提升数据的可靠性。

+   应用集成平均法能将信号的信噪比（SNR）提高到N（重复次数）的平方根。此外，最终的信号数据也不容易受到随机噪声的影响。

+   我们介绍了一个例子，利用集成平均法来确定视觉诱发反应（VER），基于特定时间的重复刺激。

# 参考文献

**[**[**1**](https://doi.org/10.1016/B978-0-12-809395-5.00002-3)**]** Semmlow, J. (2018). 时间域的信号分析。在*生物工程师的电路、信号与系统*（第51-110页）。Elsevier。 [https://doi.org/10.1016/B978-0-12-809395-5.00002-3](https://doi.org/10.1016/B978-0-12-809395-5.00002-3)

**[**[**2**](https://www.ijarcce.com/upload/2016/august-16/IJARCCE%2052.pdf)**]** Thomas, T. (2005). 集成平均滤波器用于噪声降低。*国际计算机与通信工程高级研究期刊*，*5*(8)。

**[**[**3**](https://eyewiki.aao.org/Visual_Evoked_Potential/_Response_(VEP/VER))**]** Tripathy, K., Hsu, J., & Lim, J. (2022年11月10日). *视觉诱发电位/反应（VEP/VER）*。美国眼科学会。 [https://eyewiki.aao.org/Visual_Evoked_Potential/_Response_(VEP/VER)](https://eyewiki.aao.org/Visual_Evoked_Potential/_Response_(VEP/VER))
