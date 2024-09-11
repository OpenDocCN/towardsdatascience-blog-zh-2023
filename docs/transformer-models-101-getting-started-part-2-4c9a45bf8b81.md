# Transformer 模型 101：入门指南 — 第2部分

> 原文：[https://towardsdatascience.com/transformer-models-101-getting-started-part-2-4c9a45bf8b81?source=collection_archive---------13-----------------------#2023-06-06](https://towardsdatascience.com/transformer-models-101-getting-started-part-2-4c9a45bf8b81?source=collection_archive---------13-----------------------#2023-06-06)

## Transformer 模型背后的复杂数学，用简单的词汇解释

[](https://nandinibansal1811.medium.com/?source=post_page-----4c9a45bf8b81--------------------------------)[![Nandini Bansal](../Images/a813ac8be6f89b2e856651fda66ab078.png)](https://nandinibansal1811.medium.com/?source=post_page-----4c9a45bf8b81--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4c9a45bf8b81--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4c9a45bf8b81--------------------------------) [Nandini Bansal](https://nandinibansal1811.medium.com/?source=post_page-----4c9a45bf8b81--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc41c26af0465&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransformer-models-101-getting-started-part-2-4c9a45bf8b81&user=Nandini+Bansal&userId=c41c26af0465&source=post_page-c41c26af0465----4c9a45bf8b81---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4c9a45bf8b81--------------------------------) · 7 分钟阅读 · 2023年6月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4c9a45bf8b81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransformer-models-101-getting-started-part-2-4c9a45bf8b81&user=Nandini+Bansal&userId=c41c26af0465&source=-----4c9a45bf8b81---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4c9a45bf8b81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransformer-models-101-getting-started-part-2-4c9a45bf8b81&source=-----4c9a45bf8b81---------------------bookmark_footer-----------)![](../Images/b598c04052b566c5fdd8e24dde8376ae.png)

图片由[Dariusz Sankowski](https://pixabay.com/users/dariuszsankowski-1441456/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1052010)提供，来源于[Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1052010)

在上一篇文章中，我们详细介绍了 Transformer 模型的编码器（Encoder）块的工作原理。如果你还没有阅读那篇文章，我建议你在阅读本文之前先阅读它，因为那里的概念在本文中有所延续。你可以前往：

[](/transformer-models-101-getting-started-part-1-b3a77ccfa14d?source=post_page-----4c9a45bf8b81--------------------------------) [## 变换器模型 101：入门 — 第 1 部分

### 用简单的语言解释变换器模型背后的复杂数学...

towardsdatascience.com](/transformer-models-101-getting-started-part-1-b3a77ccfa14d?source=post_page-----4c9a45bf8b81--------------------------------)

如果你已经阅读了这些内容，太棒了！让我们开始深入了解解码器块及其相关的复杂数学。

## 变换器的解码器

像变换器模型的编码器块一样，解码器块由 N 个堆叠的解码器组成，这些解码器按顺序工作，并接受来自前一个解码器的输入。然而，这并不是解码器唯一接受的输入。由编码器块生成的句子表示会被传递给解码器块中的每一个解码器。因此，我们可以得出结论，每个解码器接受两个不同的输入：

+   来自编码器块的句子表示

+   之前解码器的输出

![](../Images/1144b091a8021ae2198f3598dc9f9afe.png)

图 1\. 编码器和解码器块的协同工作（图片由作者提供）

在我们进一步探讨解码器的不同组件之前，了解解码器一般是如何生成输出句子或目标句子的直观感受是很重要的。

## 目标句子是如何生成的？

在时间步 t=1 时，只有 <sos> 标记或 ***句子开始*** 被传递给解码器块作为输入。基于 <sos>，解码器块生成目标句子的第一个单词。

在下一个时间戳，即 t=2，解码器块的输入包括 <sos> 标记以及解码器块生成的第一个单词。下一个单词是基于这个输入生成的。

类似地，每次时间戳增加时，解码器块的输入长度也会增加，因为在前一个时间戳生成的单词会被添加到当前输入句子中。

当解码器块完成整个目标句子的生成时，会生成 <eos> 或 ***句子结束*** 标记。

你可以将其视为一个递归过程！

![](../Images/c4f83c9c11b7f68dfa0f7df38ef85dfc.png)

图 2 递归生成输出标记使用解码器（图片由作者提供）

现在，这就是当输入给定到变换器模型时我们期望的输出情况。然而，在训练/微调变换器模型时，我们已经在训练数据集中拥有目标句子。那么它是如何工作的呢？

这使我们接触到解码器的一个极其重要的概念：***掩蔽多头注意力***。听起来很熟悉？当然了。在之前的部分，我们理解了在编码器块中使用的 ***多头注意力*** 的概念。现在让我们了解这两者的不同之处。

## 掩蔽多头注意力

解码器块逐字生成目标句子，因此，模型必须以类似的方式进行训练，以便即使在有限的标记集下也能做出准确预测。

因此，正如名字所示，我们在计算自注意力矩阵之前会掩蔽句子右侧尚未预测的所有标记。这将确保自注意力机制仅考虑模型在每次递归预测步骤中将可用的标记。

让我们举一个简单的例子来理解它：

![](../Images/f70b358e28c8796b99a4b8f0b931c107.png)

图 3\. 掩蔽多头注意力矩阵表示（图像来源：作者）

计算自注意力矩阵的步骤和公式与我们在编码器块中做的相同。我们将在本文中高层次地覆盖这些步骤。欲了解更深入的内容，请随时前往本文系列的上一部分。

+   *为目标句子生成嵌入并获得* ***目标矩阵 Y***

+   *通过将随机权重矩阵* ***Wq、Wk 和 Wv*** *与目标矩阵 Y 相乘，将目标句子转换为* ***Q、K 和 V***

+   *计算* ***Q 和 K 的转置*** *的点积*

+   *通过将点积除以* ***平方根*** *的嵌入维度 (****dk****) 来缩放*

+   *通过将所有* ***<mask>*** *单元替换为* ***— inf*** *来对缩放矩阵应用掩蔽*

+   *现在* ***对矩阵应用 softmax 函数***，*并将其与* ***Vi 矩阵*** *相乘以生成注意力矩阵 Zi*

+   *将多个注意力矩阵 Zi 连接成一个单一的* ***注意力矩阵 M***

这个注意力矩阵将与编码器块生成的输入句子表示一起传递到解码器块的下一个组件。现在让我们理解这两个矩阵是如何被解码器块使用的。

## 多头注意力

解码器块的这个子层也被称为***“编码器-解码器注意力层”***，因为它接收**掩蔽注意力矩阵 (M)**和**由编码器生成的句子表示 (R)**。

![](../Images/42c44b268b7f30a8506a35f82570c2be.png)

图 4\. 多头注意力机制（图像来源：作者）

自注意力矩阵的计算与前一步骤中的计算非常相似，只是有一点小变化。由于这个层有两个输入矩阵，它们被转化为 Q、K 和 V，如下所示：

+   Q 是使用 Wq 和 M 生成的

+   K 和 V 矩阵是使用 Wk 和 Wv 与 R 生成的

到现在为止，你一定已经明白了变换器模型中的每一步和计算都有非常具体的理由。类似地，每个矩阵使用不同的输入矩阵生成也是有原因的。你能猜到吗？

***快速提示：答案在于自注意力矩阵是如何计算的…***

是的，你答对了！

如果你还记得，当我们通过一个输入句子理解自注意力概念时，我们谈到了它如何计算注意力分数，同时将源句子映射到自身。源句子中的每个词都与同一句子中的每个其他词进行比较，以量化关系并理解上下文。

在这里，我们也做着相同的事情，唯一的区别是，我们将输入句子（K-转置）的每个词与目标句子词（Q）进行比较。这将帮助我们量化这两个句子彼此的相似程度，并理解单词之间的关系。

![](../Images/a2c20a503086986041db5f326a124c28.png)

图 5\. 输入句子和目标句子的注意力矩阵表示（作者提供的图像）

最终，生成的注意力矩阵 Zi 的维度将是 N X 1，其中 N = 目标句子的词数。

由于这也是一个多头注意力层，为了生成最终的注意力矩阵，多个注意力矩阵被拼接在一起。

有了这些，我们已经涵盖了 Decoder 块的所有独特组件。然而，某些其他组件的功能与 Encoder 块中的相同。我们也简要看看它们：

+   ***位置编码*** — 就像编码器块一样，为了保留目标句子的词序，我们在将其输入到 Masked Multi-attention 层之前，将位置编码添加到目标嵌入中。

+   ***前馈网络*** — 这个解码器块中的子层是经典的神经网络，具有两个全连接层和 ReLU 激活。它接受来自多头注意力层的输入，对其进行一些非线性变换，最后生成上下文化的向量。

+   ***Add & Norm 组件*** — 这是一个*残差层*，后面跟着*层归一化*。它有助于加快模型训练，同时确保不会丢失来自子层的信息。

我们在[第 1 部分](/transformer-models-101-getting-started-part-1-b3a77ccfa14d)中详细介绍了这些概念。

有了这些，我们也总结了 Decoder 块的内部工作原理。正如你可能猜到的，编码器和解码器块用于处理和生成输入句子的上下文化向量。那么实际的下一个词预测任务由谁来完成呢？让我们找出答案。

## 线性与 Softmax 层

它位于 Decoder 网络的顶部，接受由堆栈中最后一个解码器生成的输出矩阵作为输入。这个输出矩阵被转换为与词汇表大小相同的 logit 向量。然后我们对这个 logit 向量应用 softmax 函数，以生成与每个词对应的概率。具有最高概率的词被预测为下一个词。该模型使用**Adam 优化器**进行交叉熵损失优化。

为了避免过拟合，**dropout 层**在每个编码器/解码器网络的子层后面添加。

这就是整个 Transformer 模型的全部内容。通过这个，我们已经用尽可能简单的语言完成了对 Transformer 模型架构的深入讲解。

## 结论

现在你已经了解了所有关于 Transformer 模型的内容，基于此构建你的知识并深入研究更复杂的 LLM 模型架构，如 BERT、GPT 等，不应该会很困难。

你可以参考以下资源：

1.  [https://huggingface.co/blog/bert-101](https://huggingface.co/blog/bert-101)

1.  [https://jalammar.github.io/illustrated-gpt2/](https://jalammar.github.io/illustrated-gpt2/)

希望这篇两部分的文章能让 Transformer 模型的理解变得稍微不那么令人生畏。如果你觉得这篇文章有用，请传播这个好消息。

下次见！
