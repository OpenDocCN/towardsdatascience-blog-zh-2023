# 使用 Hydra 跟踪你的实验

> 原文：[`towardsdatascience.com/keep-track-of-your-experiments-with-hydra-b29937a99fc9`](https://towardsdatascience.com/keep-track-of-your-experiments-with-hydra-b29937a99fc9)

![](img/01cd12756484fac696d8c911c15cd612.png)

（图片来源：作者）

## 使用 YAML 文件配置超参数，加速你的研究！

[](https://medium.com/@marcellopoliti?source=post_page-----b29937a99fc9--------------------------------)![Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----b29937a99fc9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b29937a99fc9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b29937a99fc9--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----b29937a99fc9--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b29937a99fc9--------------------------------) ·阅读时间 5 分钟·2023 年 8 月 1 日

--

## 介绍

就像不可能在第一次尝试时编写没有错误的代码一样，第一次尝试也不可能训练出合适的模型。

有一些机器学习和深度学习经验的人知道，你经常需要**花费大量时间选择模型的正确超参数**。这些超参数例如学习率、批量大小和输出中的类别数，但这些只是最常见的一些，一个项目可以有数百个这样的参数。

通过更改超参数，我们可以得到不同的结果（更好或更差），而在某些时候**跟踪所有已进行的测试是非常困难的**。

我曾经做了很长时间的事情：我用手在 Excel 表格中写下所有这些超参数，并在旁边写下每个实验的结果，例如损失值。后来我“进化”了，开始编写超参数的配置文件，在其中放入我想测试的各种值。我曾经编写自定义 Python 函数来读取这些值并将其放入训练函数中。YAML 文件基本上是一个层次结构构建的文件，你可以在其中插入键和值，如下所示：

```py
data:
  path: "data/ESC-50"
  sample_rate: 8000
  train_folds: [1, 2, 3]
  val_folds: [4]
  test_folds: [5]
  batch_size: 8

model:
  base_filters: 32
  num_classes: 50
  optim:
    lr: 3e-4

seed: 0

trainer:
  max_epochs: 10
```

我后来发现了[Hydra](https://hydra.cc/docs/intro/)，这是一个开源框架，使整个过程更简单、更快捷。

## 让我们开始吧！

假设我们正在使用 PyTorch 开发一个简单的机器学习项目。像往常一样，我们创建一个数据集类，实例化数据加载器，创建模型并进行训练。在这个示例中，我将使用 PyTorch [Lightning](https://www.pytorchlightning.ai/index.html) 更好地组织代码，其中我们有一个 Trainer 对象，类似于你在 Keras 中所做的。如果你习惯了 PyTorch，你也会很快理解 Lightning。

对于我们的示例，我们可以使用著名的 ESC50 数据集，你可以在 [这里](https://github.com/karolpiczak/ESC-50) 免费找到。

首先，我们导入所有需要的库。我将使用 Deepnote 运行代码。

像往常一样，我们实现一个 PyTorch 类来管理数据集。

我们使用 Lightning 实现模型。我不会描述模型，因为那并不是重点，即使你没有音频相关任务的经验也没关系，重要的是如何构建和使用 Hydra 进行实验。

最后是训练函数。

## 添加 Hydra 配置

在上面的代码中，你会看到有许多硬编码的参数，比如批量大小、用于训练-验证-测试分割的折数、纪元数等。

我想找到最佳参数，然后启动各种实验并根据结果做出判断。Hydra 使这变得非常简单，让我们看看如何安装和使用这个库。

现在我们在一个名为 configs 的新文件夹中创建一个名为“default.yaml”的配置文件。

文件将包含所有我们想测试的超参数的键值对。以下是一个示例。

```py
data:
  path: "data/ESC-50"
  sample_rate: 8000
  train_folds: [1, 2, 3]
  val_folds: [4]
  test_folds: [5]
  batch_size: 8

model:
  base_filters: 32
  num_classes: 50
  optim:
    lr: 3e-4

seed: 0

trainer:
  max_epochs: 10
```

现在我们想要**动态传递这些参数到训练函数中**。**Hydra 允许我们使用装饰器来实现这一点**。

正如你所见，装饰器定义了要与参数一起使用的文件夹和文件名。并且**通过获取文件值并将其作为字典在 Python 中处理，硬编码的值已经被更改**，非常简单！

然而，你会注意到，在 Audionet 模型中，预期输入已经被更改，以便接受以下方式的参数字典。

现在一切准备就绪。

尝试运行 train()，Hydra 将生成组织良好且易于验证的结果。

这是我使用 Hydra 的输出示例。

![](img/d49b09806f6c00209fc9ffe90ce0e658.png)

(图片来源：作者)

正如我们看到的，Hydra 提供了参数配置文件的回顾，关于模型训练的 Lightning 日志，以及用于检查指标的 CSV 文件。

然而，这些只是 Hydra 的基础功能，Hydra 是一个允许你做更多事情的工具。

一个很好的功能是通过终端运行脚本并从命令行添加或更改文件参数。

例如，如果我们想快速更改种子而不修改 YAML 文件，我们可以这样做：

```py
python my_app.py 'seed=1'
```

我们还可以向作为输入传递给 Trainer 的配置文件中添加一些关键字，例如 pl.Trainer(**cfg.trainer)。

要添加关键字，我们使用来自终端的 + 命令。

```py
python train.py +trainer.fast_dev_run = True
```

有时候你可能会想把实验保存在默认文件夹以外的位置，这可以通过在命令行中覆盖 Hydra 的`hydra.run.dir`参数轻松设置。

```py
python train.py data.sample_rate=4000 hydra.run.dir=“outputs/sample_rate_4000”
```

# 最后的思考

Hydra 是建立在 OmegaConf 之上的一个框架，它让我们可以非常轻松地管理实验。一旦我们实现了代码和管道，我们的任务就是跟踪模型的进展并根据需要更改配置。

在这篇文章中，我们看到了如何使用 Hydra 的实际例子，但还有许多更高级的功能我们没有涉及，例如启动多次运行以并行进行多个实验。为此，我建议你去阅读文档。如果你对这篇文章感兴趣，可以在 Medium 上关注我！😄

💼 [Linkedin](https://www.linkedin.com/in/marcello-politi/) ️| 🐦 [Twitter](https://twitter.com/_March08_) | [💻](https://emojiterra.com/laptop-computer/) [Website](https://marcello-politi.super.site/)
