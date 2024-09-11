# 从零开始使用Pytorch构建自己的Transformer

> 原文：[https://towardsdatascience.com/build-your-own-transformer-from-scratch-using-pytorch-84c850470dcb?source=collection_archive---------0-----------------------#2023-04-26](https://towardsdatascience.com/build-your-own-transformer-from-scratch-using-pytorch-84c850470dcb?source=collection_archive---------0-----------------------#2023-04-26)

## 在Pytorch中逐步构建一个Transformer模型

[](https://arjun-sarkar786.medium.com/?source=post_page-----84c850470dcb--------------------------------)[![Arjun Sarkar](../Images/de141f1ab68c2b85c9d7a1f31be0d9b5.png)](https://arjun-sarkar786.medium.com/?source=post_page-----84c850470dcb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----84c850470dcb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----84c850470dcb--------------------------------) [Arjun Sarkar](https://arjun-sarkar786.medium.com/?source=post_page-----84c850470dcb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffa31366b2eda&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-your-own-transformer-from-scratch-using-pytorch-84c850470dcb&user=Arjun+Sarkar&userId=fa31366b2eda&source=post_page-fa31366b2eda----84c850470dcb---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----84c850470dcb--------------------------------) ·7分钟阅读·2023年4月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F84c850470dcb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-your-own-transformer-from-scratch-using-pytorch-84c850470dcb&user=Arjun+Sarkar&userId=fa31366b2eda&source=-----84c850470dcb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F84c850470dcb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-your-own-transformer-from-scratch-using-pytorch-84c850470dcb&source=-----84c850470dcb---------------------bookmark_footer-----------)![](../Images/3f54c2f78af8d6ca5052e7b2b898034a.png)

图1\. 照片由[Kevin Ku](https://unsplash.com/@ikukevk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，来自[Unsplash](https://unsplash.com/s/photos/deep-learning?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在本教程中，我们将使用 PyTorch 从头开始构建一个基本的 Transformer 模型。Transformer 模型由 Vaswani 等人提出，在论文“Attention is All You Need”中介绍，是一种针对序列到序列任务（如机器翻译和文本摘要）设计的深度学习架构。它基于自注意力机制，已成为许多最先进自然语言处理模型的基础，如 GPT 和 BERT。

要详细了解 Transformer 模型，请访问这两篇文章：

## [1\. 关于“注意力”和“Transformer”的一切——深入理解——第 1 部分](https://medium.com/towards-data-science/all-you-need-to-know-about-attention-and-transformers-in-depth-understanding-part-1-552f0b41d021)

## [2\. 关于“注意力”和“Transformer”的一切——深入理解——第 2 部分](https://medium.com/towards-data-science/all-you-need-to-know-about-attention-and-transformers-in-depth-understanding-part-2-bf2403804ada)

要构建我们的 Transformer 模型，我们将遵循以下步骤：

1.  导入必要的库和模块

1.  定义基本构建模块：多头注意力、位置逐层前馈网络、位置编码

1.  构建编码器和解码器层

1.  结合编码器和解码器层以创建完整的 Transformer 模型

1.  准备示例数据

1.  训练模型

让我们首先导入必要的库和模块。

```py
import torch
import torch.nn as nn
import torch.optim as optim
import torch.utils.data as data
import math
import copy
```

现在，我们将定义 Transformer 模型的基本构建块。

# 多头注意力

![](../Images/e8b7aaf2849d3b0b0b0bd006f7dae3aa.png)

图 2\. 多头注意力（来源：作者创建的图像）

多头注意力机制计算序列中每对位置之间的注意力。它由多个“注意力头”组成，这些头捕捉输入序列的不同方面。

```py
class MultiHeadAttention(nn.Module):
    def __init__(self, d_model, num_heads):
        super(MultiHeadAttention, self).__init__()
        assert d_model % num_heads == 0, "d_model must be divisible by num_heads"

        self.d_model = d_model
        self.num_heads = num_heads
        self.d_k = d_model // num_heads

        self.W_q = nn.Linear(d_model, d_model)
        self.W_k = nn.Linear(d_model, d_model)
        self.W_v = nn.Linear(d_model, d_model)
        self.W_o = nn.Linear(d_model, d_model)

    def scaled_dot_product_attention(self, Q, K, V, mask=None):
        attn_scores = torch.matmul(Q, K.transpose(-2, -1)) / math.sqrt(self.d_k)
        if mask is not None:
            attn_scores = attn_scores.masked_fill(mask == 0, -1e9)
        attn_probs = torch.softmax(attn_scores, dim=-1)
        output = torch.matmul(attn_probs, V)
        return output

    def split_heads(self, x):
        batch_size, seq_length, d_model = x.size()
        return x.view(batch_size, seq_length, self.num_heads, self.d_k).transpose(1, 2)

    def combine_heads(self, x):
        batch_size, _, seq_length, d_k = x.size()
        return x.transpose(1, 2).contiguous().view(batch_size, seq_length, self.d_model)

    def forward(self, Q, K, V, mask=None):
        Q = self.split_heads(self.W_q(Q))
        K = self.split_heads(self.W_k(K))
        V = self.split_heads(self.W_v(V))

        attn_output = self.scaled_dot_product_attention(Q, K, V, mask)
        output = self.W_o(self.combine_heads(attn_output))
        return output
```

MultiHeadAttention 代码用输入参数和线性变换层初始化模块。它计算注意力得分，将输入张量重塑为多个头，并结合所有头的注意力输出。前向方法计算多头自注意力，使模型能够关注输入序列的不同方面。

# 位置逐层前馈网络

```py
class PositionWiseFeedForward(nn.Module):
    def __init__(self, d_model, d_ff):
        super(PositionWiseFeedForward, self).__init__()
        self.fc1 = nn.Linear(d_model, d_ff)
        self.fc2 = nn.Linear(d_ff, d_model)
        self.relu = nn.ReLU()

    def forward(self, x):
        return self.fc2(self.relu(self.fc1(x)))
```

PositionWiseFeedForward 类扩展了 PyTorch 的 nn.Module，并实现了位置逐层前馈网络。该类初始化时包含两个线性变换层和一个 ReLU 激活函数。前向方法依次应用这些变换和激活函数以计算输出。这个过程使模型在做出预测时能够考虑输入元素的位置。

# 位置编码

位置编码用于注入输入序列中每个标记的位置信息。它使用不同频率的正弦和余弦函数来生成位置编码。

```py
class PositionalEncoding(nn.Module):
    def __init__(self, d_model, max_seq_length):
        super(PositionalEncoding, self).__init__()

        pe = torch.zeros(max_seq_length, d_model)
        position = torch.arange(0, max_seq_length, dtype=torch.float).unsqueeze(1)
        div_term = torch.exp(torch.arange(0, d_model, 2).float() * -(math.log(10000.0) / d_model))

        pe[:, 0::2] = torch.sin(position * div_term)
        pe[:, 1::2] = torch.cos(position * div_term)

        self.register_buffer('pe', pe.unsqueeze(0))

    def forward(self, x):
        return x + self.pe[:, :x.size(1)]
```

PositionalEncoding 类初始化时包含输入参数 d_model 和 max_seq_length，创建一个张量来存储位置编码值。该类根据缩放因子 div_term 计算偶数和奇数索引的正弦和余弦值。forward 方法通过将存储的位置信息编码值添加到输入张量中来计算位置编码，从而使模型能够捕捉输入序列的位置信息。

现在，我们将构建编码器和解码器层。

# 编码器层

![](../Images/9e8ab3e83decd7c9ea8305b7563b5cb0.png)

图 3\. Transformer 网络的编码器部分（来源：原始论文中的图像）

编码器层由一个多头注意力层、一个逐位置前馈层和两个层归一化层组成。

```py
class EncoderLayer(nn.Module):
    def __init__(self, d_model, num_heads, d_ff, dropout):
        super(EncoderLayer, self).__init__()
        self.self_attn = MultiHeadAttention(d_model, num_heads)
        self.feed_forward = PositionWiseFeedForward(d_model, d_ff)
        self.norm1 = nn.LayerNorm(d_model)
        self.norm2 = nn.LayerNorm(d_model)
        self.dropout = nn.Dropout(dropout)

    def forward(self, x, mask):
        attn_output = self.self_attn(x, x, x, mask)
        x = self.norm1(x + self.dropout(attn_output))
        ff_output = self.feed_forward(x)
        x = self.norm2(x + self.dropout(ff_output))
        return x
```

EncoderLayer 类初始化时包含输入参数和组件，包括一个多头注意力模块、一个逐位置前馈模块、两个层归一化模块和一个丢弃层。forward 方法通过应用自注意力、将注意力输出添加到输入张量并归一化结果来计算编码器层输出。然后，它计算逐位置前馈输出，将其与归一化的自注意力输出结合，并在返回处理后的张量之前归一化最终结果。

# 解码器层

![](../Images/28187d88b42ade5225fcc50d80e20c31.png)

图 4\. Transformer 网络的解码器部分（来源：原始论文中的图像）

解码器层由两个多头注意力层、一个逐位置前馈层和三个层归一化层组成。

```py
class DecoderLayer(nn.Module):
    def __init__(self, d_model, num_heads, d_ff, dropout):
        super(DecoderLayer, self).__init__()
        self.self_attn = MultiHeadAttention(d_model, num_heads)
        self.cross_attn = MultiHeadAttention(d_model, num_heads)
        self.feed_forward = PositionWiseFeedForward(d_model, d_ff)
        self.norm1 = nn.LayerNorm(d_model)
        self.norm2 = nn.LayerNorm(d_model)
        self.norm3 = nn.LayerNorm(d_model)
        self.dropout = nn.Dropout(dropout)

    def forward(self, x, enc_output, src_mask, tgt_mask):
        attn_output = self.self_attn(x, x, x, tgt_mask)
        x = self.norm1(x + self.dropout(attn_output))
        attn_output = self.cross_attn(x, enc_output, enc_output, src_mask)
        x = self.norm2(x + self.dropout(attn_output))
        ff_output = self.feed_forward(x)
        x = self.norm3(x + self.dropout(ff_output))
        return x
```

解码器层初始化时包含输入参数和组件，如用于掩蔽自注意力和交叉注意力的多头注意力模块、一个逐位置前馈模块、三个层归一化模块和一个丢弃层。

forward 方法通过执行以下步骤计算解码器层输出：

1.  计算掩蔽自注意力输出并将其添加到输入张量中，然后进行丢弃和层归一化。

1.  计算解码器和编码器输出之间的交叉注意力输出，并将其添加到归一化的掩蔽自注意力输出中，然后进行丢弃和层归一化。

1.  计算逐位置前馈输出并将其与归一化的交叉注意力输出结合，然后进行丢弃和层归一化。

1.  返回处理后的张量。

这些操作使解码器能够根据输入和编码器输出生成目标序列。

现在，让我们将编码器和解码器层结合起来，创建完整的 Transformer 模型。

# Transformer 模型

![](../Images/c141dc670d42086d53956ee42bada0be.png)

图 5\. Transformer 网络（来源：原始论文中的图像）

将所有内容合并在一起：

```py
class Transformer(nn.Module):
    def __init__(self, src_vocab_size, tgt_vocab_size, d_model, num_heads, num_layers, d_ff, max_seq_length, dropout):
        super(Transformer, self).__init__()
        self.encoder_embedding = nn.Embedding(src_vocab_size, d_model)
        self.decoder_embedding = nn.Embedding(tgt_vocab_size, d_model)
        self.positional_encoding = PositionalEncoding(d_model, max_seq_length)

        self.encoder_layers = nn.ModuleList([EncoderLayer(d_model, num_heads, d_ff, dropout) for _ in range(num_layers)])
        self.decoder_layers = nn.ModuleList([DecoderLayer(d_model, num_heads, d_ff, dropout) for _ in range(num_layers)])

        self.fc = nn.Linear(d_model, tgt_vocab_size)
        self.dropout = nn.Dropout(dropout)

    def generate_mask(self, src, tgt):
        src_mask = (src != 0).unsqueeze(1).unsqueeze(2)
        tgt_mask = (tgt != 0).unsqueeze(1).unsqueeze(3)
        seq_length = tgt.size(1)
        nopeak_mask = (1 - torch.triu(torch.ones(1, seq_length, seq_length), diagonal=1)).bool()
        tgt_mask = tgt_mask & nopeak_mask
        return src_mask, tgt_mask

    def forward(self, src, tgt):
        src_mask, tgt_mask = self.generate_mask(src, tgt)
        src_embedded = self.dropout(self.positional_encoding(self.encoder_embedding(src)))
        tgt_embedded = self.dropout(self.positional_encoding(self.decoder_embedding(tgt)))

        enc_output = src_embedded
        for enc_layer in self.encoder_layers:
            enc_output = enc_layer(enc_output, src_mask)

        dec_output = tgt_embedded
        for dec_layer in self.decoder_layers:
            dec_output = dec_layer(dec_output, enc_output, src_mask, tgt_mask)

        output = self.fc(dec_output)
        return output
```

Transformer 类结合之前定义的模块，创建一个完整的 Transformer 模型。在初始化期间，Transformer 模块设置输入参数并初始化各种组件，包括源序列和目标序列的嵌入层、位置编码模块、EncoderLayer 和 DecoderLayer 模块以创建堆叠层、用于投影解码器输出的线性层以及 dropout 层。

`generate_mask` 方法创建源序列和目标序列的二进制掩码，以忽略填充标记，并防止解码器关注未来的标记。`forward` 方法通过以下步骤计算 Transformer 模型的输出：

1.  使用 `generate_mask` 方法生成源序列和目标序列的掩码。

1.  计算源序列和目标序列的嵌入，并应用位置编码和 dropout。

1.  通过编码器层处理源序列，更新 `enc_output` 张量。

1.  通过解码器层处理目标序列，使用 `enc_output` 和掩码，并更新 `dec_output` 张量。

1.  将线性投影层应用于解码器输出，获取输出 logits。

这些步骤使 Transformer 模型能够处理输入序列，并基于其组件的组合功能生成输出序列。

# 准备示例数据

在这个例子中，我们将创建一个玩具数据集用于演示。实际操作中，你会使用更大的数据集，预处理文本，并为源语言和目标语言创建词汇映射。

```py
src_vocab_size = 5000
tgt_vocab_size = 5000
d_model = 512
num_heads = 8
num_layers = 6
d_ff = 2048
max_seq_length = 100
dropout = 0.1

transformer = Transformer(src_vocab_size, tgt_vocab_size, d_model, num_heads, num_layers, d_ff, max_seq_length, dropout)

# Generate random sample data
src_data = torch.randint(1, src_vocab_size, (64, max_seq_length))  # (batch_size, seq_length)
tgt_data = torch.randint(1, tgt_vocab_size, (64, max_seq_length))  # (batch_size, seq_length)
```

# 训练模型

现在我们将使用示例数据训练模型。在实际操作中，你会使用更大的数据集，并将其拆分为训练集和验证集。

```py
criterion = nn.CrossEntropyLoss(ignore_index=0)
optimizer = optim.Adam(transformer.parameters(), lr=0.0001, betas=(0.9, 0.98), eps=1e-9)

transformer.train()

for epoch in range(100):
    optimizer.zero_grad()
    output = transformer(src_data, tgt_data[:, :-1])
    loss = criterion(output.contiguous().view(-1, tgt_vocab_size), tgt_data[:, 1:].contiguous().view(-1))
    loss.backward()
    optimizer.step()
    print(f"Epoch: {epoch+1}, Loss: {loss.item()}")
```

我们可以通过这种方式从零开始在 Pytorch 中构建一个简单的 Transformer。所有的大型语言模型都使用这些 Transformer 编码器或解码器模块进行训练。因此，了解最初的网络极为重要。希望这篇文章能帮助所有希望深入了解 LLM 的人。

# 参考文献

# 《Attention is all you need》

[A. Vaswani](https://www.bibsonomy.org/person/1c9bf08cbcb15680c807e12a01dd8c929/author/0)、[N. Shazeer](https://www.bibsonomy.org/person/1c9bf08cbcb15680c807e12a01dd8c929/author/1)、[N. Parmar](https://www.bibsonomy.org/person/1c9bf08cbcb15680c807e12a01dd8c929/author/2)、[J. Uszkoreit](https://www.bibsonomy.org/person/1c9bf08cbcb15680c807e12a01dd8c929/author/3)、[L. Jones](https://www.bibsonomy.org/person/1c9bf08cbcb15680c807e12a01dd8c929/author/4)、[A. Gomez](https://www.bibsonomy.org/person/1c9bf08cbcb15680c807e12a01dd8c929/author/5)、[{. Kaiser](https://www.bibsonomy.org/person/1c9bf08cbcb15680c807e12a01dd8c929/author/6) 和 [I. Polosukhin](https://www.bibsonomy.org/person/1c9bf08cbcb15680c807e12a01dd8c929/author/7)。*《神经信息处理系统的进展》, 第 5998–6008 页\.* (*2017*)
