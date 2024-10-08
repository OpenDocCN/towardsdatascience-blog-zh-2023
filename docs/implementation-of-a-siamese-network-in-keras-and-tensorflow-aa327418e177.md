# 在 Keras 和 TensorFlow 中实现 Siamese 网络

> 原文：[`towardsdatascience.com/implementation-of-a-siamese-network-in-keras-and-tensorflow-aa327418e177`](https://towardsdatascience.com/implementation-of-a-siamese-network-in-keras-and-tensorflow-aa327418e177)

![](img/b1a1da818760b158826b71ceb1e2665f.png)

照片由 [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 学习对象检测背后的技术（以及更多）和示例代码。

[](https://rashida00.medium.com/?source=post_page-----aa327418e177--------------------------------)![Rashida Nasrin Sucky](https://rashida00.medium.com/?source=post_page-----aa327418e177--------------------------------)[](https://towardsdatascience.com/?source=post_page-----aa327418e177--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----aa327418e177--------------------------------) [Rashida Nasrin Sucky](https://rashida00.medium.com/?source=post_page-----aa327418e177--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----aa327418e177--------------------------------) ·阅读时间 7 分钟·2023 年 8 月 14 日

--

神经网络在 AI/ML 领域非常强大且流行，但它们训练所需的数据量过多。对于对象检测、签名验证、语音验证和药品识别等任务，常规神经网络技术由于数据需求过大，会变得更加耗时和昂贵。在这些类型的工作中，**Siamese 网络**可以非常强大，因为它比常规神经网络所需的数据少得多。此外，不平衡的数据集也可以表现良好。

本教程将为你提供 Siamese 网络的高级概述，并提供一个完整的示例。我在这里使用了 fashion-mnist 数据集，但这种相似的结构适用于许多其他用例。

## 什么是 Siamese 网络？

Siamese 网络包含一个或多个相同的网络，这些相同的网络具有相同的参数和权重。如果一个网络的权重更新，另一个网络的权重也会更新。它们必须是相同的。最终层通常是一个嵌入层，计算输出之间的距离。

你给它们一对输入。每个网络将计算输入的特征，并使用两张图像之间的距离来找出输入之间的相似性。因此，只有两种类别。要么图像相似，要么不相似。

> 当你实际操作一个例子时，概念会变得更加清晰。通过实践学习始终是最好的方法。

## 必要的导入和函数定义

让我们从必要的导入开始。如果有必要，我们将导入更多内容。

```py
import os
import tensorflow.keras.backend as K
import matplotlib.pyplot as plt
import numpy as np
import tensorflow as tf
from tensorflow.keras.models import Model
from tensorflow.keras.layers import Input
from tensorflow.keras.layers import Conv2D
from tensorflow.keras.layers import Dense
from tensorflow.keras.layers import Dropout
from tensorflow.keras.layers import GlobalAveragePooling2D
from tensorflow.keras.layers import MaxPooling2D
```

正如我们在前一节中讨论的，Siamese 网络一次接受**一对输入**，输出为‘yes’或‘no’。如果图像相似，则为‘yes’，否则为‘no’。或者，Siamese 网络还可以输出两张图像之间的距离，这在本教程中我们将进行。因此，我们需要以这种方式准备数据集。我们的数据集需要是图像对，而不是单张图像。对于正类，将会有两张相同类型的图像；而对于负类，将会有两张不同类型的图像。

下一个代码块定义了一个函数‘create_pairs’，该函数将生成图像对，这意味着将两张图像叠加在一起，其中有时两张图像是相同类型的，有时则是不同类型的。当两张图像匹配或是相同类型时，标签将为 1；而当图像不匹配时，标签将为 0。

```py
 def create_pairs(images, labels):
  imagePairs = []
  labelPairs = []

  #Getting the indices of each class
  numclasses = len(np.unique(labels))
  idx = [np.where(labels ==i)[0] for i in range(numclasses)]

  for ind in range(len(images)):
    #Getting current image with index
    currImage = images[ind]
    #getting the label of the image from labels.
    label = labels[ind]

    #Randomly choosing another labels from the same class
    indB = np.random.choice(idx[label])
    #corresponding image for this randomly selected label
    indImage = images[indB]

    imagePairs.append([currImage, indImage])

    labelPairs.append([1])

    #Getting a label where label is different than the current image
    diss_idx = np.where(labels != label)[0]

    #finding an image for this label
    diss_image = images[np.random.choice(diss_idx)]

    imagePairs.append([currImage, diss_image])
    labelPairs.append([0])

  return (np.array(imagePairs), np.array(labelPairs))
```

下一个函数计算两张图像之间的欧几里得距离，并遵循传统的欧几里得距离公式：

```py
def euclidean_distance(vecs):
  (imgA, imgB) = vecs
  ss = K.sum(K.square(imgA - imgB), axis = 1, keepdims=True)
  return K.sqrt(K.maximum(ss, K.epsilon()))
```

> 模型与成本

Siamese 模型与其他 TensorFlow 模型非常相似。我们将使用两组**卷积-最大池化-丢弃层、全局平均池化**，最后一层为 Dense。最后，它将返回模型。

```py
 def siamese_model(input_shape, embeddingDim = 48):
  inputs = Input(input_shape)
  x = Conv2D(128, (2, 2), padding = "same", activation = "relu")(inputs)
  x = MaxPooling2D(pool_size=(2, 2))(x)
  x = Dropout(0.4)(x)

  x = Conv2D(128, (2, 2), padding = "same", activation = "relu")(inputs)
  x = MaxPooling2D(pool_size=(2, 2))(x)
  x = Dropout(0.4)(x)

  pooling = GlobalAveragePooling2D()(x)
  outputs = Dense(embeddingDim)(pooling)
  model = Model(inputs, outputs)

  return model
```

正常的二元交叉熵损失函数对于我们做二元分类已经足够好。但对于 Siamese 网络，**对比损失**更为合适。如果你考虑一下，实际上 Siamese 网络的目标不仅仅是对相似或不相似的图像进行分类，还要区分它们。我们希望知道 Siamese 网络在区分相似或不相似图像方面做得有多好。

这是对比损失的公式：

![](img/fd701ad2009ed317c53ee8a018be9832.png)

图片来源：作者

这里，Y 是真实标签（0 或 1）

D 是欧几里得距离

边际通常为 1

让我们创建一个函数 contrastiveLoss：

```py
def contrastiveLoss(y, y_preds, margin=1):
 y = tf.cast(y, y_preds.dtype)
 y_preds_squared = K.square(y_preds)
 margin_squared = K.square(K.maximum(margin - y_preds, 0))
 loss = K.mean(y * y_preds_squared + (1 - y) * margin_squared)
 return loss
```

这些是可以用于任何其他同类型 Siamese 网络的常见函数。

## 模型训练

我们确实需要数据集来训练模型。在本教程中，我将使用[公共数据集](https://github.com/zalandoresearch/fashion-mnist/blob/master/LICENSE)（MIT 许可证），fashion_mnist 数据集，该数据集可以通过 TensorFlow 库加载。

```py
from tensorflow.keras.layers import Lambda
from tensorflow.keras.datasets import fashion_mnist
(x_train, y_train), (x_test, y_test) = fashion_mnist.load_data()
```

一些简单的数据处理任务是必要的，首先要做的是。为了缩放图像数据，我们将图像数据除以 255\。另外，还需要为训练和测试图像添加另一个维度，以使其成为三维的。

```py
x_train = x_train/255.0
x_test = x_test/255.0

x_train = np.expand_dims(x_train, axis = -1)
x_test = np.expand_dims(x_test, axis=-1)

(training_pairs, training_labels) = create_pairs(x_train, y_train)
(test_pairs, test_labels) = create_pairs(x_test, y_test)
```

接下来，我们将为图像对中的两张图像创建两个输入，并将它们传递到之前构建的 Siamese 模型中，以提取这两张图像的特征。

**欧几里得距离**函数在这里将非常有用，用于找到两个提取特征之间的距离。两个特征图像之间的距离越小，它们就越相似。

最终，模型将两张图像作为输入，并输出距离。

```py
image_shape = (28, 28, 1)
# specify the batch size and number of epochs
batch_size = 64
epochs = 70

imageA = Input(shape = image_shape)
imageB = Input(shape = image_shape)

model_build = siamese_model(image_shape)
modelA = model_build(imageA)
modelB = model_build(imageB)

distance = Lambda(euclidean_distance)([modelA, modelB])
model = Model(inputs=[imageA, imageB], outputs=distance)
```

现在编译模型以训练我们之前定义的对比损失的孪生模型。必要的参数包括图像对、标签对、批处理大小和训练轮数。

```py
model.compile(loss = contrastiveLoss, optimizer="adam")
history = model.fit(
    [training_pairs[:, 0], training_pairs[:, 1]], training_labels[:],
    validation_data=([test_pairs[:, 0], test_pairs[:, 1]], test_labels[:]),
    batch_size = batch_size,
    epochs = epochs
```

输出：

```py
Epoch 1/70
1875/1875 [==============================] - 24s 7ms/step - loss: 0.1808 - val_loss: 0.1618
Epoch 2/70
1875/1875 [==============================] - 15s 8ms/step - loss: 0.1615 - val_loss: 0.1572
Epoch 3/70
1875/1875 [==============================] - 14s 7ms/step - loss: 0.1588 - val_loss: 0.1551
Epoch 4/70
1875/1875 [==============================] - 14s 8ms/step - loss: 0.1566 - val_loss: 0.1529
Epoch 5/70
1875/1875 [==============================] - 14s 7ms/step - loss: 0.1552 - val_loss: 0.1520
.
.
.
.
Epoch 67/70
1875/1875 [==============================] - 14s 7ms/step - loss: 0.1486 - val_loss: 0.1447
Epoch 68/70
1875/1875 [==============================] - 14s 7ms/step - loss: 0.1487 - val_loss: 0.1443
Epoch 69/70
1875/1875 [==============================] - 14s 7ms/step - loss: 0.1484 - val_loss: 0.1447
Epoch 70/70
1875/1875 [==============================] - 14s 7ms/step - loss: 0.1490 - val_loss: 0.1446
```

让我们绘制一些图像对及其距离。我们将随机选择 4 对图像。可以使用**OpenCV 库**来实现。首先，它需要一些基本的图像处理，如缩放，然后将额外的维度添加到图像的两个维度中。接下来，我们将使用模型预测每对图像之间的距离。最后，你可以绘制这些图像对以查看距离和图像对。

```py
import cv2
pairs = np.random.choice(len(test_pairs), size=4)

for i in pairs:
  imageA = test_pairs[i][0]
  imageB = test_pairs[i][1]

  baseA = imageA.copy()
  baseB = imageB.copy()

  imageA = np.expand_dims(imageA, axis=-1)
  imageB = np.expand_dims(imageB, axis=-1)

  imageA = np.expand_dims(imageA, axis=0)
  imageB = np.expand_dims(imageB, axis =0)

  imageA = imageA/255.0
  imageB = imageB / 255.0

  predicts = model.predict([imageA, imageB])

  proba = predicts[0][0]

  fig = plt.figure("Pair #{}".format(i+1), figsize=(4,2))
  plt.suptitle("Distance: {}:.2f".format(proba))

  ax = fig.add_subplot(1, 2, 1)
  plt.imshow(baseA, cmap=plt.cm.gray)
  plt.axis("off")

  ax = fig.add_subplot(1, 2, 2)
  plt.imshow(baseB, cmap=plt.cm.gray)
  plt.axis("off")

  plt.show()
```

![](img/985059e1940a618b170954ce7eaf4934.png)

看看这些图片及其相应的距离。正如你所见，预测函数在这种情况下并未给出标签 0 或 1。它给出的是图像对之间的距离。当图像对中的图像更相似时，距离会更小。

> 如果需要，你可以根据使用案例设置一个阈值距离，以区分相似和不相似的图像，从而获得标签。

## 结论

正如我在介绍中提到的，这种模型和技术可以用于许多不同类型的任务。因为它可以处理较少的数据，所以数据收集部分变得更容易。希望它对你有用。

欢迎在 [Twitter](https://twitter.com/rashida048) 上关注我，并点赞我的 [Facebook](https://www.facebook.com/rashida.smith.161) 页面。

## 进一步阅读

[在 TensorFlow 中使用 Sequential 和 Function API 进行回归 | 作者：Rashida Nasrin Sucky | 数据科学导向 (medium.com)](https://medium.com/p/314e74b537ca)

[使用 Keras Tuner 对 TensorFlow 模型进行超参数调整 | 作者：Rashida Nasrin Sucky | 2023 年 7 月 | 数据科学导向 (medium.com)](https://medium.com/p/41978f53111)

[如何使用 OpenCV 进行阈值图像分割 | 作者：Rashida Nasrin Sucky | 数据科学导向 (medium.com)](https://medium.com/p/b2a78abb07ac)

[Python 初学者的一些基本图像预处理操作 | 作者：Rashida Nasrin Sucky | 数据科学导向 (medium.com)](https://medium.com/p/7d297316853b)

[如何在 TensorFlow 中定义自定义层、激活函数和损失函数 | 作者：Rashida Nasrin Sucky | 数据科学导向 (medium.com)](https://medium.com/p/bdd7e78eb67)

[逐步教程：在 TensorFlow 中开发多输出模型 | 作者：Rashida Nasrin Sucky | 数据科学导向 (medium.com)](https://medium.com/p/ec9f13e5979c)
