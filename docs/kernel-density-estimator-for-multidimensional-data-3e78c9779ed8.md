# 多维数据的核密度估计器

> 原文：[`towardsdatascience.com/kernel-density-estimator-for-multidimensional-data-3e78c9779ed8?source=collection_archive---------1-----------------------#2023-10-04`](https://towardsdatascience.com/kernel-density-estimator-for-multidimensional-data-3e78c9779ed8?source=collection_archive---------1-----------------------#2023-10-04)

## 使用实际数据集演示 KDE

[](https://medium.com/@jaroslaw.drapala?source=post_page-----3e78c9779ed8--------------------------------)![Jaroslaw Drapala](https://medium.com/@jaroslaw.drapala?source=post_page-----3e78c9779ed8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3e78c9779ed8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3e78c9779ed8--------------------------------) [Jaroslaw Drapala](https://medium.com/@jaroslaw.drapala?source=post_page-----3e78c9779ed8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fce6da4a69810&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fkernel-density-estimator-for-multidimensional-data-3e78c9779ed8&user=Jaroslaw+Drapala&userId=ce6da4a69810&source=post_page-ce6da4a69810----3e78c9779ed8---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----3e78c9779ed8--------------------------------) ·11 分钟阅读·2023 年 10 月 4 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3e78c9779ed8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fkernel-density-estimator-for-multidimensional-data-3e78c9779ed8&user=Jaroslaw+Drapala&userId=ce6da4a69810&source=-----3e78c9779ed8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3e78c9779ed8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fkernel-density-estimator-for-multidimensional-data-3e78c9779ed8&source=-----3e78c9779ed8---------------------bookmark_footer-----------)![](img/d0bff3735f3ef37d3f870c1c5ffd8fab.png)

照片由[Marco Bianchetti](https://unsplash.com/@marcobian?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我希望通过考虑多维数据来扩展[我之前的故事](https://medium.com/towards-data-science/kernel-density-estimation-explained-step-by-step-7cc5b5bc4517)，讲解核密度估计器（KDE）。

我将首先提供一个关于主题的数学概述，然后你将获得 Python 代码以实验双变量 KDE。接下来，我将讲解高斯核的一些属性。最后但同样重要的是，使用 *高度和权重* 和 *系外行星* 数据集，我将演示如何将 KDE 应用于实际数据。

# 1\. 数学介绍

KDE 是一个由几个类似的核函数组成的复合函数。我选择了高斯核，因为它易于分析。这个函数是多维高斯的原型

![](img/2e20771a70291a41c80118b70d397b2e.png)

这本身是一个扩展

![](img/1f5814cfde0b6b60d5ae7436eb0a7408.png)

到许多维度。

向量 **x** 总共有 *d* 维（特征），上标表示特征的索引：

![](img/bb3c9787ea5bb097bf6706256173d9cd.png)

**H** 是一个 *d* 乘 *d* 的系数矩阵，决定了函数的形式。以下是一个二维 (*d =* 2) 的例子：

![](img/cbce5370a462ce0479e6c992abff661c.png)

也许你还记得，只有曲线下的面积为单位的函数才能加入 PDF 俱乐部。因此，为了获得多变量高斯核函数，我们必须添加一些归一化项：

![](img/f609b9ba6669c68a81089822a94972e5.png)

你可以自行验证，插入 *d =* 1 会得到一个标准的单维高斯函数。

矩阵 **H** 作为协方差矩阵。在上面的双变量案例 (*d =* 2) 中，*h*₁₁ 和 *h*₂₂ 分别对应于 *x*⁽¹⁾ 和 *x*⁽²⁾ 的方差，而 *h*₁₂ = *h*₂₁ 代表 *x*⁽¹⁾ 与 *x*⁽²⁾ 的协方差。这就是为什么矩阵 **H** 被认为是对称的。因此，在双变量情况下，用户可以通过三个参数来改变核函数。

核函数是一个自定义的模板函数，用于对每个数据点应用，以便使用简单的求和构建整个数据集的 PDF：

![](img/f6282de19b1b713749cc8b3b97acaec9.png)

其中 **x***ᵢ* 是第 *i* 个数据点：

![](img/81050e75748e124d4c7b938038913b49.png)

如果所有这些数学内容让你感到不安，不用担心。我将提供 Python 代码来创建可视化，展示其工作原理。要记住的主要点是：

> 核密度估计器是一个复合函数，由分配给每个数据点的核函数实例组成。

# 2\. Python 代码使双变量 KDE 变为现实

这里有可以作为实验平台的代码，用于双变量高斯核和 KDE 实验。

导入部分先行：

```py
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.cm as cm
import pandas as pd
import seaborn as sns

# to generate a visually appealing images with dataframes
import dataframe_image as dfi

# to make functions with some arguments fixed
from functools import partial

# Latex typefaces will be used for math symbols in figures.
plt.rcParams.update({
    "text.usetex": True,
    "font.family": "sans-serif",
    "font.sans-serif": ["Helvetica"]
})
```

双变量高斯核函数 `K` 需要一个 2 乘 2 的 numpy 数组 `H`。函数 `K` 接受一个类似网格的数组作为参数 `x`。

```py
def K(x, H):

    # unpack two dimensions
    x1, x2 = x

    # extract four components from the matrix inv(H)
    a, b, c, d = np.linalg.inv(H).flatten()

    # calculate scaling coeff to shorten the equation
    scale = 2*np.pi*np.sqrt( np.linalg.det(H))

    return np.exp(-(a*x1**2 + d*x2**2 + (b+c)*x1*x2)/2) / scale
```

`KDE` 为每个数据点调用核函数 `K` 并累加所有结果，如 *f*(**x**) 所述。如果你不打算在应用程序中直接调用 `K`，可以将其定义嵌套在 `KDE` 中。

```py
def KDE(x, H, data):

    # unpack two dimensions
    x1, x2 = x

    # prepare the grid for output values
    output = np.zeros_like(x1)

    # process every sample
    for sample in data:
        output += K([x1-sample[0], x2-sample[1]], H)

    return output
```

注意，对于单个数据点，*f*(**x**)等于*K*(**x**）。

最后的函数`show_pdf`显示二维函数`func`并将数据点`data`添加到其中，但`data`不必与函数`func`相关。PDF 有两种视角：轮廓图和表面图。

```py
def show_pdf(func, data, 
             resolution = 100, 
             contours_density = 8,
             surf_density = 40,
             figsize=(10,5), cmap=cm.cividis,
             aspect='equal', margins='auto',
             s = 40, edgecolor='black', 
             markeralpha=1, markercolor='white'
            ):

    # range for x and y axis is determined from the dataset
    x1_min, x2_min = data.min(axis=0)
    x1_max, x2_max = data.max(axis=0)

    # plus some extra margins
    if margins == 'auto':
        x1_pad = max(3, int(0.3*(x1_max-x1_min)))
        x2_pad = max(3, int(0.3*(x2_max-x2_min)))
    else:
        x1_pad = int(margins*(x1_max-x1_min))
        x2_pad = int(margins*(x2_max-x2_min)) 

    x1_range = np.linspace(start=x1_min-x1_pad, 
                           stop=x1_max+x1_pad, num=resolution)
    x2_range = np.linspace(start=x2_min-x2_pad, 
                           stop=x2_max+x2_pad, num=resolution)

    X1_grid, X2_grid = np.meshgrid(x1_range, x2_range)

    # the given func is called here 
    Z_grid = func([X1_grid, X2_grid])

    # draw a figure
    fig = plt.figure(figsize=figsize)
    ax = fig.add_subplot(1, 2, 1)
    c = ax.contourf(X1_grid, X2_grid, Z_grid, 
                    contours_density, cmap=cmap)
    c2 = ax.contour(X1_grid, X2_grid, Z_grid, 
                    contours_density, colors='black')
    ax.set_xlabel(r'$x^{(1)}$', fontsize=16, labelpad=7)
    ax.set_ylabel(r'$x^{(2)}$', fontsize=16, rotation=0, labelpad=8)
    ax.scatter(data[:,0], data[:,1], marker='s', 
               color=markercolor, s=s, edgecolor=edgecolor, alpha=markeralpha)
    ax.set_aspect(aspect)
    plt.clabel(c2, inline=True, fontsize=10)

    ax = fig.add_subplot(1, 2, 2, projection='3d')
    ax.plot_surface(X1_grid, X2_grid, Z_grid, 
                    rcount=surf_density, 
                    ccount=surf_density, cmap=cmap) 
    ax.set_xlabel(r'$x^{(1)}$', fontsize=14)
    ax.set_ylabel(r'$x^{(2)}$', fontsize=14)

    plt.show()
```

# 3\. 高斯核的属性

让我们从最简单的情况开始，用单位矩阵**H**表示：

![](img/88337531474446b59f65f118c985f6c1.png)

坐标轴的原点作为一个数据点。

为了使用提供的代码，你首先需要定义数组`H`和至少一个数据点（`data`数组必须是二维的）。然后你可以对这些数据调用`KDE`。请注意，`show_pdf`接受一个函数`func`作为输入，并将其与网格状数组`x`作为第一个参数进行调用。因此，我们调用了`functools`库中的`partial`方法，它生成了`KDE_partial`函数，执行与`KDE`相同的操作，只是第二个参数`H`是固定的。这就是我将在本故事中使用代码的方法。

```py
# covariance matrix
H = [[1, 0],
     [0, 1]]

# a single point to make PDF
data = np.array([[0,0]])

# fix arguments 'H' and 'data' of KDE function for further calls
KDE_partial = partial(KDE, H=np.array(H), data=data)

# draw contour and surface plots
show_pdf(func=KDE_partial, data=data)
```

高斯核围绕数据点居中。

![](img/0bbb91bfdbf5e2609fc73a3e4a2e7db4.png)

具有 0 协方差的高斯核。

让我们通过将非对角元素固定为 0.5 来引入一些相关性：

![](img/d816f50bc890e382b464db1ab1ecb0f3.png)

KDE 的形状变得倾斜和更薄。半主轴平行于*x*⁽¹⁾ = *x*⁽²⁾线。

![](img/64966317c7fbbf2dda3e4283a240925a.png)

具有 0.5 协方差的高斯核。

随着协方差系数的增加，KDE 变得更加狭窄。由于函数值变化更快，我提高了图形的分辨率。

```py
H = [[1, 0.9],
     [0.9, 1]]

KDE_partial = partial(KDE, H=np.array(H), data=data)

# when the function gets too sharp, boost the resolution
show_pdf(func=KDE_partial, data=data, 
         resolution=300, surf_density=50)
```

![](img/863acbf8dfe28de0447d40598522095c.png)

具有 0.9 协方差的高斯核。

你可以很容易地预测当协方差变为负数时会发生什么。

![](img/6bd75856a6906b7bc097c5a59d184d34.png)![](img/d9cb85c2a2ba083d96878163e492a73e.png)

具有-0.5 协方差的高斯核。

这些示例清楚地展示了高斯 PDF 如何跟随数据的相关结构。

你可能会想知道如何旋转高斯 PDF。为此，请取一个[旋转矩阵](https://en.wikipedia.org/wiki/Rotation_matrix) **R**（我更喜欢顺时针版本）：

![](img/3802a41300a12c961403a1e532af83e1.png)

并将**H**替换为**RHR**ᵀ。以下是一个方便的函数，它返回表示按α度（以弧度为单位）旋转的矩阵的数组。

```py
def angle2rotation_matrix(angle):
    return np.array([ [np.cos(angle), np.sin(angle)],
                     [-np.sin(angle), np.cos(angle)] ])

R = angle2rotation_matrix(angle=(-1/10)*np.pi)
```

由于旋转对称高斯没有意义，我通过改变矩阵**H**的对角组件来拉伸它。

```py
# the first axis scale is expanded twice
H = np.array([[2, 0],
              [0, 0.2]])

# rotation
H = R @ H @ R.T

data = np.array([[0,0]])

KDE_partial = partial(KDE, H=np.array(H), data=data)
show_pdf(func=KDE_partial, data=data)
```

注意，第一个轴的尺度扩展了两倍，而第二个尺度缩小了五倍，这是因为对矩阵**H**的对角元素应用了不同的值。

![](img/e113438e7222e716c1f9e3ab7b46b126.png)

拉伸和旋转的高斯核。

# 4\. 身高和体重数据集

机器学习仓库中有几个现成的数据集。这就是为什么我感到惊讶的是，几乎每个常见的包含身高和体重列的数据集都是合成生成的。为了获得一些真实世界的数据，我要求我的学生提交他们的身高和体重，这个数据集现在可以在我的[Github 仓库](https://raw.githubusercontent.com/jdrapala/datasets/main/HeightsWeightsGender_dataset.xlsx)中找到。

让我们来看看这些数据。

```py
filename = 'HeightsWeightsGender_dataset.xlsx'

# my Github repo
URL = "https://raw.githubusercontent.com/jdrapala/datasets/main/" + filename

# download the data form URL as a DataFrame
df = pd.read_excel(URL)

# make it look appealing
df_styled = df.sample(7).style.background_gradient(axis=0, gmap=df['Weight'], cmap='cividis')

# and export to file
dfi.export(df_styled, 'filename.png', dpi=300)

# extract numpy array from DataFrame
data = df[['Height','Weight']].to_numpy()
```

![](img/7ff9db24d12858a41c7a6fdbe8b562e4.png)

从我的学生那里收集的身高和体重数据样本。

让*x*⁽¹⁾表示身高，*x*⁽²⁾表示体重。

以单位矩阵**H**作为第一次尝试可视化数据集。

```py
H = [[1, 0],
     [0, 1]]

KDE_partial = partial(KDE, H=np.array(H), data=data)

show_pdf(func=KDE_partial, data=data, aspect='auto', margins=0.15)
```

![](img/752275bfb233e340711646a47176ac38.png)

使用的核函数过小。

结果很糟糕；PDF 在数据点周围的狭窄区域内有峰值，而其他地方几乎降至零。

我们通过将整个矩阵**H**乘以常数*s*（用于大小）来增大核函数。取*s*=10。

```py
H = [[1, 0],
     [0, 1]]

s = 10

KDE_partial = partial(KDE, H=s*np.array(H), data=data)
show_pdf(func=KDE_partial, data=data, aspect='auto', margins=0.15)
```

![](img/745f23b3646767636ddfd979f6ea455c.png)

核大小应进一步增加。

单个峰值结合成一个漂亮的 PDF，但它仍然显得过于详细。因此，将*s*增加到 64，以获得以下结果。

![](img/c8f2bb3d94109172ef9f6a50746d8243.png)

核函数似乎大小合适。

这个核函数的大小似乎适合这个数据集。

将我们手工制作的 PDF 输出与 seaborn KDE 进行比较。

```py
# draw PDF
ax = sns.kdeplot(x='Height', y='Weight', data=df, 
                 fill=True, cmap=cm.cividis, 
                 bw_adjust=1.1)
# draw datapoints 
ax.plot(df['Height'], df['Weight'], 's', color='white', 
        markersize=6, markeredgecolor='black', markeredgewidth=0.7)
plt.show()
```

![](img/3d43103059eb959061d632c6c2ba086d.png)

矩阵**H**中的非零协方差元素对 PDF 会产生什么影响？对于协方差条目值为 0.8，返回以下 PDF。

![](img/153164c3772bb1beb7bee3ae9b3dae78.png)

这似乎是前一图形的一个风天变体。

PDF 质量明显下降。

类似的实验表明，带有单位矩阵**H**的高斯核在实际情况中是足够的。因此，一切可能归结为选择适当的参数*s*值，这决定了核函数所覆盖的区域，就像[单变量情况下的带宽*h*](https://medium.com/towards-data-science/kernel-density-estimation-explained-step-by-step-7cc5b5bc4517)一样。

作为一个练习，考虑为我在众多医学数据分析研究项目中收集的大型[身高和体重数据](https://raw.githubusercontent.com/jdrapala/datasets/main/cardiac_patients.xlsx)创建一个 PDF 表示。然而，请记住，这些数据来自于心脏病、糖尿病和其他疾病的患者，因此在为一般人群得出结论时要谨慎。我对这些数据的质量也有一些担忧，因为它是从许多医院收集的。

![](img/790f91ac223496f9a8b25a3d31ef8764.png)

从各种医院收集的身高和体重数据。

# 5\. 外行星数据集

KDE 在多模态分散数据中尤其有用。因此，我给你介绍另一个数据集。我相信我们都对鸢尾花数据集感到厌倦了。

这个数据集是直接从[Exoplanet Orbit Database 网页](http://exoplanets.org/)下载的。由于[文件](http://exoplanets.org/csv-files/exoplanets.csv)很大且包含混合数据类型，我不得不在`read_csv`方法中设置`low_memory=False`。

我选择了两列：系外行星的质量（以 Jupyter 质量为单位）和它与母星的距离（以天文单位为半长轴）。

```py
URL = "http://exoplanets.org/csv-files/exoplanets.csv"

df = pd.read_csv(URL, low_memory=False)

# drop rows containing missing values
df = df[['NAME', 'MSINI', 'A']].dropna()

# i don't like orignal columns names
df = df.rename(columns={'NAME': 'Name', 
                        'MSINI':'Mass', 
                        'A': 'Distance'}).set_index('Name').astype(float)

# some masses are not missing but equal to zero, let's get rid of them too
df = df.drop(df[df['Mass'] == 0].index)

# # make it look appealing
df_styled = df.sample(7).style.background_gradient(axis=0, 
                                                   gmap=df['Mass'], 
                                                   cmap='cividis')
dfi.export(df_styled, 'filename.png', dpi=300)
```

![](img/24703ed607177b6b8940eafa5d7771f1.png)

一份系外行星数据集的示例。

让我们快速查看数据在对数尺度上的分布情况。

```py
with plt.style.context('seaborn'):
    sns.scatterplot(data=df, x='Mass', y='Distance')

plt.xscale('log')
plt.yscale('log')
plt.show()
```

![](img/54cbc0c0a97e65d8fe4de21cadc8ed79.png)

对数尺度上的系外行星数据。

KDE 为*s* = 0.7 生成了视觉上令人满意的 PDF，使用了这些数据。

```py
# logarithmic scale is more appropriate for this data
data = np.log(df[['Mass','Distance']].to_numpy())

H = [[1, 0],
     [0, 1]]

# size of the kernel
s = 0.7

KDE_partial = partial(KDE, H=s*np.array(H), data=data)

show_pdf(func=KDE_partial, data=data, aspect='auto', 
         markeralpha=1, s=1.5, margins=0.15)
```

![](img/8af68eaa3539d512596a6e60e84ba521.png)

系外行星数据被分为三个簇，尽管左侧的簇明显更稀疏。

我建议你尝试 Scikit-learn 库中的`[KernelDensity](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KernelDensity.html)`方法，它允许你在 KDE 拟合数据后轻松生成合成数据。

```py
from sklearn.neighbors import KernelDensity

# fit KDE to the dataset
kde = KernelDensity(kernel='gaussian', bandwidth=0.4).fit(data)

# Generate random samples from the model
synthetic_data = kde.sample(1000)

df = pd.DataFrame({'x1': synthetic_data[:,0], 
                   'x2': synthetic_data[:,1]})

with plt.style.context('seaborn'):
    sns.scatterplot(df, x='x1',y='x2')

plt.xlabel('Mass', fontsize=16)
plt.ylabel('Distance', fontsize=16)
plt.show()
```

![](img/7ab44485c6550063eeac3a1e414b342f.png)

使用 Scikit-learn 合成生成的系外行星数据。

# 最后的话

精美的可视化有助于从数据中学习并得出适当的结论。KDE 使数据分布以视觉上令人愉悦的方式呈现。因此，其大多数数据探索应用归结为二变量情况。

请记住，在单点计算 KDE 值时，需要处理所有数据点，这在大规模计算中可能会耗时。在这种情况下，你应该考虑使用高斯混合模型。不过那是另一回事……

# 致谢

本研究使用了[exoplanets.org](http://exoplanets.org/)上的系外行星轨道数据库和系外行星数据探索器。

***除非另有说明，所有图片均由作者提供。***

# 参考文献

[1] Kristan, M., Leonardis, A., Skočaj, D. [带高斯核的多变量在线核密度估计](https://pdf.sciencedirectassets.com/272206/1-s2.0-S0031320311X00067/1-s2.0-S0031320311001233/main.pdf?X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLWVhc3QtMSJGMEQCIESmajJC%2FTm04hxqPfbYr6kAhdZvgQiBEGFYdOdtqHEQAiBC8RMFSczi8WU3KfQojbtqCm6nvFR%2BqZ%2BQRgt4W3IQGiqzBQh8EAUaDDA1OTAwMzU0Njg2NSIMN7fEICU7zOOWmDNzKpAFyvinHQU2qBx0wD7jtsxjm2ztwinuBy0qGzKyJagx2anwYwb3iKdgYT6m8g3f9slQ87NNdId1xPrNml89D68ejO1oA2dwP5p7c07A9ZwnM3Tcn9uHwOJNcXe0QqQlc6qDhYZH0cgUd3j%2F2jsK1MJCqNyjOIvJA1%2FWSA5bE9nrPOhdysbc%2FU7OULr5ppX1Z14LuuSUCPY2zFJwXAQc7q%2FuE1KJ7uEQg%2BkA6i65Xt5PUrjqxyodqqiYx16lkUEM9hhLvkoInD9n6hVXfk6xBillcv%2FlT9QqSNmNIteuxoUONPZkR29CRXL74%2F4gysMWXjvsOgIR3qaKOWtG5Xh34gNW6VxKfoSfSiz8FLxSBFqJoSvED9LiVL4Bwsk9rsUJTesz8rrGPYco%2F%2Bjy%2FXgYNDFRtLbHrjo%2F%2B6Jy5KTPU7MY7AmiikVG1kXm09Mu6nCzJNOzumo%2Ffo802dWf8e0xXhNpJt%2Bkr9GG2uQvvBBU9GG%2BmMywhb8w2ykMzYShJ%2FLpzTiHawBKgtqtx0BHdiankCPae%2F5mvYv%2BDUBea%2F2piAWSfQXqARy1fQP4jafReC6iL5e4UaWeJO6H%2BdsJjZefyLzteBkpumQ%2Blp%2FjnbzmqhqP8n3oCLsJwtLXMV%2BSwdjlchcLeAeq34kU6NrqWtIHbgnfIkrXeGo3p7UdQZL9Ongwv%2FWPjFXaFWZTlthtbf9HL6epGxuDrj0uL75TtDdsPsWBBZMiBwZTI0BqAoCbN3VS0O9h1jQ4AqTl4z73xAhqAjP%2FGNQSBcyqtLVklT0amW08bmOH5Ut1PdwgbLPBpyZsUPOucRO3IjIVNOxWjQMuKWc4HNLmL9UCIUZd2ksfis3hWSzw8Hf2NtpWIfc7qaFwNTgw5tTMqAY6sgFepI5a7IP8yRlOwSQS4LNRJJ87QB%2B%2BFxD1eTkpNi4%2BQRHNVeJkk5XgdXNbQxXr4GFEOtw14qwYaku453DXyttvQ8weJag0tFd5UUEkx0xrpcfBmwTT2BF3mlu8DlGZhT0DeYz4hhK292mz4qefanyJMhsrOQS0p0P9td0Y9KCKZnLnP4kbi77js8VYa%2Bl3zgkd8tmQPB%2BnCCwl4rmPQy%2BuVf%2F6To0AIgf0H5ebX0%2Bt1DhK&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20230926T192304Z&X-Amz-SignedHeaders=host&X-Amz-Expires=300&X-Amz-Credential=ASIAQ3PHCVTYYCHLCD4W%2F20230926%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=f86b983326f340f62ce4a5455f34ccc230581a7a8f44d266a74152bf8854d4f5&hash=1a47a3164b9811107af215d158b808ea0561ba9a833e4658ad1ee510610ccf9e&host=68042c943591013ac2b2430a89b270f6af2c76d8dfd086a07176afe7c76c2c61&pii=S0031320311001233&tid=spdf-73b3669b-7711-4ae2-8024-1a98651656df&sid=30bec3965af1f544e80850e6c3dac8580543gxrqb&type=client&tsoh=d3d3LnNjaWVuY2VkaXJlY3QuY29t&ua=160f5750000300525556&rr=80cde11778e3bfd0&cc=pl) (2011). *模式识别*, 44(10–11), 第 2630–2642 页.

[2] S. Węglarczyk*,* [核密度估计及其应用](https://www.itm-conferences.org/articles/itmconf/pdf/2018/08/itmconf_sam2018_00037.pdf) (2018), ITM web of conferences, vol. 23, EDP Sciences.

[3] Han, E., Wang, S. X., Wright, J. T., Feng, Y. K., Zhao, M., Fakhouri, O., Brown, J. I., Hancock, C. [外行星轨道数据库 II. 对 exoplanets.org 的更新](https://ui.adsabs.harvard.edu/abs/2014PASP..126..827H/abstract) (2014). *太平洋天文学会出版物*, 126(943), 827.
