# 使用 Transformer 的序列建模示例

> 原文：[`towardsdatascience.com/an-example-of-sequence-modelling-with-transformer-7d4c7dc85fc3`](https://towardsdatascience.com/an-example-of-sequence-modelling-with-transformer-7d4c7dc85fc3)

## NLP 深度学习

## 一些关于 transformer 的概念以及如何使用 transformer 构建序列模型的示例

[](https://medium.com/@jhwang1992m?source=post_page-----7d4c7dc85fc3--------------------------------)![Jiahui Wang](https://medium.com/@jhwang1992m?source=post_page-----7d4c7dc85fc3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7d4c7dc85fc3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7d4c7dc85fc3--------------------------------) [Jiahui Wang](https://medium.com/@jhwang1992m?source=post_page-----7d4c7dc85fc3--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----7d4c7dc85fc3--------------------------------) ·5 分钟阅读·2023 年 2 月 10 日

--

![](img/7516b578384db3e4826b2bf637a5ba5f.png)

照片由[Patrick Tomasso](https://unsplash.com/@impatrickt?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

用户行为序列数据可以用来预测用户的个人信息和偏好。例如，我们可以通过用户的历史购买记录来了解用户的性别，或者根据用户的还款历史来预测用户是否会偿还贷款。

在这篇文章中，我们将尝试通过对手机上应用安装序列建模来预测用户的财务风险水平。在这里，我们将应用安装序列建模任务视为一个 NLP 问题。用户安装的应用形成一个语料库。每个应用类似于一个词，而每个用户的应用安装序列类似于 NLP 问题中的一个句子。为了解决这个应用安装序列建模任务，我们选择使用带有 transformers 的神经网络。

在这篇文章中，将涵盖以下内容：

+   Transformers 及其有效性

+   神经网络与 Transformers 在 Spark 上的开发和部署

# 1\. Transformers 及其有效性

自从 Google 发布了论文[***Attention Is All You Need***](https://arxiv.org/abs/1706.03762)以来，变压器在 NLP 问题中得到了广泛应用。注意力模块使得变压器与 RNN 结构有所不同。Ketan Doshi 在**Medium 上有一系列精彩的帖子**很好地解释了变压器和注意力机制，包括数学和视觉方面。如果你感兴趣，不要忘了查看他的系列帖子。在这里，我将总结一些我认为特别有助于理解变压器和注意力机制的关键要点。

1.  在每个注意力模块中，嵌入输入经过三个线性变换形成 Query、Key 和 Value。这三个线性变换通过矩阵乘法实现，其中变换矩阵包含可训练的参数。在训练过程中，这些可训练的参数将被调整，以使输入嵌入与优化的预测输出对齐。

1.  注意力模块中的注意力分数是 Query 和 Key 之间的点积得到 Query-Key 分数，然后再计算 Query-Key 和 Value 之间的点积。通过使用注意力分数，每个词都会关注输入序列中的所有其他词。对于每个词，计算到输入序列中所有其他词的注意力分数，较大的注意力分数意味着更高的相关性。

1.  变压器结构有许多变种。包含编码器堆栈和解码器堆栈的变压器可以处理序列到序列的 NLP 问题，而包含编码器堆栈后接分类层的变压器可以用于分类任务。

1.  在输入序列传递给变压器之前，输入序列将经过嵌入层和位置编码层。嵌入层包含了输入的语义，而位置编码层提取了输入词的位置信息。与 RNN 不同的是，RNN 中保留了输入序列的位置信息，而注意力机制并行计算注意力分数，而不考虑位置信息。因此，位置编码层有助于重新添加位置信息。

# 2\. 使用变压器的神经网络代码示例

在本次会议中，我们将详细讲解应用序列建模的整个过程。这里使用的变压器参考了由 TensorFlow 发布的[**语言理解的变压器模型教程**](https://www.tensorflow.org/text/tutorials/transformer)和由 Keras 发布的[**使用变压器进行文本分类教程**](https://keras.io/examples/nlp/text_classification_with_transformer/)。

这个示例中使用的输入是 Pandas DataFrame df_app_sequence，该 DataFrame 有三列：userid、label 和 app_sequence。

以下代码展示了应用序列建模的整个过程。

## 1\. 分词和序列填充

app_sequence 是一个包含安装的 app_id 的字符串列表。由于神经网络不接受字符串输入，我们首先需要做的是对 app_sequence 进行分词，并将每个 app_id 从字符串转换为整数。这里的 NUM_APPS 是语料库中的应用程序数量。

分词后，我们需要将每个用户安装的应用序列填充到相同的长度。这里 MAX_LENGTH 是一个超参数。长度超过 MAX_LENGTH 的应用序列将被截断，而长度短于 MAX_LENGTH 的应用序列将在末尾填充零。

## 2\. 定义编码器层

在这篇文章发布时，`tensorflow.keras.layers.MultiHeadAttention` 仅在 2.4.0 以上版本的 TensorFlow 中得到支持。在这里，我们将实现由 [语言理解 Transformer 模型教程](https://www.tensorflow.org/text/tutorials/transformer) 提供的 MultiHeadAttention。

缩放点积函数：

MultiHeadAttention 函数：

点对点前馈层：

编码器层：

编码器：

## 3\. 定义神经网络

在神经网络中，我们实现了 transformer 编码器的堆叠，后面跟着一系列分类层。

神经网络结构是：

![](img/ecc0c0fca986b6ca6043cce360752d72.png)

神经网络结构

代码如下：

## 4\. 训练和评估

使用了 Adam 优化器。训练完成后可以检查训练情况。

## 5\. 保存和重新加载模型

由于模型包含自定义对象（编码器层），我们选择将模型保存为 TensorFlow SavedModel 格式。TensorFlow SavedModel 格式和 Keras 模型都可以用来保存包含自定义对象的模型，不过我们发现 TensorFlow SavedModel 格式更简单直接。如果你有兴趣了解 TensorFlow SavedModel 和 Keras 模型在保存自定义对象模型上的区别，我建议你查看 [**Murat Karakaya 的文章**](https://medium.com/deep-learning-with-keras/save-load-keras-models-with-custom-layers-8f55ba9183d2)。

在这里，模型通过 `tf.function` 保存为 TensorFlow SavedModel。

模型可以重新加载并用于预测。

## 6\. 在 Spark 上部署

在模型训练期间，我们使用的是单 GPU 服务器上的 Pandas DataFrame。然而，当涉及到批量推断的模型部署时，我们希望在 Spark 上部署训练好的 TensorFlow 模型，并在分布式 Spark 环境中对大规模数据集进行推断。

在 Spark 上部署带有自定义对象（编码器层）的 TensorFlow 模型的关键挑战是如何序列化训练好的模型。我找到了一篇 [**有用的文章**](https://github.com/tensorflow/tensorflow/issues/31421) 解决这个问题。

解决方案是先将训练好的模型库从本地路径上传到 HDFS。

然后，使用 SparkFiles 将 HDFS 模型库上传到集群中的每个工作节点。通过设置 recursive=True，可以上传整个库。

第三步，封装预测函数。

最终，在 Pandas UDF 函数中调用 get_model_prediction 函数，Spark 上的推断就完成了！

## 总结

在这篇文章中，我们涵盖了：

1.  关于 Transformer 的一些关键概念。

1.  一个使用 Transformer 构建序列模型的示例。

1.  如何在 Spark 上部署序列模型进行批量推断。

希望你喜欢阅读！
