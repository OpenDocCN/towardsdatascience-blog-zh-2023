# 用于神经退行性疾病分类的多层神经网络

> 原文：[`towardsdatascience.com/multi-layer-neural-network-for-neurodegenerative-disorder-classification-27c0a5efc766`](https://towardsdatascience.com/multi-layer-neural-network-for-neurodegenerative-disorder-classification-27c0a5efc766)

![](img/b8e6cdffb84e992c90f9e59b694fb71b.png)

照片由 [Jon Tyson](https://unsplash.com/es/@jontyson?utm_source=medium&utm_medium=referral) 拍摄，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 关于开发多层神经网络以分类阿尔茨海默病磁共振图像的逐步指南

[](https://luca-zammataro.medium.com/?source=post_page-----27c0a5efc766--------------------------------)![Luca Zammataro](https://luca-zammataro.medium.com/?source=post_page-----27c0a5efc766--------------------------------)[](https://towardsdatascience.com/?source=post_page-----27c0a5efc766--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----27c0a5efc766--------------------------------) [Luca Zammataro](https://luca-zammataro.medium.com/?source=post_page-----27c0a5efc766--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----27c0a5efc766--------------------------------) ·阅读时间 23 分钟·2023 年 3 月 3 日

--

在 2019 年 12 月，在*Medium*和*Toward Data Science*的支持下，我开始撰写一系列文章和 Python 教程，解释机器学习（ML）是如何工作的，以及它应如何应用于生物医学数据。从零开始构建机器学习算法的想法，没有像*TensorFlow*或*Scikit-learn*这样的框架的支持，这是有意为之的，目的是解释机器学习的基础，特别是对那些对所有概念有深入理解的读者有益。此外，过度引用的框架是强大的工具，需要特别关注参数设置。因此，深入研究它们的基础可以向读者解释如何充分利用它们的潜力。

随着时间的推移，我意识到*诊断成像*可能代表了生物医学领域主要受益于人工智能帮助的部分。如果没有其他因素，这个领域为优化机器学习算法提供了许多线索。确实，诊断成像是一个医学领域，其中变异性和定义难度与需要抽象化病理过程的本质相碰撞。

今天，更高效的人工智能系统辅助医生和科学家完成挑战性任务，例如基于影像的早期疾病诊断。人工智能的贡献不仅限于帮助快速诊断疾病，还可以熟练揭示病理过程的根本原因以及组织或器官何时开始退化。以阿尔茨海默病（AD）为例，这是一种导致神经退化和脑组织丧失的慢性神经疾病（McKhann 等人，1984 年）：照顾 AD 患者的成本趋于上升，因此及时诊断变得越来越必要。

在这篇文章中，我希望引起您的注意，我们将构建一个多层感知器（MLP 神经网络），作为阿尔茨海默病磁共振成像数据（ADMRI）图像分类系统，数据来自 Kaggle。在我之前的文章中，我描述了基于逻辑回归（LR）的图像检测和结果预测的其他直接方法。在那些文章中，我们看到如何成功地对 MNIST 手写数字或 Zalando 数据进行分类，准确率达到 97%。然而，当训练集的结构达到一定复杂性时，例如 ADMRI，这些方法可能不会那么高效。

对于本文中实现的代码，我们将使用 Python。我建议使用[*Jupyter notebook*](https://jupyter.org/install)和 Python 版本≥3.8。代码需要最低限度的优化计算，特别是涉及线性代数的部分。我们将使用如*Pandas*、*NumPy*、*matplotlib*和*SciPy*等包，这些包是[ SciPy.org](http://scipy.org/)的一部分，是一个基于 Python 的数学、科学和工程的开源软件生态系统。此外，我们还将导入*matplotlib*，一个 Python 数据可视化库。此外，还将创建一个*opt*对象，来自*scipy.optimize*，用于对梯度进行优化。最后一个库是“pickle”，它对于打开 pickle 格式的文件至关重要。

> 建议：
> 
> 为了更好地理解概念，例如 sigmoidal 函数、成本函数和线性与逻辑回归中的梯度下降，我建议读者首先阅读我之前关于这些主题的文章：
> 
> 1\. 一元或多元线性回归；
> 
> [2\. 用于癌症恶性预测的逻辑回归](https://medium.com/towards-data-science/logistic-regression-for-malignancy-prediction-in-cancer-27b1a1960184)；
> 
> 3\. 使用 One-vs-All 检测黑色素瘤；
> 
> 4\. Python 中用于图像识别的 One-vs-All 逻辑回归。
> 
> 这些是这里将理所当然地使用的基本准备概念。此外，许多概念将在这里简要提及，因此在阅读这些建议的数字顺序的其他文章之前，请等待开始阅读本文。

## 数据集。

阿尔茨海默病磁共振成像数据（ADMRI）可以从[Kaggle](https://www.kaggle.com/datasets/uraninjo/augmented-alzheimer-mri-dataset)下载。数据包含两个文件夹：第一个包含 33984 张增强的 MRI 图像，第二个包含原始图像。数据有四类图片在训练和测试集中：1) *非痴呆，2) 中度痴呆，3) 轻度痴呆，4) 极轻度痴呆*。每张图片包含约 200 x 200 像素，对应许多特征（40000）。

对于这个练习，我提供了相同数据集的转换版本，你可以从以下[链接](https://www.kaggle.com/datasets/lucazammataro/augmented-alzheimer-mri-dataset-lowres-50x50)下载。这“轻量”版本包含了缩减到 50 x 50 分辨率的整个增强数据集。解压后得到的 pickle 文件是一个*pandas dataframe*，由两列组成，一列包含每张图片的 2500 个特征，另一列包含相应的标签，定义如下：

```py
Label 1: Non-Demented
Label 2: Moderate Demented
Label 3: Mild Demented
Label 4: Very Mild Demented
```

首先，我们需要上传所有必要的库来打开数据集和显示图像：

```py
import pandas as pd
import numpy as np 
from matplotlib.pyplot import imshow
import pickle
```

然后，打开 pickle 文件，发现包含的数据框，输入以下代码到 Jupyter notebook 单元格中：

```py
with open('AugmentedAlzheimer.50X50.training.dataset.pickle', 'rb') as handle:
    ALZ = pickle.load(handle)
```

我们在这里称之为“ALZ”的数据框包含了所有图片及其相应标签。在新单元格中输入“ALZ”将显示数据结构：

![](img/b10247f4015da2557b34866f8a1cc8c5.png)

图 1\. 训练数据集结构（图片由作者提供）

现在，将每个数据框的列转换为两个不同的*numpy*对象，*X*为训练数据集，*y*为标签向量*。（*将数据转换为 numpy 对象是线性代数计算所必需的*）：

```py
X = np.stack(ALZ['X'])
y = np.int64(ALZ['y']) 
```

*X*向量包含 33,984 个项目，每个项目包含一个 2500 灰度值的向量。numpy 的*shape*方法显示 X 的结构：

![](img/f0895939d76115c8ff8dc330726a5e90.png)

操作*X*时，我们可以通过索引访问单个项目；例如，要访问第一张图片（索引=0）的前 250 个值，输入：

![](img/05bb898dce13dfbc463cc9fa2d42bc04.png)

图 2\. 显示 X[0]内容（前 250 个值）（图片由作者提供）

每个像素对应 0–255 范围内的特定灰度值，其中 0 是黑色，255 是白色。现在我们可以访问 X 向量，*matplot*函数*imshow()*将显示一个 2500 像素的灰度图像，框架为 50 x 50 像素的 2D 表示：

```py
'''
Reshape a 2500-values vector extracted from one of the images
stored in the vector X (i.e.: #12848).
Use the NumPy method .reshape, specifiying the double argument '50'
then show the image with the function imshow, specifying the argument 
cmap='gray'
'''

image = X[12848].reshape(50, 50)
print('image:', 12848)
print('label:', y[12848])

imshow(image, cmap='gray')
```

运行代码时，图像编号#12848 显示如下：

![](img/f15831bca5811592bb39dbbe7cf91ab9.png)

图 3\. 来自增强阿尔茨海默 MRI 数据集的灰度图像，表示图像#12848 对应于标签 2：中度痴呆。（图片来源于[`www.kaggle.com/datasets/uraninjo/augmented-alzheimer-mri-dataset`](https://www.kaggle.com/datasets/uraninjo/augmented-alzheimer-mri-dataset)）

我们更愿意使用一个简单的函数随机显示从数据集中挑选的五十张图像，而不是逐一操作。以下是代码：

```py
'''
plotSamplesRandomly
Function for visualizing fifty randomly picked images, from the dataset
'''

def plotSamplesRandomly(X, y):

    from random import randint
    import matplotlib.pyplot as plt
    %matplotlib inline

    # create a list of randomly picked indexes.
    # the function randint creates the list, picking numbers in a 
    # range 0-33983, which is the length of X

    randomSelect = [randint(0, len(X)) for i in range(0, 51)]

    # reshape all the pictures on the n X n pixels, 
    # where n = sqrt(size of X), in this case 50 = sqrt(2500)
    w, h =int(np.sqrt(X.shape[1])), int(np.sqrt(X.shape[1]))
    fig=plt.figure(figsize=(int(np.sqrt(X.shape[1])), int(np.sqrt(X.shape[1]))))

    # Define a grid of 10 X 10 for the big plot. 
    columns = 10
    rows = 10

    # The for loop
    for i in range(1, 51):

        # create the 2-dimensional picture
        image = X[randomSelect[i]].reshape(w,h)
        ax = fig.add_subplot(rows, columns, i)

        # create a title for each pictures, containing #index and label
        title = "#"+str(randomSelect[i])+"; "+"y:"+str(y[randomSelect[i]])

        # set the title font size
        ax.set_title(title, fontsize=np.int(np.sqrt(X.shape[1])/2))                

        # don't display the axis
        ax.set_axis_off()

        # plot the image in grayscale
        plt.imshow(image, cmap='gray')

    # Show some sample randomly
    print('\nShow samples randomly:')
    plt.show()
```

运行`plotSampleRandomly()`函数，传入参数 X 和 y：

```py
plotSamplesRandomly(X, y)
```

输出见图 4：

![](img/2a2fc41a94a83b38ebf5fe41611273d5.png)

图 4\. 可视化从阿尔茨海默病磁共振成像数据集中随机挑选的图像。每个瓦片代表一幅磁共振图像，附有其识别编号和标签（例如，#10902; y:2，表示图像编号 10902 对应于“中度痴呆”诊断），这些图像来自[`www.kaggle.com/datasets/uraninjo/augmented-alzheimer-mri-dataset`](https://www.kaggle.com/datasets/uraninjo/augmented-alzheimer-mri-dataset)

此外，我还制作了原始阿尔茨海默病磁共振成像数据集的简化版本，可以从这个 Kaggle [link](https://www.kaggle.com/datasets/lucazammataro/alzheimer-original-dataset-lowres-version)下载。可以使用此数据集进行测试。

> **重要的是：本练习的唯一目的是展示将 MLP 神经网络应用于阿尔茨海默病磁共振图像的基本概念，并且仅用于实验目的，不用于临床使用。**

## Logistic Unit 模型

让我们从一些历史定义开始：在神经网络（NN）中，我们定义了一类从生物神经细胞网络中汲取灵感的 AI 算法。它们诞生的具体目的是试图模拟人脑功能。其运作假设的基础，正是在八十年前由著名神经科学家进行的研究中建立的。1943 年，沃伦·麦卡洛克和沃尔特·皮茨发表了一项具有重大意义的工作，题为*"神经活动中固有思想的逻辑演算"*，首次描述了生物神经网络如何协同工作以完成复杂任务的计算模型。

从那一年开始，许多 NN 模型接踵而至。例如，1957 年，弗兰克·罗森布拉特定义了*感知器*的概念，这是一种能够对输入和输出之间的线性关系进行分类的人工神经元。我们可以毫无疑问地说，人工智能在那一刻诞生了。然而，尽管对这些模型的兴趣大幅增长，但当时的巨大硬件限制使得人工智能仍停留在希望和梦想的世界中，而非现实世界。我们不得不等到 1986 年，D. E. Rumelhart 发表了一篇论文，引入了*反向传播训练*的概念，从而提出了一种能够找到成本函数最小值的新算法，自动化计算成本函数的导数，简而言之，Rumelhart 发明了*梯度下降*。

当我们实现神经网络时，以现代意义上的神经网络，我们指的是对一组生物神经元所做的简化模型。在这种简化中，神经元只是一个**逻辑单元**，通过各种树突连接到其他类似的输入单元，并通过其轴突产生输出值。

![](img/4f9c9a099f0265d51a2cd96b631e528f.png)

图 5\. 逻辑单元模型（作者提供的图片）

逻辑单元模型简化了生物神经元。体部（图 5 中的蓝色圆圈）整合来自输入神经元（X 向量）的值，包括基础神经元（红色），通过电缆连接到逻辑单元，并通过其轴突产生输出值。该输出对应于假设模型*hθ*。表示*hθ*的一种方式是*g(z)*函数，它是一个 sigmoid 函数（或逻辑函数），因此是非线性的，其公式可以写成这样：

![](img/a76e172d7b37bcbb8c879c06f938a021.png)

g(z)函数使用翻译后的*θ* *向量*与***X*** *向量*的乘积（我们称这个乘积为***z***）作为参数，可以定义为：

![](img/6c437414bd8785b65f8a08a73f7fbd65.png)

假设 X 向量取值为 0 或 1，g(z) 当 z = [θ₀X₀ + θ₁X₁ + θ₂X₂] 时，计算输出可能为 0 或 1 的概率。Sigmoid 逻辑函数也称为*激活函数*，这个定义源于每个生物神经元的生理功能，它根据接收到的输入类型被激活。然后，这个输入与*θ 向量*（逻辑单元中的权重向量）代数地相乘。对于所有回归模型（线性或逻辑），逻辑单元都有一个*成本函数*，它可以记录我们距离*hθ*的最小值有多远，并帮助我们找到最佳的 θ。

但在理解神经网络的成本函数之前，让我们通过一些直觉和两个实际示例来了解神经网络是如何工作的。我们将描述的这个神经网络是故意简单的；它没有隐藏层，仅由一个逻辑单元和几个输入神经元组成。

## 简单神经网络直觉 I：AND 门

例如，假设我们希望我们的神经网络学习逻辑门 AND：

![](img/e2ffddf3ab7b0910843ea1e67c33e762.png)

图 6a. 将 AND 门应用于简单神经网络（作者提供的图片）

图 6a 展示了将 AND 门实现到逻辑单元中的示例，它利用了我们故意指定的三个*θ*值：*θ*向量确实等于三元组θ₀*=*-3，θ₁*=*+2，θ₂*=*+2（绿色值）。红色的表格部分表示 AND 真值表。两个 AND 操作数是 X1 和 X2，它们可以取 0 或 1 的值，而 X0 是表示偏置的输入神经元，默认值为 1。输出函数 Y 对应于假设模型 hθ，由函数 g(z)实现。根据*z*的不同，函数*g(z)*返回接近 0 或 1 的值，这取决于*z*位置映射到的 sigmoid。

![](img/f142e2e8c366459e1ce573696f1def16.png)

图 6b：AND 门逻辑函数的放大图（作者提供的图像）

例如，对于 X1 = 0 和 X2 = 0，g(z)将等于 0.05，因为 X 向量中唯一不等于 0 的值是 X0，即存储在偏置神经元中的值（-3）。实际上，g(-3)=0.05 映射到 sigmoid 时接近于零。相反，当 X1 = 1 和 X2 = 1（唯一使 X1 AND X2 为真的条件）时，g(z)等于 g(-3+2+2) = g(1)，映射到 sigmoid 时返回 0.73，这个值远远超过 0.5 的阈值，接近 1。在其他两个剩余情况下，即 X1=0，X2=1 和 X1=1，X2=0，g(z)等于 g(-1)=0.27，这个值再次接近 0（图 6b）。所有四种情况下的 g(z)输出（图 6a 中的黄色）返回了 AND 门的预期结果。sigmoid 逻辑函数的优点是它可以平滑地计算输入神经元信号组合的 0/1 概率，像生物神经元一样生理地激活。此外，sigmoid 函数将有助于保证*梯度下降*算法寻求*θ*值时收敛到全局最小值，避免处理具有多个局部最优的非凸*成本函数*的问题（详见[癌症恶性预测的逻辑回归](https://medium.com/towards-data-science/logistic-regression-for-malignancy-prediction-in-cancer-27b1a1960184)以获取有关非凸成本函数问题的详细讨论）。sigmoid 逻辑函数的 Python 实现如下：

```py
# Sigomid Logistic (Activation) Function
def sigmoid(z): 
    g = 1.0 / (1.0 + np.exp(-z))
    return g
```

请复制并粘贴到新的笔记本单元中。你可以运行该函数，将不同的*z*值作为参数传递给它，并查看它产生的输出，例如：

![](img/415bfd171f27ec75154f5eb97d05a00e.png)

或者是否可以实现一个出色的函数来可视化 g(z)及其输出，如下所示：

```py
# Plot the Sigmoid function, and its output:

def plotSigmoid(z):
    import numpy as np
    from matplotlib import pyplot as plt

    plt.rcParams["figure.figsize"] = [7.50, 3.50]
    plt.rcParams["figure.autolayout"] = True

    x = np.linspace(-10, 10, 100)

    # setting the axes at the centre
    fig = plt.figure()
    ax = fig.add_subplot(1, 1, 1)
    ax.spines['left'].set_position('center')
    ax.spines['bottom'].set_position('zero')
    ax.spines['right'].set_color('none')
    ax.spines['top'].set_color('none')
    ax.xaxis.set_ticks_position('bottom')
    ax.yaxis.set_ticks_position('left')

    plt.plot(x, sigmoid(x), color='blue')

    plt.plot(0, sigmoid(0), marker="o", markersize=5, 
             markeredgecolor="black", markerfacecolor="black")    
    if(z!=0):
        plt.text(0, sigmoid(0), "  0.5")

    plt.plot(z, sigmoid(z), marker="o", markersize=5,
             markeredgecolor="red", markerfacecolor="red")

    value = "{:5.3f}".format(sigmoid(z))
    plt.text(z, sigmoid(z), str("  z="+str(z)+"; value="+str(value)))

    plt.show()
```

例如，运行 plotSigmoid，对于 z = 1，即 0.73，将显示如下：

![](img/0bddf5a3371b6ff3aac56ff61d42de42.png)

（作者提供的图像）

## 简单神经网络直观 II：OR 门

关于 AND 门，我们可以用一个涉及 OR 门的其他示例来进行实验。

![](img/196c488d17377c02a3f87248db4bd0d2.png)

图 7a：OR 门在逻辑单元中的应用（图片由作者提供）

通过将偏置神经元 X0 的θ值从 -3 更改为 -1 并重新进行所有计算，输出现在反映了 OR 门：我们有三个 X1 和 X2 输入对，它们被映射到 g(z) sigmoid 函数中，产生接近 > 0.5、接近 1 的值。

![](img/f269d3655e2c2215eb7ea1bdd8629c1f.png)

图 7b：OR 门的逻辑函数放大图（图片由作者提供）

## 多层感知机（MLP）模型

多层神经网络由一个输入层、一个或多个隐藏层以及一个输出层组成。所有层（但输出层除外）都包含一个偏置神经元，所有单元完全连接。隐藏层的数量区别于经典的 MLP 和深度学习神经网络。由于其架构的特性，神经网络可以帮助我们提出复杂的非线性假设。

![](img/f2a3a47656b42d422d653bd375e3f336.png)

图 8：多层神经网络（图片由作者提供）

观察 MLP 的模型表示，我们发现 MLP 运作的基础计算与逻辑回归中的计算类似。这一观察确认了 MLP 实质上是一个“超级”逻辑回归器。结构（图 8）显示了三个神经层。第一层是输入层，由*X 向量*组成。第二层由基于*X* 乘以 θ 的结果激活的隐藏节点组成。这些激活的结果成为第三层，即输出层的输入。第三层也包括激活单元。唯一的区别是，它的结果应对应于假设模型 *hθ*。在这里，我们必须使用条件动词，因为神经网络的核心问题正是尝试最小化假设与来自 X 乘以 θ 产品的预测结果之间的误差。

![](img/80017a601e9f5611a3e59ea3a8889ecd.png)

图 9：多层神经网络的模型表示。每个激活单元的计算根据图 8 中表示隐藏层单元的彩色圆盘进行颜色编码。（图片由作者提供）

最小化过程体现在所谓的训练阶段。它包括找到那些θ值（权重），使得*Xθ*能够通过*梯度下降*符合*hθ*的要求。我们必须将一个维度为*Sj*乘以*(n + 1)*的θ矩阵相乘，其中*Sj*是*J*层的激活节点数量：

![](img/09e5baf77e7dc830098a42feac4fa6a3.png)

（图片由作者提供）

和，

![](img/9b0e48c8eb4a8ef5e78d28a2b525402d.png)

（图片由作者提供）

## 实现 MLP 神经网络以增强阿尔茨海默病 MRI 分类

我们将创建一个单层 MLP。隐藏层的单元数计算为输入单元的 2/3 加上输出单元的数量。这个设置保证了训练成功，尽管其他架构在隐藏层使用了更多或更少的激活单元。输入单元的数量取决于数据集中单个图像的像素数量，而输出单元的数量对应于类别标签。整个算法分为三个主要函数：

```py
1\. init
2\. training
3\. testing
```

这三个主要函数包含其他函数，我们将在适当的时候进行描述。

这里介绍的 NN 是一个 *前向传播 MLP*；因此，输入数据通过网络向前传播以生成输出。隐藏层处理数据并移动到下一个层。在前向传播过程中，激活发生在每个隐藏层和输出层节点上。激活函数是加权和的计算。基于加权和，激活函数被应用于使神经网络通过使用偏置非线性流动。

## 初始化

上传数据集后，定义 *X* 和 *y* 向量：

```py
with open('AugmentedAlzheimer.50X50.training.dataset.pickle', 'rb') as handle:
    ALZ = pickle.load(handle) 
X = np.stack(ALZ['X'])
y = np.int64(ALZ['y']) 
```

*X* 向量将包含 33,984 个项目，每个项目包含一个 2500 灰度值的向量。*init* 函数使用方法 *X.shape* 根据 X 的大小初始化 NN 架构：

```py
'''
function "init"
Neural Network initialization and parameter setup
'''
def init(X, y):
    I  = X.shape[1]  # n x n MRI input size (2500)
    H1 = int(X.shape[1]*2/3) + max(set(y)) # hidden units size: 
                        # 2/3 of the input units + the number of output units (1670)
    O = max(set(y)) # output units size or labels (4)
    m = X.shape[0] 

    ini_Th1 = thetasRandomInit(I, H1)
    ini_Th2 = thetasRandomInit(H1, O)

    # Unroll parameters 
    ini_nn_params = np.concatenate((ini_Th1.T.flatten(), ini_Th2.T.flatten()))
    ini_nn_params = ini_nn_params.reshape(len(ini_nn_params), 1)

    print('\nNeural Network Parameters initialized!\n')
    print('\nNeural Network structure:\n')
    print('Input layer neurons:', I)
    print('1st hidden layer neurons:', H1)
    print('Output layer neurons:', O)

    return ini_nn_params, I, H1, O
```

参数 *I, H1, 和 O* 对应于输入层、隐藏层和输出层的大小；*ini_nn_params* 包含 *θ* 向量的“展开”版本（*Th1* 对于输入层，*Th2* 对于隐藏层）。使用展开的扁平化向量将减少处理过多向量的复杂性，使代码更易读。*init* 函数接受向量 *X* 和 *y* 作为参数，但需要子函数 *thetasRandomInit* 嵌套在内部，用于随机化 *Th1* 和 *Th2*。（这些必须在训练阶段之前随机生成）。

*thetasRandomInit* 函数可以如下实现：

```py
'''
function "thetasRandomInit"
Random initialization of thetas
'''
def thetasRandomInit(L_in, L_out):
    e_init = 0.12
    W = np.random.rand(L_out, 1 + L_in) * 2 * e_init - e_init
    return W
```

W 矩阵包含一个随机值向量，它是基于一个 *e_init* 值实现的，默认设置为 0.12。请将之前的代码复制粘贴到一个新单元格中并运行。调用 *init* 函数并定义 NN 参数 *ini_nn_params, I, H1, O* 类型如下：

```py
ini_nn_params, I, H1, O = init(X, y)
```

init 函数将初始化这四个变量，并生成输出：

```py
Neural Network Parameters initialized!

Neural Network structure:

Input layer neurons: 2500
1st hidden layer neurons: 1670
Output layer neurons: 4
```

该函数描述了 NN 的结构，包括 2500 个输入单元，1670 个隐藏单元，以及四个输出单元。

## 训练

对于使用我们上传的 Alzheimer’s MRIs 数据集（*X* 和 *y* 向量）训练 NN，我们需要创建一个 *training* 函数，这只是一个将 *nnCostFunction* 传递给 *opt* 对象的“包装”代码：

```py
'''
function "training"
Neural Network training
'''
def training(ini_nn_params, I, H1, O, X, y):
    import scipy.optimize as opt

    lambd = 1
    print('\nTraining Neural Network... \n')

    result = opt.minimize(fun=nnCostFunction, 
                          x0=ini_nn_params,
                          args=(I, H1, O, X, y, lambd),
                          method='TNC', 
                          jac=True, 
                          options={'maxiter': 1000, "disp": True})

    params_trained = result.x

    Th1_trained = np.reshape(params_trained[0:(H1 * (I + 1)), ],
                                 (H1, I + 1))
    Th2_trained = np.reshape(params_trained[(H1 * (I + 1)):, ],
                                 (O, H1 + 1))

    print('\nTrained.\n')

    return Th1_trained, Th2_trained
```

函数 *training* 使用 *scipy* 对象 *opt* 来最小化神经网络成本函数 (*nnCostFunction*) 的 *梯度下降*。该函数通过 “maxiter” 参数确定实现最小化目标所需的迭代次数。正如 *opt.minimize* 的参数列表中所指定的，这里使用的方法是 TNC（截断牛顿法），它可以最小化标量函数，如 *nnCostFunction*，无论是单变量还是多变量。最后，*training* 将返回两个经过训练并准备好用于测试阶段的 *θ* 向量，*Th1* 和 *Th2*。

*nnCostFunction* 代表了这里展示的最复杂的例程，是整个算法的核心。现在我们将更详细地解释它的作用。我建议在学习 *nnCostFunction* 代码时考虑图 8 中的模式和图 9 中的计算。

```py
'''
nnCostFunction
Implements the Neural Network Cost Function
'''
def nnCostFunction(nn_params, I, H1, O, X, y, lambd):

    # 1\. RESHAPING
    # Reshape nn_params back into the parameters Th1 and Th2, 
    # the weight matrices

    Th1 = np.reshape(nn_params[0:(H1 * (I + 1)), ],
                         (H1, I + 1))
    Th2 = np.reshape(nn_params[(H1 * (I + 1)):, ],
                         (O, H1 + 1))

    # 2\. SETUP OF Y
    # Setup the output (y) layout
    m = X.shape[0] 

    Y = np.zeros((m, O))
    for i in range(m):
        Y[i, y[i] - 1] = 1   

    # 3\. INITIALIZE J, and THETAS
    J = 0
    Th1_grad = np.zeros(Th1.shape) 
    Th2_grad = np.zeros(Th2.shape) 

    # 4\. PREPARE ALL THE VECTORS FOR THE FORWARD PROPAGATION
    # Six new vectors are generated here: a1, z2, a2, z3, a3, and h.
    # The vector a1 equals X (the input matrix), 
    # with a column of 1's added (bias units) as the first column.

    a1 = np.hstack((np.ones((m, 1)), X))  

    # z2 equals the product of a1 and Th1
    z2 = np.dot(a1, Th1.T) 

    # The vector a2 is created by adding a column of bias units 
    # after applying the sigmoid function to z2.
    a2 = np.hstack((np.ones((m, 1)), sigmoid(z2))) 

    # z3 equals the product of a2 and Th2
    z3 = np.dot(a2, Th2.T) 
    a3 = sigmoid(z3) 

    # The Hypotheses h is = a3
    h = a3 

    # 5\. MAKE REGUKARIZED COST FUNCTION.
    # calculate P
    p = np.sum([
        np.sum(np.sum([np.power(Th1[:, 1:], 2)], 2)),
        np.sum(np.sum([np.power(Th2[:, 1:], 2)], 2))
    ])

    # Calculate Cost Function
    J = np.sum([
        np.divide(np.sum(np.sum([np.subtract(np.multiply((-Y), np.log(h)), 
                                             np.multiply((1-Y), np.log(1-h)))], 2)), m),
        np.divide(np.dot(lambd, p), np.dot(2, m))
    ])    

    # 6\. FORWARD PROPAGATION
    # d3 is the difference between a3 and y. 
    d3 = np.subtract(a3, Y)

    z2 = np.hstack((np.ones((m, 1)), z2))
    d2 = d3.dot(Th2) * gradientDescent(z2)    
    d2 = d2[:, 1:]

    # GRADIENTS
    # Delta1 is the product of d2 and a1\. 
    delta_1 = np.dot(d2.T, a1)

    # Delta2 is the product of d3 and a2\. 
    delta_2 = np.dot(d3.T, a2)

    # Regularized Gradients.
    P1 = (lambd/m) * np.hstack([np.zeros((Th1.shape[0], 1)), Th1[:, 1:]])
    P2 = (lambd/m) * np.hstack([np.zeros((Th2.shape[0], 1)), Th2[:, 1:]])

    Theta_1_grad = (delta_1/m) + P1
    Theta_2_grad = (delta_2/m) + P2

    grad = np.hstack((Theta_1_grad.ravel(), Theta_2_grad.ravel()))

    return J, grad
```

函数 *nnCostFunction()* 接受参数 *nn_params*，即我们之前创建的 *ini_nn_params*，它包含了 *θ* 向量的展开版本；*I, H1, O*（分别是输入层、隐藏层和输出层的大小）；两个向量 *X, y*；以及对应于正则化参数 *lambda* 的 *lambd*。该函数返回 J 和 *θ 梯度向量*。

1.  首先，*nnCostFunction* 将 *ini_nn_params* 重新调整为参数 *Th1* 和 *Th2*，考虑到这两个向量的原始大小。

1.  代码需要创建输出层的设置，首先生成一个由 *n-label-columns * n-X_samples* 组成的向量 *Y*，初始值为零，然后用 1 填充，以这种方式对输出进行编码：

```py
Structure of Y (the output layer):

1,0,0,0 for label 1
0,1,0,0 for label 2
0,0,1,0 for label 3
0,0,0,1 for lable 4
```

新的编码随后被赋值给 *y* 向量。

3\. 用零初始化 *成本 J* 和 *θ* 向量，用于 *梯度下降*。

4\. 为 “前向传播” 准备所有向量。这里生成了六个新向量：*a1, z2, a2, z3, a3* 和 *h*。向量 *a1* 等于 *X*（输入矩阵），在第一列添加了一列 1（偏置单元）。向量 *z2* 等于 *a1* 和 *θ1 (Th1)* 的乘积。向量 *a2* 是在对 *z2* 应用 sigmoid 函数后添加了一列偏置单元创建的。向量 *z3* 等于 *a2* 和 *θ1 (Th2)* 的乘积。最后，向量 *z3* 被复制到向量 *h* 中，表示假设 *hθ*。这个步骤表面上看似不重要，但为了更好的可读性，复制 *z3* 到 *h* 是必要的。

5\. 该部分函数实现了我们希望被正则化的 *成本函数*。请回顾逻辑回归成本函数的公式：

![](img/9551b290ed9653b47b9a8f82e0d82784.png)

图 10：逻辑回归成本函数。（作者提供的图片）

正则化版本是：

![](img/d3a35c3698cd85275c4be73626ed0dea.png)

图 11：正则化逻辑回归成本函数。（作者提供的图片）

该实现通过将 *lambda* 乘以 *probability p*（在公式中未出现）来优化正则化；*p* 计算为 *θ1* 和 *θ2* 的幂的总和。

6\. 这部分代码将执行前向传播。在执行此任务时，代码必须创建一个新的向量 *d3*，用于收集 *a3* 和 *y* 之间的差异；在这里，*z2* 被处理，添加了一列 1，而新的向量 *d2* 将包含 *d3* 和 *Th2* 的乘积，以及 *Gradient Descent* 的输出，应用于 *z2*。

*Gradient Descent* 函数的实现如下：

```py
'''
gradientDescent
returns the gradient of the sigmoid function

'''
def gradientDescent(z):
    g  = np.multiply(sigmoid(z), (1-sigmoid(z)))
    return g
```

要运行 *training* 函数，请调用它，并指定 *ini_nn_params*、*I*、*H1*、*O*、*X* 和 *y* 作为参数：

```py
Th1_trained, Th2_trained = training(ini_nn_params, I, H1, O, X, y)
```

> 注意：
> 
> 训练可能需要十二小时的计算。两个训练好的 *θ* 向量可以通过这两个链接获得：[Theta1](https://www.kaggle.com/datasets/lucazammataro/tds-mlp-nn?select=ALZ.50.df_theta_1.pickle)，[Theta2](https://www.kaggle.com/datasets/lucazammataro/tds-mlp-nn?select=ALZ.50.df_theta_2.pickle)

下载后，你可以通过输入以下命令上传这两个文件：

```py
# Upload thetas from the two files
with open('ALZ.50.df_theta_1.pickle', 'rb') as handle:
        Th1_trained = pickle.load(handle).values
with open('ALZ.50.df_theta_2.pickle', 'rb') as handle:
        Th2_trained = pickle.load(handle).values 
```

## 测试

测试函数用于测试神经网络训练：

```py
'''
testing()
'''
def testing(Th1, Th2, X, y):

    m, n = X.shape
    X = np.hstack((np.ones((m, 1)), X))
    a2 = sigmoid(X.dot(Th1.T))
    a2 = np.hstack((np.ones((m, 1)), a2))
    a3 = sigmoid(a2.dot(Th2.T))

    pred = np.argmax(a3, axis=1)
    pred += 1  
    print('Training Set Accuracy:', np.mean(pred == y) * 100)
    return pred
```

该函数非常简单，并且在概念上重新追溯前向传播路径。在将 1 的偏置列应用于 *X* 后，*a2* 加载 *X* 乘以 *θ1* 的 sigmoid 值，并且只有在将 1 的偏置列添加到 *a2* 后，*a3* 才加载 *X* 乘以 *θ2* 的 sigmoid 值。参数 *pred*（预测）等于 *a3* 的最大值。最后，函数返回 *pred* 和 *y* 向量之间成功结果匹配的百分比。

在运行代码之前，上传测试版本的相同数据集：

```py
# LOAD TESTING DATASET
with open('Alz.original.50X50.testing.dataset.pickle', 'rb') as handle:
    ALZ = pickle.load(handle) 
X = np.stack(ALZ['X'])
y = np.int64(ALZ['y']) 
```

要运行 *testing*，请调用它，并指定两个训练好的 *θ* 向量、*X* 和 *Y* 作为参数：

```py
pred = testing(Th1_trained, Th2_trained, X, y)
```

该神经网络架构在阿尔茨海默 MRI 数据集上的准确率为 **95%**

看到神经网络的实际运行效果将非常棒，展示一些具体结果。以下函数表示 *plotSampleRandomly* 的修改版，即我们用来随机显示数据集中 MRI 图像的函数，适合我们。它接受向量 *X* 和 *y* 作为参数，还有在测试阶段创建的向量 *pred*：

```py
'''
plotSamplesRandomlyWithPrediction
Function for visualizing fifty randomly picked images with their prediction.
'''

def plotSamplesRandomlyWithPrediction(X, y, pred):
    from random import randint
    from matplotlib import pyplot as plt

    # create a list of randomly picked indexes.
    # the function randint creates the list, picking numbers in a 
    # range 0-Xn, which is the length of X

    randomSelect = [randint(0, len(X)) for i in range(0, 51)]

    # reshape all the pictures on the n X n pixels, 
    w, h =int(np.sqrt(X.shape[1])), int(np.sqrt(X.shape[1]))
    fig=plt.figure(figsize=(int(np.sqrt(X.shape[1])), int(np.sqrt(X.shape[1]))))

    # Define a grid of 10 X 10 for the big plot. 
    columns = 10
    rows = 10

    # The for loop
    for i in range(1, 51):

        # create the 2-dimensional picture
        image = X[randomSelect[i]].reshape(w,h)
        ax = fig.add_subplot(rows, columns, i)

        # create a title for each pictures, containing #index and label
        #title = "#"+str(randomSelect[i])+"; "+"y="+str(y[randomSelect[i]])
        title = "#"+str(randomSelect[i])+"; "+"y:"+str(y[randomSelect[i]])+"; "+"p:"+str(pred[randomSelect[i]])

        # set the title font size
        ax.set_title(title, fontsize=np.int(np.sqrt(X.shape[1])/2))        

        # don't display the axis
        ax.set_axis_off()

        # plot the image in grayscale
        plt.imshow(image, cmap='gray')

    plt.show()
```

并按如下方式运行函数：

```py
plotSamplesRandomlyWithPrediction(X, y, pred)
```

*plotSamplesRandomlyWithPrediction* 的一个可能输出如下：

![](img/3945417eabebfcd3c4deee498f09ea34.png)

图 12\. 可视化原始阿尔茨海默数据集中随机挑选的预测结果。图像来源于 [`www.kaggle.com/datasets/uraninjo/augmented-alzheimer-mri-dataset`](https://www.kaggle.com/datasets/uraninjo/augmented-alzheimer-mri-dataset)

## 结论

神经网络是分类复杂图像的绝佳解决方案，尤其是当这些图像来自神经成像时。在其他专门讨论类似问题的文章中，我们看到对比度较差的图像可能在分类中遇到困难。Kaggle 提供的增强型阿尔茨海默病 MRI 数据集展示了一些优势，因为每张图像的对比度良好。然而，各病理类别特征的模式多样性所带来的复杂性很高，并且仅在偶尔的情况下才能一眼识别。

这篇文章旨在确定一种计算快速且相对高效的分类和模式预测技术，测试准确率约为 ~ 95%。此类工具为快速诊断提供了新的机会，并且在病理学中提供了一种巧妙的方法来提取关键特征。

这篇文章再次提供了详细解释构建和训练多层神经网络机制的机会。揭示了一些关于神经网络在逻辑门中的计算过程及其与逻辑回归的强相关性的直觉，展示了神经网络实际上是一个超级逻辑回归模型。

扩展文章中描述的代码可能会增加隐藏层的数量。由于编辑原因，我没有分享这些代码扩展，增加此特定应用中的隐藏层不会提高准确性百分比；相反，会使其变得更差。无论如何，有兴趣的人可以获得两层和三层隐藏层的代码。

## 参考文献

1.  来自 [Kaggle](https://www.kaggle.com/datasets/uraninjo/augmented-alzheimer-mri-dataset) 的增强型阿尔茨海默病 MRI 数据集采用 [GNU 较小公共许可证](https://www.gnu.org/licenses/lgpl-3.0.html)。它代表了 [Kaggle 阿尔茨海默病数据集（4 类图像）](https://www.kaggle.com/datasets/tourist55/alzheimers-dataset-4-class-of-images) 的一个分支，采用 [开放数据公共开放数据库许可证（ODbL）v1.0](https://opendatacommons.org/licenses/odbl/1-0/)

1.  卢卡·扎马塔罗，**一个或多个变量的线性回归**，《数据科学前沿》，2019

1.  [卢卡·扎马塔罗，**癌症恶性预测的逻辑回归**，《数据科学前沿》，2019](https://medium.com/towards-data-science/logistic-regression-for-malignancy-prediction-in-cancer-27b1a1960184)

1.  卢卡·扎马塔罗，**通过一对多检测黑色素瘤**，《数据科学前沿》，2020

1.  卢卡·扎马塔罗，**用于图像识别的 Python 中的一对多逻辑回归**，《数据科学前沿》，2022

1.  [安德鲁·恩吉，**机器学习** | Coursera](https://www.coursera.org/learn/machine-learning)

1.  克里斯·阿尔邦，**Python 机器学习烹饪书**，O'Reilly，ISBN-13：978–1491989388。

1.  Aurélien Géron, **《动手学机器学习：使用 Scikit-Learn、Keras 和 TensorFlow 构建智能系统的概念、工具和技术》** 第 2 版，O’Reilly，ISBN-10: 1492032646

1.  [Wen, J. 等人：用于阿尔茨海默病分类的卷积神经网络：概述与可重复评估](https://arxiv.org/pdf/1904.07773.pdf)
