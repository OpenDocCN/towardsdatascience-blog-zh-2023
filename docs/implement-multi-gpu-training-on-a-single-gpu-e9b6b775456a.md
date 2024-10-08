# 在 3 分钟内在单个 GPU 系统上进行多 GPU 训练

> 原文：[`towardsdatascience.com/implement-multi-gpu-training-on-a-single-gpu-e9b6b775456a`](https://towardsdatascience.com/implement-multi-gpu-training-on-a-single-gpu-e9b6b775456a)

## TensorFlow 高级指南

[](https://medium.com/@SaschaKirch?source=post_page-----e9b6b775456a--------------------------------)![Sascha Kirch](https://medium.com/@SaschaKirch?source=post_page-----e9b6b775456a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e9b6b775456a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e9b6b775456a--------------------------------) [Sascha Kirch](https://medium.com/@SaschaKirch?source=post_page-----e9b6b775456a--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----e9b6b775456a--------------------------------) ·阅读时间 3 分钟·2023 年 5 月 15 日

--

![](img/dbabbc57ed819e80bd93ba392243de25.png)

图片由[Chris Liverani](https://unsplash.com/@chrisliverani?utm_source=medium&utm_medium=referral)提供，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我想和你分享一个有趣的小技巧，关于如何在单个 GPU 上测试我的多 GPU 训练代码。

# 动机

我想问题很明显，你可能自己也经历过。你想训练一个深度学习模型，并希望利用多个 GPU、TPU 或甚至多个工作节点来获得额外的速度或更大的批量大小。但当然，你不能（或者说不应该，因为我见过很多 😅）占用通常共享的硬件进行调试，甚至在付费云实例上花费大量金钱。

让我告诉你，重要的不是你的系统有多少个物理 GPU，而是你的软件认为它有多少个。关键词是：***（设备）虚拟化***。

# 让我们实现它

首先，我们来看看你通常如何**检测和连接到你的 GPU**：

代码 1：检测所有可用的 GPU，初始化相应的作用域，并在策略作用域内初始化你的模型、优化器和检查点。

你首先需要列出所有可用的设备，然后选择合适的策略，并在策略作用域内初始化你的模型、优化器和检查点。如果你使用标准训练循环中的***model.fit()***，你就完成了。如果你使用自定义训练循环，你需要实现一些额外的步骤。

> 查看我关于使用 Google 的 TPU 进行加速分布式训练的教程，了解更多关于自定义训练循环的分布式训练细节。

上述代码中有一个重要细节。你注意到我用了函数 ***list_logical_devices(“GPU”)*** 而不是 ***list_physical_devices(“GPU”)*** 吗？逻辑设备是所有对软件可见的设备，但这些设备不一定与实际的物理设备关联。如果我们现在运行代码块，这可能是你会看到的输出：

![](img/4d0a65689a23f310b1f78f3fce32ab08.png)

图 1：运行 Code 1 之后的输出截图，并连接到一个逻辑 GPU 和一个相关的物理 GPU。由作者拍摄。

我们将利用逻辑设备定义来定义一些逻辑设备，然后列出所有逻辑设备并连接到它们。具体来说，我们将定义 4 个逻辑 GPU 与一个物理 GPU 相关联。这是操作步骤：

Code 2：创建与单个物理 GPU 相关联的多个逻辑 GPU 设备。

如果我们再次打印逻辑设备与物理设备的数量，你会看到：

![](img/e1724aa8e47fc229bf375fbce66810d9.png)

图 2：运行 Code 2 之后的输出截图。在 Code 1 之前，并连接到四个逻辑 GPU 和一个相关的物理 GPU。由作者拍摄。

就这样，你现在可以像在 4 个 GPU 上进行分布式训练一样，在单个 GPU 上测试你的代码。

## 有几点需要注意：

1.  你实际上并没有进行分布式训练，因此没有通过并行化获得性能提升。

1.  你需要在连接硬件之前分配逻辑设备，否则会引发异常。

1.  它只测试算法的正确实现，你可以检查输出的形状和值是否符合预期。它不能保证多 GPU 配置中的所有驱动程序和硬件都是正确的。

如果这个技巧对你有用，或者你已经知道这个功能，请在评论中告诉我！对我来说，这真是一个游戏改变者。

## 祝你测试愉快！💪
