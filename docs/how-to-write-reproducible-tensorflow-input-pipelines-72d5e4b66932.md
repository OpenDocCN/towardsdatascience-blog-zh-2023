# 如何编写可重复的 TensorFlow 输入管道

> 原文：[`towardsdatascience.com/how-to-write-reproducible-tensorflow-input-pipelines-72d5e4b66932`](https://towardsdatascience.com/how-to-write-reproducible-tensorflow-input-pipelines-72d5e4b66932)

## 通过使用种子来修复输入顺序

[](https://pascaljanetzky.medium.com/?source=post_page-----72d5e4b66932--------------------------------)![Pascal Janetzky](https://pascaljanetzky.medium.com/?source=post_page-----72d5e4b66932--------------------------------)[](https://towardsdatascience.com/?source=post_page-----72d5e4b66932--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----72d5e4b66932--------------------------------) [Pascal Janetzky](https://pascaljanetzky.medium.com/?source=post_page-----72d5e4b66932--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----72d5e4b66932--------------------------------)·阅读时间 6 分钟·2023 年 1 月 9 日

--

在准备机器学习实验时，输入管道在数据准备中起着关键作用。虽然构建它们通常很简单 - 机器学习框架使这相对容易 - 但它们缺乏可重复性。

![](img/9f8ae83ad000df4f9014a188dab704b4.png)

照片由[Quinten de Graaf](https://unsplash.com/@quinten149?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

这是默认设置：输入数据中的随机性，例如在每个时期后应用洗牌，已被证明在神经网络训练中起着重要作用。因此，“启用”默认的随机性是一个明显的选择。然而，当我们想要更深入地了解我们的训练时，我们希望尽可能保持许多影响/参数固定。输入顺序就是其中之一。

修复输入顺序是否意味着我们必须完全放弃洗牌或其他随机化操作？不，幸运的是不必如此。让我解释一下为什么。在处理随机性时，我们有一个选择：设置引入随机性的操作的种子。机器学习框架从这个种子中推断出任何其他操作及其顺序，通常是一个整数，如 42、111 或 1337。

在这里，想象一下从列表中随机选择一个数字。当此操作未设置种子时，每次运行抽取时我们都会得到不同的数字。然而，如果我们设置一个固定的种子，我们将获得可重复的结果。这个例子很简单，但在机器学习框架的背后，这就是发生的事情。因此，在创建可重复的输入管道时，我们必须专注于设置种子。

## 全局种子

在本文中，我将使用 TensorFlow 作为首选的机器学习框架，但这个要点也适用于其他任何包。在 TensorFlow 中，我们有两个可以自定义的地方。第一个地方是全局种子。

要设置全局种子，TensorFlow 提供了两种方法，[*set_seed*](https://www.tensorflow.org/api_docs/python/tf/random/set_seed)和[*set_random_seed*](https://www.tensorflow.org/api_docs/python/tf/keras/utils/set_random_seed)，我更喜欢使用后者，因为它还设置了其他标准包的随机种子。无论如何，如果我们只指定全局种子，每当调用一个基于随机性的操作（如 shuffle）时，包都会从这个全局种子派生一个所谓的操作级种子。在相同的系统和相同的版本上，这是可重现的。然而，在不同的计算环境和 TensorFlow 版本中，派生的种子可能会有所不同。要改变这一点，我们还需要设置操作级种子。

## 操作级种子

我们如何做到这一点呢？根据我的经验，我们需要传递种子的地方可以通过阅读函数文档或该函数的可能参数来轻松识别。以[shuffle 操作](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#shuffle)为例。这个操作对一个 TensorFlow 数据集进行操作，并随机打乱其元素。如果我们阅读相应的文档（或检查函数的参数），我们会注意到它包含一个叫做*seed*的参数；这就是我们要找的。现在，我们只需在这里设置一个任意的整数，并*保持不变*（这很重要！）。

在设置了操作级种子之后，我们可以对所有其他提供*seed*参数的操作进行相同的设置。那么问题来了，我们如何确保我们的种子策略有效呢？例如，我们如何知道我们总是以相同的顺序获得相同的数据样本？

为了回答这个问题，我准备了一个简单的 Python 脚本。这个脚本可以在[GitHub 上找到](https://github.com/phrasenmaeher/reproducible_pipelines)，它从 TensorFlow 的数据集合[TensorFlow Datasets](https://www.tensorflow.org/datasets/catalog/overview#all_datasets)（简称 TFDS）中加载一个数据集，应用用户定义的预处理流程，并打印元素的顺序。这里有一个注意事项：对于从 TFDS 加载的所有数据集，我们可以使用样本的唯一 ID（在其对应的数据集中）来跟踪其在多个流水线运行中的顺序。对于自定义数据集，这样的 ID 不一定存在。不过，这很容易添加。例如，如果你将 NumPy 数组加载到 TF 数据流水线中，不要传递元组（x，y），而是传递元组（x，y，id），其中 id 列对应于*x*的标识号。

另外，如果你使用 TFRecord 文件，过程也是类似的。在这里，当创建一个[*tf.train.Example*](https://www.tensorflow.org/api_docs/python/tf/train/Example)时，添加一个额外的特征。在我的 TFRecords 实践指南中，可以在定义示例字典的地方添加用户定义的 ID。

在我们加载包含样本 ID 的数据集之后，我们可以打印 ID 来查看排序。在 TensorFlow 中，一个 ID 看起来像这样：‘mnist-train.tfrecord-00000-of-00001__209’。不想让你感到无聊的是，我们可以通过一个小的实用函数（[来自 TensorFlow 文档](https://www.tensorflow.org/datasets/determinism#finding_the_dataset_examples_ids)）来从这个字符串中推断出实际的 ID。

## 不可复现的流水线

为了说明目的，默认情况下，我的脚本使用了小的 MNIST 数据集。它四次加载数据集，使用了一个故意很小的批次大小为四个样本（这样可以更容易地比较样本的顺序）。前两次调用不使用全局和操作级别的种子，说明了当我们不关心数据顺序时会发生什么：

我已经打印了截断的输出如下：

```py
First call:
[209, 329, 361, 133, 414, 171, 194, 176, 116, 485, 260, 297, 320, 179, 189, 199, 236, 112, 35, 135]
Loaded non-reproducible dataset in 0.16 seconds

Second call:
[304, 507, 323, 483, 207, 171, 473, 64, 90, 119, 494, 153, 35, 436, 71, 259, 307, 464, 173, 22]
Loaded non-reproducible dataset in 0.08 seconds
```

在这里，我们可以看到排序和实际选择的元素是不同的。注意：出于可读性的原因，我只打印了 5 个数据批次的元素（=20 个项目；4 [批次大小] x 5 [批次数]）。你可以通过在前面的代码的第 5 行和第 13 行传递任何其他正整数来增加计数。

## 可复现的流水线

在前面两次运行之后，让我们看看在使用种子时会发生什么。为此，我们调用*run_pipeline_reproducible*：

在这个函数内部，我们调用*seed_everything*方法，这个方法会为所有引入随机性的包（如 NumPy、random、TensorFlow 本身）设置全局种子，利用了前面提到的*set_random_seed*方法：

在设置了种子之后，我们再次调用数据流水线，这次传递*reproducible=True*。

这个参数会传递给所有的数据修改方法，比如下面的*load_data*和“create_pipeline”（请参阅博客末尾的附录）。

“可复现”标志在可能的情况下设置操作级别的种子，使用一个任意选择的整数。在脚本中，种子用于从 TFDS 中的 shuffle 和*.load()*操作。设置了这个数字之后，输出表明我们确实建立了一个可复现的数据流水线：

```py
First call:
[412, 48, 507, 226, 104, 304, 262, 199, 84, 512, 263, 486, 300, 520, 134, 381, 7, 275, 149, 240]
Loaded reproducible dataset in 0.08 seconds
Second call:
[412, 48, 507, 226, 104, 304, 262, 199, 84, 512, 263, 486, 300, 520, 134, 381, 7, 275, 149, 240]
Loaded reproducible dataset in 0.08 seconds
```

关键是，即使我们关闭 IDE/shell 并重新运行脚本，输出顺序也是相同的。

这篇博客文章的总结很简单：设置全局种子和操作级种子。为此，请使用之前展示的*seed_everything()*函数，并寻找可以传递种子的地方。在这些地方，给一个任意的整数 — 我最喜欢的是 42 和 1337 — 然后保持它们。就是这样。

要自己运行流水线，请前往 GitHub [这里](https://github.com/phrasenmaeher/reproducible_pipelines)。

## 附录

如果你想尝试其他数据类型，比如音频，你需要采用以下函数中的第 4 行和第 10 行：

同样，如果你想添加更多的数据处理操作，比如增强操作，同样修改*create_pipeline*函数。在这些情况下，只要可以设置种子就设置一个。这样可以保证，例如，数据增强是可重现的。

请看下面的片段以便理解：

默认情况下，增强操作是没有种子的（等同于传递*seed=None*）。因此，为了实现可重现性，在这些地方设置一个种子。
