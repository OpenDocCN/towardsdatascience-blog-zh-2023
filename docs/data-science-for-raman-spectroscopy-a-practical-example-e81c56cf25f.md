# 拉曼光谱的数据科学：一个实际示例

> 原文：[https://towardsdatascience.com/data-science-for-raman-spectroscopy-a-practical-example-e81c56cf25f?source=collection_archive---------5-----------------------#2023-01-16](https://towardsdatascience.com/data-science-for-raman-spectroscopy-a-practical-example-e81c56cf25f?source=collection_archive---------5-----------------------#2023-01-16)

## 光谱预处理和建模的实际示例

[](https://medium.com/@nicopez?source=post_page-----e81c56cf25f--------------------------------)[![Nicolas Coca, PhD](../Images/548630c393526a802cf560344990a1e3.png)](https://medium.com/@nicopez?source=post_page-----e81c56cf25f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e81c56cf25f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e81c56cf25f--------------------------------) [Nicolas Coca, PhD](https://medium.com/@nicopez?source=post_page-----e81c56cf25f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F60149d1ba899&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-science-for-raman-spectroscopy-a-practical-example-e81c56cf25f&user=Nicolas+Coca%2C+PhD&userId=60149d1ba899&source=post_page-60149d1ba899----e81c56cf25f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e81c56cf25f--------------------------------) ·10分钟阅读·2023年1月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe81c56cf25f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-science-for-raman-spectroscopy-a-practical-example-e81c56cf25f&user=Nicolas+Coca%2C+PhD&userId=60149d1ba899&source=-----e81c56cf25f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe81c56cf25f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-science-for-raman-spectroscopy-a-practical-example-e81c56cf25f&source=-----e81c56cf25f---------------------bookmark_footer-----------)![](../Images/b3e7021c38a14b4e51219a154932f8b1.png)

**拉曼光谱的数据科学**。从光谱预处理到建模：峰值检测与去除、基线扣除、平滑处理以及经典最小二乘法应用于成分量化。[作者提供的图片]。

# 介绍

拉曼光谱学提供了分子振动模式的信息，以拉曼光谱的形式展现。这些光谱可以像结构指纹一样，用于识别和表征分子和材料。从中我们可以了解材料的组成、温度、材料内部的应力、是否有施加的电磁场等。然而，为了提取这些信息，我们需要先清洗和处理数据，然后应用化学计量学或机器学习模型。在本文草稿中，我将解释数据科学管道如何在拉曼光谱数据上工作，并给出定量光谱分析的示例化学计量学。

本文是《Python 拉曼光谱数据科学系列》的一个部分。本篇将帮助总结之前的主题，并将其用于完整的处理和建模周期。请查看之前在[Towards Data Science](https://towardsdatascience.com/)发布的文章：

+   [用异常检测去除拉曼光谱中的尖峰](https://medium.com/towards-data-science/removing-spikes-from-raman-spectra-8a9fdda0ac22)

+   [Python中用于定量光谱分析的经典最小二乘法](https://medium.com/towards-data-science/classical-least-squares-method-for-quantitative-spectral-analysis-with-python-1926473a802c)

**一个例子**

本文的目的是展示拉曼光谱数据分析和建模的典型工作流程。为此，我们生成了一个基于三个不同组件的合成拉曼光谱。为了增加真实感，添加了不同类型的噪声。应用了最常见的预处理步骤以恢复干净的光谱。最后，使用经典的最小二乘法来估计三个不同组件的浓度。

本笔记本因此分为三部分：

1) 合成光谱生成

2) 光谱的预处理

3) 应用经典最小二乘法计算光谱中不同组件的量

让我们首先加载将要使用的库。

```py
# Loading the required packages:
import numpy as np
import matplotlib.pyplot as plt
from scipy import sparse
from scipy.sparse.linalg import spsolve
from scipy.signal import savgol_filter, general_gaussian
import sklearn.linear_model as linear_model
```

# 合成数据生成

对于这个实际例子，我们将模拟一些拉曼光谱，以便知道预期结果。

1) 我们将生成三个组件，并按给定比例混合。

2) 为了让数据更真实，我们将添加噪声：随机噪声、尖峰和基线。

