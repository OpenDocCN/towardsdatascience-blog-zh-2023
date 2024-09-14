# GPT 模型的 Transformer 架构

> 原文：[`towardsdatascience.com/the-transformer-architecture-of-gpt-models-b8695b48728b`](https://towardsdatascience.com/the-transformer-architecture-of-gpt-models-b8695b48728b)

## 了解 Transformer 架构的详细信息

[](https://medium.com/@bea_684?source=post_page-----b8695b48728b--------------------------------)![Beatriz Stollnitz](https://medium.com/@bea_684?source=post_page-----b8695b48728b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b8695b48728b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b8695b48728b--------------------------------) [Beatriz Stollnitz](https://medium.com/@bea_684?source=post_page-----b8695b48728b--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b8695b48728b--------------------------------) ·阅读时间 22 分钟·2023 年 7 月 25 日

--

![](img/b27fc545e177f9112abf27cfac0ceebb.png)

图片由 [fabio](https://unsplash.com/@fabioha?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

2017 年，谷歌的作者们发布了一篇名为[《Attention is All You Need》](https://arxiv.org/abs/1706.03762)的论文，在其中引入了 Transformer 架构。这一新架构在语言翻译任务中取得了前所未有的成功，该论文很快成为任何从事这一领域的必读文献。像许多人一样，当我第一次阅读这篇论文时，我能看出其创新思想的价值，但没有意识到这篇论文对更广泛的 AI 领域会产生如此巨大的影响。在短短几年内，研究人员将 Transformer 架构应用于语言翻译之外的许多任务，包括图像分类、图像生成和蛋白质折叠问题。特别是，Transformer 架构革新了文本生成，为 GPT 模型和我们当前在 AI 领域经历的指数级增长铺平了道路。

鉴于 Transformer 模型在当前行业和学术界的广泛应用，理解它们的工作细节是每位 AI 从业者的重要技能。本文将主要关注 GPT 模型的架构，这些模型是使用原始 Transformer 架构的一个子集构建的，但最后也会涉及原始 Transformer。关于模型代码，我将从我找到的最清晰的原始 Transformer 实现开始：[哈佛大学的注释 Transformer](http://nlp.seas.harvard.edu/annotated-transformer/)。我将保留与 GPT transformer 相关的部分，移除不相关的部分。在此过程中，我会避免对代码做任何不必要的修改，以便你可以轻松地将 GPT 类似版本的代码与原始代码进行比较，理解它们的差异。

本文面向经验丰富的数据科学家和机器学习工程师。特别是，我假设你对张量代数非常熟悉，已经从头实现了神经网络，并且对 Python 使用自如。此外，尽管我尽力使这篇文章独立完整，如果你读过我之前关于 [GPT 模型如何工作的文章](https://bea.stollnitz.com/blog/how-gpt-works-technical/)，理解起来会更容易。

本文中的代码可以在相关的 [GitHub 项目](https://github.com/bstollnitz/gpt-transformer) 中找到。

## **如何调用我们的 GPT Transformer**

在我们深入了解如何构建 GPT 模型之前，先来理解如何调用它。我们暂时假设已经有一个可工作的 GPT 模型，重点讨论如何准备输入、调用模型以及解读输出。总体思路是提供几个单词作为输入来启动生成，并返回可能跟随该输入的文本。例如，如果我们给 GPT 模型输入“很久以前”，模型可能会返回“在一个遥远的星系”。

让我们看看用于调用模型的代码，传入输入“很久以前”并生成 10 个新词。我使用了注释来展示每个张量的形状。代码之后我会解释更多细节。

```py
import tiktoken

def tokenize(text, batch_size):
    """Convert text to numerical tokens and repeat batch_size times."""
    encoding = tiktoken.encoding_for_model("davinci")
    token_list = encoding.encode(text)
    token_tensor = torch.tensor(token_list, dtype=torch.long) # (input_seq_len)
    token_tensor = token_tensor.unsqueeze(0) # (1, input_seq_len)
    token_tensor = token_tensor.repeat(batch_size, 1) # (batch_size, input_seq_len)
    return encoding, token_tensor

def limit_sequence_length(input_tokens, block_size):
    """Limit the input to at most block_size tokens."""
    input_seq_len = input_tokens.size(1)
    seq_len = min(input_seq_len, block_size)
    block_tokens = input_tokens[:, -seq_len:] # (batch_size, seq_len)
    return block_tokens

def generate_next_token(model, tokens):
    """Use the highest probability from the Transformer model to choose the next token."""
    mask = subsequent_mask(tokens.size(1)) # (1, seq_len, seq_len)
    decoder_output = model.decode(tokens, mask) # (batch_size, seq_len, vocab_size)
    distribution = model.generator(decoder_output[:, -1, :]) # (batch_size, vocab_size)
    next_token = torch.argmax(distribution, dim=1, keepdim=True) # (batch_size, 1)
    return next_token

# Define constants.
input_text = "A long time ago"
new_token_count = 10
batch_size = 1
block_size = 1024

# Tokenize the input text.
encoding, tokens = tokenize(input_text, batch_size)

# Create the model.
model = make_model(encoding.n_vocab)

# Iterate until we've generated enough new tokens.
for _ in range(new_token_count):
    block_tokens = limit_sequence_length(tokens, block_size) # (batch_size, seq_len)
    next_token = generate_next_token(model, block_tokens) # (batch_size, 1)
    tokens = torch.cat([tokens, next_token], dim=1) # (batch_size, input_seq_len + 1)

# Print each of the generated token sequences.
print(tokens)
for row in tokens:
    print(encoding.decode(row.tolist()))
```

由于我们从字符串“A long time ago”开始，你可能会倾向于认为 Transformer 接收字符串作为输入。然而，和其他神经网络一样，Transformer 需要数值输入，因此输入字符串必须首先转换为一系列数字。我们在`tokenize`函数中使用一个*分词器*（在我们的示例中是来自 OpenAI 的`tiktoken`）进行转换，它将文本拆分为几字母的块，并为每个唯一的块分配一个称为*令牌*的数字。为了获得 Transformer 的正确输入，我们将令牌序列放入一个张量中，并扩展它以包含一个批量维度。这是因为，和其他类型的神经网络一样，Transformer 最有效的训练方式是使用批量处理，以利用 GPU 上的并行计算。我们的示例代码在一个序列上运行推断，因此我们的`batch_size`为一，但如果你想一次生成多个序列，可以尝试更大的数字。

在对输入进行分词后，我们使用`make_model`函数创建 Transformer 模型，稍后我们将详细讨论这个函数。你可能会认为调用模型会返回几个令牌作为输出，因为这通常是文本生成的场景。然而，Transformer 每次只能生成一个令牌。由于我们希望生成多个令牌，我们使用`for`循环多次调用模型，在每次迭代中，我们使用`torch.cat`将新生成的令牌附加到原始令牌序列中。

![](img/2949d05852f9bb6bd22a324497cd76c5.png)

GPT 风格的 Transformer 模型通常有一个明确定义的令牌限制：例如，`gpt-35-turbo`（Chat GPT）的限制为 4096 个令牌，而`gpt-4-32k`的限制为 32768 个令牌。由于我们将输入令牌和到目前为止生成的所有输出令牌连接传递给 Transformer 模型，模型的令牌限制指的是输入加输出令牌的总数。在我们的代码中，我们使用`block_size`常量定义这个令牌限制，并通过在`limit_sequence_length`函数中简单地将更长的令牌序列截断到最大支持长度来处理更长的序列。

我们在`generate_next_token`函数中通过调用`model.decode`和`model.generator`来调用 Transformer 模型，这对应于 Transformer 架构的两个主要部分。解码部分需要一个掩码，我们使用`subsequent_mask`函数创建这个掩码。我们将在本文后面详细分析这些函数。生成阶段返回一个概率分布序列，我们选择最后一个（稍后我们将看到原因），用它来预测下一个令牌。这个分布包含每个可能的令牌的概率值，表示该令牌在句子中接下来出现的可能性。

![](img/6b990788b66e90c73a5781e85742a804.png)

为了简化和提高示例代码的可读性，我们选择输出分布中概率最高的标记（使用 `torch.argmax`）。在实际的 GPT 模型中，下一个标记是通过从概率分布中进行采样来选择的，这会在输出中引入一些变异，使得文本感觉更加自然。如果你可以访问 Azure AI Studio 的“Completions playground”，你可能会注意到“Temperature”和“Top probabilities”滑块，这些滑块可以让你控制采样的方式。

```py
tensor([[   32,   890,   640,  2084,  3556, 48241, 26430, 34350, 28146, 43264,
          3556,  6787, 45859, 13884]])
A long time ago</ spaghetti Rapiddx Rav unresolved</ rail MUCHkeeper
```

在这篇文章中，我们将重点关注 Transformer 模型架构的代码，而不是模型训练的代码，因为这正是 Transformer 创新大部分所在的地方。最后我会给出一些训练模型的指引，以防你有兴趣扩展这段代码以生成更好的结果。

我们现在对 Transformer 模型的输入和输出有了很好的理解，以及如何实例化和调用它。接下来，我们将深入探讨模型本身的实现细节。

## **Transformer 架构概述**

让我们熟悉一下 GPT transformer 的高层架构：

![](img/3f05be2dbf9b40771180a956ce812d96.png)

在这个图示中，数据从下到上流动，这在 Transformer 插图中是传统的。最初，我们的输入标记经过几个编码步骤：首先使用嵌入层进行编码，然后是位置编码层，最后将这两种编码加在一起。接下来，我们的编码输入经过一系列 *N* 解码步骤，然后是一个归一化层。最后，我们将解码后的数据通过一个线性层和一个 softmax，得到一个概率分布，用于选择下一个标记。

在接下来的部分中，我们将仔细研究这个架构中的每一个组件。

## **嵌入层**

嵌入层将输入序列中的每个标记转换为长度为 `d_model` 的向量。Transformer 的输入由标记序列的批次组成，形状为 `(batch_size, seq_len)`。嵌入层接受每个标记（一个单一的数字），计算其嵌入（一个长度为 `d_model` 的数字序列），并返回一个张量，用每个嵌入替代对应的原始标记。因此，这一层的输出形状为 `(batch_size, seq_len, d_model)`。

```py
import torch.nn as nn

class Embeddings(nn.Module):

    def __init__(self, d_model, vocab_size):
        super(Embeddings, self).__init__()
        self.lut = nn.Embedding(vocab_size, d_model)
        self.d_model = d_model

    # input x: (batch_size, seq_len)
    # output: (batch_size, seq_len, d_model)
    def forward(self, x):
        out =  self.lut(x) * math.sqrt(self.d_model)
        return out
```

使用嵌入而不是原始标记的目的是为了确保对于语义相似的标记，我们有类似的数学向量表示。例如，考虑“she”和“her”这两个词。这两个词在语义上是相似的，因为它们都指代女性或女孩，但相应的标记可能完全不同（例如，当使用 OpenAI 的`tiktoken`分词器时，“she”对应的标记是 7091，而“her”对应的标记是 372）。这两个标记的嵌入在开始时也会非常不同，因为嵌入层的权重是随机初始化的，并在训练过程中学习。但如果这两个词在训练数据中经常出现在一起，最终嵌入表示将趋于相似。

## **位置编码**

位置编码层添加了每个标记在序列中的绝对位置和相对距离的信息。与递归神经网络（RNN）或卷积神经网络（CNN）不同，变换器本身并没有固有地认识到每个标记在序列中的位置。因此，为了捕捉序列中标记的顺序，变换器依赖于位置编码。

编码标记位置的方法有很多。例如，我们可以通过使用另一个嵌入模块（类似于前面的层）来实现位置编码层，如果我们将每个标记的位置而不是每个标记的值作为输入。同样，我们会从随机选择的权重开始。然后在训练阶段，权重会学习捕捉每个标记的位置。

[The Annotated Transformer](http://nlp.seas.harvard.edu/annotated-transformer/)的作者决定实现一个更复杂的算法，该算法预计算了序列中标记位置的表示。由于我们希望尽可能紧密地跟随他们的代码，我们将使用相同的方法：

```py
class PositionalEncoding(nn.Module):

    def __init__(self, d_model, dropout, max_len=5000):
        super(PositionalEncoding, self).__init__()
        self.dropout = nn.Dropout(p=dropout)

        pe = torch.zeros(max_len, d_model)
        position = torch.arange(0, max_len).unsqueeze(1)  # (max_len, 1)
        div_term = torch.exp(
            torch.arange(0, d_model, 2) *
            -(math.log(10000.0) / d_model))  # (d_model/2)

        pe[:, 0::2] = torch.sin(position * div_term)  # (max_len, d_model)
        pe[:, 1::2] = torch.cos(position * div_term)  # (max_len, d_model)
        pe = pe.unsqueeze(0)  # (1, max_len, d_model)
        self.register_buffer('pe', pe)

    # input x: (batch_size, seq_len, d_model)
    # output: (batch_size, seq_len, d_model)
    def forward(self, x):
        x = x + Variable(self.pe[:, :x.size(1)], requires_grad=False)
        return self.dropout(x)
```

这种位置编码使用不同频率的正弦和余弦函数来填充`pe`张量。例如，在下面的插图中，蓝色和红色的值是使用两种不同频率的正弦波计算得出的，而橙色和绿色的值是使用相同频率的余弦波计算得出的。

![](img/eddfe97cb8a32e8a6ace80d47d93f803.png)

正弦和余弦图的值最终填充了`pe`张量的列，如下所示：

![](img/f02bcdd47bf836c66fb929b97e81bf5c.png)

然后在“前向”阶段，我们将前一个嵌入层的结果`x`作为输入，并返回`x`和`pe`的和。

预计算位置编码的值（而不是使用可训练的嵌入）的主要优点是我们的模型训练参数减少。这种参数减少提高了训练性能，这在处理大型语言模型时至关重要。

## **解码器**

正如我们在 Transformer 架构的图示概述中看到的那样，嵌入和位置编码层之后的下一个阶段是解码器模块。解码器由*N*个解码器层副本组成，后面跟着一个层归一化。以下是`Decoder`类，它将单个`DecoderLayer`实例作为类初始化器的输入：

```py
class Decoder(nn.Module):

    def __init__(self, layer, N):
        super(Decoder, self).__init__()
        self.layers = clones(layer, N)
        self.norm = LayerNorm(layer.size)

    def forward(self, x, mask):
        for layer in self.layers:
            x = layer(x, mask)
        return self.norm(x)
```

“克隆”函数简单地创建一个包含*N*个模块副本的 PyTorch 列表：

```py
def clones(module, N):
    return nn.ModuleList([copy.deepcopy(module) for _ in range(N)])
```

层归一化接收形状为`(batch_size, seq_len, d_model)`的输入，并在其最后一个维度上进行归一化。经过这一步，每个嵌入分布将开始时为单位正态分布（以零为中心，标准差为一）。然后，在训练过程中，分布会随着参数`a_2`和`b_2`在我们场景中的优化而改变形状。你可以在[层归一化](https://arxiv.org/abs/1607.06450)论文中了解更多关于层归一化的内容，这篇论文来自 2016 年。

```py
class LayerNorm(nn.Module):

    def __init__(self, features, eps=1e-6):
        super(LayerNorm, self).__init__()
        self.a_2 = nn.Parameter(torch.ones(features))
        self.b_2 = nn.Parameter(torch.zeros(features))
        self.eps = eps

    def forward(self, x):
        mean = x.mean(-1, keepdim=True)
        std = x.std(-1, keepdim=True)
        return self.a_2 * (x - mean) / (std + self.eps) + self.b_2
```

我们克隆的`DecoderLayer`类具有以下架构：

![](img/95fa0df26b006228e9d1be7650d21926.png)

以下是对应的代码：

```py
class DecoderLayer(nn.Module):

    def __init__(self, size, self_attn, feed_forward, dropout):
        super(DecoderLayer, self).__init__()
        self.size = size
        self.self_attn = self_attn
        self.feed_forward = feed_forward
        self.sublayer = clones(SublayerConnection(size, dropout), 2)

    def forward(self, x, mask):
        x = self.sublayer0)
        return self.sublayer1
```

从高层次看，`DecoderLayer` 包含两个主要步骤：注意力步骤，负责令牌之间的通信，以及前馈步骤，负责预测令牌的计算。在这些步骤周围，我们有残差（或跳跃）连接，用图中的加号表示。残差连接为数据在神经网络中的流动提供了替代路径，从而允许跳过某些层。数据可以通过残差连接中的层流动，也可以直接通过残差连接跳过其中的层。实际上，残差连接通常用于深度神经网络，因为它们有助于训练更好地收敛。你可以在论文[深度残差学习用于图像识别](https://arxiv.org/abs/1512.03385)中了解更多关于残差连接的内容，这篇论文来自 2015 年。我们使用`SublayerConnection`模块来实现这些残差连接：

```py
class SublayerConnection(nn.Module):

    def __init__(self, size, dropout):
        super(SublayerConnection, self).__init__()
        self.norm = LayerNorm(size)
        self.dropout = nn.Dropout(dropout)

    def forward(self, x, sublayer):
        return x + self.dropout(sublayer(self.norm(x)))
```

前馈步骤通过两个线性层实现，中间夹一个 Rectified Linear Unit (ReLU) 激活函数：

```py
class PositionwiseFeedForward(nn.Module):

    def __init__(self, d_model, d_ff, dropout=0.1):
        super(PositionwiseFeedForward, self).__init__()
        self.w_1 = nn.Linear(d_model, d_ff)
        self.w_2 = nn.Linear(d_ff, d_model)
        self.dropout = nn.Dropout(dropout)

    def forward(self, x):
        return self.w_2(self.dropout(F.relu(self.w_1(x))))
```

注意力步骤是 Transformer 中最重要的部分，因此我们将在下一节中专门讨论它。

## **掩蔽多头自注意力**

上图中的多头注意力部分可以扩展成以下架构：

![](img/608a3f4c3aec9470e1b387a9c6ca014a.png)

顾名思义，多头注意力模块并行处理多个注意力计算实例，并对数据进行一些额外的前处理和后处理。

```py
class MultiHeadedAttention(nn.Module):

    def __init__(self, h, d_model, dropout=0.1):
        super(MultiHeadedAttention, self).__init__()
        assert d_model % h == 0
        self.d_k = d_model // h
        self.h = h
        self.linears = clones(nn.Linear(d_model, d_model), 4)
        self.attn = None
        self.dropout = nn.Dropout(p=dropout)

    def forward(self, query, key, value, mask=None):
        if mask is not None:
            mask = mask.unsqueeze(1)  # (1, 1, seq_len, seq_len)

        nbatches = query.size(0)  # batch_size

        # (batch_size, seq_len, d_model) => (batch_size, h, seq_len, d_k)
        query, key, value = \
            [l(x).view(nbatches, -1, self.h, self.d_k).transpose(1, 2)
             for l, x in zip(self.linears, (query, key, value))]

        # (batch_size, h, seq_len, d_k)
        x, self.attn = attention(query,
                                 key,
                                 value,
                                 mask=mask,
                                 dropout=self.dropout)

        # (batch_size, h, seq_len, d_k) => (batch_size, seq_len, d_model)
        x = x.transpose(1, 2).contiguous().view(nbatches, -1, self.h * self.d_k)
        return self.linears-1
```

多头注意力层的输入包括三个张量，分别称为`query` (Q)、`key` (K) 和 `value` (V)。在我们的特定模型中，我们将前一层的输出 `x` 传递给这三个参数，它的形状为 `(batch_size, seq_len, d_model)`（这就是为什么我们称之为 *自注意力*）。我们通过先将每个张量传递通过一个线性层，然后将它们分割成 `h` 个注意力头，每个注意力头的大小为 `d_k`，其中 `h*d_k = d_model`，得到形状为 `(batch_size, seq_len, h, d_k)` 的张量。然后我们交换维度 1 和 2，得到形状为 `(batch_size, h, seq_len, d_k)` 的张量。接下来，我们为每个头计算注意力，得到相同形状的张量。最后，我们的后处理将所有头连接回形状为 `(batch_size, seq_len, d_model)` 的张量，并通过另一个线性层。通过使用张量操作在每个头中并行进行所有注意力计算，我们可以充分利用 GPU。

注意力的计算使用以下公式：

![](img/962b20dcb548b8b373b072c539af57d8.png)

这里是实现公式的代码：

```py
# Dimensions of query, key, and value: (batch_size, h, seq_len, d_k)
# Dimensions of mask: (1, 1, seq_len, seq_len)
def attention(query, key, value, mask=None, dropout=None):
    d_k = query.size(-1)
    # (batch_size, h, seq_len, d_k) x (batch_size, h, d_k, seq_len) -> (batch_size, h, seq_len, seq_len)
    scores = torch.matmul(query, key.transpose(-2, -1)) / math.sqrt(d_k)
    if mask is not None:
        scores = scores.masked_fill(mask == 0, -1e9)
    p_attn = F.softmax(scores, dim=-1) # (batch_size, h, seq_len, seq_len)
    if dropout is not None:
        p_attn = dropout(p_attn)
    # (batch_size, h, seq_len, seq_len) x (batch_size, h, seq_len, d_k) -> (batch_size, h, seq_len, d_k)
    return torch.matmul(p_attn, value), p_attn
```

从高层次来看，注意力算法决定输入序列中哪些标记应该受到更多关注，然后使用这些信息来预测下一个标记。在下图中，深色的橙色表示在预测中更相关的标记。

![](img/decf787bcf0f9655c11266682370e0d0.png)

更具体地说，注意力实际上预测了我们输入序列的多个部分的下一个标记。它查看第一个标记，并预测第二个标记是什么，然后它查看第一个和第二个标记，并预测第三个标记是什么，以此类推。

![](img/b433dbae627e98570b657cb710bc5c41.png)

在推理过程中，这似乎有些浪费，因为我们只对最后的预测感兴趣。然而，这在训练期间极为有用。如果你给 Transformer *n* 个标记作为输入，它将被训练接收从 *1* 到 *n-1* 长度的输入，从而使模型在未来更好地处理不同长度的输入。

上图中的思想由代码中的 `p_attn` 张量表示。这个张量的形状为 `(batch_size, h, seq_len, seq_len)`，但我们暂时忽略批量大小和头的数量（每个批量和每个头的工作方式相同），只考虑一个形状为 `(seq_len, seq_len)` 的张量切片。`p_attn` 张量中的每一行包含一个概率分布，表示其他所有键标记对该行对应的查询标记的相关性。结果张量封装了前一图像中显示的所有值：

![](img/8ea8ebdbee6193baf5ccf9b81eb3f21f.png)

你可以在代码中看到这个张量是如何计算的。我们首先对查询和转置的键进行矩阵乘法。如果忽略批量大小和头数，查询和键由形状为`(seq_len, d_k)`的嵌入序列组成，这些嵌入是将输入`x`通过不同线性层得到的。当我们将形状为`(seq_len, d_k)`的查询张量与形状为`(d_k, seq_len)`的转置键张量相乘时，我们实际上是在对查询中的每个嵌入和键中的所有其他嵌入进行点积，最终得到一个形状为`(seq_len, seq_len)`的`scores`张量。点积的较大值表示查询中的某个嵌入对键中的某个嵌入“感兴趣”，换句话说，模型发现了输入序列中两个位置之间的关联。大致而言，我们现在有了一个表示每个标记在序列中发现其他所有标记的“有趣”或“重要”的张量。

下一步是对`scores`张量应用掩码，使其上三角的值被忽略（这就是我们称之为*masked*注意力的原因）。我们这样做是因为在 GPT 风格的文本生成场景中，模型在预测下一个标记时仅查看过去的标记。我们使用以下代码定义一个掩码，它在对角线和下三角中包含`True`值，在上三角中包含`False`值：

```py
def subsequent_mask(size):
    """
    Mask out subsequent positions.
    """
    attn_shape = (1, size, size)
    subsequent_mask = torch.triu(torch.ones(attn_shape),
                                 diagonal=1).type(torch.uint8)
    return subsequent_mask == 0
```

我们使用`masked_fill`函数将这个掩码应用到`scores`张量中，用一个非常大的负数替换掉上三角的所有值。

最后，我们应用一个 softmax，将张量中的每一行转换为概率分布。记得 softmax 的公式吗？

![](img/80756c319ea5c0a355c5493e03228324.png)

由于*e*的负幂非常大的情况下接近零，`p_attn`张量的上三角中的所有值本质上变为零。剩余的值（在下三角和对角线中）变成每行总和为一的概率。

你可能注意到，在代码中，当我们将查询和键张量相乘时，我们将结果矩阵中的所有值除以`d_k`的平方根。这样做是为了保持方差接近于一，这样可以确保 softmax 给出的概率值在整个范围内（从零到一）分布良好。如果我们不这样做，softmax 计算的分布可能接近于 one-hot 向量，其中一个值为一，其他值都为零——这会使模型的输出显得可预测且机械。

此时，我们有一个包含概率分布的`p_attn`张量，表示各标记之间的相互兴趣程度。下一步是利用这种兴趣程度来确定在生成输出标记时我们应该对每个输入标记给予多少关注。自然地，我们会对最感兴趣的标记给予更多关注。我们通过将概率张量与`value`张量相乘来生成下一个标记，`value`张量包含经过线性层处理后的输入标记嵌入`x`。结果张量将包含每个标记子序列的预测：

![](img/bdc02745464241ba99f093e8059aede5.png)

这个图表的直观解释是：对于输入子序列“A”，我们对唯一的输入标记给予完全的关注，并可能产生下一个标记预测，比如“person”。对于输入子序列“A long”，我们的模型已经训练得更加关注标记“long”而非标记“A”，并可能产生下一个标记预测“dress”，依此类推。在推理时，我们希望考虑到完整的输入序列“A long time ago”，所以我们只关注这个图表中的最后一行。我们最关注“ago”，次之是“long”，对其他两个标记的关注最少，我们产生下一个标记预测“in”。

在我们计算了所有头的注意力，并将结果重新拼接在一起后，我们得到一个维度为`(batch_size, seq_len, d_model)`的输出张量。这个张量包含每个子序列的标记预测，几乎准备好返回给用户。但是在此之前，我们需要最后一步来最终确定其形状和内容。

## **生成器**

我们的 Transformer 中的最后一步是生成器，它包括一个线性层和一个顺序执行的 softmax：

```py
class Generator(nn.Module):

    def __init__(self, d_model, vocab):
        super(Generator, self).__init__()
        self.proj = nn.Linear(d_model, vocab)

    def forward(self, x):
        return F.log_softmax(self.proj(x), dim=-1)
```

线性层的目的是将张量的第三维度从内部的`d_model`嵌入维度转换为`vocab_size`维度，这是调用我们 Transformer 的代码所理解的。结果是一个维度为`(batch_size, seq_len, vocab_size)`的张量。softmax 的目的是将第三维度中的值转换为概率分布。这一概率分布张量就是我们返回给用户的内容。

你可能还记得，在本文开头，我们解释了 Transformer 的输入由形状为`(batch_size, seq_len)`的标记序列批次组成。而现在我们知道 Transformer 的输出由形状为`(batch_size, seq_len, vocab_size)`的概率分布序列批次组成。每个批次包含一个预测第一个输入标记之后的标记的分布，另一个预测第一个和第二个输入标记之后的标记的分布，依此类推。每个批次的最后一个概率分布使我们能够预测整个输入序列之后的标记，这也是我们在做推理时所关心的。

生成器是我们 Transformer 架构的最后一个部分，因此我们准备好将所有组件结合起来了。

## **将所有组件结合起来**

我们使用`DecoderModel`模块来封装 Transformer 架构的三个主要部分：嵌入层、解码器和生成器。

```py
class DecoderModel(nn.Module):

    def __init__(self, decoder, embed, generator):
        super(DecoderModel, self).__init__()
        self.embed = embed
        self.decoder = decoder
        self.generator = generator

    def forward(self, x, mask):
        return self.decode(x, mask)

    def decode(self, x, mask):
        return self.decoder(self.embed(x), mask)
```

调用`decode`仅执行嵌入层和解码器，所以如果我们想执行 Transformer 的所有步骤，我们需要调用`decode`，然后调用`generator`。这正是我们在本文开头展示的推理代码中的`generate_next_token`函数所做的。

推理代码还调用了一个`make_model`函数，该函数返回`DecoderModel`的一个实例。这个函数初始化了我们迄今为止讨论的所有组件，并按照本文开头的架构图将它们结合在一起：

```py
def make_model(vocab_size, N=6, d_model=512, d_ff=2048, h=8, dropout=0.1):
    c = copy.deepcopy
    attn = MultiHeadedAttention(h, d_model)
    ff = PositionwiseFeedForward(d_model, d_ff, dropout)
    position = PositionalEncoding(d_model, dropout)
    model = DecoderModel(
        Decoder(DecoderLayer(d_model, c(attn), c(ff), dropout), N),
        nn.Sequential(Embeddings(d_model, vocab_size), c(position)),
        Generator(d_model, vocab_size))

    for p in model.parameters():
        if p.dim() > 1:
            nn.init.xavier_uniform_(p)
    return model
```

我们现在已经拥有了实现 GPT 风格 Transformer 架构所需的所有组件！

我们将以一些关于训练的思考结束，并且对完整的 Transformer 与 GPT 风格子集进行简要比较。

## **训练**

训练 GPT 风格的 Transformer 的代码与训练任何其他神经网络的代码相同——只是，在我们的场景中，对于每个输入的标记序列，我们期望输出是从右边一个位置开始的序列。例如，如果我们给它输入“A long time ago”，我们期望模型返回的概率采样会产生“long time ago in”。

如果你有训练神经网络的经验，并且希望自己训练 Transformer，你可以重用你过去编写的任何代码，并将其调整到我们的场景中。如果你需要指导，我建议你参考[Annotated Transformer 的第二部分](http://nlp.seas.harvard.edu/annotated-transformer/#part-2-model-training)中的代码和解释。无论如何，你都需要访问快速 GPU，无论是本地还是云端。自然，我更倾向于在 Azure 云端进行训练！

## **与完整 Transformer 的比较**

一旦你理解了 GPT 风格 Transformer 的架构，你就离理解[Attention is all you need](https://arxiv.org/abs/1706.03762)论文中呈现的完整 Transformer 只差一步。下面你可以看到论文中呈现的 Transformer 架构图，我们在本文中覆盖的部分被橙色框圈出。

![](img/6fa18631106401a5fff1c12a1a4b3385.png)

*插图改编自* [*Attention is all you need*](https://arxiv.org/abs/1706.03762) *论文*

完整的 Transformer 有一个编码器部分（在左侧）和一个解码器部分（在右侧）。论文的原始意图是提出一种用于机器翻译的架构。在这种背景下，编码器用于处理输入语言，而解码器用于生成输出语言。

你可以看到，除了 GPT 风格 Transformer 中使用的掩码多头自注意力外，完整的 Transformer 还有另外两个多头注意力块。编码器中的那个未被掩码，这意味着我们在注意力部分看到的`p_attn`张量在其上三角区域的值没有被置零。这是因为在机器翻译中，生成一个输出语言的单一令牌可能需要模型关注输入语言中所有序列位置的令牌，包括早期和晚期位置。解码器部分的附加多头注意力块是一个“交叉注意力”（与“自注意力”相对）层，这意味着它的键和值来自不同于查询的源，如图所示。这是因为模型需要理解在预测输出语言中的令牌时，应该关注输入语言中的每个令牌的程度。图中其余部分类似于 GPT 风格 Transformer 的部分，并已在本文中解释过。

## **结论**

在这篇文章中，我们详细讨论了 GPT 风格的 Transformer 模型的架构，并在较高层次上覆盖了原始 Transformer 的架构。鉴于 GPT 模型在行业中的日益流行，以及最近论文中经常出现的原始 Transformer 模型的变体，我希望你能在工作或学习中找到这些知识的用处。如果你想深入了解，我鼓励你克隆相关的 [GitHub 项目](https://github.com/bstollnitz/gpt-transformer) 并探索代码。设置断点于你不清楚的代码部分，运行它，并检查其变量。如果你有 GPU 访问权限，可以编写代码训练模型，并查看其预测性能的提升。

希望你运用新获得的知识，帮助人工智能社区揭开一些与这些模型相关的误解。许多人误以为这些模型对世界有更高层次的理解，而你现在知道事实并非如此。它们只是由数学和统计运算组成，旨在基于之前的训练数据预测下一个令牌。也许未来的生成模型会对世界有更好的理解，并生成更准确的预测？如果是这样，我会在这里告诉你所有的细节。

## **注意**

所有图片均由作者提供，除非另有说明。你可以在这篇博客文章中出于任何目的使用原始图片，并需注明出处（即链接到本文）。
