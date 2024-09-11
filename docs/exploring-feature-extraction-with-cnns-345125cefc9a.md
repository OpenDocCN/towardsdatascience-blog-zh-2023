# 探索 CNNs 中的特征提取

> 原文：[`towardsdatascience.com/exploring-feature-extraction-with-cnns-345125cefc9a?source=collection_archive---------0-----------------------#2023-11-25`](https://towardsdatascience.com/exploring-feature-extraction-with-cnns-345125cefc9a?source=collection_archive---------0-----------------------#2023-11-25)

## 使用卷积神经网络检查特征提取中的专业化

[](https://medium.com/@rodrigopesilva?source=post_page-----345125cefc9a--------------------------------)![Rodrigo Silva](https://medium.com/@rodrigopesilva?source=post_page-----345125cefc9a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----345125cefc9a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----345125cefc9a--------------------------------) [Rodrigo Silva](https://medium.com/@rodrigopesilva?source=post_page-----345125cefc9a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F222e82adf972&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-feature-extraction-with-cnns-345125cefc9a&user=Rodrigo+Silva&userId=222e82adf972&source=post_page-222e82adf972----345125cefc9a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----345125cefc9a--------------------------------) ·8 min read·2023 年 11 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F345125cefc9a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-feature-extraction-with-cnns-345125cefc9a&user=Rodrigo+Silva&userId=222e82adf972&source=-----345125cefc9a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F345125cefc9a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-feature-extraction-with-cnns-345125cefc9a&source=-----345125cefc9a---------------------bookmark_footer-----------)![](img/a922079bb33abb945ff04ccc0fd86159.png)![](img/b37cab9a98460c70520e728500584f40.png)

(**左**) 使用 vgg19 CNN 架构对狮子图像进行的特征提取（图像由作者提供）。 (**右**) 狮子的原始照片（公共领域，来源于 [Pexels](https://www.pexels.com/pt-br/foto/africa-africano-animais-selvagens-animal-41315/)）。

卷积神经网络（Convolutional Neural Networks，CNN）是当今利用机器学习进行图像分类任务的基石。然而，在分类之前，它们执行的另一个非常有用的任务是从图像中提取相关特征。特征提取是 CNN 识别图像关键模式的方式，以便对其进行分类。本文将展示如何使用 TensorFlow 和 Keras 函数 API 执行特征提取的示例。但首先，为了正式化这些 CNN 概念，我们需要先讨论像素空间。

# 背景

## 像素空间

像素空间正是其名字所暗示的：它是将图像转换为数值矩阵的空间，其中每个值对应一个单独的像素。因此，我们看到的原始图像，在输入 CNN 时，被转换为一个数字矩阵。在灰度图像中，这些数字通常范围从 0（黑色）到 255（白色），中间值是灰度。在本文中，所有图像都已经过归一化处理，即每个像素都被 255 除以，使其值位于[0, 1]区间内。

## CNN 和像素空间

CNN 在像素表示中对图像所做的事情是应用滤波器并处理它，以提取相关像素，从而做出最终的“决策”，即将该图像归入某个类别。例如，在页面顶部的图像中，该 CNN 特别关注狮子的嘴、舌头、眼睛（以及总体上的强轮廓），随着我们深入神经网络，这些特征进一步被提取。因此，可以说，CNN 在分类方面越专业，它在识别图像关键特征方面就越好。

## 目标

说到这一点，目标很简单：看看 CNN 在进行特征提取时的专业程度。

## 方法

为此，我使用了相同架构但训练大小不同的两个 CNN：一个使用了 50K 张图像（这是*基准*，智能的那一个），另一个使用了 10K 张图像（这是*虚拟的*）。之后，我切割了 CNN 的层，以检查算法看到的图像及其理解。

## 数据集

本项目使用的数据集是广泛使用的[cifar10](https://keras.io/api/datasets/cifar10/)图像数据集[1]，这是一个公共领域数据集，包含 60K 张图像，分为 10 个类别，其中 10K 张图像用作保留验证集。这些图像大小为 32x32 像素，是 RGB 彩色图像，即有 3 个颜色通道。

为了防止数据泄漏，我保留了一张图像作为特征识别的测试图像，因此该图像未在任何训练中使用。现在，我向大家介绍我们的小白鼠：青蛙。

![](img/7848cfb5764c67c4872f6e6b80a5455d.png)

青蛙。

# 在 TensorFlow 中的实现

实现代码见下方代码片段。为了正确切片 CNN 的层，必须使用 TensorFlow 中的 Keras 功能 API 而不是顺序 API。它像级联一样工作，下一层调用在上一层之上。

```py
import tensorflow as tf
from tensorflow.keras.models import Model
from tensorflow.keras.layers import Input, Conv2D, MaxPool2D, Dense, Dropout, Flatten
from tensorflow.keras.callbacks import ModelCheckpoint, EarlyStopping

def get_new_model(input_shape):
  '''
  This function returns a compiled CNN with specifications given above.
  '''

  #Defining the architecture of the CNN
  input_layer = Input(shape=input_shape, name='input')
  h = Conv2D(filters=16, kernel_size=(3,3), 
             activation='relu', padding='same', name='conv2d_1')(input_layer)
  h = Conv2D(filters=16, kernel_size=(3,3), 
             activation='relu', padding='same', name='conv2d_2')(h)

  h = MaxPool2D(pool_size=(2,2), name='pool_1')(h)

  h = Conv2D(filters=16, kernel_size=(3,3), 
             activation='relu', padding='same', name='conv2d_3')(h)
  h = Conv2D(filters=16, kernel_size=(3,3), 
             activation='relu', padding='same', name='conv2d_4')(h)

  h = MaxPool2D(pool_size=(2,2), name='pool_2')(h)

  h = Conv2D(filters=16, kernel_size=(3,3), 
             activation='relu', padding='same', name='conv2d_5')(h)
  h = Conv2D(filters=16, kernel_size=(3,3), 
             activation='relu', padding='same', name='conv2d_6')(h)

  h = Dense(64, activation='relu', name='dense_1')(h)
  h = Dropout(0.5, name='dropout_1')(h)
  h = Flatten(name='flatten_1')(h)
  output_layer = Dense(10, activation='softmax', name='dense_2')(h)

  #To generate the model, we pass the input layer and the output layer
  model = Model(inputs=input_layer, outputs=output_layer, name='model_CNN')

  #Next we apply the compile method
  model.compile(optimizer='adam',
                loss='categorical_crossentropy',
                metrics=['accuracy'])

  return model
```

架构规格如下图 1 所示。

![](img/d4c391b63dc5594ae120d213fc2c0e44.png)

图 1：使用的 CNN 概述。图像由作者提供。

使用的优化器是 Adam，损失函数是类别交叉熵，用于评估的指标仅仅是准确度，因为数据集完全平衡。

# 切片 CNN

现在我们可以切片两个 CNN 的一些关键层，以检查图像的处理水平。代码实现如下：

```py
benchmark_layers = model_benchmark.layers
benchmark_input = model_benchmark.input

layer_outputs_benchmark = [layer.output for layer in benchmark_layers]
features_benchmark = Model(inputs=benchmark_input, outputs=layer_outputs_benchmark)
```

这里发生的情况如下：第一行访问模型的每一层，第二行返回整个 CNN 的输入层。然后在第三行，我们列出每一层的输出，最后我们创建一个新模型，其输出是这些层的输出。这样我们可以查看层之间发生了什么。

编写了非常相似的代码来访问我们虚拟模型的层，因此在此省略。现在让我们继续查看在不同层处理后的青蛙图像。

## 第一层卷积层

***虚拟***

图 2 显示了卷积层（conv2d_1）的 16 个滤波器的图像。我们可以看到这些图像并未被过度处理，而且有很多冗余。有人可能会认为这只是第一层卷积层，这也是处理不那么复杂的原因，这确实是一个合理的观察。为了解决这个问题，我们将查看基准测试的第一层。

![](img/2f0315aa8278c50a79032051a500ceb6.png)

图 2：虚拟分类器的第一层卷积层。图像由作者提供。

***基准测试***

基准测试分类器显示了一个经过处理的图像，处理得更加彻底，以至于这些图像大部分已经无法识别。请记住：这只是第一层卷积层。

![](img/dbf27ae108e871c0d24006df6b3ac155.png)

图 3：基准测试分类器的第一层卷积层。图像由作者提供。

## 最后一层卷积层

***虚拟***

正如预期的那样，图像现在已经无法识别，因为我们已经经过了 6 层卷积层和 2 层池化层，这解释了图像尺寸的减小。让我们看看基准测试的最后一层是什么样的。

![](img/64db99c5f1fb96533bab7541f60a7917.png)

图 4：虚拟分类器的最后一层卷积层。图像由作者提供。

***基准测试***

这已经处理得更为彻底，大部分像素变成了黑色，这表明重要的特征已被选出，其余的图像基本上被丢弃了。

![](img/69c94f6b4d741d94c41d91738863d0c6.png)

图 5：基准测试分类器的最后一层卷积层。图像由作者提供。

# 这与信息有何关联？

我们可以看到，对于网络的相同切片，处理程度差异很大。定性分析表明，基准模型在从输入中提取有用信息时更加激进。这在第一次卷积层比较中尤为明显：青蛙图像的输出在虚拟模型上扭曲较少，更加可识别，而在基准模型上则较为模糊。

这表明基准在预测类别时，能够更有效地丢弃图像中无用的元素，而虚拟分类器则对如何进行处理不确定，因此考虑了更多的特征。从图 6 中我们可以看到，基准（蓝色）丢弃的颜色像素比虚拟模型（红色）更多，后者在其颜色分布中显示出更长的尾部。

![](img/8edcd5cc82e055a8f94bdc9f5b6ec445.png)

图 6：最后一层的像素概率分布。我们可以看到基准的像素（蓝色）更集中在零附近，而虚拟模型的像素（红色）显示出更长的尾部。

如果我们查看原始青蛙图像的像素分布，图 7 显示了一个更对称的分布，大致集中在 0.4 左右。

![](img/7d09e89780f1d6bda77dec70a9a27b49.png)

图 7：我们原始青蛙图像的颜色分布。图像由作者提供。

从信息理论的角度来看，原始图像和卷积层后得到的图像之间概率分布的差异代表了巨大的信息增益。

从图 6 和图 7 比较来看，我们对前者中将要找到哪些像素比对后者中的像素更为确定。因此，信息获得有所增加。这是对信息理论的非常简要和定性探讨，并为广阔的领域开启了一扇门。有关信息（带有双关语）的更多信息，请参见[这篇文章](https://medium.com/towards-data-science/neural-network-via-information-68af7f49b978)。

最后，查看分类器答案的不确定性的一种方法是观察类别的概率分布。这是我们 CNN 末尾的 softmax 函数的输出。图 8（左）显示基准对类别的确定性更高，分布在青蛙类别上峰值；而图 8（右）显示了一个困惑的虚拟分类器，最高概率出现在错误的类别上。

![](img/5393621906d0a791da424f130adb204a.png)![](img/4e5c10e6837620ee0aa0e9c463fcd0f6.png)

图 8：每个分类器分配的概率分布。图像由作者提供。

这一分析展示了预期的结果：更大的训练集导致更好的特征捕捉算法。

本项目的笔记本可在[这里](https://github.com/rodrigops123/ML_projects/blob/main/Feature_extraction.ipynb)找到。

# 参考文献

[1] Krizhevsky, Alex 和 Geoffrey Hinton。[从小图像中学习多个特征层](https://www.cs.toronto.edu/~kriz/learning-features-2009-TR.pdf)。（2009 年）：7。

[2] M. Jogin 等人，[使用卷积神经网络（CNN）和深度学习的特征提取](https://ieeexplore.ieee.org/document/9012507)（2018 年），IEEE 国际电子信息与通信技术近期趋势会议（RTEICT）

[3] K. Simonyan 和 A. Zisserman，[用于大规模图像识别的深度卷积网络](https://arxiv.org/abs/1409.1556)（2015 年），作为会议论文发表在 ICLR 2015

[4] Z. Matthew 和 F. Rob。[可视化与理解卷积网络](https://arxiv.org/abs/1311.2901)（2013 年），欧洲计算机视觉大会

[5] B. Jyostna 和 N. Veeranjaneyulu，[使用深度卷积神经网络的特征提取与分类](https://journals.riverpublishers.com/index.php/JCSANDM/article/view/5341)（2018 年），《网络安全与移动性期刊》