# 生成由三个组件组成的混合光谱

即使这个话题更复杂，为了这个例子，我们假设信号由高斯峰组成。为此，我们定义一个高斯函数为

```py
def Gauss(x, mu, sigma, A = 1):
    # This def returns the Gaussian function of x
    # x is an array
    # mu is the expected value
    # sigma is the square root of the variance
    # A is a multiplication factor

    gaussian = A/(sigma * np.sqrt(2*np.pi)) * np.exp(-0.5*((x-mu)/sigma)**2)

    return gaussian
```

我们首先生成这三个组件：

```py
# X-axis (Wavelengths)
x_range =  np.linspace(650, 783, 1024)

# Let's create three different components

# Component A
mu_a1 = 663
sigma_a1 = 1
intensity_a1 = 1

mu_a2 = 735
sigma_a2 = 1
intensity_a2 = 0.2

mu_a3 = 771
sigma_a3 = 1
intensity_a3 = 0.3

gauss_a =  Gauss(x_range, mu_a1, sigma_a1, intensity_a1) + Gauss(x_range, mu_a2, sigma_a2, intensity_a2) + Gauss(x_range, mu_a3, sigma_a3, intensity_a3)

# Component B
mu_b = 700
sigma_b = 1
intensity_b = 0.2

mu_b1 = 690
sigma_b1 = 2
intensity_b1 = 0.5

mu_b2 = 710
sigma_b2 = 1
intensity_b2 = 0.75

mu_b3 = 774
sigma_b3 = 1.5
intensity_b3 = 0.25

gauss_b = Gauss(x_range, mu_b, sigma_b, intensity_b) + Gauss(x_range, mu_b1, sigma_b1, intensity_b1) + Gauss(x_range, mu_b2, sigma_b2, intensity_b2) + Gauss(x_range, mu_b3, sigma_b3, intensity_b3)

# Component C
mu_c1 = 660
sigma_c1 = 1
intensity_c1 = 0.05

mu_c2 = 712
sigma_c2 = 4
intensity_c2 = 0.7

gauss_c = Gauss(x_range, mu_c1, sigma_c1, intensity_c1) + Gauss(x_range, mu_c2, sigma_c2, intensity_c2)

# Component normalization
component_a = gauss_a/np.max(gauss_a)
component_b = gauss_b/np.max(gauss_b)
component_c = gauss_c/np.max(gauss_c)

# How do they look?
plt.plot(x_range, component_a, label = 'Component 1')
plt.plot(x_range, component_b, label = 'Component 2')
plt.plot(x_range, component_c, label = 'Component 3')
plt.title('Known components in our mixture', fontsize = 15)
plt.xlabel('Wavelength', fontsize = 15)
plt.ylabel('Normalized intensity', fontsize = 15)
plt.legend()
plt.show()
```

![](../Images/1a8e4f31d9d08339b431aab3d15bf48c.png)

三个（已知的）合成组件光谱。[作者提供的图片]。

我们可以基于这三个不同的组件生成一个混合光谱：

```py
# What concentrations we want these components to have in our mixture:
c_a = 0.5
c_b = 0.3
c_c = 0.2

comps = np.array([c_a, c_b, c_c])

# Let's build the spectrum to be studied: The mixture spectrum
mix_spectrum = c_a * component_a + c_b * component_b + c_c *component_c

# How does it look?
plt.plot(x_range, mix_spectrum, color = 'black', label = 'Mixture spectrum with noise')
plt.title('Mixture spectrum', fontsize = 15)
plt.xlabel('Wavelength', fontsize = 15)
plt.ylabel('Intensity',  fontsize = 15)
plt.show()
```

![](../Images/09dc5f1c14e54043a82f854528072574.png)

合成光谱。 [作者提供的图片]。

为了让我们的混合物更加真实，现在我们添加一些噪声：

