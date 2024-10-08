# 在大学 HPC 集群上训练模型的 9 个技巧

> 原文：[`towardsdatascience.com/9-tips-for-training-models-on-your-universitys-hpc-cluster-a703eb87f3d6`](https://towardsdatascience.com/9-tips-for-training-models-on-your-universitys-hpc-cluster-a703eb87f3d6)

## 如何在资源受限的环境中有效运行和调试代码

[](https://conorosullyds.medium.com/?source=post_page-----a703eb87f3d6--------------------------------)![Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----a703eb87f3d6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a703eb87f3d6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a703eb87f3d6--------------------------------) [Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----a703eb87f3d6--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----a703eb87f3d6--------------------------------) ·6 分钟阅读·2023 年 3 月 28 日

--

![](img/c5fc4bc25739d8d8186821bd8e9d602a.png)

图片由[Martijn Baudoin](https://unsplash.com/ja/@martijnbaudoin?utm_source=medium&utm_medium=referral)拍摄，刊登在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

排队作业，等待 24 小时，**cuda runtime error:** 内存不足

排队作业，等待 24 小时，**FileNotFoundError:** 没有这样的文件或目录

排队作业，等待 24 小时，**RuntimeError:** 堆栈期望每个张量……

啊啊啊啊啊!!!

在高性能计算（HPC）集群上调试代码可能非常令人沮丧。更糟糕的是，在大学里你将与其他学生共享资源。作业将被添加到队列中。你可能要等待几个小时才能知道代码是否有错误。

最近我在大学的 HPC 上训练模型。我学到了一些东西（是艰难的方式）。我想分享这些技巧，希望能让你的体验更顺利。我会保持通用，以便你可以将它们应用到任何系统。

## 尽可能在个人机器上进行开发

HPC 集群有一个目标——处理数据。没有华丽的 IDE，没有调试器，也没有副驾驶。你真的想用 vim 编写整个项目吗？

为了避免发疯（把那些留给你的论文吧），你应该在自己的机器上开发代码。用**小样本**训练模型至少**1 个周期**。包含测试以确保数据正确加载并且所有结果都已保存。训练模型 50 个周期却发现忘记保存最佳结果是毫无乐趣的（是的，我确实做过这个）。

## 保持代码简单

你在代码中包含的任何额外步骤都会增加运行失败的可能性。你应该只运行需要大量计算能力的进程。例如，模型训练完成后，你会想要对其进行评估。这可以在本地机器上完成。如果你的测试集足够小，甚至 CPU 也可以使用。

## 不要硬编码任何路径或变量

当将代码从本地机器移动到 HPC 时，某些事情不可避免地需要更改。例如，加载数据和保存结果的文件路径。我是在 Mac 上开发的，但 HPC 使用的是 Linux 操作系统。这意味着我需要将设备从“mps”更改为“cuda”。

![](img/049220951feba249272f10d2004a4fe4.png)

图 1：待更新变量示例（来源：作者）

不要硬编码任何需要更改的内容。使用变量并在脚本顶部定义它们。这使得它们易于更改，并且你不会忘记任何东西。相信我！你不想用 vim 滚动查看代码行。

## 增加批量大小

在上面的**图 1**中，你可以看到我包含的一个变量是**batch_size**。这是每次迭代中加载和用于更新模型参数的样本数量。较大的批量大小意味着你可以在 GPU 上并行处理更多样本，从而加快训练时间。

在本地机器上增加这一点很快会导致“**cuda runtime error:** out of memory”错误。相比之下，HPC 可以处理更大的批量大小。然而，[重要的是不要增加过多](https://medium.com/mini-distill/effect-of-batch-size-on-training-dynamics-21c14f7a716e)，因为这可能会对模型准确性产生负面影响。我只是将批量大小从 32 加倍到 64。

## 对任何实验使用系统参数

尽可能避免在 HPC 上编辑代码。同时，为了训练最佳模型，你需要进行实验。为了解决这个问题，使用系统参数。如**图 2**所示，这些参数允许你在命令行中更新脚本中的变量。

![](img/69e87f0472b4305dd0f21d9519f6409b.png)

图 2：系统参数示例（来源：作者）

第一个变量允许我更新最终模型的保存路径（**model_nam**e）。其他变量允许我以各种方式**采样、缩放**和**清理**数据（见**图 3**）。你甚至可以更新模型架构。例如，通过传递一个列表[x,y,z]来定义神经网络隐藏层中的节点数量。

![](img/1739baee9a425eb964921541c87ec490.png)

图 3：用 4 个数据清理实验训练 U-Net（来源：作者）

## 包含一个示例参数

**sample** 系统参数特别有用。我上面包含的这个是一个二进制标志。如果设置为 true，建模代码将使用 1000 个样本的子集运行。你也可以传递实际的样本数量作为整数。

我通常会在两个作业之间安排紧接着的运行——一个是采样数据集运行，另一个是完整数据集运行。采样运行通常会在几分钟内完成（除非有人占用了所有的 GPU!!）。这有助于指出那些烦人的错误。如果出现问题，我有机会在一天结束前停止完整数据集的运行，进行修正，然后重新启动。

## 详尽说明

不，我不是在谈论你那个主修金融的朋友。你的代码应该尽可能地告诉你更多信息。这将帮助修复任何出现的错误。使用那些打印语句！我发现每次运行时打印的一些有用信息包括：

+   系统设备（即 cuda 或 CPU）

+   数据集的长度、训练集和验证集

+   数据在任何转换前后的样本

+   每个 epoch 和 batch 的训练损失和验证损失

+   模型架构

+   任何系统参数的值

你永远不知道会出什么问题。对于机器学习来说，逻辑错误可能很难被识别。你的代码可能运行得很好，但产生的模型却是完全无用的。拥有一个过程记录将帮助你追溯错误的来源。如果你打算进行多次实验，这一点尤其重要。

## 使用 diff 比较工具

事情会出错的。非常糟糕。我打破了自己的规则，在 HPC 上对脚本进行了几处修改。好吧，我做了很多修改。我失去了对自己所做更改的追踪，代码无法正常运行。在这种情况下，[diff 比较工具](https://www.diffchecker.com/text-compare/#editor)就派上了用场。

它会逐行比较一个文档中的文本与另一个文档中的文本。我用它来比较 HPC 上的脚本与我本地机器上的一个脚本。这指出了我所做的更改，我可以立即识别出问题。

![](img/dfd059907e9e9f6d1e4e3d24fee2d307.png)

图 4：指出 HPC 和本地脚本中的差异（来源：[diff 比较工具](https://www.diffchecker.com/text-compare/#editor)）

## 对你的研究保持现实

目前的提示还缺乏细节。你可以做很多事情来[加快特定模型包（例如 XGBoost）的训练速度](https://medium.com/towards-data-science/how-to-speed-up-xgboost-model-training-fcf4dc5dbe5f)。[更好地跟踪你的数据和模型](https://medium.com/@iamleonie/intro-to-mlops-data-and-model-versioning-fa623c220966)也可以帮助避免错误和混乱。但实际上，你能做的也有限。我的最终建议是对你在可用资源下能实现的目标保持现实。

在 LLM 时代，这可能会令人沮丧。所有主要的突破似乎都是通过增加计算能力来实现的。你必须考虑到，一个像 GPT-3 这样大小的模型[训练成本估计为 **45 万美元**](https://www.mosaicml.com/blog/gpt-3-quality-for-500k#)。这与一些部门的整个预算相当！你根本无法竞争。

然而，您仍然可以做出有价值的贡献。微调模型不需要那么多资源。收集和标注数据往往比在基准测试中追求 SOTA 更为重要。每个应用机器学习的子领域都会带来自己的挑战。通常，资源的匮乏会促使我们对这些挑战提出更具创新性的解决方案。所以要感谢那些漫长的工作队列……

哦，我在自欺欺人！！有谁愿意买给我一块 GPU 吗？

希望您喜欢这篇文章！您可以通过成为我的 [**推荐会员**](https://conorosullyds.medium.com/membership) **:)** 来支持我

[](https://conorosullyds.medium.com/membership?source=post_page-----a703eb87f3d6--------------------------------) [## 通过我的推荐链接加入 Medium — Conor O’Sullivan]

### 作为 Medium 会员，您的会员费的一部分会分配给您阅读的作者，您可以全面访问每个故事……

[conorosullyds.medium.com](https://conorosullyds.medium.com/membership?source=post_page-----a703eb87f3d6--------------------------------)

| [Twitter](https://twitter.com/conorosullyDS) | [YouTube](https://www.youtube.com/channel/UChsoWqJbEjBwrn00Zvghi4w) | [Newsletter](https://mailchi.mp/aa82a5ce1dc0/signup) — 免费注册获取 [Python SHAP 课程](https://adataodyssey.com/courses/shap-with-python/)

## 参考文献

[Michael Galarnyk](https://medium.com/u/c07aac64b6e1?source=post_page-----a703eb87f3d6--------------------------------) **如何加速 XGBoost 模型训练** [`medium.com/towards-data-science/how-to-speed-up-xgboost-model-training-fcf4dc5dbe5f`](https://medium.com/towards-data-science/how-to-speed-up-xgboost-model-training-fcf4dc5dbe5f)

[Leonie Monigatti](https://medium.com/u/3a38da70d8dc?source=post_page-----a703eb87f3d6--------------------------------) **MLOps 简介：数据和模型版本管理** [`medium.com/@iamleonie/intro-to-mlops-data-and-model-versioning-fa623c220966`](https://medium.com/@iamleonie/intro-to-mlops-data-and-model-versioning-fa623c220966)

[Kevin Shen](https://medium.com/u/c1ed18ad484c?source=post_page-----a703eb87f3d6--------------------------------) **批量大小对训练动态的影响** [`medium.com/mini-distill/effect-of-batch-size-on-training-dynamics-21c14f7a716e`](https://medium.com/mini-distill/effect-of-batch-size-on-training-dynamics-21c14f7a716e)
