# 使用 fastai 实现深度学习——图像分类

> 原文：[`towardsdatascience.com/implementing-deep-learning-using-fastai-eff2fa05449e`](https://towardsdatascience.com/implementing-deep-learning-using-fastai-eff2fa05449e)

## 快速而轻松地入门深度学习，无需涉猎所有细节

[](https://weimenglee.medium.com/?source=post_page-----eff2fa05449e--------------------------------)![Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----eff2fa05449e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eff2fa05449e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----eff2fa05449e--------------------------------) [Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----eff2fa05449e--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eff2fa05449e--------------------------------) ·阅读时间 9 分钟·2023 年 4 月 19 日

--

![](img/86fd1ac707e69199d29b754c8a87c85b.png)

图片来源：[NASA](https://unsplash.com/@nasa?utm_source=medium&utm_medium=referral) 通过 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

近年来，人工智能 (AI) 受到了广泛关注，尤其是最近几个月 ChatGPT 的发布。人工智能中的基础技术之一是*深度学习*。深度学习是一种机器学习技术，使用神经网络来学习数据集中特征和标签之间的关系。一个神经网络通常如下所示：

![](img/73331b93543e8d1466f6b645d021cd5f.png)

所有图片由作者提供

上述各个圆圈称为*神经元*（或*节点*）。每个神经元有一个值叫做**偏置**，每个神经元彼此连接。每个连接都有一个值叫做**权重**。最左侧的神经元层是**输入层**（你将数据发送到此处进行预测），而最右侧的层称为**输出层**（预测结果在此处显示）。神经网络可以有任意多（或少）的隐藏层。

学习深度学习需要了解一些内容：

+   什么是层、权重和偏置

+   激活函数

+   损失函数

+   优化器

+   反向传播

此外，还有几种类型的神经网络，例如：

+   人工神经网络 (ANN)

+   卷积神经网络 (CNN)

+   循环神经网络 (RNN)

深度学习初学者通常需要掌握这些概念，然后才能开始构建有效的模型。

这就是 **fastai** 的作用所在。**fastai** 是一个深度学习库，允许初学者和从业者快速入门标准的深度学习模型，同时提供定制所构建模型的能力。

在这篇文章中，我将帮助你开始使用 **fastai** 构建你的第一个深度学习模型。

> **fastai** 由 Jeremy Howard 和 Rachel Thomas 于 2016 年创立，旨在普及深度学习。他们通过提供一个名为“编程人员实用深度学习”（Practical Deep Learning for Coders）的开放在线课程（MOOC）来实现这一目标，该课程唯一的先决条件是掌握 Python 编程语言。来源：[`en.wikipedia.org/wiki/Fast.ai`](https://en.wikipedia.org/wiki/Fast.ai)

# **fastai** 到底是什么？

对于那些熟悉 **TensorFlow** 的人来说，**fastai** 对 **PyTorch** 的作用就像 **Keras** 对 **TensorFlow** 的作用一样——简而言之，它是 PyTorch 的一个封装：

![](img/1f12e8b795798e441af119ac91bc9aba.png)

那么 **PyTorch** 是什么呢？PyTorch 是一个基于 **Torch** 库的机器学习框架，用于计算机视觉和自然语言处理等应用，最初由 Meta AI 开发，现在是 Linux 基金会的一部分。它是一个自由开源的软件，发布在修改版 BSD 许可证下（来源：[`en.wikipedia.org/wiki/PyTorch`](https://en.wikipedia.org/wiki/PyTorch)）。

**TensorFlow** 另一方面是另一个开源机器学习和人工智能库（由 Google 开发）。它的主要优势在于深度神经网络的训练和推断。由于 TensorFlow 的使用较为复杂，**Keras** 提供了一个高层次的库，能够在 TensorFlow 之上工作。Keras 使得深度学习对开发者来说更加容易上手。

这意味着，你现在可以构建深度学习神经网络，而不必真正理解神经网络是如何构建的细节。

**fastai** 的主要目标之一是使深度学习变得 *易于接近* 和 *迅速高效*。**fastai** 提供了四个主要应用领域的 API：

+   视觉

+   文本

+   表格和时间序列分析

+   协同过滤

在本文中，作为对 **fastai** 的快速入门，我将通过使用 **fastai** 构建一个视觉模型来识别图像。

# 安装 fastai

在本文中，我将使用 Jupyter Notebook。要安装 **fastai**，请在新单元格中输入以下命令：

```py
!pip install fastai
```

# 训练图像识别的视觉模型

使用 **fastai**，你可以非常快速地训练深度学习模型，而无需过多接触底层实现。

> 虽然我说你可以在不直接接触 fastai 的情况下构建深度学习模型，但如果你对深度学习有一些背景知识——什么是神经网络、权重和偏差是什么、不同类型的神经网络（特别是本文中使用的卷积神经网络），以及如何训练和测试神经网络——那会非常有用。如果你感兴趣，请参考我在 Code Magazine 上关于深度学习工作原理的文章。

[## Introduction to Deep Learning](https://www.codemag.com/Article/2003071/Introduction-to-Deep-Learning?source=post_page-----eff2fa05449e--------------------------------)

### 作者：Wei-Meng Lee 发表在：CODE Magazine：2020 年 3 月/4 月 最后更新：2022 年 8 月 31 日 人工智能…

[www.codemag.com](https://www.codemag.com/Article/2003071/Introduction-to-Deep-Learning?source=post_page-----eff2fa05449e--------------------------------)

## 使用示例图片

**fastai**附带了一组你可以下载并用于训练的示例图片。首先，从`fastai`中导入`vision`库：

```py
from fastai.vision.all import *
```

然后，打印出`URLs.PETS`的值：

```py
URLs.PETS
```

它将返回以下 URL：

```py
'https://s3.amazonaws.com/fast-ai-imageclas/oxford-iiit-pet.tgz'
```

这个 URL 指向一个包含一系列猫和狗图片的 tar 文件。

除了`PETS`项目，`URLs`对象还包含其他示例图片，如下所示：

![](img/9038b1ea3043548ba7da134700eb55a6.png)

要下载示例图片并解压它，请使用`untar_data()`函数，如下所示：

```py
path = untar_data(URLs.PETS)/'images'    # path to images
path
```

图片现在将被扩展到`/Users/weimenglee/.fastai/data/oxford-iiit-pet/images`目录。

在**images**文件夹中，你可以看到包含各种猫和狗品种的图片列表：

![](img/d58f1b989ce837230ff6c92a04404934.png)

如你所见，文件名以动物名称开头，后跟一个索引。

## 使用 ImageDataLoaders 加载图片

**images**文件夹包含 37 种猫和狗品种的图片，因此你需要一种方法为每张图片分配标签，并将它们放入适当的类别中。对于标签提取，我们将提取文件名中最后一个下划线字符（“`_`”）之前的部分，并将其用作动物的标签：

![](img/bf8b0a5ff161673457d144d1073aca20.png)

我们如何对这些图片进行分类？幸运的是，你可以使用`ImageDataLoaders`类。`ImageDataLoaders`类可以自动从图片列表中构建验证和训练数据。使用`from_name_func()`函数，你可以从存储在文件夹中的图片系列中加载训练和验证数据。

首先，你需要定义一个函数来提取图片的标签：

```py
def animal_labels(filename):    
    return filename[:filename.rfind('_')]  # extract the filename until the last underscore
```

然后，使用`ImageDataLoaders`类中的`from_name_func()`函数，并传递以下参数：

```py
 dls = ImageDataLoaders.from_name_func(
    path,                                # path to images 
    get_image_files(path),               # get the list of path for all images
    valid_pct = 0.25,                    # percentages to use for validation
    seed = 42,                           # random seed
    label_func = animal_labels,          # labelling for the images
    item_tfms = Resize(224))             # transform the images to 224x224
```

> `dls`变量的类型是`fastai.data.core.DataLoaders`。

结果现在可以用于训练。

## 打印标签

如果你对训练数据集中各种猫狗品种感到好奇，可以使用`vocab`属性在`ImageDataLoaders`对象中将它们打印出来：

```py
for vocab in dls.vocab:
    print(vocab)
```

这里列出了 37 种猫狗品种：

```py
Abyssinian
Bengal
Birman
Bombay
British_Shorthair
Egyptian_Mau
Maine_Coon
Persian
Ragdoll
Russian_Blue
Siamese
Sphynx
american_bulldog
american_pit_bull_terrier
basset_hound
beagle
boxer
chihuahua
english_cocker_spaniel
english_setter
german_shorthaired
great_pyrenees
havanese
japanese_chin
keeshond
leonberger
miniature_pinscher
newfoundland
pomeranian
pug
saint_bernard
samoyed
scottish_terrier
shiba_inu
staffordshire_bull_terrier
wheaten_terrier
yorkshire_terrier
```

## 模型训练

由于我们正在构建一个视觉模型，我们将使用`vision_learner()`函数：

```py
learn = vision_learner(dls, 
                       resnet34, 
                       metrics = error_rate)
learn.fine_tune(1)
```

`vision_learner()`函数接受三个参数：

+   数据加载器

+   使用的预训练模型

+   用于评估模型的度量标准

> **Resnet34**是什么？Resnet34 是一个图像分类模型。它是一个结构为 34 层的卷积神经网络（CNN）（因此得名）。Resnet34 在 ImageNet 数据集上进行了预训练，该数据集包含超过 100,000 张图像，涵盖 200 个类别。CNN 通常用于图像分类目的。

在这里，我们使用一个预训练模型——`resnet34`。预训练模型是已经用其自身数据集训练过的模型。还记得在上一节我们需要将图像调整为 224x224 吗？这是因为`resnet34`是在这种尺寸的图像上进行训练的。你可能会问为什么不使用更大的图像？其实，虽然更大的图像确实会更好，但这会带来速度和内存需求的增加。

在这个示例中，我们使用*迁移学习*。迁移学习是一种机器学习方法，其中一个为某个任务开发的模型被用作第二个任务模型的起点。迁移学习减少了你在训练上需要花费的时间。下图展示了迁移学习的工作原理。上部分展示了预训练模型的网络结构。在卷积神经网络（CNN）中，早期的层（从左开始）已经训练来识别基本形状、边缘、颜色等。而最右侧的几层则训练来识别模型已被训练识别的特定对象：

![](img/7d920a038110c6fbdc39372f99b45a2e.png)

在迁移学习中，你不需要重新训练整个模型——你可以保留那些能够识别基本形状的早期层，同时保留最右侧的几层以识别你想训练的特定对象：

![](img/2540483616daca39500d8e424b60c615.png)

要开始迁移学习过程，你可以使用`fine_tune()`函数：

```py
learn.fine_tune(1)
```

你指定的`1`是*epoch*，即模型对你的图像进行训练的次数。

> 你应该使用的 epoch 数量取决于你有多少时间用于训练。较高的 epoch 需要较多的时间，但结果通常比低 epoch 更准确。另一方面，指定较高的 epoch 意味着模型会重复查看你的图像，这可能导致过拟合。过拟合意味着你的模型现在记住了你的图像，当给它一个之前从未见过的新图像时，它的表现会很差。

训练之后，你应该得到一些关于训练的统计数据：

![](img/49569ba4f44487b950dedd4f89dcd41a.png)

## 进行预测

我们的模型现在已经训练完成，是时候进行测试了！为此，我下载了一张美国斗牛犬的图像（**1024px-Bulldog_inglese.jpg**）：

![](img/fe37e35f0554c916ed70416729a23d10.png)

来源：[`en.wikipedia.org/wiki/Bulldog#/media/File:Bulldog_inglese.jpg`](https://en.wikipedia.org/wiki/Bulldog#/media/File:Bulldog_inglese.jpg)

将图像保存到与 Jupyter Notebook 相同的目录中。现在，你可以加载该图像并在 Jupyter Notebook 中显示它：

```py
img = PILImage.create('1024px-Bulldog_inglese.jpg')
img.to_thumb(192)
```

然后，你可以使用 `predict()` 函数发送进行预测：

```py
animal_type, index, probs = learn.predict(img)
print(f"Predicted animal: {animal_type}.") 
print(f"Predicted animal probability: {probs[index]}")
print(f"Probabilities: {probs}")
```

`predict()` 函数返回三个值：

+   预测标签

+   一个 `torch.Tensor` 对象，表示预测标签的索引

+   每个标签的概率

以下是预测结果：

```py
Predicted animal: american_bulldog.
Predicted animal probability: 0.6161370277404785
Probabilities: tensor([7.9024e-05, 4.4181e-04, 6.9500e-05, 8.0544e-05, 1.8523e-04, 7.2056e-05,
        2.3234e-05, 7.4391e-04, 3.1683e-05, 7.3335e-05, 1.9286e-04, 2.3213e-04,
        6.1614e-01, 4.6759e-03, 2.0838e-02, 4.2569e-04, 7.0168e-02, 4.3025e-05,
        2.1922e-05, 4.6172e-05, 1.0497e-04, 2.4546e-04, 2.3985e-04, 6.9188e-04,
        1.0650e-04, 2.1837e-04, 1.5202e-04, 1.7642e-04, 8.6259e-05, 1.4461e-02,
        2.5644e-01, 2.9418e-04, 4.5159e-04, 1.2193e-04, 1.0669e-02, 4.7959e-04,
        4.7566e-04])
```

从结果中可以看出，模型正确预测了图像是美国斗牛犬。

**如果你喜欢阅读我的文章，并且它对你的职业/学习有帮助，请考虑注册成为 Medium 会员。每月 $5，你可以无限制访问 Medium 上所有文章（包括我的）。如果你通过以下链接注册，我将获得少量佣金（对你没有额外费用）。你的支持意味着我将能花更多时间写像这样的文章。**

[## 使用我的推荐链接加入 Medium - Wei-Meng Lee](https://weimenglee.medium.com/membership?source=post_page-----eff2fa05449e--------------------------------)

### 阅读 Wei-Meng Lee 的每一篇故事（以及 Medium 上成千上万其他作家的故事）。你的会员费直接支持…

[weimenglee.medium.com](https://weimenglee.medium.com/membership?source=post_page-----eff2fa05449e--------------------------------)

# 总结

我希望这个快速入门能对如何使用 **fastai** 提供一些帮助。本文中讨论了一些深度学习的术语，但一开始你不必详细了解它们。你应该专注于让模型训练好，然后使用它进行预测。未来的文章中，我会给你更多如何使用 **fastai** 构建一些非常有趣的模型的例子。祝你好运！
