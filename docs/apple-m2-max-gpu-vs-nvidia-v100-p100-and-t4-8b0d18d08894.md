# Apple M2 Max GPU 对比 Nvidia V100、P100 和 T4

> 原文：[`towardsdatascience.com/apple-m2-max-gpu-vs-nvidia-v100-p100-and-t4-8b0d18d08894`](https://towardsdatascience.com/apple-m2-max-gpu-vs-nvidia-v100-p100-and-t4-8b0d18d08894)

## 比较 Apple Silicon M2 Max GPU 的性能与 Nvidia V100、P100 和 T4，在 TensorFlow 中训练 MLP、CNN 和 LSTM 模型。

[](https://fabrice-daniel.medium.com/?source=post_page-----8b0d18d08894--------------------------------)![Fabrice Daniel](https://fabrice-daniel.medium.com/?source=post_page-----8b0d18d08894--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8b0d18d08894--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8b0d18d08894--------------------------------) [Fabrice Daniel](https://fabrice-daniel.medium.com/?source=post_page-----8b0d18d08894--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8b0d18d08894--------------------------------) ·阅读时长 6 分钟·2023 年 11 月 2 日

--

![](img/9c4460ec6c7c0b9a501cae279c02a10f.png)

图片由作者提供

2020 年 11 月发布的 [**Apple M1**](https://en.wikipedia.org/wiki/Apple_M1) 是在被 Intel 主导的计算机世界中的一场革命。这些新的 M1 Mac 在许多基准测试中表现出色，因为 M1 在能耗的极小部分下，比大多数高端台式计算机更快。

以下是我之前对 M1 的基准测试：

[**M1 与 Xeon®、Core i5、K80 和 T4 的基准测试**](https://medium.com/towards-data-science/benchmark-m1-vs-xeon-vs-core-i5-vs-k80-and-t4-e3802f27421c)

[**M1 在 TensorFlow 训练中与 20 核 Xeon® 竞争**](https://medium.com/towards-data-science/benchmark-m1-part-2-vs-20-cores-xeon-vs-amd-epyc-16-and-32-cores-8e394d56003d)

2023 年 1 月，苹果宣布了新的 [**M2 Pro 和 M2 Max**](https://en.wikipedia.org/wiki/Apple_M2)。它们的规格让我们期待性能的显著提升，特别是在 GPU 方面。

![](img/e66a1ec49c63c10d2ba6765b0f8cefc6.png)

将 M2 Max 与 M1 规格进行比较

*这个 M2 Max 具有 30 个 GPU 核心，因此我们根据 38 核 GPU 版本的 13.6 TFlops 估算了 10.7 TFLOPS。*

![](img/a1c7ee55fcac13bdff32d65895caf633.png)

GPU 比较

相比之下，M2 Max 38 核 GPU 达到 13.6 TFlops。我的测试将显示，仅凭 TFlops 无法估计这些 GPU 的实际性能。

为了获得可比较的结果，我在默认的 TensorFlow FP32 浮点精度下运行每个测试。

你可以通过运行来验证这一精确度：

```py
tf.keras.backend.floatx()
'float32'
```

# **设置**

在这篇文章中，我对[**M2 Max**](https://en.wikipedia.org/wiki/Apple_M2) GPU 与 Nvidia 的[**V100**](https://www.nvidia.com/en-us/data-center/v100/)、[**P100**](https://www.nvidia.com/en-us/data-center/tesla-p100/)、[**T4**](https://www.nvidia.com/en-us/data-center/tesla-t4/)在 MLP、CNN 和 LSTM[**TensorFlow**](https://www.tensorflow.org)模型上的表现进行了基准测试。

![](img/f3ecb7d91531c5b327914f809fbca04f.png)

基准测试设置

在 M1 和 M2 Max 计算机上，环境是在[**miniforge**](https://github.com/conda-forge/miniforge)下创建的。仅安装了以下软件包：

```py
conda install python=3.10
pip install tensorflow-macos==2.12
pip install tensorflow-metal==0.8.0
conda install pandas
```

与我以前的文章不同，TensorFlow 现在可以直接与 Apple Silicon 兼容，无论是从 pip、anaconda 还是 miniforge 安装。

要启用 GPU 使用，请安装由苹果公司分发的[**tensorflow-metal**](https://developer.apple.com/metal/tensorflow-plugin/)包，并使用[**TensorFlow PluggableDevices**](https://blog.tensorflow.org/2021/06/pluggabledevice-device-plugins-for-TensorFlow.html)。请注意，[**Metal**](https://developer.apple.com/metal/)加速也适用于[**PyTorch**](https://developer.apple.com/metal/pytorch/)和[**JAX**](https://developer.apple.com/metal/jax/)。

苹果表示

> 随着对 Metal 后端支持的更新，你可以利用新功能如自定义内核和混合精度训练，更快地训练更广泛的网络。

# 模型

这个基准测试包含一个运行[**MLP**](https://en.wikipedia.org/wiki/Multilayer_perceptron)、[**CNN**](https://en.wikipedia.org/wiki/Convolutional_neural_network)和[**LSTM**](https://en.wikipedia.org/wiki/Long_short-term_memory)模型的 Python 程序，这些模型在[**Fashion MNIST**](https://github.com/zalandoresearch/fashion-mnist)**¹**数据集上进行训练，批量大小为 32、128、512 和 1024 样本。

它还使用验证集以保持与实际应用中大多数模型训练方法的一致性。然后，使用测试集在训练后评估模型，以确保一切正常。所以，训练集、验证集和测试集的大小分别为 50000、10000 和 10000。

我使用与[上一篇文章](https://medium.com/towards-data-science/benchmark-m1-vs-xeon-vs-core-i5-vs-k80-and-t4-e3802f27421c)中描述的相同测试程序。提醒一下，这三个模型是简单的，下面总结了它们的特点。

**MLP**

```py
model = tf.keras.models.Sequential([
    tf.keras.layers.Flatten(input_shape=X_train.shape[1:]),
    tf.keras.layers.Dense(512,activation='relu'),
    tf.keras.layers.Dropout(rate=0.2),
    tf.keras.layers.Dense(64,activation='relu'),  
    tf.keras.layers.Dense(10,activation='softmax')
])
```

**CNN**

```py
model = tf.keras.models.Sequential([
    tf.keras.layers.Conv2D(32,(3,3),activation = 'relu',input_shape=X_train.shape[1:]),
    tf.keras.layers.MaxPooling2D((2,2)),
    tf.keras.layers.Conv2D(64,(3,3),activation = 'relu'),
    tf.keras.layers.MaxPooling2D((2,2)),
    tf.keras.layers.Conv2D(64,(3,3),activation = 'relu'),
    tf.keras.layers.Flatten(),
    tf.keras.layers.Dense(64,activation='relu'),
    tf.keras.layers.Dense(10,activation='softmax')
])
```

**LSTM**

```py
model = tf.keras.models.Sequential([
    tf.keras.layers.LSTM(128,input_shape=X_train.shape[1:]),
    tf.keras.layers.Dense(10,activation='softmax')
])
```

它们都使用以下优化器和损失函数。

```py
model.compile(optimizer='adam',
              loss='sparse_categorical_crossentropy',
              metrics=['accuracy'])
```

请注意，与以前的基准测试文章不同，现在的急切模式对 GPU 有效。

# 结果

让我们首先比较 M2 Max 与 M1。

![](img/633645acf4ea7899e96db3182146af59.png)

M2 Max 与 M1 的比较

正如预期的那样，M2 Max 始终比 M1 更快。以下显示了具体的倍数。

![](img/a72117e77c3d3ce486b11e1fcd9d6d3e.png)

M2 Max 在 GPU 训练中的速度是 M1 的多少倍

下面是速度的平均、最小和最大增幅。我们发现使用 M2 Max 进行 Convnet 训练的速度可以比使用 M1 GPU 快多达 4.38 倍。

![](img/cf56c6b28fb1b238f344010927a7a520.png)

M2 Max 在 GPU 训练中比 M1 快多少倍

现在让我们比较一下 M2 Max 与 Google Cloud、AWS、Google Colaboratory 或 Kaggle 平台上其他 Nvidia 高端 GPU 的表现。

![](img/de94672cc997005e5fe3ba7975a16a9f.png)

M2 Max 与 Nvidia T4、V100 和 P100 的比较

虽然在 batch size 为 32 和 128 时训练性能相似，但 M2 Max 在 batch size 为 512 和 1024 时显示出所有 GPU 中最好的性能。

P100 是其他 GPU 中速度最快的，而从规格来看 V100 是预期中的最优选择。这可能与实例本身有关，因为每张卡的三个不同版本的特性无法解释这一点。检查细节时，Kaggle P100 实例有 4 个 vCPU，而 Colab V100 只有 2 个 vCPU。也许这会影响整体性能，需要进一步检查。

在这里，我们比较 M2 Max 与 Nvidia P100 的速度差异。

![](img/efa2db05d34bf6c8f42c6a78008f7635.png)

M2 Max 在 GPU 训练中比 P100 快多少倍

下面是速度的平均、最小和最大增幅。我们发现，平均而言，M2 Max 与 P100 相同，并且虽然在 batch size 为 32 和 128 时较慢，但在 batch size 为 1024 时，MLP 的速度可以快达 1.77 倍，LSTM 快达 1.43 倍，CNN 快达 1.24 倍。

![](img/26a1ac1af363faec738f73f479eac797.png)

M2 Max 在 GPU 训练中比 P100 快多少倍

# M2 Max 上 GPU 与 CPU 的好处

过去，使用旧版本的 TensorFlow 时，常常观察到 MLP 和 LSTM 在 CPU 上的训练效率高于 GPU。只有 CNN 受益于 GPU。但现在，随着 TensorFlow 的最新版本，LSTM 的情况发生了变化。

所以最后一个有趣的测试是检查模型训练在 GPU 与 CPU 的比较中是否有好处。

![](img/97b57ea5b1725f773b9fd58c2c84afa7.png)

M2 Max GPU 的训练速度比 CPU 快多少倍

MLP 并未从 GPU 加速中受益。正如预期的那样，CNN 在每种情况下都受益，但从 batch size 为 128 开始，差异巨大，速度提高到约 5 倍。

但真正的惊喜来自 LSTM，其训练速度比 GPU 快多达 6.77 倍，尤其是在 batch size 为 1024 时。

# 结论

从这些测试来看，

+   M2 Max 比 M1 快得多，因此 Mac 用户可以从这一升级中受益。

+   与 T4、P100 和 V100 相比，M2 Max 在 batch size 为 512 和 1024 时始终更快。

+   性能差异不仅仅是 TFlops 的问题。M2 Max 理论上比 P100 快 15%，但在真实测试中，对于 batch size 为 1024 的情况，CNN 的性能提高了 24%，LSTM 提高了 43%，MLP 提高了 77%。

当然，这些指标只能用于与本测试中使用的神经网络类型和深度相似的情况。

需要对更大模型和数据集进行新的测试。

现在 TensorFlow 可以在 Apple Silicon 上轻松安装和高效运行。GPU 现在可以用于任何模型，大大提高了任何类型模型的训练性能，特别是 LSTM 模型。

我们可以得出结论，M2 Max 是一个非常好的机器学习工程师平台。它能够以非常好的性能在 GPU 上训练模型，甚至比常见的 T4、P100 或 V100 云实例更好。它还包含 12 个 CPU 核心，使其比这些 GPU 实例更灵活，且能耗仅为它们的一小部分。

> **更新 (2023 年 12 月 11 日)**：最新版本的 tensorflow-metal（> 0.8.0）存在一个错误，使得一些模型无法收敛。为避免此问题，我已通过设置 TensorFlow 和 tensorflow-metal 版本来更新了文章中的安装说明。随后，我使用这个新软件包组合重新进行了测试，以确保基准测试结果没有变化。我确认这不会改变性能结果。

## 参考资料

**图像和代码**：除非另有明确说明，否则本文中的所有图像和代码均由作者提供。

**数据集许可**：[Fashion-MNIST](https://github.com/zalandoresearch/fashion-mnist) 按照 [MIT 许可证](https://github.com/zalandoresearch/fashion-mnist/blob/master/LICENSE) 进行许可。

[1] Han Xiao 和 Kashif Rasul 和 Roland Vollgraf，[Fashion-MNIST：用于基准测试机器学习算法的新型图像数据集](https://arxiv.org/abs/1708.07747)（2017 年）
