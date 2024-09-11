# 简化变换器：使用你理解的词汇的最先进的NLP — 第5部分 — 解码器与最终输出

> 原文：[https://towardsdatascience.com/simplifying-transformers-state-of-the-art-nlp-using-words-you-understand-part-5-decoder-and-cd2810c6ad40?source=collection_archive---------7-----------------------#2023-10-05](https://towardsdatascience.com/simplifying-transformers-state-of-the-art-nlp-using-words-you-understand-part-5-decoder-and-cd2810c6ad40?source=collection_archive---------7-----------------------#2023-10-05)

## Transformer系列的最后一部分

[](https://medium.com/@chenmargalit?source=post_page-----cd2810c6ad40--------------------------------)[![Chen Margalit](../Images/fb37720654b3d1068b448d4d9ad624d5.png)](https://medium.com/@chenmargalit?source=post_page-----cd2810c6ad40--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cd2810c6ad40--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cd2810c6ad40--------------------------------) [Chen Margalit](https://medium.com/@chenmargalit?source=post_page-----cd2810c6ad40--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff8e6113b0479&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimplifying-transformers-state-of-the-art-nlp-using-words-you-understand-part-5-decoder-and-cd2810c6ad40&user=Chen+Margalit&userId=f8e6113b0479&source=post_page-f8e6113b0479----cd2810c6ad40---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cd2810c6ad40--------------------------------) · 6 分钟阅读 · 2023年10月5日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcd2810c6ad40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimplifying-transformers-state-of-the-art-nlp-using-words-you-understand-part-5-decoder-and-cd2810c6ad40&user=Chen+Margalit&userId=f8e6113b0479&source=-----cd2810c6ad40---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcd2810c6ad40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimplifying-transformers-state-of-the-art-nlp-using-words-you-understand-part-5-decoder-and-cd2810c6ad40&source=-----cd2810c6ad40---------------------bookmark_footer-----------)

由于这是一个持续更新的系列，如果你还没有开始，可能需要考虑从之前的部分开始：[第一部分](/transformers-part-1-2a2755a2af0e)，[第二部分](/transformers-part-2-input-2a8c3a141c7d)，[第三部分](/transformers-part-3-attention-7b95881714df)，以及[第四部分](/simplifying-transformers-state-of-the-art-nlp-using-words-you-understand-part-4-feed-foward-264bfee06d9)。

![](../Images/91777af6677623aba65cadd069389647.png)

[来自原始论文的图片。](https://proceedings.neurips.cc/paper_files/paper/2017/file/3f5ee243547dee91fbd053c1c4a845aa-Paper.pdf)

本系列的第4部分将 heavily 基于 [第2部分](https://medium.com/towards-data-science/transformers-part-2-input-2a8c3a141c7d)、[第3部分](https://medium.com/towards-data-science/transformers-part-3-attention-7b95881714df) 和 [第4部分](https://medium.com/towards-data-science/simplifying-transformers-state-of-the-art-nlp-using-words-you-understand-part-4-feed-foward-264bfee06d9)，因此如果你还没有阅读这些内容，并且不确定上面图像中架构的左侧如何工作，我建议你从这些部分开始。你发现的不清楚或未解释的术语表可能在之前的部分中已经讲解过。

**解码器**

Transformers 是一种编码器-解码器架构。输入进入后，被编码（转换）成数学表示（以某种形式的数字，通常是向量）。然后，它被传送到另一个处理单元，称为解码器，在那里它从数字转换回请求的输出。在语言模型的情况下，这将是一个单词。

解码器的第一个重要任务是从目标序列（我们想要给出的答案）中创建值矩阵。它接收整个目标序列，将其转换为嵌入（Embeddings），并以与编码器相同的方式添加位置编码。然后，它会将嵌入传递给一个掩码多头注意力层（Masked Multi-Head Attention layer），该层将创建值矩阵。这个矩阵将帮助模型决定用户提示和预期目标如何配合。

在解释 Transformers 如何工作的过程中存在一种悖论，因为你需要了解这些组成部分才能理解最终结果的发生方式，但你需要理解最终结果的发生方式才能理解这些组成部分。考虑到这一悖论，我们将短暂展望未来，并解释 Transformer 训练的两个方面：

首先，在编码器中，用户提示被转换为嵌入，然后我们添加位置编码。编码器组（原始论文中为6个）处理数据并生成文本的数值表示。接下来，在解码器中，真实值（我们希望模型回应的内容）会被前置一些标记，表示这是句子的第一个标记。类似于<BOS>（句子开始），但它可以是模型训练的任何其他符号。这个输入被转换为嵌入，并添加位置编码。解码器组（最初也是6个）将这些向量与编码器的输出一起生成新的词表示。该词表示被转换为概率分布，从模型的整个数据集中选择概率最高的词。最后，根据模型选择的词与模型的真实值之间的差距计算损失函数。该损失用于生成对反向传播（计算权重如何根据各自对总体误差的贡献进行调整的算法）重要的梯度。

现在我们了解了整体流程，让我们看看一个小但重要的细节：一种叫做**教师强迫**的技术。

**教师强迫** 想象一个数学测试，你有3个作业：

1\. 取数字4，加上5，并保持得分。

2\. 从练习1中得到结果，乘以2，并保持得分。

3\. 从练习2中得到结果，除以2。

你将根据每个练习的结果单独评分，无论对错。你看到了问题吗？如果你在练习1中犯了错误，那么练习2和3也会有错误。**教师强迫**技术可以处理这个问题。由于语言模型也是基于序列的（例如，预测第二个词依赖于第一个词），为了在未来做出正确的预测，你必须在之前做出正确的预测。在我们的例子中，**教师强迫**是给你练习2中的正确答案，然后在练习3中也给你正确答案，这样你实际测试的是乘法/除法而不是加法。你仍然会在每个练习上单独评分，你只是不必因为练习1中的错误而在练习2中受苦。

具体来说，在我们做的事情中，**教师强迫**帮助我们更快地训练。我们在每一步都给模型真实标签，确保第x个词的预测。例如，第4个词将基于3个真实（正确）标签，而不是基于可能是错误的预测标签，这样模型就可以正确地继续进行，而不是因为之前的错误。

好的，我们现在掌握了重要部分。回到正题。在转换为嵌入并添加位置编码后，我们将输入通过**掩蔽多头注意力**。

**掩蔽多头注意力** 我们之前已经了解了注意力层的功能及其存在的原因。掩蔽注意力层与之前的注意力层非常相似，用于相同的原因，但有一个重要的区别。由于解码器处理的是完整的期望输出，因此模型在构建注意力分数时可以使用整个序列。由于这些注意力分数是关键部分，我们必须确保它们的准确性。假设我需要生成句子：“惊人的技术，我喜欢它”。在当前阶段，我们试图预测单词 technology。单词 technology 在不同上下文中可能有不同的表示，但在推断时（即在实际使用模型时），模型不会拥有整个句子，只会有之前的单词。因此，我们需要确保训练时也仅能访问之前预测的单词，而不能访问未来的单词。为此，我们通过掩蔽（隐藏）未来单词来实现这一点。

正如你可能猜到的，由于机器学习深深根植于数学中，我们不会简单地删除单词，而是有一种更高级的做法。我们将数学转换为被动攻击模式。我们忽略它。具体来说，在计算注意力分数时，我们添加负无穷大（一个非常小的负数，例如 -86131676513135468），这会使下一阶段的 softmax 将这些负数转换为 0。这一重要技术确保模型在没有访问下一个单词时无法使用它。

![](../Images/e65a9446715016e8668ae7d0486d6856.png)

图片由作者提供

计算掩蔽注意力分数后，输入通过一个添加和归一化层，方式和原因与我们之前解释的相同。它还接收来自注意力计算前一层的跳跃连接。之后，值矩阵将继续进入下一阶段。我们现在从编码器中获取 Q（查询）和 K（键）矩阵，这些矩阵表示用户提示和对该查询的可能建议。解码器带来了自己的值矩阵，以决定关注编码器输入的哪个部分。结合这三个矩阵（两个来自编码器，一个来自解码器），我们计算“常规”注意力分数。

接下来，我们有另一个前馈 + 添加和归一化层，它接收另一个跳跃连接，就像我们[之前解释过的](https://medium.com/p/264bfee06d9/edit)那样，然后……我们完成了解码器！

我们现在来到了最后一步。堆叠解码器中的最后（第6个）解码器将其输出通过一个线性层。这个线性层使我们能够生成任意数量的数值表示。在语言模型中，我们希望表示的数量与词汇表的大小匹配。如果模型的整个词汇表（它所见过的所有唯一单词）有1000个，那么我们希望有1000个数字来表示词汇表中的每一个可能的单词。我们对每个位置的每个单词都这样做。如果最终输出包含10个单词，我们将为这10个单词中的每一个计算1000个数字。然后，我们将其传递给Softmax层，Softmax层为每个单词提供一个概率，概率最高的单词就是我们将使用的那个。Softmax返回一个索引，例如3。模型输出词汇表中的索引3，并得到新的预测单词。如果我们的词汇表是[‘a’, ‘man’, ‘works’, ‘nice’, ‘go’]，那么选择的单词将是‘nice’。

就是这样……完成了！你现在对变压器架构有了全面的了解。这是一次相当长的旅程，你勇敢地完成了它。希望这个系列能帮助你理解这个复杂但又易于理解且重要的主题。

祝你度过愉快的时光。
