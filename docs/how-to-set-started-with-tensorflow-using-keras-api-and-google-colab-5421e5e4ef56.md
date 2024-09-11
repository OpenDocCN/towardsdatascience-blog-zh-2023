# 如何使用 Keras API 和 Google Colab 开始使用 TensorFlow

> 原文：[https://towardsdatascience.com/how-to-set-started-with-tensorflow-using-keras-api-and-google-colab-5421e5e4ef56?source=collection_archive---------0-----------------------#2023-01-19](https://towardsdatascience.com/how-to-set-started-with-tensorflow-using-keras-api-and-google-colab-5421e5e4ef56?source=collection_archive---------0-----------------------#2023-01-19)

## 逐步教程，使用神经网络分析人类活动

[](https://medium.com/@jonas_dieckmann?source=post_page-----5421e5e4ef56--------------------------------)[![Jonas Dieckmann](../Images/e1f2d236e6bda6ec1e14fd5eaa9d205e.png)](https://medium.com/@jonas_dieckmann?source=post_page-----5421e5e4ef56--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5421e5e4ef56--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5421e5e4ef56--------------------------------) [Jonas Dieckmann](https://medium.com/@jonas_dieckmann?source=post_page-----5421e5e4ef56--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1c8d1cf684f2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-set-started-with-tensorflow-using-keras-api-and-google-colab-5421e5e4ef56&user=Jonas+Dieckmann&userId=1c8d1cf684f2&source=post_page-1c8d1cf684f2----5421e5e4ef56---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5421e5e4ef56--------------------------------) ·11分钟阅读·2023年1月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5421e5e4ef56&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-set-started-with-tensorflow-using-keras-api-and-google-colab-5421e5e4ef56&user=Jonas+Dieckmann&userId=1c8d1cf684f2&source=-----5421e5e4ef56---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5421e5e4ef56&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-set-started-with-tensorflow-using-keras-api-and-google-colab-5421e5e4ef56&source=-----5421e5e4ef56---------------------bookmark_footer-----------)![](../Images/353e84cb61462300012ad7c85781491c.png)

图片由 [Serghei Trofimov](https://unsplash.com/@sergeytrofimov) 提供，来源于 [Unsplash](https://unsplash.com/)

本初学者教程旨在简要概述 TensorFlow 的基本知识，并指导你完成一个实践项目。如果你符合以下条件，本教程可能对你有价值：

+   你已经建立了第一个传统的机器学习模型，现在你对如何开始使用第一个 TensorFlow 模型感到好奇。

+   你已经探讨了 TensorFlow 的基本概念，但正在寻找一个实际挑战来提升你的技能。

如果你对数据科学或机器学习模型完全陌生，我建议你先集中关注其他教程，因为首先具备基本的理解是至关重要的。

本文结构分为三个主要部分：

#1 TensorFlow 和 Keras API 的简要介绍

#2 使用 Google Colab 设置 TensorFlow 的教程（免费）

#3 实践项目：人体活动分类

# #1 TensorFlow 和 Keras API 的简要介绍

如果你对 TensorFlow 完全陌生，我推荐以下视频，其中主要概念和不同的层结构以简短但有效的方式进行了解释。这不是一门全面的讲座，但它对主题本身提供了一个很好的介绍。

虽然 TensorFlow 是基础的机器学习平台，但 Keras 另一方面是一个 API，可以帮助你快速设置模型，并减少手动编码工作。

> Keras 是一个用 Python 编写的深度学习 API，运行在机器学习平台 TensorFlow 之上。它的开发重点是实现**快速实验**。能够尽快从想法到结果是做出好研究的关键。[[https://keras.io/about](https://keras.io/about/)]

开发团队表示 Keras 是：

+   **简单** — 但不简单化。Keras 减少了开发者的*认知负担*，让你可以专注于问题中重要的部分。

+   **灵活** — Keras 采用了*逐步披露复杂性的原则*：简单的工作流程应该快速且简单，而任意高级的工作流程应该*可能*通过一个清晰的路径构建在你已经学到的东西上。

+   **强大** — Keras 提供了行业级的性能和可扩展性：包括 NASA、YouTube 和 Waymo 在内的组织和公司都在使用它。

*[再次来源于* [*https://keras.io/about*](https://keras.io/about/)*]*

# #2 使用 Google Colab 设置 TensorFlow 的教程（免费）

使用 TensorFlow 的一个好建议是将其运行在图形处理单元（GPU）或张量处理单元（TPU）上，而不是普通的中央处理单元（CPU）加速器。虽然简单的模型和计算可能仍能在 CPU 上运行，但你可能会注意到，TensorFlow 的全部功能只能在图形硬件上充分发挥。

使用 GPU 最简单直接的方法是使用 [Google Colaboratory (“Colab”)](https://colab.research.google.com/)，这有点像“*一个* *无需设置且完全在云端运行的免费 Jupyter notebook 环境*。”虽然这个教程更多地谈论了 Colab 的简单性和优势，但也有缺点，如 GPU 小时限制和计算能力降低，与真正的云环境相比。然而，我相信 Colab 可能不是一个不好的服务，可以用来迈出 TensorFlow 的第一步。

要在Colab中为TensorFlow设置基本环境，你可以按照以下几个步骤进行：

1.  打开[https://colab.research.google.com/](https://colab.research.google.com/)并注册一个免费账户

1.  在Colab中创建一个新的*notebook*

1.  从菜单中选择*Runtime*并*更改运行时类型*

1.  从*硬件加速器*选项中选择*GPU* - 点击*保存*

![](../Images/9b681b429296fef2ef10424b207c5d52.png)

这张截图由作者从[Google Colab](https://colab.research.google.com/)拍摄

现在，你可以导入TensorFlow，并使用以下几行代码检查一切是否设置妥当：

现在你应该能看到输出中显示一个版本（例如“2.5.0”）以及一个指示GPU使用的物理设备消息。你的Notebook现在可以实际使用TensorFlow了！

# #3 实践项目：人类活动分类

以下教程旨在分析一个关于人类活动分类的数据集。该数据集由携带不同活动的人的智能手机惯性传感器记录的时间序列组成。

## 背景与数据集信息

了解简单的智能手机测量是否可以用来分类人类正在进行的身体活动是很有趣的。六种活动类别如下：

1.  走路

1.  上楼

1.  下楼

1.  坐着

1.  站立

1.  躺着

描述测量和不同活动的短视频可以在这里找到：

这种基于传感器的分类模型的结果可以被健康应用程序或其他智能手机应用程序使用，以改善用户体验、根据当前记录建议活动或跟踪一天中的身体活动。

给定的数据集包含来自智能手机惯性传感器的测量，这些智能手机由执行不同活动的人携带。数据集总共有10,299条记录，分为70%训练数据（7,352条记录）和30%测试数据（2,947条记录）。传感器信号（加速度计和陀螺仪）经过噪声过滤器预处理，然后在固定宽度滑动窗口中采样。每个信号记录了三个坐标（x，y，z），可以视为具有128个时间戳及其对应值的时间序列记录。目标列包含活动标签：WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING。

训练和测试数据集的所有特征都是数字（浮点数），经过归一化处理，并限制在-1和1之间。图1中展示了沿128个时间戳的两个示例测量。显示出每个人类活动在三个坐标和时间上具有其特征。因此，有可能识别时间序列中的模式和趋势，这些模式和趋势指示活动类别。

关于数据集和背景的更多信息可以在readme文件中找到，或[在此网站上](https://archive.ics.uci.edu/ml/datasets/human+activity+recognition+using+smartphones)。

## 步骤 01：初始数据加载和所需库

要开始项目，需要将数据集加载到 Colab 环境中。不要过于担心下面的代码，它只是将所有必需的文件移动到你的工作空间：

不出所料，我们需要导入一些必需的库来简化工作。其中一个重要的库是 *当然* TensorFlow 本身：

为了简化我们的教程，我们将仅使用来自惯性信号的可用身体数据。数据集已经分为训练集（7352 条数据）和测试集（2947 条数据），包含 128 个时间序列测量和 3 个坐标。为了获得格式为 (n, 128, 3) 的数据集，我们可以使用以下代码：

准备的最后一个重要步骤是将目标变量转换为 *独热编码* 测量。我们不再有表示类别的数值（例如 0 = WALKING），而是得到包含每个可用类别概率的数组（例如 [1, 0, 0, 0, 0, 0] = WALKING 或 [0, 0, 0, 1, 0, 0] = SITTING），在这种情况下，某个类别的概率为 100%。这很重要，因为 TensorFlow 会计算每个可能类别的概率。因此，我们需要相应地准备训练数据。Keras API 提供了一个简单的内置函数来满足这一要求。你会注意到，目标变量的形状从 (n,) 变为 (n, 6)：

## 步骤 02：绘制一些示例时间序列

在本教程中，我们将忽略适当的探索性分析部分，因为我们想更多地关注 TensorFlow 的使用。然而，在这个阶段绘制一些示例时间序列可能是有意义的，因为这将帮助我们更好地理解我们希望用于分类的数据。

我们现在可以使用以下代码来绘制数据集中的一些随机测量。我选择了测量 992（走路）和 2481（躺着）来展示数据中的差异：

输出如下图所示。你可能已经可以识别出人身体测量中的一些差异，取决于所进行的活动。最后，我们希望并有机会在数据上运行一个神经网络模型，该模型可能很好地预测我们的活动类别。让我们看看吧！

![](../Images/9804705a088a2015ab802c038476e947.png)

截图由作者从 [Google Colab](https://colab.research.google.com/) 获取。

![](../Images/2a39669469c55a96d9a290ec1f892ecf.png)

截图由作者从 [Google Colab](https://colab.research.google.com/) 获取。

## 步骤 03：使用 Keras API 构建和训练神经网络

要在 TensorFlow 中构建和训练神经网络，你需要将可用的*训练*数据拆分为*训练和验证*子集。然后，TensorFlow 将使用这两部分来训练模型，并使用验证损失和验证准确度来评估进展。你可以调整准确度数据集的大小，但我在这种情况下使用了原始训练数据的 20%。可以设置随机状态以在不同时间重现可识别的数据拆分。

监控模型在训练过程中的最佳实践是设置*TensorBoard*。你可以使用以下代码准备你的笔记本，其中加载扩展并创建一个*日志写入器*：

要设置神经网络，你首先需要决定你想构建哪种类型的网络。最简单的架构是一个线性层堆栈，称为顺序模型。你可以通过将层实例的列表传递给构造函数来创建顺序模型。开始时，你可以初始化你的模型：

```py
model = tf.keras.Sequential()
```

从这一点开始，你可以根据需要添加任何你想使用的层。网上有很多优秀的教程描述了这些层的不同功能。在本教程中，我们将仅引导你完成一个简单的工作设置，以便你成功运行第一个模型。随意添加和修改架构以提高我的准确率！

首先，我们添加**输入层**，其维度为我们的数据集（128,3）：

```py
model.add(tf.keras.layers.InputLayer(input_shape=(128,3)))
```

其次，我们添加一个**一维卷积层**，在这里我们可以设置滤波器数量和卷积核大小。该层之后将跟随一个**批量归一化层**，它将转换输入，使其标准化，即均值为零，标准差为一。

```py
model.add(tf.keras.layers.Conv1D(filters=256, kernel_size=10))model.add(tf.keras.layers.BatchNormalization())
```

其次，我们添加一个**ReLu层**和一个**全局平均池化层**。最后，我们需要一个**Dense层**，将我们的网络激活为六类输出。由于这是一个分类问题，我们使用**softmax激活**函数，并设置六个单元（因为我们有六个类别）。

```py
model.add(tf.keras.layers.ReLU())model.add(tf.keras.layers.GlobalAveragePooling1D())model.add(tf.keras.layers.Dense(units=6, activation=tf.nn.softmax))
```

要训练模型，我们首先需要用合适的优化器编译模型。在本教程中，我选择了 Adam 优化器，你可以调整学习率。

```py
model.compile(optimizer=tf.keras.optimizers.Adam(0.001), loss='categorical_crossentropy', metrics=['accuracy'])
```

要训练模型，我们需要决定运行多少个周期以及每个批次的批量大小。一个周期是指每次通过所有训练集样本时的时间步长。批量大小是每个批次的数据条目数量。为了将我们的模型与 TensorBoard 监控相链接，我们添加一个回调并设置日志目录。

```py
callbacks = [tf.keras.callbacks.TensorBoard(log_dir=logdir)] model.fit(x_train, y_train, epochs=100, batch_size=32, callbacks=callbacks, validation_data=(x_valid, y_valid))
```

当你运行代码时，你会看到单元格下方的一些输出，这些输出指示每个训练周期的情况。在 TensorBoard 中，你将能够看到训练和验证数据的准确率逐渐提高以及损失逐渐减少。训练完成所有周期后，它将自动停止。以下是模型拟合的完整代码：

现在让我们尝试理解我们所构建的模型以及在训练过程中模型如何发展。正如下面的截图所示，在前20到30个训练周期中有显著的提升，之后的提升则较为温和。蓝线表示训练数据集的准确性和损失，红线表示验证数据集。

![](../Images/9fb8b6fdab2383445ab9fde605df3eb1.png)

截图由作者从[Google Colab](https://colab.research.google.com/)中获取

![](../Images/540c177f0a4ab914878eb15fc248191b.png)

截图由作者从[Google Colab](https://colab.research.google.com/)中获取

总体而言，我们做得还不错，达到了85.38%的验证准确性。训练损失和验证损失之间的可比发展表明训练过程没有过拟合。然而，仍有进一步改进模型的机会。请随意自行探索。以下是一些想法：

+   尝试不同的训练周期、批量大小和学习率

+   修改Conv1D层中的滤波器数量和内核大小

+   添加更多层并尝试不同的架构

+   将其他数据集添加到模型中（除了仅有的身体数据）

最后，评估模型在测试数据集上的准确性始终是最后一步。到目前为止，我们只使用训练数据和验证数据训练了模型。要将最终模型应用于测试数据集，可以使用下面的代码。我们使用模型计算每个类别的概率。之后，我们取每行的最大概率，从而识别该行的离散类别。

我的方法中测试准确性为84.42%，因此稍低于验证准确性，但与训练准确性处于相似水平。

恭喜你：你使用TensorFlow应用了第一个神经网络！

*请注意，你将会获得不同的准确性和损失值，因为TensorFlow不能以相同的方式重现。然而，我预期你的值会在75%到90%之间。*

# 总结

上述教程的目的是为你提供一个实际的入门思路，开始使用TensorFlow和Keras API。我知道这远不够详细或对特性的全面描述。然而，我希望这能*帮助一些初学者独立运行他们的第一个模型*，并理解TensorFlow周围的基本组成部分。

当然，还有更多的内容等待探索。试着超越我们最初的准确性成就，并在评论中分享你的成果！

[](https://medium.com/@jonas_dieckmann?source=post_page-----5421e5e4ef56--------------------------------) [## Jonas Dieckmann - Medium

### 阅读Jonas Dieckmann在Medium上的文章。他是菲利普斯的分析经理和产品负责人，对…充满热情并撰写文章。

[medium.com](https://medium.com/@jonas_dieckmann?source=post_page-----5421e5e4ef56--------------------------------)

希望你觉得有用。告诉我你的想法！也欢迎在 LinkedIn 上与我联系 [https://www.linkedin.com/in/jonas-dieckmann/](https://www.linkedin.com/in/jonas-dieckmann/) 或在 Medium 上关注我。

# 另见我其他的一些文章：

[](/case-study-applying-a-data-science-process-model-to-a-real-world-scenario-93ae57b682bf?source=post_page-----5421e5e4ef56--------------------------------) [## 案例研究：将数据科学过程模型应用于现实世界场景

### 供应链材料规划中的机器学习模型开发

towardsdatascience.com](/case-study-applying-a-data-science-process-model-to-a-real-world-scenario-93ae57b682bf?source=post_page-----5421e5e4ef56--------------------------------) [](/introduction-to-ica-independent-component-analysis-b2c3c4720cd9?source=post_page-----5421e5e4ef56--------------------------------) [## ICA简介：独立成分分析

### 你是否曾经遇到过需要分析一个复杂且高度相关的数据集的情况……

towardsdatascience.com](/introduction-to-ica-independent-component-analysis-b2c3c4720cd9?source=post_page-----5421e5e4ef56--------------------------------)

# 参考文献

Reyes-Ortiz, Jorge, Anguita, Davide, Ghio, Alessandro, Oneto, Luca & Parra, Xavier. (2012). 使用智能手机进行人类活动识别。UCI 机器学习库

本数据集使用了[知识共享署名 4.0 国际](https://creativecommons.org/licenses/by/4.0/legalcode)（CC BY 4.0）许可证。这允许出于任何目的共享和改编数据集，前提是给予适当的署名。

欲了解更多信息，请访问[这个网站](https://archive-beta.ics.uci.edu/dataset/240/human+activity+recognition+using+smartphones)。