```py
# Let's add some noise for a bit of realism:

# Random noise:
mix_spectrum = mix_spectrum +  np.random.normal(0, 0.02, len(x_range))

# Spikes: 
mix_spectrum[800] = mix_spectrum[800] + 1
mix_spectrum[300] = mix_spectrum[300] + 0.3

# Baseline as a polynomial background:
poly = 0.2 * np.ones(len(x_range)) + 0.0001 * x_range + 0.000051 * (x_range - 680)**2 
mix_spectrum = mix_spectrum + poly

# How does it look now?
plt.plot(x_range, mix_spectrum, color = 'black', label = 'Mixture spectrum with noise')
plt.title('Mixture spectrum', fontsize = 15)
plt.xlabel('Wavelength', fontsize = 15)
plt.ylabel('Intensity',  fontsize = 15)
plt.show()
```

![](../Images/b05e1b4afaf9209121f4a4074d9aa52a.png)

添加噪声的合成光谱。 [作者提供的图片]。

我们现在有了一个合成的测量光谱。

# 光谱的预处理

在我们进行任何建模之前，需要对光谱进行预处理。这些是我们在实际测量光谱时会做的一些工作：清除尖峰、平滑噪声和减去基线。为此，我们将使用一些简单的算法，如下所述。

关于拉曼光谱处理步骤的更完整列表，建议读者查阅B. Barton等人的文章（《应用光谱学》76 (9), 1021–1041）或O. Ryabchykov等人的文章 ([https://doi.org/10.1515/psr-2017-0043](https://doi.org/10.1515/psr-2017-0043))。

# i) 去尖峰光谱

第一步是定位并修正尖峰。为此，我们使用了基于修改后的z-score的算法。修改后的z-score计算如下：

z(i) = 0.6745 (x(i)-M) / MAD

其中MAD = median(|x-M|)，|…| 表示绝对值，x 是差分光谱的值。

更多信息请参见我之前的 [文章](/removing-spikes-from-raman-spectra-8a9fdda0ac22) 或 [这个jupyter笔记本](https://github.com/nicocopez/Outlier_detection_for_Spikes_Removal_from_Raman_Spectra/blob/master/Despiking_Raman_spectra_1.ipynb)。

```py
# The next function calculates the modified z-scores of a diferentiated spectrum

def modified_z_score(ys):
    ysb = np.diff(ys) # Differentiated intensity values
    median_y = np.median(ysb) # Median of the intensity values
    median_absolute_deviation_y = np.median([np.abs(y - median_y) for y in ysb]) # median_absolute_deviation of the differentiated intensity values
    modified_z_scores = [0.6745 * (y - median_y) / median_absolute_deviation_y for y in ysb] # median_absolute_deviationmodified z scores
    return modified_z_scores

# The next function calculates the average values around the point to be replaced.
def fixer(y,ma):
    threshold = 7 # binarization threshold
    spikes = abs(np.array(modified_z_score(y))) > threshold
    y_out = y.copy()
    for i in np.arange(len(spikes)):
        if spikes[i] != 0:
            w = np.arange(i-ma,i+1+ma)
            we = w[spikes[w] == 0]
            y_out[i] = np.mean(y[we])
    return y_out
```

接下来，我们应用去尖峰算法：

```py
despiked_spectrum = fixer(mix_spectrum,ma=10)
```

并与原始混合光谱进行比较：

![](../Images/ef5db90d71bf420f25b95cd892fc4616.png)

尖峰检测。 [作者提供的图片]。

# ii) 基线分离

为了计算基线，我们使用了一种基于非对称最小二乘法的基线估计算法，如Eilers和Boelens在2005年论文中所述。

```py
# Baseline stimation with asymmetric least squares
# According to paper: "Baseline Correction with Asymmetric Least Squares Smoothing" 
# by Paul H. C. Eilers and Hans F.M. Boelens. October 21, 2005

# We need the following packages here:
from scipy import sparse
from scipy.sparse.linalg import spsolve

# Baseline stimation function:
def baseline_als(y, lam, p, niter=100):
    L = len(y)
    D = sparse.diags([1,-2,1],[0,-1,-2], shape=(L,L-2))
    w = np.ones(L)
    for i in range(niter):
        W = sparse.spdiags(w, 0, L, L)
        Z = W + lam * D.dot(D.transpose())
        z = spsolve(Z, w*y)
        w = p * (y > z) + (1-p) * (y < z)
    return z

# For more info see the paper and https://stackoverflow.com/questions/29156532/python-baseline-correction-library
```

基线减法参数：

正如他们在论文中所说，“有两个参数：**p表示非对称性**和**l表示平滑度**。这两个参数都需要根据手头的数据进行调整。我们发现通常0.001 < p < 0.1 是一个不错的选择（对于有正峰的信号），10² < l < 10⁹。” 参见Eilers和Boelens，2005。

```py
# Parameters for this case:
l = 10000000 # smoothness
p = 0.05 # asymmetry
```

借助这个算法，我们可以估计基线并将其从测量光谱中减去。

```py
# Estimation of the baseline:
estimated_baselined = baseline_als(despiked_spectrum, l, p)

# Baseline subtraction:
baselined_spectrum = despiked_spectrum - estimated_baselined

# How does it look like?
fig, (ax1, ax2) = plt.subplots(1,2, figsize=(16,4))

# We compared the original mix spectrum and the estimated baseline:
ax1.plot(x_range, despiked_spectrum, color = 'black', label = 'Mix spectrum with noise' )
ax1.plot(x_range, estimated_baselined, color = 'red', label = 'Estimated baseline')
ax1.set_title('Baseline estimation', fontsize = 15)
ax1.set_xlabel('Wavelength', fontsize = 15)
ax1.set_ylabel('Intensity',  fontsize = 15)
ax1.legend()

# We plot the mix spectrum after baseline subtraction
ax2.plot(x_range, baselined_spectrum, color = 'black', label = 'Baselined spectrum with noise' )
ax2.set_title('Baselined spectrum', fontsize = 15)
ax2.set_xlabel('Wavelength', fontsize = 15)
ax2.set_ylabel('Intensity',  fontsize = 15)
plt.show()
```

![](../Images/6c866522ebbd05d1f08ffaf54e1a8ce6.png)

基线减法。 [作者提供的图片]。

# iii) 平滑

为了平滑光谱，我们使用了在SciPy库中实现的Savitzky-Golay滤波器。窗口参数（点数）w和多项式阶数p可以针对每组光谱进行优化。

```py
from scipy.signal import savgol_filter, general_gaussian

# Parameters:
w = 9 # window (number of points)
p = 2 # polynomial order

smoothed_spectrum = savgol_filter(baselined_spectrum, w, polyorder = p, deriv=0)

# Some more information on the implementation of this method can be found here:
# https://nirpyresearch.com/savitzky-golay-smoothing-method/

# We plot the mix spectrum after baseline subtraction
plt.plot(x_range, baselined_spectrum, color = 'black', label = 'Baselined spectrum with noise' )
plt.plot(x_range, smoothed_spectrum, color = 'red', label = 'Smoothed spectrum' )
plt.title('Smoothed spectrum', fontsize = 15)
plt.xlabel('Wavelength', fontsize = 15)
plt.ylabel('Intensity',  fontsize = 15)
plt.legend()
plt.show()
```

![](../Images/550d47e11792bc83e80628b284fd17b9.png)

噪声平滑。 [作者提供的图片]。

我们最终得到了一个处理过的光谱，可以在其上应用模型。

# 经典最小二乘（CLS）方法用于定量光谱分析

现在我们已经预处理了我们的混合光谱，可以应用 CLS 来量化其组成。有关 CLS 的更多信息，你可以查看我之前的帖子 [这里](https://medium.com/towards-data-science/classical-least-squares-method-for-quantitative-spectral-analysis-with-python-1926473a802c)。

```py
query_spectrum = smoothed_spectrum # Let's just rename it
```

我们首先生成成分矩阵 K

```py
# Generate the components matrix or K matrix
components = np.array([component_a, component_b, component_c]) 
```

我们可以利用 Scikit-learn 实现 CLS，因此我们导入了该库。

```py
import sklearn.linear_model as linear_model
```

并应用 CLS 计算浓度

```py
cs = linear_model.LinearRegression().fit(components.T, query_spectrum).coef_
#We print the result:
print('The expected concentrations for components A, B and C are: ' + str(cs)) 
```

![](../Images/6a1d19aa42100e9305c88367a4e45b35.png)

让我们从图形上来看一下：

```py
# Does the result match the original data?

# Plot the original data:
plt.plot(x_range, query_spectrum, color = 'black', label = 'Mix spectrum' )

# Plot the separate components times its calculated concentration:
for i in np.arange(len(cs)):
    plt.plot(x_range, cs[i]*components[i], label = 'c' + str(i)+ ' = ' + str(np.round(cs[i], 3)))

# Plot the result: the sum of separate components times its calculated concentration:
plt.plot(x_range, np.dot(cs,components), color = 'red', linewidth = 2, label = 'Calculation')

plt.title('Mixture spectrum and calculated components', fontsize = 15)
plt.xlabel('Wavelength', fontsize = 15)
plt.ylabel('Intensity', fontsize = 15)
plt.legend()
plt.ylim(ymin = -0.1)
plt.show()
```

![](../Images/7fd96b96a65895d3b76074a8eff3ea41.png)

光谱拟合。[作者提供的图片]。

图例中显示的拟合浓度与合成光谱的设定值完全一致。

总结一下，我们已经看到了如何预处理光谱，并应用了一个简单的最小二乘模型来恢复不同的成分及其浓度。这些简单而强大的工具可能会帮助你最大程度地利用你的光谱数据。重要的是，这种方法的不同步骤不仅可以应用于拉曼光谱，还可以应用于任何类型的光谱数据，例如红外光谱、X 射线衍射光谱等。

现在轮到你了，合成不同的光谱，调整不同算法的参数，或将其应用于你自己测量的光谱。

… 如果你有任何**问题、评论或建议**，请随时通过消息或我的 LinkedIn 账号 **联系我**：[nicolascocalopez](https://www.linkedin.com/in/nicolascocalopez/)。

# **Jupyter notebook：**

查看完整的 jupyter notebook [这里](https://github.com/nicocopez/Data-Science-for-Raman-spectroscopy-a-practical-example/blob/main/Workshop%20ML%20with%20Python%20-%20DS%20for%20Raman%20spectroscopy%20-%20An%20example%20by%20NCL.ipynb)。

# **参考文献**：

你可以在我之前的帖子 [这里](https://medium.com/towards-data-science/removing-spikes-from-raman-spectra-8a9fdda0ac22) 阅读更多关于去尖峰光谱的内容，以及在 [这里](https://medium.com/towards-data-science/classical-least-squares-method-for-quantitative-spectral-analysis-with-python-1926473a802c) 阅读关于经典最小二乘法的内容，或者在我的 GitHub 或 Medium 账户中查看：

+   [https://medium.com/@nicopez](https://medium.com/@nicopez)

+   [https://github.com/nicocopez/](https://github.com/nicocopez/)

原始文献：

+   [修改过的 z 分数去尖峰算法](https://chemrxiv.org/engage/api-gateway/chemrxiv/assets/orp/resource/item/60c73e33469df41c2af4281c/original/a-simple-algorithm-for-despiking-raman-spectra.pdf)。

    Whitaker 等人，《化学计量学与智能实验室系统》第179卷，2018年8月15日。

+   使用不对称最小二乘法进行基线校正。

    “基线修正与不对称最小二乘平滑”由 Paul H. C. Eilers 和 Hans F.M. Boelens 著。2005年10月21日。

关于如何处理和应用化学计量学于拉曼光谱的完整指南，请参见：

+   Barton, B., Thomson, J., Diz, E. L., & Portela, R. (2022). 拉曼光谱学化学计量学的协调. 应用光谱学, 76(9), 1021–1041.

+   Ryabchykov, O., Guo, S., & Bocklitz, T. (2019). 拉曼光谱数据的分析. 物理科学评论, 4(2).
