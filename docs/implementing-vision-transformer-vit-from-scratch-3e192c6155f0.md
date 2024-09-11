# 从零实现 Vision Transformer (ViT)

> 原文：[https://towardsdatascience.com/implementing-vision-transformer-vit-from-scratch-3e192c6155f0?source=collection_archive---------9-----------------------#2023-03-07](https://towardsdatascience.com/implementing-vision-transformer-vit-from-scratch-3e192c6155f0?source=collection_archive---------9-----------------------#2023-03-07)

## 通过从零实现 Vision Transformer (ViT) 了解其工作原理

[Tin Nguyen](https://medium.com/@tintn03?source=post_page-----3e192c6155f0--------------------------------) [![Tin Nguyen](../Images/f5a69125e3d42be7906c8cd51f827854.png)](https://medium.com/@tintn03?source=post_page-----3e192c6155f0--------------------------------) [![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3e192c6155f0--------------------------------) [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3e192c6155f0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F78d51d946a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplementing-vision-transformer-vit-from-scratch-3e192c6155f0&user=Tin+Nguyen&userId=78d51d946a3&source=post_page-78d51d946a3----3e192c6155f0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3e192c6155f0--------------------------------) ·10 分钟阅读·2023年3月7日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3e192c6155f0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplementing-vision-transformer-vit-from-scratch-3e192c6155f0&source=-----3e192c6155f0---------------------bookmark_footer-----------)

Vision Transformer (ViT) 是将 Transformer 模型适配于计算机视觉任务的一种方法。它由 Google 研究人员在 2020 年提出，并因其在各种图像分类基准测试中的卓越表现而获得广泛关注。ViT 已显示出在多个计算机视觉任务中实现了最先进的性能，并引起了计算机视觉社区的极大兴趣。

在这篇文章中，我们将从头开始实现ViT用于图像分类，使用PyTorch。我们还将用CIFAR-10数据集训练我们的模型，这是一个流行的图像分类基准。通过这篇文章，你应该能很好地理解ViT的工作原理以及如何将其应用于自己的计算机视觉项目。

实现的代码可以在[这个仓库](https://github.com/tintn/vision-transformer-from-scratch)中找到。

# ViT架构概述

![](../Images/0d112d70a71929fa8a2a543c38d2e8fc.png)

改编自 [https://arxiv.org/abs/2010.11929](https://arxiv.org/abs/2010.11929)

ViT的架构灵感来源于BERT，一个仅包含编码器的transformer模型，通常用于NLP监督学习任务，如文本分类或命名实体识别。ViT的主要思想是图像可以被视为一系列小块，这些小块可以在NLP任务中作为token处理。

输入图像被分割成小块，然后将这些小块展平为向量序列。这些向量随后由一个transformer编码器处理，使得模型能够通过自注意力机制学习小块之间的交互。transformer编码器的输出被送入分类层，输出输入图像的预测类别。

在接下来的部分，我们将逐一实现模型的每个组件，并使用PyTorch进行实现。这将帮助我们理解ViT模型的工作原理及其在计算机视觉任务中的应用。

# 将图像转换为嵌入

![](../Images/175b1e3332f4ceb1ba3e7eaf3051caf2.png)

为了将输入图像馈送到Transformer模型中，我们需要将图像转换为向量序列。这通过将图像拆分成不重叠的小块，然后将这些小块线性投影以获得每个小块的固定大小嵌入向量来完成。我们可以使用PyTorch的`nn.Conv2d`层来实现这一点：

```py
class PatchEmbeddings(nn.Module):
    """
    Convert the image into patches and then project them into a vector space.
    """

    def __init__(self, config):
        super().__init__()
        self.image_size = config["image_size"]
        self.patch_size = config["patch_size"]
        self.num_channels = config["num_channels"]
        self.hidden_size = config["hidden_size"]
        # Calculate the number of patches from the image size and patch size
        self.num_patches = (self.image_size // self.patch_size) ** 2
        # Create a projection layer to convert the image into patches
        # The layer projects each patch into a vector of size hidden_size
        self.projection = nn.Conv2d(self.num_channels, self.hidden_size, kernel_size=self.patch_size, stride=self.patch_size)

    def forward(self, x):
        # (batch_size, num_channels, image_size, image_size) -> (batch_size, num_patches, hidden_size)
        x = self.projection(x)
        x = x.flatten(2).transpose(1, 2)
        return x
```

`kernel_size=self.patch_size` 和 `stride=self.patch_size` 是为了确保层的过滤器应用于不重叠的小块。

在小块被转换为嵌入序列后，[CLS] token被添加到序列的开头，它将在分类层中用于对图像进行分类。[CLS] token的嵌入在训练过程中学习。

由于来自不同位置的小块可能对最终预测的贡献不同，我们还需要一种方法将小块的位置编码到序列中。我们将使用可学习的位置嵌入将位置信息添加到嵌入中。这类似于在NLP任务的Transformer模型中使用位置嵌入的方式。

```py
class Embeddings(nn.Module):
    """
    Combine the patch embeddings with the class token and position embeddings.
    """

    def __init__(self, config):
        super().__init__()
        self.config = config
        self.patch_embeddings = PatchEmbeddings(config)
        # Create a learnable [CLS] token
        # Similar to BERT, the [CLS] token is added to the beginning of the input sequence
        # and is used to classify the entire sequence
        self.cls_token = nn.Parameter(torch.randn(1, 1, config["hidden_size"]))
        # Create position embeddings for the [CLS] token and the patch embeddings
        # Add 1 to the sequence length for the [CLS] token
        self.position_embeddings = \
            nn.Parameter(torch.randn(1, self.patch_embeddings.num_patches + 1, config["hidden_size"]))
        self.dropout = nn.Dropout(config["hidden_dropout_prob"])

    def forward(self, x):
        x = self.patch_embeddings(x)
        batch_size, _, _ = x.size()
        # Expand the [CLS] token to the batch size
        # (1, 1, hidden_size) -> (batch_size, 1, hidden_size)
        cls_tokens = self.cls_token.expand(batch_size, -1, -1)
        # Concatenate the [CLS] token to the beginning of the input sequence
        # This results in a sequence length of (num_patches + 1)
        x = torch.cat((cls_tokens, x), dim=1)
        x = x + self.position_embeddings
        x = self.dropout(x)
        return x
```

在这一步，输入图像被转换为带有位置信息的嵌入序列，并准备好输入transformer层。

# 多头注意力

![](../Images/d58fa5483fd06acc1e2a3ed1b025a6b1.png)

在深入 transformer 编码器之前，我们首先探索多头注意力模块，这是其核心组件。多头注意力用于计算输入图像中不同块之间的交互。多头注意力由多个注意力头组成，每个头部是一个单一的注意力层。

让我们实现多头注意力模块的一个头部。该模块接受一个嵌入序列作为输入，并为每个嵌入计算查询、键和值向量。查询和键向量随后用于计算每个令牌的注意力权重。注意力权重被用于使用值向量的加权和计算新的嵌入。我们可以将这种机制视为数据库查询的软版本，其中查询向量在数据库中找到最相关的键向量，并检索值向量以计算查询输出。

```py
class AttentionHead(nn.Module):
    """
    A single attention head.
    This module is used in the MultiHeadAttention module.
    """
    def __init__(self, hidden_size, attention_head_size, dropout, bias=True):
        super().__init__()
        self.hidden_size = hidden_size
        self.attention_head_size = attention_head_size
        # Create the query, key, and value projection layers
        self.query = nn.Linear(hidden_size, attention_head_size, bias=bias)
        self.key = nn.Linear(hidden_size, attention_head_size, bias=bias)
        self.value = nn.Linear(hidden_size, attention_head_size, bias=bias)

        self.dropout = nn.Dropout(dropout)

    def forward(self, x):
        # Project the input into query, key, and value
        # The same input is used to generate the query, key, and value,
        # so it's usually called self-attention.
        # (batch_size, sequence_length, hidden_size) -> (batch_size, sequence_length, attention_head_size)
        query = self.query(x)
        key = self.key(x)
        value = self.value(x)
        # Calculate the attention scores
        # softmax(Q*K.T/sqrt(head_size))*V
        attention_scores = torch.matmul(query, key.transpose(-1, -2))
        attention_scores = attention_scores / math.sqrt(self.attention_head_size)
        attention_probs = nn.functional.softmax(attention_scores, dim=-1)
        attention_probs = self.dropout(attention_probs)
        # Calculate the attention output
        attention_output = torch.matmul(attention_probs, value)
        return (attention_output, attention_probs)
```

所有注意力头的输出随后被拼接并线性映射，以获得多头注意力模块的最终输出。

```py
class MultiHeadAttention(nn.Module):
    """
    Multi-head attention module.
    This module is used in the TransformerEncoder module.
    """

    def __init__(self, config):
        super().__init__()
        self.hidden_size = config["hidden_size"]
        self.num_attention_heads = config["num_attention_heads"]
        # The attention head size is the hidden size divided by the number of attention heads
        self.attention_head_size = self.hidden_size // self.num_attention_heads
        self.all_head_size = self.num_attention_heads * self.attention_head_size
        # Whether or not to use bias in the query, key, and value projection layers
        self.qkv_bias = config["qkv_bias"]
        # Create a list of attention heads
        self.heads = nn.ModuleList([])
        for _ in range(self.num_attention_heads):
            head = AttentionHead(
                self.hidden_size,
                self.attention_head_size,
                config["attention_probs_dropout_prob"],
                self.qkv_bias
            )
            self.heads.append(head)
        # Create a linear layer to project the attention output back to the hidden size
        # In most cases, all_head_size and hidden_size are the same
        self.output_projection = nn.Linear(self.all_head_size, self.hidden_size)
        self.output_dropout = nn.Dropout(config["hidden_dropout_prob"])

    def forward(self, x, output_attentions=False):
        # Calculate the attention output for each attention head
        attention_outputs = [head(x) for head in self.heads]
        # Concatenate the attention outputs from each attention head
        attention_output = torch.cat([attention_output for attention_output, _ in attention_outputs], dim=-1)
        # Project the concatenated attention output back to the hidden size
        attention_output = self.output_projection(attention_output)
        attention_output = self.output_dropout(attention_output)
        # Return the attention output and the attention probabilities (optional)
        if not output_attentions:
            return (attention_output, None)
        else:
            attention_probs = torch.stack([attention_probs for _, attention_probs in attention_outputs], dim=1)
            return (attention_output, attention_probs)
```

# Transformer 编码器

![](../Images/efd1e5da157ff66d35e382535c4e39fc.png)

transformer 编码器由一系列 transformer 层组成。每个 transformer 层主要由我们刚刚实现的多头注意力模块和一个前馈网络组成。为了更好地扩展模型和稳定训练，transformer 层中添加了两个层归一化层和跳过连接。

让我们实现一个 transformer 层（在代码中称为`Block`，因为它是 transformer 编码器的构建块）。我们将从前馈网络开始，它是一个简单的两层 MLP，中间有 GELU 激活函数。

```py
class MLP(nn.Module):
    """
    A multi-layer perceptron module.
    """

    def __init__(self, config):
        super().__init__()
        self.dense_1 = nn.Linear(config["hidden_size"], config["intermediate_size"])
        self.activation = NewGELUActivation()
        self.dense_2 = nn.Linear(config["intermediate_size"], config["hidden_size"])
        self.dropout = nn.Dropout(config["hidden_dropout_prob"])

    def forward(self, x):
        x = self.dense_1(x)
        x = self.activation(x)
        x = self.dense_2(x)
        x = self.dropout(x)
        return x
```

我们已经实现了多头注意力和 MLP，可以将它们结合起来创建 transformer 层。跳过连接和层归一化被应用于每一层的输入。

```py
class Block(nn.Module):
    """
    A single transformer block.
    """

    def __init__(self, config):
        super().__init__()
        self.attention = MultiHeadAttention(config)
        self.layernorm_1 = nn.LayerNorm(config["hidden_size"])
        self.mlp = MLP(config)
        self.layernorm_2 = nn.LayerNorm(config["hidden_size"])

    def forward(self, x, output_attentions=False):
        # Self-attention
        attention_output, attention_probs = \
            self.attention(self.layernorm_1(x), output_attentions=output_attentions)
        # Skip connection
        x = x + attention_output
        # Feed-forward network
        mlp_output = self.mlp(self.layernorm_2(x))
        # Skip connection
        x = x + mlp_output
        # Return the transformer block's output and the attention probabilities (optional)
        if not output_attentions:
            return (x, None)
        else:
      return (x, attention_probs)
```

transformer 编码器将多个 transformer 层按顺序堆叠在一起：

```py
class Encoder(nn.Module):
    """
    The transformer encoder module.
    """

    def __init__(self, config):
        super().__init__()
        # Create a list of transformer blocks
        self.blocks = nn.ModuleList([])
        for _ in range(config["num_hidden_layers"]):
            block = Block(config)
            self.blocks.append(block)

    def forward(self, x, output_attentions=False):
        # Calculate the transformer block's output for each block
        all_attentions = []
        for block in self.blocks:
            x, attention_probs = block(x, output_attentions=output_attentions)
            if output_attentions:
                all_attentions.append(attention_probs)
        # Return the encoder's output and the attention probabilities (optional)
        if not output_attentions:
            return (x, None)
        else:
            return (x, all_attentions)
```

# 用于图像分类的 ViT

将图像输入到嵌入层和 transformer 编码器后，我们获得了图像块和[CLS]标记的新嵌入。此时，嵌入经过 transformer 编码器处理后应该具有一些用于分类的有用信号。类似于 BERT，我们将仅使用[CLS]标记的嵌入传递给分类层。

分类层是一个全连接层，它接受[CLS]嵌入作为输入，并输出每张图像的 logits。以下代码实现了用于图像分类的 ViT 模型：

```py
class ViTForClassfication(nn.Module):
    """
    The ViT model for classification.
    """

    def __init__(self, config):
        super().__init__()
        self.config = config
        self.image_size = config["image_size"]
        self.hidden_size = config["hidden_size"]
        self.num_classes = config["num_classes"]
        # Create the embedding module
        self.embedding = Embeddings(config)
        # Create the transformer encoder module
        self.encoder = Encoder(config)
        # Create a linear layer to project the encoder's output to the number of classes
        self.classifier = nn.Linear(self.hidden_size, self.num_classes)
        # Initialize the weights
        self.apply(self._init_weights)

    def forward(self, x, output_attentions=False):
        # Calculate the embedding output
        embedding_output = self.embedding(x)
        # Calculate the encoder's output
        encoder_output, all_attentions = self.encoder(embedding_output, output_attentions=output_attentions)
        # Calculate the logits, take the [CLS] token's output as features for classification
        logits = self.classifier(encoder_output[:, 0])
        # Return the logits and the attention probabilities (optional)
        if not output_attentions:
            return (logits, None)
        else:
            return (logits, all_attentions)
```

要训练模型，可以遵循训练分类模型的标准步骤。你可以在[这里](https://github.com/tintn/vision-transformer-from-scratch/blob/main/train.py)找到训练脚本。

# 结果

由于目标不是实现最先进的性能，而是展示模型的工作原理，我训练的模型远小于论文中描述的原始 ViT 模型，这些模型至少有 12 层，隐藏层大小为 768。我用于训练的模型配置是：

```py
{
    "patch_size": 4,
    "hidden_size": 48,
    "num_hidden_layers": 4,
    "num_attention_heads": 4,
    "intermediate_size": 4 * 48,
    "hidden_dropout_prob": 0.0,
    "attention_probs_dropout_prob": 0.0,
    "initializer_range": 0.02,
    "image_size": 32,
    "num_classes": 10,
    "num_channels": 3,
    "qkv_bias": True,
}
```

该模型在 CIFAR-10 数据集上训练了 100 轮，批量大小为 256。学习率设置为 0.01，并且没有使用学习率调整。经过 100 轮训练后，模型达到了 75.5% 的准确率。下图展示了训练期间的训练损失、测试损失和测试集上的准确率。

![](../Images/2c4978640e8c8022af2514760286c84f.png)

下图展示了模型对一些测试图像的注意力图。你可以看到模型能够识别不同类别的对象。它学会了关注对象并忽略背景。

![](../Images/453dfa740f9d00871fd682243e39ef2f.png)

# 结论

在这篇文章中，我们学习了 Vision Transformer 的工作原理，从嵌入层到变换器编码器，最后到分类层。我们还学习了如何使用 PyTorch 实现模型的每个组件。

由于该实现不用于生产环境，如果你打算训练全尺寸模型或在大型数据集上训练，建议使用更成熟的变换器库，如 [HuggingFace](https://github.com/huggingface/transformers)。

*最初发布于* [*https://tintn.github.io*](https://tintn.github.io/Implementing-Vision-Transformer-from-Scratch/) *2023年3月7日。*
