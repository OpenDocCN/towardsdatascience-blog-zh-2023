# 学习变压器代码第一部分第二部分——GPT 亲密接触

> 原文：[`towardsdatascience.com/learning-transformers-code-first-part-2-gpt-up-close-and-personal-1635b52ae0d7`](https://towardsdatascience.com/learning-transformers-code-first-part-2-gpt-up-close-and-personal-1635b52ae0d7)

## 深入研究通过 nanoGPT 生成预训练变压器

[Lily Hughes-Robinson](https://medium.com/@oaguy1?source=post_page-----1635b52ae0d7--------------------------------)![Lily Hughes-Robinson](https://medium.com/@oaguy1?source=post_page-----1635b52ae0d7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1635b52ae0d7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1635b52ae0d7--------------------------------) [Lily Hughes-Robinson](https://medium.com/@oaguy1?source=post_page-----1635b52ae0d7--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1635b52ae0d7--------------------------------) ·13 分钟阅读·2023 年 7 月 13 日

--

![](img/f90478c1b96dad6cd2667bfcf0da1f03.png)

图片由 [Luca Onniboni](https://unsplash.com/it/@lucaonniboni?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

欢迎来到我的项目的第二部分，在这里我将深入研究变压器和基于 GPT 的模型的复杂性，使用 [TinyStories 数据集](https://huggingface.co/datasets/roneneldan/TinyStories) 和 [nanoGPT](https://github.com/karpathy/nanoGPT/tree/master)，所有这些都在一台老旧的游戏笔记本电脑上进行训练。在第一部分中，我准备了输入到字符级生成模型的数据集。你可以在下面找到第一部分的链接。

[链接](https://towardsdatascience.com/nanogpt-learning-transformers-code-first-part-1-f2044cf5bca0?source=post_page-----1635b52ae0d7--------------------------------) [## 学习变压器代码第一部分

### 新系列的第一部分，我努力使用 nanoGPT 学习变压器代码

[链接](https://towardsdatascience.com/nanogpt-learning-transformers-code-first-part-1-f2044cf5bca0?source=post_page-----1635b52ae0d7--------------------------------)

在这篇文章中，我的目标是剖析 GPT 模型、其组件及其在 nanoGPT 中的实现。我选择了 nanoGPT，因为它是一个约 300 行代码的 GPT 模型的直接 Python 实现，且其训练脚本也同样易于理解。掌握必要的背景知识后，人们可以通过阅读源代码快速理解 GPT 模型。坦白说，当我第一次检查代码时，我缺乏这种理解。有些材料仍然让我感到困惑。然而，我希望凭借我所学到的知识，这个解释能够为那些希望直观理解 GPT 风格模型内部运作的人提供一个起点。

在为这篇文章做准备时，我阅读了各种论文。最初，我以为仅仅阅读奠基性论文“[Attention is All You Need](https://arxiv.org/abs/1706.03762)”就足以让我对这个领域有足够的了解。这是一个天真的假设。虽然这篇论文确实介绍了变换器模型，但后续的论文才将其适应于更高级的任务，如文本生成。“AIAYN” 只是一个更广泛主题的介绍。尽管如此，我想起了一篇 HackerNews 上的文章，其中提供了一个阅读清单以全面理解大语言模型。经过快速搜索，我找到了这篇文章[在这里](https://sebastianraschka.com/blog/2023/llm-reading-list.html)。我没有按顺序阅读所有内容，但我打算在完成这一系列之后重新审视这个阅读清单，继续我的学习之旅。

话虽如此，让我们开始吧。要详细理解 GPT 模型，我们必须从变换器开始。变换器使用一种称为缩放点积注意力的自注意力机制。以下解释来源于这篇[关于缩放点积注意力的深刻文章](https://peterbloem.nl/blog/transformers)，我推荐阅读，以便更深入地理解。基本上，对于输入序列的每个元素（第 *i* 个元素），我们希望将输入序列乘以一个加权平均值，该加权平均值由序列中所有元素与第 *i* 个元素的点积计算得出，然后应用 softmax，以使权重值在 0 和 1 之间。在原始的“Attention is All You Need”论文中，这些输入被称为**查询**（整个序列）、**键**（第 *i* 个元素的向量）和**值**（也是整个序列）。传递给注意力机制的权重被初始化为随机值，并随着神经网络中更多的迭代而学习。

nanoGPT 实现了缩放点积注意力，并将其扩展为多头注意力，这意味着同时进行多个注意力操作。它还将其实现为 `torch.nn.Module`，这使得它可以与其他网络层组合。

```py
import torch
import torch.nn as nn
from torch.nn import functional as F

class CausalSelfAttention(nn.Module):

    def __init__(self, config):
        super().__init__()
        assert config.n_embd % config.n_head == 0
        # key, query, value projections for all heads, but in a batch
        self.c_attn = nn.Linear(config.n_embd, 3 * config.n_embd, bias=config.bias)
        # output projection
        self.c_proj = nn.Linear(config.n_embd, config.n_embd, bias=config.bias)
        # regularization
        self.attn_dropout = nn.Dropout(config.dropout)
        self.resid_dropout = nn.Dropout(config.dropout)
        self.n_head = config.n_head
        self.n_embd = config.n_embd
        self.dropout = config.dropout
        # flash attention make GPU go brrrrr but support is only in PyTorch >= 2.0
        self.flash = hasattr(torch.nn.functional, 'scaled_dot_product_attention')
        if not self.flash:
            print("WARNING: using slow attention. Flash Attention requires PyTorch >= 2.0")
            # causal mask to ensure that attention is only applied to the left in the input sequence
            self.register_buffer("bias", torch.tril(torch.ones(config.block_size, config.block_size))
                                        .view(1, 1, config.block_size, config.block_size))

    def forward(self, x):
        B, T, C = x.size() # batch size, sequence length, embedding dimensionality (n_embd)

        # calculate query, key, values for all heads in batch and move head forward to be the batch dim
        q, k, v  = self.c_attn(x).split(self.n_embd, dim=2)
        k = k.view(B, T, self.n_head, C // self.n_head).transpose(1, 2) # (B, nh, T, hs)
        q = q.view(B, T, self.n_head, C // self.n_head).transpose(1, 2) # (B, nh, T, hs)
        v = v.view(B, T, self.n_head, C // self.n_head).transpose(1, 2) # (B, nh, T, hs)

        # causal self-attention; Self-attend: (B, nh, T, hs) x (B, nh, hs, T) -> (B, nh, T, T)
        if self.flash:
            # efficient attention using Flash Attention CUDA kernels
            y = torch.nn.functional.scaled_dot_product_attention(q, k, v, attn_mask=None, dropout_p=self.dropout if self.training else 0, is_causal=True)
        else:
            # manual implementation of attention
            att = (q @ k.transpose(-2, -1)) * (1.0 / math.sqrt(k.size(-1)))
            att = att.masked_fill(self.bias[:,:,:T,:T] == 0, float('-inf'))
            att = F.softmax(att, dim=-1)
            att = self.attn_dropout(att)
            y = att @ v # (B, nh, T, T) x (B, nh, T, hs) -> (B, nh, T, hs)
        y = y.transpose(1, 2).contiguous().view(B, T, C) # re-assemble all head outputs side by side

        # output projection
        y = self.resid_dropout(self.c_proj(y))
        return y
```

让我们进一步剖析这段代码，从构造函数开始。首先，我们验证注意力头的数量（`n_heads`）是否能整除嵌入的维度（`n_embed`）。这非常重要，因为当嵌入被分成每个头的部分时，我们希望覆盖所有的嵌入空间而没有任何空隙。接下来，我们初始化两个线性层，`c_att` 和 `c_proj`：`c_att` 是保存所有用于计算缩放点积注意力的矩阵的工作空间的层，而 `c_proj` 存储计算的最终结果。在 `c_att` 中，嵌入维度被放大三倍，因为我们需要为注意力的三个主要组成部分**查询**、**键**和**值**留出空间。

我们还设置了两个丢弃层，`attn_dropout`和`resid_dropout`。丢弃层根据给定的概率随机使输入矩阵的元素失效。根据[PyTorch 文档](https://pytorch.org/docs/stable/generated/torch.nn.Dropout.html#torch.nn.Dropout)，这旨在减少模型的过拟合。`config.dropout`中的值是指定样本在丢弃层中被丢弃的概率。

我们通过验证用户是否可以访问 PyTorch 2.0 来完成构造函数，PyTorch 2.0 拥有优化版本的缩放点积注意力。如果可用，类将使用它；否则，我们将设置一个偏置掩码。这个掩码是注意力机制的可选掩码功能的一部分。[torch.tril](https://pytorch.org/docs/stable/generated/torch.tril.html) 方法生成一个将上三角部分转换为零的矩阵。当与 torch.ones 方法结合使用时，它有效地生成一个由 1 和 0 组成的掩码，注意力机制使用这个掩码生成给定样本输入的预期输出。

接下来，我们深入研究类的`forward`方法，其中应用了注意力算法。最初，我们确定输入矩阵的大小，并将其分成三个维度：**B**atch 大小，**T**ime（或样本数量），**C**orpus（或嵌入大小）。nanoGPT 采用批处理学习过程，我们将在检查使用此注意力层的变换器模型时详细探讨。现在，了解我们处理的是批量数据就足够了。然后，我们将输入`x`传递到线性变换层`c_attn`，它将维度从`n_embed`扩展到三倍的`n_embed`。该变换的输出被分割成我们的`q`、`k`和`v`变量，这些变量是注意力算法的输入。随后，`view`方法被用来将这些变量中的数据重新组织成 PyTorch `scaled_dot_product_attention` 函数期望的格式。

当优化后的函数不可用时，代码会默认使用手动实现的缩放点积注意力。它首先计算`q`和`k`矩阵的点积，其中`k`被转置以适应点积函数，然后结果会按`k`的大小的平方根进行缩放。接着，我们使用先前创建的偏置缓冲区来遮蔽缩放后的输出，将 0 替换为负无穷。接下来，对`att`矩阵应用[softmax 函数](https://pytorch.org/docs/stable/generated/torch.nn.Softmax.html)，将负无穷转换回 0，并确保所有其他值在 0 和 1 之间缩放。然后，我们应用一个丢弃层以避免过拟合，然后计算`att`矩阵和`v`的点积。

无论使用何种缩放点积实现，多头输出都会在通过最终的 dropout 层之前重新组织成并排的形式，然后返回结果。这是在不到 50 行的 Python/PyTorch 代码中完成的注意力层实现。如果你没有完全理解上述代码，建议花些时间复习它，然后再继续阅读文章的其余部分。

在我们深入了解整合所有内容的 GPT 模块之前，我们需要两个额外的构建块。第一个是简单的多层感知机（MLP）——在“Attention is All You Need”论文中被称为前馈网络——和注意力块，它将注意力层与 MLP 结合在一起，完成论文中表示的基本 transformer 架构。两者都在 nanoGPT 的以下代码片段中实现。

```py
class MLP(nn.Module):
    """
    Multi Layer Perceptron
    """

    def __init__(self, config):
        super().__init__()
        self.c_fc    = nn.Linear(config.n_embd, 4 * config.n_embd, bias=config.bias)
        self.gelu    = nn.GELU()
        self.c_proj  = nn.Linear(4 * config.n_embd, config.n_embd, bias=config.bias)
        self.dropout = nn.Dropout(config.dropout)

    def forward(self, x):
        x = self.c_fc(x)
        x = self.gelu(x)
        x = self.c_proj(x)
        x = self.dropout(x)
        return x

class Block(nn.Module):

    def __init__(self, config):
        super().__init__()
        self.ln_1 = LayerNorm(config.n_embd, bias=config.bias)
        self.attn = CausalSelfAttention(config)
        self.ln_2 = LayerNorm(config.n_embd, bias=config.bias)
        self.mlp = MLP(config)

    def forward(self, x):
        x = x + self.attn(self.ln_1(x))
        x = x + self.mlp(self.ln_2(x))
        return x
```

MLP 层，尽管在代码行数上看似简单，却给模型增加了一层额外的复杂性。实质上，Linear 层通过线性变换将每个输入层的元素与输出层的每个元素连接起来，传递它们之间的值。在上述代码中，我们从嵌入大小`n_embed`开始，作为参数的数量，然后在输出中将其四倍增加。这里的四倍增加是任意的；MLP 模块的目的是通过增加更多节点来增强网络的计算能力。只要在 MLP 开始时维度增加和在 MLP 结束时维度减少是等效的，得到相同的初始输入/最终输出维度，那么缩放数只是另一个超参数。另一个关键要素是激活函数。这种 MLP 实现包括两个线性层，使用 GELU 激活函数连接起来。原始论文使用的是 ReLU 函数，但 nanoGPT 使用 GELU 以确保与 GPT2 模型检查点的兼容性。

接下来，我们来研究 Block 模块。这个模块完成了我们在“Attention”论文中概述的变压器块。基本上，它通过一个归一化层将输入传递，然后传递到注意力层，再将结果加回输入中。这个加法的输出被再次归一化，然后传递到 MLP，并再次加回到自身。这一过程实现了“Attention”论文中描述的变压器解码器部分。在文本生成中，通常只使用解码器，因为它不需要根据输入序列以外的内容来调整解码器的输出。变压器最初设计用于机器翻译，这需要考虑输入标记编码和输出标记编码。然而，在文本生成中，只使用一个标记编码，从而消除了通过编码器进行交叉注意力的需求。nanoGPT 的作者 Andrei Karpathy 在他的视频中提供了这一点的全面解释，[链接在此](https://youtu.be/kCc8FmEb1nY?t=6161)，视频地址在本系列的第一篇文章中。

最后，我们进入主要组件：GPT 模型。大约 300 行的文件中大部分内容都用于 GPT 模块。它管理一些有益的功能，如模型微调和为模型训练设计的实用工具（这是本系列下一篇文章的主题）。因此，我在下面展示了 nanoGPT 存储库中可用的简化版本。

```py
class GPT(nn.Module):

    def __init__(self, config):
        super().__init__()
        assert config.vocab_size is not None
        assert config.block_size is not None
        self.config = config

        self.transformer = nn.ModuleDict(dict(
            wte = nn.Embedding(config.vocab_size, config.n_embd),
            wpe = nn.Embedding(config.block_size, config.n_embd),
            drop = nn.Dropout(config.dropout),
            h = nn.ModuleList([Block(config) for _ in range(config.n_layer)]),
            ln_f = LayerNorm(config.n_embd, bias=config.bias),
        ))
        self.lm_head = nn.Linear(config.n_embd, config.vocab_size, bias=False)
        # with weight tying when using torch.compile() some warnings get generated:
        # "UserWarning: functional_call was passed multiple values for tied weights.
        # This behavior is deprecated and will be an error in future versions"
        # not 100% sure what this is, so far seems to be harmless. TODO investigate
        self.transformer.wte.weight = self.lm_head.weight # https://paperswithcode.com/method/weight-tying

        # init all weights
        self.apply(self._init_weights)
        # apply special scaled init to the residual projections, per GPT-2 paper
        for pn, p in self.named_parameters():
            if pn.endswith('c_proj.weight'):
                torch.nn.init.normal_(p, mean=0.0, std=0.02/math.sqrt(2 * config.n_layer))

    def _init_weights(self, module):
        if isinstance(module, nn.Linear):
            torch.nn.init.normal_(module.weight, mean=0.0, std=0.02)
            if module.bias is not None:
                torch.nn.init.zeros_(module.bias)
        elif isinstance(module, nn.Embedding):
            torch.nn.init.normal_(module.weight, mean=0.0, std=0.02)

    def forward(self, idx, targets=None):
        device = idx.device
        b, t = idx.size()
        assert t <= self.config.block_size, f"Cannot forward sequence of length {t}, block size is only {self.config.block_size}"
        pos = torch.arange(0, t, dtype=torch.long, device=device) # shape (t)

        # forward the GPT model itself
        tok_emb = self.transformer.wte(idx) # token embeddings of shape (b, t, n_embd)
        pos_emb = self.transformer.wpe(pos) # position embeddings of shape (t, n_embd)
        x = self.transformer.drop(tok_emb + pos_emb)
        for block in self.transformer.h:
            x = block(x)
        x = self.transformer.ln_f(x)

        if targets is not None:
            # if we are given some desired targets also calculate the loss
            logits = self.lm_head(x)
            loss = F.cross_entropy(logits.view(-1, logits.size(-1)), targets.view(-1), ignore_index=-1)
        else:
            # inference-time mini-optimization: only forward the lm_head on the very last position
            logits = self.lm_head(x[:, [-1], :]) # note: using list [-1] to preserve the time dim
            loss = None

        return logits, loss

    @torch.no_grad()
    def generate(self, idx, max_new_tokens, temperature=1.0, top_k=None):
        """
        Take a conditioning sequence of indices idx (LongTensor of shape (b,t)) and complete
        the sequence max_new_tokens times, feeding the predictions back into the model each time.
        Most likely you'll want to make sure to be in model.eval() mode of operation for this.
        """
        for _ in range(max_new_tokens):
            # if the sequence context is growing too long we must crop it at block_size
            idx_cond = idx if idx.size(1) <= self.config.block_size else idx[:, -self.config.block_size:]
            # forward the model to get the logits for the index in the sequence
            logits, _ = self(idx_cond)
            # pluck the logits at the final step and scale by desired temperature
            logits = logits[:, -1, :] / temperature
            # optionally crop the logits to only the top k options
            if top_k is not None:
                v, _ = torch.topk(logits, min(top_k, logits.size(-1)))
                logits[logits < v[:, [-1]]] = -float('Inf')
            # apply softmax to convert logits to (normalized) probabilities
            probs = F.softmax(logits, dim=-1)
            # sample from the distribution
            idx_next = torch.multinomial(probs, num_samples=1)
            # append sampled index to the running sequence and continue
            idx = torch.cat((idx, idx_next), dim=1)

        return idx
```

让我们从类的构造函数开始。不同的层被组装到 PyTorch 的 ModuleDict 中，这提供了一些结构。我们从两个 Embedding 层开始：一个用于标记嵌入，另一个用于位置嵌入。`nn.Embedding` 模块旨在稀疏地填充值，相比其他层模块，它优化了存储能力。接下来是一个 dropout 层，然后是 `n_layer` 个 Block 模块，这些模块构成了我们的注意力层，然后是另一个单独的 dropout 层。`lm_head` Linear 层将注意力 Blocks 的输出减少到词汇表大小，作为 GPT 的主要输出，除了损失值之外。

一旦层定义完成，在我们开始训练模块之前，还需要进行额外的设置。在这里，Andrej 将位置编码的权重链接到输出层的权重上。根据[代码注释中链接的论文](https://paperswithcode.com/method/weight-tying)，这样做是为了减少模型的最终参数，同时提高其性能。构造函数还会初始化模型的权重。由于这些权重将在训练过程中学习，它们被初始化为高斯分布的随机数，并且模块的偏置被设置为 0。最后，利用了来自[GPT-2 论文](https://www.semanticscholar.org/paper/Language-Models-are-Unsupervised-Multitask-Learners-Radford-Wu/9405cc0d6169988371b2755e573cc28650d14dfe)的修改，其中任何残差层的权重都按层数的平方根进行缩放。

在通过网络进行前向传播时，**batch**大小和样本数量（此处为`t`）从输入大小中提取。然后，我们在训练设备上为将成为**位置**嵌入的内容创建内存。接着，我们将输入标记嵌入到后续的标记嵌入中`wte`。随后，在`wpe`层计算位置嵌入。这些嵌入在经过丢弃层之前会被相加。结果会通过每一个`n_layer`块进行处理并进行归一化。最后的结果传递到线性层`lm_head`，该层将嵌入权重转换为每个词汇表中标记的概率评分。

在计算损失时（例如，在训练期间），我们通过交叉熵计算预测标记与实际标记之间的差异。如果没有损失，则为`None`。损失和标记概率都会作为前向传播函数的一部分返回。

与之前的模块不同，GPT 模块有额外的方法。对我们最相关的是生成函数，这对于任何使用过生成模型的人来说都很熟悉。给定一组输入标记`idx`、一个`max_new_tokens`数量和一个`temperature`，它会生成`max_new_tokens`个标记。让我们深入了解它是如何完成的。首先，它修剪输入标记以适应`block_size`（其他人称之为上下文长度），如有必要，从输入的末尾开始采样。接下来，这些标记被输入到网络中，输出会根据输入的`temperature`进行缩放。温度越高，模型越具创造性，也更容易产生幻觉。较高的温度还会导致输出更难预测。接着，应用 softmax 将模型输出权重转换为 0 到 1 之间的概率。然后使用采样函数从概率中选择下一个标记，并将该标记添加到输入向量中，该向量会被反馈到 GPT 模型中进行下一个字符的生成。

感谢你耐心阅读这篇详尽的文章。虽然检查注释过的源代码是理解代码片段功能的宝贵方法，但亲自操作代码的各个部分和参数仍是无可替代的。为此，我提供了来自 nanoGPT 库的完整`model.py`源代码的链接。

[](https://github.com/karpathy/nanoGPT/blob/master/model.py?source=post_page-----1635b52ae0d7--------------------------------) [## nanoGPT/model.py at master · karpathy/nanoGPT

### 最简单、最快的中型 GPT 训练/微调库。 - nanoGPT/model.py at master ·…

github.com](https://github.com/karpathy/nanoGPT/blob/master/model.py?source=post_page-----1635b52ae0d7--------------------------------)

在即将发布的文章中，我们将探索 nanoGPT 的`train.py`脚本，并在 TinyStories 数据集上训练一个字符级模型。请在 Medium 上关注我，以确保不会错过！

我利用了大量资源来创建这篇文章，其中许多资源已经在本文及上一篇文章中链接了。然而，如果我不与大家分享这些资源以便进一步探索任何话题或替代解释概念，我就会失职。

+   [《让我们从零开始构建 GPT：代码实现详解》](https://www.youtube.com/watch?v=kCc8FmEb1nY) — YouTube

+   [LLM 阅读清单](https://sebastianraschka.com/blog/2023/llm-reading-list.html) — 博客

+   [“注意力机制是你需要的一切”](https://arxiv.org/pdf/1706.03762.pdf?) — 论文

+   [“语言模型是无监督多任务学习者”](https://www.semanticscholar.org/paper/Language-Models-are-Unsupervised-Multitask-Learners-Radford-Wu/9405cc0d6169988371b2755e573cc28650d14dfe) — GPT-2 论文

+   多层感知器解释和插图 — Medium

+   [权重绑定](https://paperswithcode.com/method/weight-tying) — Papers With Code

+   [《变换器神经网络的插图指南：逐步解释》](https://www.youtube.com/watch?v=4Bdc55j80l8) — YouTube

*使用 GPT-4 和自定义 LangChain 脚本编辑。*
