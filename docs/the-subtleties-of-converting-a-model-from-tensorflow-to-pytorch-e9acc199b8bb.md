# 从 TensorFlow 转换到 PyTorch 的细微差别

> 原文：[https://towardsdatascience.com/the-subtleties-of-converting-a-model-from-tensorflow-to-pytorch-e9acc199b8bb?source=collection_archive---------10-----------------------#2023-02-07](https://towardsdatascience.com/the-subtleties-of-converting-a-model-from-tensorflow-to-pytorch-e9acc199b8bb?source=collection_archive---------10-----------------------#2023-02-07)

## 确保成功的建议和技巧

[](https://medium.com/@gil.shomron?source=post_page-----e9acc199b8bb--------------------------------)[![Gil Shomron](../Images/4cd9c4770757a863f477a381d119de32.png)](https://medium.com/@gil.shomron?source=post_page-----e9acc199b8bb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e9acc199b8bb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e9acc199b8bb--------------------------------) [Gil Shomron](https://medium.com/@gil.shomron?source=post_page-----e9acc199b8bb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6f73b54221a8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-subtleties-of-converting-a-model-from-tensorflow-to-pytorch-e9acc199b8bb&user=Gil+Shomron&userId=6f73b54221a8&source=post_page-6f73b54221a8----e9acc199b8bb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e9acc199b8bb--------------------------------) · 5分钟阅读 · 2023年2月7日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe9acc199b8bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-subtleties-of-converting-a-model-from-tensorflow-to-pytorch-e9acc199b8bb&user=Gil+Shomron&userId=6f73b54221a8&source=-----e9acc199b8bb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe9acc199b8bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-subtleties-of-converting-a-model-from-tensorflow-to-pytorch-e9acc199b8bb&source=-----e9acc199b8bb---------------------bookmark_footer-----------)![](../Images/33e5c81524956775a1c3ab84475d8eb7.png)

图片由 [Jan Böttinger](https://unsplash.com/@bttngr?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

将模型检查点从一个框架转换到另一个框架可能是一个复杂的过程，如果你想保持相同的性能水平。

过去，我被要求评估 [我的工作](https://arxiv.org/abs/2004.09309) 在 [MLPerf 推理基准套件](https://arxiv.org/abs/1911.02549) 上的表现。MLPerf 是一个用于测量机器学习（ML）模型在部署场景中表现的行业基准。该基准提供了一个标准化框架，用于比较不同 ML 系统的性能，使其成为评估 ML 模型硬件性能的宝贵工具。在我的工作中，使用 MLPerf 推理基准套件中的模型和检查点使我能够呈现符合广泛接受的行业标准的结果，从而提高了读者对我工作的信心。然而，我的整个模拟框架建立在 PyTorch 之上，而我所需的模型权重则编码在 TensorFlow 中。

在这篇博文中，我想分享将 TensorFlow 模型迁移到 PyTorch 的过程，突显其中的细微差别。重要的是要注意，一般来说，*任何* 两个不同框架之间可能存在小差异；因此，在转换任何两个框架时，请考虑这里的提示。最终结果，即 MLPerf ResNet-50 和 MobileNet-v1 PyTorch 检查点，可以从我的 [GitHub 仓库](https://github.com/gilshm/mlperf-pytorch) 获得。

# 使用 PB 文件

TensorFlow `pb`（protobuf）文件包含模型图的描述以及层权重和参数。因此，第一步是解析模型 pb 文件。MLPerf 检查点文件可以从 [这里](https://github.com/mlperf/inference/tree/master/vision/classification_and_detection) 下载。受到 [这个](https://leimao.github.io/blog/Save-Load-Inference-From-TF2-Frozen-Graph/) 博文的启发，我使用了这个类来从 pb 文件中提取所有必要的数据：

一旦创建了 `NeuralNetPB` 类，它将包含所有的权重在其“weights”属性中。拥有权重张量后，就可以将它们分配到 PyTorch 模型中的适当层。

# 层映射

首先，重要的是要注意，我们没有模型定义的 TensorFlow Python 文件。尽管如此，由于我们使用的是知名的模型架构，这些层大体上是相似的。

> **提示 #1：** 使用 [Netron](https://lutzroeder.github.io/netron/) 来可视化你的模型图。这将有助于理解 `pb` 文件中的模型结构，以及将 pb 文件中的层映射到 PyTorch。

现在，让我们开始映射。我将给出一些 MobileNet-v1 的例子：

1.  `MobilenetV1/Conv2d_0/weights` (3-3-3-32) — 这是第一个卷积层。我们将其映射到 `model.0.1.weight`。

1.  `MobilenetV1/Conv2d_0/BatchNorm/gamma` (32) — 这是第一个批量归一化层。gamma对应于PyTorch中的层权重，因此我们将其映射为`model.0.2.weight`。相同的BatchNorm层还包含`beta`、`moving_mean`和`moving_variance`字段，它们分别对应于PyTorch中的`bias`、`running_mean`和`running_var`。

1.  `MobilenetV1/Conv2d_1_depthwise/depthwise_weights` (3, 3, 32, 1) — 这是第二个卷积层（或第一个深度卷积层），它映射到`model.1.weight`。

DNN模型结构通常是重复的，所以一旦掌握了思路，你就可以在`for`循环中编写部分代码。

由于`NeuralNetPB`属性不是PyTorch张量，因此需要使用`torch.FloatTensor(...)`进行转换。

> **细节 #1: 排列。** TensorFlow卷积层的权重张量排列方式不同。根据TensorFlow [文档](https://www.tensorflow.org/api_docs/python/tf/nn/conv2d)，权重张量的形状是`[H, W, IN_C, OUT_C]`，而PyTorch的权重张量形状是`[OUT_C, IN_C, H, W]`。因此，重新排列张量：`torch.FloatTensor(...).permute(3, 2, 0, 1)`。话虽如此，深度卷积应该用`.permute(2, 3, 0, 1)`进行重新排列。

完成后，你应该保存所有内容，使用`torch.save(...)`。

# 模型修改

完成前一部分后，你的PyTorch模型应该在层组成方面与TensorFlow参考模型匹配。然而，还有一些额外的细节需要注意。

> **细节 #2: 参数。** 权重并不是唯一的参数。例如，批量归一化层包含一个`epsilon`属性（用于数值稳定性）。`epsilon`的默认值在TensorFlow和PyTorch中是不同的。但即使它们相等，TensorFlow ResNet-50模型也会修改epsilon，所以要注意。另一个例子是Dropout层（如果存在的话）。
> 
> **细节 #3: 填充。** TensorFlow中的填充参数包含一个PyTorch中不存在的选项：`SAME`。`SAME`意味着输入特征图将被填充，使输出特征图（即卷积操作结果）的空间维度与输入维度相等。然而，如果填充是不对称的，会发生什么？TensorFlow会在左侧还是右侧填充更多？**TensorFlow在右侧填充，而PyTorch在左侧填充。** 同样的逻辑适用于垂直方向，即**底部可能会有一行额外的零，而在PyTorch中，额外的行将出现在顶部。**

你可以查看我稍微修改过的[ResNet-50](https://github.com/gilshm/mlperf-pytorch/blob/master/models/resnet.py)和[MobileNet-v1](https://github.com/gilshm/mlperf-pytorch/blob/master/models/mobilenet_v1.py)模型，看看我如何相应地进行修改。

# 预处理

如果你想要复现与参考模型（在这个例子中是 MLPerf 模型）完全相同的结果，那么你必须复现完全相同的预处理阶段。处理 ImageNet 数据集时的常见预处理阶段包括（1）调整大小为 256；（2）中心裁剪为 224；（3）归一化为[0, 1]之间的值（即除以 255）；（4）每个 RGB 通道的均值和标准差修正。然而，MLPerf 中的标准差没有归一化，偏差的归一化方式也不同（详见[此处](https://github.com/mlperf/inference/blob/master/vision/classification_and_detection/python/dataset.py)）。

此外，即使进行完全相同的预处理，我发现 MLPerf 中的插值算法与使用 PIL 的 PyTorch 实现方式不同。由于 MLPerf 预处理整个 ImageNet 验证集并将其保存为 NumPy 数组，我决定避免使用 PyTorch 预处理，直接加载 NumPy 数组。在这一点上，我也达到了与 MLPerf 报告完全相同的性能——ResNet-50 和 MobileNet-v1 的顶级 1 准确率分别为 76.456% 和 71.676%。

# 调试

所以你映射了权重，并且（你认为）已经根据参考模型微调了所有参数和预处理阶段。你开始推断却什么也没有发生——准确率为 0%。接下来怎么办？没有简单的答案，你必须进行调试。我所做的是逐层比较张量值。结果应该几乎相同（允许有一些浮点差异）。例如，使用 NeuralNetPB 测试函数 `nn_pb.test(input, 'import/MobilenetV1/MobilenetV1/Conv2d_1_depthwise/Relu6:0')` 可以得到其中一个 ReLU 结果。你应该已经从映射阶段知道了所有的层名称。

# 结论

要有耐心，这最为重要。将任何模型在框架之间迁移都需要注意所有细节。可能有一些工具可以帮助自动转换，但即使你成功使用了它们，它们也可能忽略了我在这里提到的微妙差异，从而无法创建一对一的副本，这意味着模型表现会有所不同——这就是我遇到的情况。

你可以在[我的 GitHub 仓库](https://github.com/gilshm/mlperf-pytorch)中找到我的 MLPerf ResNet-50 和 MobileNet-v1 PyTorch 检查点和模型。

随时可以在[LinkedIn](https://www.linkedin.com/in/gilsho/)上联系我，或者在 Medium 上关注我。

# 参考文献

[1] Shomron, Gil, and Uri Weiser. “Non-Blocking Simultaneous Multithreading: Embracing the Resiliency of Deep Neural Networks.” *国际微架构研讨会 (MICRO)*. IEEE, 2020.

[2] Reddi, Vijay Janapa, et al. “MLPerf Inference Benchmark.” *国际计算机架构研讨会 (ISCA)*. IEEE, 2020.
