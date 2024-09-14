# 用 Python 微调大型语言模型

> 原文：[`towardsdatascience.com/fine-tune-a-large-language-model-with-python-b1c09dbc58b2`](https://towardsdatascience.com/fine-tune-a-large-language-model-with-python-b1c09dbc58b2)

![](img/1d8727d8fba25a5fd2ed429d267bf0c1.png)

[Manouchehr Hejazi](https://unsplash.com/@patrol?utm_source=medium&utm_medium=referral) 的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 学习如何在自定义数据集上从头开始微调 BERT

[](https://medium.com/@marcellopoliti?source=post_page-----b1c09dbc58b2--------------------------------)![Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----b1c09dbc58b2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b1c09dbc58b2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b1c09dbc58b2--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----b1c09dbc58b2--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b1c09dbc58b2--------------------------------) ·4 分钟阅读·2023 年 4 月 18 日

--

在这篇文章中，我们将处理使用 PyTorch 的**BERT 情感分类微调**。BERT 是一个大型语言模型，提供了受欢迎程度与模型大小之间的良好平衡，可以**使用简单的 GPU**进行微调。我们可以从 Hugging Face (HF) 下载**预训练的 BERT**，因此无需从头开始训练。特别是，我们将使用名为**Distil-BERT**的精简（更小）版本。

[Distil-BERT](https://huggingface.co/docs/transformers/model_doc/distilbert#:~:text=DistilBERT%20is%20a%20small%2C%20fast,the%20GLUE%20language%20understanding%20benchmark.) 在生产中广泛使用，因为它比 BERT uncased**少了 40% 的参数**。它运行**快 60% 并保持 95% 的性能**在 [GLUE](https://arxiv.org/abs/1804.07461) 语言理解基准中。

我们首先安装所有必要的库。第一行是捕获安装输出，并保持你的笔记本整洁。

我将使用 Deepnote 来运行本文中的代码，但如果你更喜欢，也可以使用 Google Colab。

你还可以使用以下代码行检查你正在使用的库的版本。

现在你需要指定一些常规设置，包括训练轮次和设备硬件。我们设置了一个固定的随机种子，以帮助实验的可重复性。

## 加载 IMDb 电影评论数据集

让我们来看一下如何准备和标记 IMDb 电影评论数据集，并微调 Distilled BERT。获取压缩数据并解压缩它。

和往常一样，我们需要将数据拆分为训练集、验证集和测试集。

# 标记数据集

让我们使用从预训练模型类继承的分词器实现**将文本分词为单个单词标记**。

> 分词化用于自然语言处理，将段落和句子分割成更小的单位，以便更容易赋予其意义。

**通过 Hugging Face，你将总是能找到与每个模型相关联的分词器**。如果你不是在研究或实验分词器，通常更推荐使用标准分词器。

**填充**是一种通过向较短的句子添加特殊填充标记来确保张量是矩形的策略。另一方面，有时序列可能太长，模型无法处理。在这种情况下，你需要将序列截断为较短的长度。

让我们将所有内容打包到一个名为 IMDbDataset 的 Python 类中。我们还将使用这个自定义数据集来创建相应的数据加载器。

encodings 变量存储了大量关于分词文本的信息。我们可以通过字典推导提取出最相关的信息。字典包含：

+   **input_ids**：是句子中每个标记对应的索引。

+   **标签**：类别标签

+   **attention_mask**：指示一个标记是否应该被关注（例如填充标记将不会被关注）。

让我们构建数据集及相应的数据加载器。

## 加载和微调 BERT

最后，我们完成了数据预处理，可以开始微调我们的模型。我们来定义一个模型和一个优化算法，这里使用的是 Adam。

*DistilbertForSequenceCLassification* 指定了我们希望微调模型的下游任务，这在这里是序列分类。注意，“uncased”意味着模型不区分大小写字母。

在训练模型之前，我们需要定义一些指标来比较模型的改进。在这个简单的例子中，我们可以使用传统的分类准确率。请注意，这个函数相当长，因为我们按批次加载数据集以绕过 RAM 和 GPU 限制。通常，在微调大型数据集时，这些资源总是不够的。

在 compute_accuracy 函数中，我们加载一个给定的批次，然后从输出中获取预测标签。在此过程中，我们通过变量 num_examples 跟踪示例的总数。同样，我们通过 correct_pred 变量跟踪正确预测的数量。在迭代完整个数据加载器后，我们可以通过最后的除法计算准确率。

你还可以注意到如何在 compute_accuracy 函数中使用模型。我们将 input_ids 以及指示标记是否为实际文本标记或填充的 attention_mask 信息传递给模型。模型返回一个 SequenceClassificatierOutput 对象，我们从中获取 logits 并通过 argmax 函数将其转换为类别。

## 训练（微调）循环

如果你知道如何在 PyTorch 中编写训练循环，你就不会有任何问题理解这个微调循环。像任何神经网络一样，我们给网络输入数据，计算输出，计算损失，并根据这个损失进行参数更新。

每经过几个周期，我们就打印训练进度以获得反馈。

# 最终思考

在这篇文章中，我们已经学习了如何仅使用 PyTorch 对像 BERT 这样的预训练大语言模型进行微调。实际上，有一种更快、更智能的方法可以通过 Hugging Face 的 Transformers 库来实现。这个库允许我们创建一个用于微调的 Trainer 对象，我们可以在几行代码中指定如周期数等参数。如果你对下一篇文章中的实现感到好奇，跟随我吧！[😉](https://emojipedia.org/it/apple/ios-15.4/faccina-che-fa-l-occhiolino/)

# 结束

*Marcello Politi*

[领英](https://www.linkedin.com/in/marcello-politi/)，[推特](https://twitter.com/_March08_)， [网站](https://marcello-politi.super.site/)
