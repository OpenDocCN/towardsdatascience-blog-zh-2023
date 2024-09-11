# 从零实现 LoRA

> 原文：[https://towardsdatascience.com/implementing-lora-from-scratch-20f838b046f1?source=collection_archive---------0-----------------------#2023-12-12](https://towardsdatascience.com/implementing-lora-from-scratch-20f838b046f1?source=collection_archive---------0-----------------------#2023-12-12)

## 如何从零实现 LoRA 以及一些实用技巧

[](https://medium.com/@martin.p.dittgen?source=post_page-----20f838b046f1--------------------------------)[![马丁·迪特根](../Images/b469995c47e0cc4859225d225ab373db.png)](https://medium.com/@martin.p.dittgen?source=post_page-----20f838b046f1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----20f838b046f1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----20f838b046f1--------------------------------) [马丁·迪特根](https://medium.com/@martin.p.dittgen?source=post_page-----20f838b046f1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F733d27d88c18&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplementing-lora-from-scratch-20f838b046f1&user=Martin+Dittgen&userId=733d27d88c18&source=post_page-733d27d88c18----20f838b046f1---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----20f838b046f1--------------------------------) ·17分钟阅读·2023年12月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F20f838b046f1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplementing-lora-from-scratch-20f838b046f1&user=Martin+Dittgen&userId=733d27d88c18&source=-----20f838b046f1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F20f838b046f1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplementing-lora-from-scratch-20f838b046f1&source=-----20f838b046f1---------------------bookmark_footer-----------)![](../Images/503b0cc8bc85127a81c5479c847024b8.png)

*由 DALLE 创建的 LoRA 抽象艺术表现*

在这篇博客文章中，我将向你展示如何从零开始实现 LoRA。

LoRA，即*低秩适配*（*Low-Rank Adaptation*）或*低秩适配器*（*Low-Rank Adaptors*），提供了一种高效且轻量级的方法来微调现有的语言模型。这包括像*BERT*和*RoBERTa*这样的掩码语言模型，以及像*GPT*、*Llama*和*Mistral*这样的因果（或聊天机器人）模型。

低秩适配器的主要优点之一是其高效性。通过使用更少的参数，LoRA 显著降低了计算复杂性和内存使用。这使我们能够在消费级 GPU 上训练大型模型，并轻松将我们紧凑（以兆字节为单位）的 LoRA 分发给他人。

此外，LoRA 可以提高泛化性能。通过限制模型复杂性，它们有助于防止过拟合，特别是在训练数据有限的情况下。这导致模型更能适应新的、未见过的数据，或者至少保留其初始训练任务的知识。

此外，低秩适配器可以无缝集成到现有的神经网络架构中。这种集成允许在最小的额外训练成本下进行预训练模型的微调和适应，使其非常适用于迁移学习应用。

我们将首先深入探讨 LoRA 的功能，然后我将演示如何为 *RoBERTa* 模型从头开始开发它，并使用 *GLUE* 和 *SQuAD* 基准测试我们的实现，并讨论一般的技巧和改进。

# LoRA 的工作原理

LoRA 的基本理念是保持预训练的矩阵（即原始模型的参数）冻结（即保持固定状态），只向原始矩阵添加一个小的 delta，其参数比原始矩阵少。

例如考虑矩阵 *W*，它可以是完全连接层的参数，或者是 transformer 的自注意机制中的一个矩阵之一：

![](../Images/1acfe2d8a621aeb34bb51fe08b784d9c.png)

显然，如果***W****-orig* 的尺寸为 *n×m*，我们只需初始化一个具有相同尺寸的新的 delta 矩阵进行微调，我们将毫无收获；相反，我们将会增加参数的数量。

这个技巧在于通过从较低维度矩阵 *B* 和 *A* 的矩阵乘法构建 **ΔW** 比原始矩阵更少*“维度化”*。

![](../Images/9ad4155288d76a9fbc316fef34fc12bf.png)

我们首先定义一个秩 *r*，要显著小于基本矩阵的维度 *r≪n* 和 *r≪m*。然后矩阵 ***B*** 是 *n×r*，矩阵 ***A*** 是 *r×m*。将它们相乘得到一个具有相同尺寸的 ***W*** 矩阵，但是是由较低参数数量构建而成的。

显然，我们希望在训练开始时我们的 delta 是零，这样微调才能像原始模型一样开始。因此，***B*** 通常初始化为全零，***A*** 初始化为随机（通常是正态分布）值。

例如，这可能看起来像这样：

![](../Images/ea8acfaa08916bdc36979d8989788b40.png)

*一个 LoRA 可能在实际矩阵中如何看的示例*

想象一种情况，我们的基础维度是 1024，并且我们选择了 LoRA 的秩 *r* 为 4，则：

+   ***W*** 具有 1024 * 1024 ≈ 100 万个参数

+   ***A*** 和 ***B*** 每个都有 r * 1024 = 4 * 1024 ≈ 4k 个参数，总共是 8k

+   因此，我们只需要训练 0.8% 的参数来使用 LoRA 更新我们的矩阵

顺便说一句，在 LoRA 论文中，他们使用 alpha 参数对 delta 矩阵进行加权：

![](../Images/2c10bf0da1628cfad229a9daefd7501a.png)

如果你只是将*α*设置为你实验的第一个*r*并微调学习率，通常可以在以后改变*r*参数，而无需再次微调学习率（至少大致如此）。虽然在我们的实现中可以忽略这一细节，但这是许多其他LoRA库（如Hugging Face的PEFT）的常见特性。

# 实现LoRA

对于我们的实现，我们希望紧密跟随原始的LoRA论文。在那里，他们测试了变换器中实际需要替换哪些矩阵。他们发现，在对GPT-3微调任务进行不同策略比较时，仅适配自注意力机制的查询和数值向量就足够了。

注意到现在很多人忽视了这种评估，并允许每个矩阵进行微调，无论任务或模型如何（参见QLoRA论文）。

我们的实现将在PyTorch中完成，但应该很容易适配到不同的框架中。

对于这篇博客文章，我简化了一些代码，以便更容易阅读，同时仍展示了核心要素。完整代码和一些训练好的LoRA权重可以在这里找到：[https://github.com/Montinger/Transformer-Workbench](https://github.com/Montinger/Transformer-Workbench)。

# 重新实现自注意力模型

我们希望适配的模型是来自Huggingface的RoBERTa模型。最直接的方法是重新包装原始的自注意力机制`RobertaSelfAttention`。新类`LoraRobertaSelfAttention`将初始化LoRA矩阵。所有的B矩阵将初始化为零，所有的A矩阵将用正态分布的随机数初始化。

```py
class LoraRobertaSelfAttention(RobertaSelfAttention):
    """
    Extends RobertaSelfAttention with LoRA (Low-Rank Adaptation) matrices.
    LoRA enhances efficiency by only updating the query and value matrices.
    This class adds LoRA matrices and applies LoRA logic in the forward method.

    Parameters:
    - r (int): Rank for LoRA matrices.
    - config: Configuration of the Roberta Model.
    """
    def __init__(self, r=8, *args, **kwargs):
        super().__init__(*args, **kwargs)
        d = self.all_head_size

        # Initialize LoRA matrices for query and value
        self.lora_query_matrix_B = nn.Parameter(torch.zeros(d, r))
        self.lora_query_matrix_A = nn.Parameter(torch.randn(r, d))
        self.lora_value_matrix_B = nn.Parameter(torch.zeros(d, r))
        self.lora_value_matrix_A = nn.Parameter(torch.randn(r, d))
```

给定这些矩阵，我们现在定义新的类方法`lora_query`和`lora_value`。这些方法计算***ΔW***矩阵，即***BA***，并将其添加到原始矩阵中，这些原始矩阵由原始方法`query`和`value`调用。

```py
class LoraRobertaSelfAttention(RobertaSelfAttention):
    # ...

    def lora_query(self, x):
        """
        Applies LoRA to the query component. Computes a modified query output by adding 
        the LoRA adaptation to the standard query output. Requires the regular linear layer 
        to be frozen before training.
        """
        lora_query_weights = torch.matmul(self.lora_query_matrix_B, self.lora_query_matrix_A)
        return self.query(x) + F.linear(x, lora_query_weights)

    def lora_value(self, x):
        """
        Applies LoRA to the value component. Computes a modified value output by adding 
        the LoRA adaptation to the standard value output. Requires the regular linear layer 
        to be frozen before training.
        """
        lora_value_weights = torch.matmul(self.lora_value_matrix_B, self.lora_value_matrix_A)
        return self.value(x) + F.linear(x, lora_value_weights)
```

现在是难看的部分：为了使用这些方法，我们必须重写`RobertaSelfAttention`的原始前向函数。虽然这有点硬编码（参见后续改进讨论），但其实很简单。首先，我们从[https://github.com/huggingface/transformers/blob/main/src/transformers/models/roberta/modeling_roberta.py](https://github.com/huggingface/transformers/blob/main/src/transformers/models/roberta/modeling_roberta.py)复制原始前向代码。然后我们将每个`query`调用替换为`lora_query`，每个`value`调用替换为`lora_value`。函数看起来如下：

```py
class LoraRobertaSelfAttention(RobertaSelfAttention):
    # ...
    def forward(self, hidden_states, *args, **kwargs):
        """Copied from
https://github.com/huggingface/transformers/blob/main/src/transformers/models/roberta/modeling_roberta.py
        but replaced the query and value calls with calls to the
        lora_query and lora_value functions.
        We will just sketch of how to adjust this here. 
        Change every call to self.value and self.query in the actual version.
        """
        # original code for query:
        ## mixed_query_layer = self.query(hidden_states)
        # updated query for LoRA:
        mixed_query_layer = self.lora_query(hidden_states)

        # The key has no LoRA, thus leave these calls unchanged
        key_layer = self.transpose_for_scores(self.key(hidden_states))

        # original code for value:
        ## value_layer = self.transpose_for_scores(self.value(hidden_states))
        # updated value for LoRA:
        value_layer = self.transpose_for_scores(self.lora_value(hidden_states))

        # ... (rest of the forward code, unchanged)
```

轰隆隆，我们完成了：我们的LoRA自注意力实现。现在唯一剩下的任务是将原始RoBERTa模型中的注意力模块替换出来。

# 替换模块

很好，我们已经用我们自己的实现替换了自注意力；但是我们如何将这个新类加入旧的RoBERTa模型中呢？实质上，我们必须遍历RoBERTa模型的每个命名组件，检查它是否是`RobertaSelfAttention`类，如果是，则替换为`LoraRobertaSelfAttention`，同时确保保留原始的权重矩阵。

为了实现这一点，我们将编写一个新的包装函数来进行此替换。此外，我们还希望稍后在一些实际任务上对RoBERTa模型进行微调。

```py
class LoraWrapperRoberta(nn.Module):
    def __init__(self, task_type, num_classes=None, dropout_rate=0.1, model_id="roberta-large",
                 lora_rank=8, train_biases=True, train_embedding=False, train_layer_norms=True):
        """
        A wrapper for RoBERTa with Low-Rank Adaptation (LoRA) for various NLP tasks.
        - task_type: Type of NLP task ('glue', 'squad_v1', 'squad_v2').
        - num_classes: Number of classes for classification (varies with task).
        - dropout_rate: Dropout rate in the model.
        - model_id: Pre-trained RoBERTa model ID.
        - lora_rank: Rank for LoRA adaptation.
        - train_biases, train_embedding, train_layer_norms: 
            Flags whether to keep certain parameters trainable 
            after initializing LoRA.

        Example:
            model = LoraWrapperRoberta(task_type='glue')
        """
        super().__init__()
        # 1\. Initialize the base model with parameters
        self.model_id = model_id
        self.tokenizer = RobertaTokenizer.from_pretrained(model_id)
        self.model = RobertaModel.from_pretrained(model_id)
        self.model_config = self.model.config

        # 2\. Add the layer for the benchmark tasks
        d_model = self.model_config.hidden_size
        self.finetune_head_norm = nn.LayerNorm(d_model)
        self.finetune_head_dropout = nn.Dropout(dropout_rate)
        self.finetune_head_classifier = nn.Linear(d_model, num_classes)

        # 3\. Set up the LoRA model for training
        self.replace_multihead_attention()
        self.freeze_parameters_except_lora_and_bias()
```

正如您所见，我们在初始化中调用了两个辅助方法：

1.  `self.replace_multihead_attention`：这将使用我们之前编写的`LoraRobertaSelfAttention`替换所有神经网络部分的注意力。

1.  `self.freeze_parameters_except_lora_and_bias`：这将冻结所有主要参数，以便在训练中仅应用于LoRA参数以及我们希望保持可训练的其他偏置和层归一化参数。

```py
class LoraWrapperRoberta(nn.Module):
    # ...

    def replace_multihead_attention_recursion(self, model):
        """
        Replaces RobertaSelfAttention with LoraRobertaSelfAttention in the model.
        This method applies the replacement recursively to all sub-components.

        Parameters
        ----------
        model : nn.Module
            The PyTorch module or model to be modified.
        """
        for name, module in model.named_children():
            if isinstance(module, RobertaSelfAttention):
                # Replace RobertaSelfAttention with LoraRobertaSelfAttention
                new_layer = LoraRobertaSelfAttention(r=self.lora_rank, config=self.model_config)
                new_layer.load_state_dict(module.state_dict(), strict=False)
                setattr(model, name, new_layer)
            else:
                # Recursive call for child modules
                self.replace_multihead_attention_recursion(module)
```

我们必须递归循环遍历所有模型部分，在PyTorch中，这些部分（实际上是RoBERTa的一部分）可以打包到一个单独的PyTorch模块中。

现在我们必须冻结所有不想再训练的参数：

```py
class LoraWrapperRoberta(nn.Module):
    # ...

    def freeze_parameters_except_lora_and_bias(self):
        """
        Freezes all model parameters except for specific layers and types based on the configuration.
        Parameters in LoRA layers, the finetune head, bias parameters, embeddings, and layer norms 
        can be set as trainable based on class settings.
        """
        for name, param in self.model.named_parameters():
            is_trainable = (
                "lora_" in name or
                "finetune_head_" in name or
                (self.train_biases and "bias" in name) or
                (self.train_embeddings and "embeddings" in name) or
                (self.train_layer_norms and "LayerNorm" in name)
            )
            param.requires_grad = is_trainable
```

此外，我们还必须实现前向方法，以考虑我们将在其上进行微调的任务，以及两种保存和加载LoRA权重的方法，以便我们可以加载先前训练模型的适配器。

悬念：有一种方法，可以让代码变得更加简洁，并且更容易推广到其他网络架构（因为我们的代码相对于RoBERTa模型而言相当硬编码）。你能想到这可能是什么吗？在下面的*可能的改进*部分讨论之前，你有时间思考这个问题。但在此之前：让我们测试一些基准，看看我们的实现是否真的有效。

# 使用GLUE和SQuAD进行基准测试结果

我们的实现现在已准备好使用GLUE（通用语言理解评估）和SQuAD（斯坦福问答数据集）基准进行评估。

GLUE基准测试是一套八项多样化的NLP任务，评估语言模型的全面理解能力。它包括情感分析、文本蕴涵和句子相似性等挑战，提供了模型语言适应能力和熟练度的强有力衡量。

另一方面，SQuAD侧重于评估问答模型。它涉及从维基百科段落中提取答案，模型识别相关的文本片段。更高级的版本SQuAD v2引入了无法回答的问题，增加了复杂性，模拟了现实中模型必须识别文本缺失答案的情况。

请注意，对于以下基准测试，我没有调整任何超参数，没有进行多次运行（特别是较小的GLUE数据集容易受到随机噪声的影响），没有进行任何早停策略，并且没有从前一个GLUE任务的精细调整开始（通常用于减少小数据集噪声的可变性和防止过拟合）。

所有运行：

+   从RoBERTa-base模型中刚初始化的LoRA注入开始，其秩为8

+   每个任务确切地进行了6个epoch的训练，没有任何早停策略。

+   在前2个epoch期间，学习率线性增加到最大值，然后在剩余的4个epoch期间线性衰减至零。

+   所有任务的最大学习率为5e-4。

+   所有任务的批处理大小为16

RoBERTa-base模型有1.246亿个参数。包括LoRA参数、偏差和层规范化，我们只有42万个未冻结参数需要训练。这意味着我们实际上只对原始参数的0.34%进行了训练。

LoRA为这些特定任务引入的参数数量非常少，实际磁盘大小仅为1.7 MB。您可以在Git仓库的*Output*文件夹中找到训练过的LoRA。

训练后，我们重新加载了LoRA参数，重新应用它们，并在每个任务的验证集上测试性能。以下是结果：

![](../Images/e92c5face32556fe13d7a5f16513bf11.png)

使用LoRA在GLUE基准测试中的性能

![](../Images/a5ccaa0cbca95310a416bd7cb67f45e4.png)

使用LoRA在SQuAD数据集上的性能

很可能这些结果可以通过一些超参数的微调大大改善。尽管如此，这清楚地证明了我们的LoRA实现是有效的，我们注入的低秩矩阵正在学习中。

# 可能的改进

回顾我们的实现，人们可能会想：“是否存在比重新编码自注意力类和执行复杂替换更有效、更可推广（即适用于其他网络架构）的方法？”

实际上，我们可以简单地在pytorch的`nn.Linear`函数周围实现一个包装器，并具体说明我们想要替换的层的名称。同样地，您可以编写包装器来适应大多数基本的pytorch层，并能够快速调整LoRA以适应新的网络架构。以下是如何快速实现这一点的简要草图：

```py
class LoraLinear(nn.Linear):
    """
    Extends a PyTorch linear layer with Low-Rank Adaptation (LoRA).
    LoRA adds two matrices to the layer, allowing for efficient training of large models.
    """
    def __init__(self, in_features, out_features, r=8, *args, **kwargs):
        super().__init__(in_features, out_features, *args, **kwargs)

        # Initialize LoRA matrices
        self.lora_matrix_B = nn.Parameter(torch.zeros(out_features, r))
        self.lora_matrix_A = nn.Parameter(torch.randn(r, in_features))

        # Freeze the original weight matrix
        self.weight.requires_grad = False

    def forward(self, x: Tensor) -> Tensor:
        # Compute LoRA weight adjustment
        lora_weights = torch.matmul(self.lora_matrix_B, self.lora_matrix_A)
        # Apply the original and LoRA-adjusted linear transformations
        return super().forward(x) + F.linear(x, lora_weights)
```

实际上，这接近了huggingface PEFT（参数高效微调）库实现LoRA的方式。对于任何实际应用场景，如果您不打算学习，我强烈建议使用它，而不是编写自己的代码。

同样，将 LoRA 注入所有线性层（即自注意力的所有矩阵以及全连接前向网络的两个线性层）也已成为一种相当常见的做法。通常，除了 LoRA 参数外，保持偏置和层归一化可训练也是个好主意。由于它们已经很小，你不需要对它们进行低秩注入。

量化原始矩阵权重以节省 GPU VRAM 也是明智的，这样可以在给定 GPU 上训练更大的模型。这可以通过使用 bits-and-bytes 库有效完成，该库现在已完全与 Hugging Face 集成（见参考文献）。

总结一下，这里是在严肃环境中低秩适配的五大法则：

![](../Images/6222aa9676676f2a24a822f2250a2bd2.png)

*低秩适配的五大法则*

如果你觉得刻有法则的石碑难以阅读，这里以纯文本重新呈现：

> 低秩适配的五大法则
> 
> 1\. 利用 LoRA 高效地对模型进行微调，重点保持参数规模最小。
> 
> 2\. 使用 PEFT 库进行 LoRA 实现，避免复杂的编码工作。
> 
> 3\. 将 LoRA 适配扩展到所有线性层，增强整体模型能力。
> 
> 4\. 保持偏置和层归一化可训练，因为它们对模型适应性至关重要，不需要低秩适配。
> 
> 5\. 应用量化 LoRA — QLoRA — 以保护 GPU VRAM 并训练模型，使训练更大模型成为可能。

请记住，使用 QLoRA 训练可能比 LoRA 慢一些，因为它涉及在每次乘法期间对矩阵进行反量化。例如，在微调像 Llama-7B 这样的大型模型时，QLoRA 需要约 75% 更少的 VRAM，但比标准 LoRA 慢大约 40%。更多见解，请查看我在参考文献中链接的博客文章。

# PEFT 实现的逐步指南

让我们看看如何真正遵守我们的法则，并通过 PEFT 实现更好的版本。

首先，让我们以量化的方式加载模型。得益于 bitsandbytes 与 Huggingface transformers 库的集成（于 2023 年 5 月推出），这变得非常简单。

我们必须指定一个配置文件，然后直接从 huggingface 加载模型以进行量化。一般来说，最好使用 transformers 中的*AutoModel*对象。将量化模型作为较大、新定义的`nn.module`对象的子模块加载是困难的。你通常应该使用 huggingface 的原始模型，因此直接导入 GLUE 任务的`AutoModelForSequenceClassification`和 SQuAD 基准的`AutoModelForQuestionAnswering`。在配置中，我们还可以指定不进行量化的参数：在这里，我们必须注册分类或 qa 输出头，因为我们希望对这些头进行完整的训练，即不使用 LoRA，因为这些头是为微调而新初始化的，且从未成为预训练基础模型的一部分。

```py
import bitsandbytes as bnb
from transformers import AutoModel, AutoModelForSequenceClassification, BitsAndBytesConfig

# Configuration to load a quantized model
bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,  # Enable 4-bit loading
    bnb_4bit_quant_type="nf4",
    bnb_4bit_compute_dtype=torch.bfloat16,
    llm_int8_skip_modules=['classifier', 'qa_outputs'],  # Skip these for quantization
)

# Load the model from Huggingface with quantization
model = AutoModelForSequenceClassification.from_pretrained('roberta-base',
          torch_dtype="auto", quantization_config=bnb_config)
```

你可以通过检查模型的模块和参数数据类型来验证4位加载：

```py
# Verify 4-bit loading
print("Verifying 4-bit elements (Linear4bit) in the attention layer:")
print(model.roberta.encoder.layer[4].attention)

print("Checking for uint8 data type:")
print(model.roberta.encoder.layer[4].attention.self.query.weight.dtype)
```

现在开始使用PEFT注入LoRA参数。请注意，PEFT库在处理自定义模型或其他复杂结构时更加灵活，因此只要您只进行LoRA而不是QLoRA（量化通常是棘手的部分）。

PEFT库通过它们的名称来定位要替换的模块；因此，我们必须查看模型的`model.named_parameters()`。这是在非量化roberta-base模型中的样子。

```py
Module                                                        Parameters
----------------------------------------------------------  ------------
roberta.embeddings.word_embeddings.weight                     38_603_520
roberta.embeddings.position_embeddings.weight                    394_752
roberta.embeddings.token_type_embeddings.weight                      768
roberta.embeddings.LayerNorm.weight                                  768
roberta.embeddings.LayerNorm.bias                                    768
roberta.encoder.layer.0.attention.self.query.weight              589_824
roberta.encoder.layer.0.attention.self.query.bias                    768
roberta.encoder.layer.0.attention.self.key.weight                589_824
roberta.encoder.layer.0.attention.self.key.bias                      768
roberta.encoder.layer.0.attention.self.value.weight              589_824
roberta.encoder.layer.0.attention.self.value.bias                    768
roberta.encoder.layer.0.attention.output.dense.weight            589_824
roberta.encoder.layer.0.attention.output.dense.bias                  768
roberta.encoder.layer.0.attention.output.LayerNorm.weight            768
roberta.encoder.layer.0.attention.output.LayerNorm.bias              768
roberta.encoder.layer.0.intermediate.dense.weight              2_359_296
roberta.encoder.layer.0.intermediate.dense.bias                    3_072
roberta.encoder.layer.0.output.dense.weight                    2_359_296
roberta.encoder.layer.0.output.dense.bias                            768
roberta.encoder.layer.0.output.LayerNorm.weight                      768
roberta.encoder.layer.0.output.LayerNorm.bias                        768
roberta.encoder.layer.1.attention.self.query.weight              589_824
...
roberta.encoder.layer.11.output.LayerNorm.bias                       768
classifier.dense.weight                                          589_824
classifier.dense.bias                                                768
classifier.out_proj.weight                                         1_536
classifier.out_proj.bias                                               2
----------------------------------------------------------  ------------
TOTAL                                                        124_647_170
```

然后，我们可以指定LoRA目标以选择这些字符串。检查的方法是，如果其完整名称中包含指定的子字符串，则为真。因此，写`query`和`value`等效于我们的从头开始实现上述内容。对于密集层，我们必须更加小心，因为分类器还具有密集输出。如果我们希望微调其他密集层，我们必须通过`intermediate.dense`和`output.dense`更为具体。

所有未注入LoRA参数的参数都会自动冻结，即不会接收任何梯度更新。如果有任何我们希望以其原始形式训练的层，我们可以通过将列表传递给Lora-Config的`modules_to_save`参数来指定它们。在我们的情况下，我们想在这里添加`LayerNorm`和GLUE以及SQuAD的微调头。请注意，列表的每个元素不必匹配某个内容。我们可以简单地将`classifier`和`qa_outputs`添加到此列表中，然后拥有一个可以正确工作于两个任务的单个配置文件。

对于偏置参数，你可以使用方便的配置参数`bias`。你可以指定*all*以重新训练所有模块的所有偏置，*lora_only*以仅训练注入的偏置，或者*none*在训练期间保持所有偏置不变。

以下示例注入了一个秩为2的LoRA。我们用上面的8指定alpha参数，因为这是我们首先尝试的秩，并且应该允许我们保持从头开始示例的原始学习率。

```py
import peft

# Config for the LoRA Injection via PEFT
peft_config = peft.LoraConfig(
    r=2, # rank dimension of the LoRA injected matrices
    lora_alpha=8, # parameter for scaling, use 8 here to make it comparable with our own implementation
    target_modules=['query', 'key', 'value', 'intermediate.dense', 'output.dense'], # be precise about dense because classifier has dense too
    modules_to_save=["LayerNorm", "classifier", "qa_outputs"], # Retrain the layer norm; classifier is the fine-tune head; qa_outputs is for SQuAD
    lora_dropout=0.1, # dropout probability for layers
    bias="all", # none, all, or lora_only
)

model = peft.get_peft_model(model, peft_config)
```

请记住，为LoRA注入指定更多模块可能会增加VRAM要求。如果遇到VRAM限制，请考虑减少目标模块的数量或LoRA秩。

对于训练，特别是使用QLoRA时，选择与量化矩阵兼容的优化器。用bitsandbytes变体替换你的标准torch优化器，如下所示：

```py
import torch
import bitsandbytes as bnb

# replace this
optimizer = torch.optim.AdamW(args here)
# with this
optimizer = bnb.optim.AdamW8bit(same args here)
```

然后，您可以像以前一样训练此模型，而无需在训练过程中明确担心QLoRA。

训练完成后，保存和重新加载模型的过程非常简单。使用`model.save_pretrained`保存您的模型，并指定所需的文件名。PEFT库将在此位置自动创建一个目录，其中存储模型权重和配置文件。此文件包括基础模型和LoRA配置参数等重要细节。

要重新加载模型，请使用 `peft.AutoPeftModel.from_pretrained`，并将目录路径作为参数传递。一个关键点是，LoRA 配置当前不保留 `AutoModelForSequenceClassification` 初始化时的类别数量。在使用 `from_pretrained` 时，你需要手动输入这个类别数量作为附加参数。如果不这样做，将会导致错误。

重新加载的模型将包括应用了 LoRA 适配器的原始基础模型。如果你决定将 LoRA 适配器永久集成到基础模型矩阵中，只需执行 `model.merge_and_unload()`。

要获得更为实操的理解和详细的说明，请查看 GitHub 仓库。在那里，你会找到两个名为*Train-QLoRA-with-PEFT.ipynb*和*Load-LoRA-Weights-PEFT.ipynb*的笔记本，提供了使用 PEFT 训练和加载模型的逐步示例。

# 结论

> *“我们不会停止探索，我们所有的探索最终将是到达我们开始的地方，并第一次了解这个地方。”*
> 
> *—— 摘自 T.S. 艾略特的《小吉丁》*

这段旅程带领我们从简单的、尽管是硬编码的 LoRA 实现，深入了解了低秩适配器、它们的实际应用以及基准测试。

我们探讨了一种更高效的实现策略，并深入了解了像 PEFT 这样的现有库在 LoRA 集成中的优雅。

我们的冒险以实际的 LoRA 使用指南结束，这些指南被概括为“**五项戒律**”，确保在实际应用中有效且高效地使用这一技术，并提供了逐步实施的指南。

# 参考资料

*所有图片，除非另有说明，均由作者提供。*

+   原始 LoRA 论文: [https://arxiv.org/pdf/2106.09685.pdf](https://arxiv.org/pdf/2106.09685.pdf)

+   QLoRA 论文: [https://arxiv.org/abs/2305.14314](https://arxiv.org/abs/2305.14314)

+   Sentdex 关于 QLoRA 微调的指南: [https://www.youtube.com/watch?v=J_3hDqSvpmg](https://www.youtube.com/watch?v=J_3hDqSvpmg)

+   关于 Llama 上 LoRA 微调的博客文章: [https://www.anyscale.com/blog/fine-tuning-llms-lora-or-full-parameter-an-in-depth-analysis-with-llama-2](https://www.anyscale.com/blog/fine-tuning-llms-lora-or-full-parameter-an-in-depth-analysis-with-llama-2)

+   bitsandbytes Hugging Face 集成: [https://huggingface.co/blog/4bit-transformers-bitsandbytes](https://huggingface.co/blog/4bit-transformers-bitsandbytes)

+   LoRA 训练见解: [https://lightning.ai/pages/community/lora-insights/](https://lightning.ai/pages/community/lora-insights/)

+   细化 Llama 模型时 LoRA 与 QLoRA 的预期 VRAM 节省: [https://cloud.google.com/vertex-ai/docs/model-garden/lora-qlora](https://cloud.google.com/vertex-ai/docs/model-garden/lora-qlora)

+   我用于石板文本的字体，以防你想自己制作：[https://www.fontspace.com/sharp-objects-nbp-font-f14469](https://www.fontspace.com/sharp-objects-nbp-font-f14469)
