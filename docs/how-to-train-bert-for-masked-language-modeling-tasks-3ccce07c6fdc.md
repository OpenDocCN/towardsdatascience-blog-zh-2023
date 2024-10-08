# 如何训练 BERT 进行掩码语言建模任务

> 原文：[`towardsdatascience.com/how-to-train-bert-for-masked-language-modeling-tasks-3ccce07c6fdc`](https://towardsdatascience.com/how-to-train-bert-for-masked-language-modeling-tasks-3ccce07c6fdc)

## 从零开始使用 Python 和 Transformers 库构建 MLM 任务语言模型的实用指南

[Medium](https://ransakaravihara.medium.com/?source=post_page-----3ccce07c6fdc--------------------------------) ![Ransaka Ravihara](https://ransakaravihara.medium.com/?source=post_page-----3ccce07c6fdc--------------------------------) [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3ccce07c6fdc--------------------------------) ![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3ccce07c6fdc--------------------------------) [Ransaka Ravihara](https://ransakaravihara.medium.com/?source=post_page-----3ccce07c6fdc--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3ccce07c6fdc--------------------------------) ·阅读时长 7 分钟·2023 年 10 月 17 日

--

![](img/f2df84bc5e99738d8d9c3264785f2390.png)

# 介绍

近年来，大型语言模型（LLMs）吸引了整个机器学习社区的关注。在 LLMs 出现之前，我们经历了一个重要的研究阶段，研究了各种语言建模技术，包括掩码语言建模、因果语言建模和序列到序列语言建模。

从上述列表来看，诸如 BERT 这样的掩码语言模型在分类和聚类等下游 NLP 任务中变得更加实用。由于 Hugging Face Transformers 等库的帮助，调整这些模型以适应下游任务变得更加可及和可管理。感谢开源社区，我们有大量的语言模型可以选择，涵盖了广泛使用的语言和领域。

本教程的 GitHub 仓库

[GitHub](https://github.com/Ransaka/Train-BERT-for-Masked-Language-Modeling-Tasks?source=post_page-----3ccce07c6fdc--------------------------------) [## GitHub - Ransaka/Train-BERT-for-Masked-Language-Modeling-Tasks: GitHub repo for TDS article "How to…

### GitHub 仓库用于 TDS 文章 "如何训练 BERT 进行掩码语言建模任务" - GitHub …

[GitHub](https://github.com/Ransaka/Train-BERT-for-Masked-Language-Modeling-Tasks?source=post_page-----3ccce07c6fdc--------------------------------)

# 微调还是从头开始构建？

在将现有语言模型适应于特定用例时，有时我们可以在不进一步调整（即所谓的微调）的情况下使用现有模型。例如，如果你需要一个英语情感/意图检测模型，你可以进入 HuggingFace.co 并找到适合你用例的模型。

![](img/b7b3c524c332f97ac2b6e822de2a7ac0.png)

然而，您只能对现实世界中遇到的一些任务抱有这种期望。这就是我们需要一种称为微调的额外技术的地方。首先，您必须选择一个将要微调的基础模型。在这里，您必须小心选择的模型和目标语言的词汇相似性。

然而，如果您找不到适用于所需语言的合适模型，请考虑从头开始构建一个。在本教程中，我们将实现用于掩码语言模型的 BERT 模型。

# BERT 架构

尽管描述 BERT 架构超出了本教程的范围，但为了清晰起见，我们还是非常简略地介绍一下。BERT，或**双向编码器表示**来自**变换器**，属于仅编码器的变换器家族。它于 2018 年由 Google 的研究人员引入。

## 论文摘要：

> 我们介绍了一种新的语言表示模型，称为 BERT，代表**双向编码器表示**来自**变换器**。与最近的语言表示模型不同，BERT 旨在通过在所有层中共同考虑左右上下文来从未标记文本中预训练深度双向表示。因此，预训练的 BERT 模型可以通过添加一个额外的输出层来进行微调，从而创建广泛任务的最先进模型，如问答和语言推理，而无需对任务特定的架构进行 substantial 修改。
> 
> BERT 从概念上讲很简单，从经验上讲却非常强大。它在十一项自然语言处理任务中获得了新的最先进结果，包括将 GLUE 分数推高至 80.5%（绝对提高 7.7 个百分点），MultiNLI 准确率提升至 86.7%（绝对提高 4.6 个百分点），SQuAD v1.1 问答测试 F1 提升至 93.2（绝对提高 1.5 个点），以及 SQuAD v2.0 测试 F1 提升至 83.1（绝对提高 5.1 个点）。
> 
> 论文：[`arxiv.org/abs/1810.04805`](https://arxiv.org/abs/1810.04805)

在上面，我们可以看到一个有趣的关键词——双向。双向特性赋予了 BERT 类似于人类的能力。假设您需要填入一个像下面这样的空白，

> “战争有时可能是一种必要的恶行。但无论多么必要，它始终是一种 _____，从来不是一种好事。”

要猜测单词填入空白位置，您需要注意以下几点：空白前的单词、空白后的单词，以及整个句子的上下文。根据这种人类特性，BERT 也以相同的方式工作。在训练过程中，我们隐藏一些单词，并要求 BERT 尝试预测这些单词。训练完成后，BERT 可以根据前后单词来预测被掩码的标记。为此，模型应对输入序列中的单词分配不同的注意力，这可能会显著影响对掩码标记的预测。

![](img/b479a89ad5558c20c076ccbff0a93ddc.png)

图片来源：作者，[`huggingface.co/spaces/exbert-project/exbert`](https://huggingface.co/spaces/exbert-project/exbert)

如您所见，模型将适合隐藏位置的词视为 *evil*，并且将第一个句子的 *evil* 视为进行此预测所必需的。这是一个显著的点，表明模型理解输入序列的上下文。这种上下文感知使得 BERT 能够为给定任务生成有意义的句子嵌入。进一步，这些嵌入可以用于下游任务，如聚类和分类。说够了 BERT；让我们从头开始构建一个。

# 定义 BERT 模型

我们通常有 BERT*(base)* 和 BERT*(large)*。两者每个头部都有 64 维度。*large* 变体包含 24 个编码器层，而 *base* 变体只有 12 层。不过，我们并不局限于这些配置。令人惊讶的是，我们可以完全控制使用 Hugging Face Transformers 库定义模型。我们需要做的就是使用 *BertConfig* 类定义所需的模型配置。

我选择了 6 个头和 384 个总模型维度，以符合原始实现。这样，每个头都有 64 个维度，类似于原始实现。让我们初始化我们的 BERT 模型。

```py
from transformers import BertConfig, BertForMaskedLM

config = BertConfig(
    hidden_size = 384,
    vocab_size= tokenizer.vocab_size,
    num_hidden_layers = 6,
    num_attention_heads = 6,
    intermediate_size = 1024,
    max_position_embeddings = 256
)

model = BertForMaskedLM(config=config)
print(model.num_parameters()) #10457864
```

# 训练标记器

在这里，我不会描述标记化的底层工作原理。相反，让我们使用 Hugging Face tokenizers 库从零开始训练一个标记器。请注意，原始 BERT 实现中使用的标记器是 WordPiece 标记器，这是一种基于子词的标记化方法。您可以通过下面的 HuggingFace 资源深入了解这种标记化。

[## WordPiece tokenization — Hugging Face NLP Course](https://huggingface.co/learn/nlp-course/chapter6/6?fw=pt&source=post_page-----3ccce07c6fdc--------------------------------)

### 我们正在通过开源和开放科学来推进和普及人工智能。

[huggingface.co](https://huggingface.co/learn/nlp-course/chapter6/6?fw=pt&source=post_page-----3ccce07c6fdc--------------------------------)

这里使用的数据集是 Sinhala-400M 数据集 *(under apache-2.0)*。您可以使用任何您拥有的数据集来进行相同的操作。

![](img/377b2ebfccb2a7f8d25df41b71c6627c.png)

正如您可能注意到的，有些僧伽罗语单词也用英语键入。让我们为这些语料库训练一个标记器。

首先让我们导入必要的模块。使用 Hugging Face Tokenizers 库训练标记器的好处是，我们可以使用现有的标记器并仅根据我们的训练语料库替换词汇表（并在适用时合并）。这意味着标记化步骤，如预标记化和后标记化，将被保留。为此，我们可以使用 *train_new_from_iterator* 方法来自 *BertTokenizer* 类。

```py
from tokenizers.implementations import ByteLevelBPETokenizer
from tokenizers.processors import BertProcessing
from transformers import AutoTokenizer
from datasets import Dataset
import pandas as pd

#load base tokenizer to train on dataset
tokenizer_base = AutoTokenizer.from_pretrained("bert-base-cased")
# convert pandas dataset to HF dataset
dataset = Dataset.from_pandas(df.rename(columns={"comment":'text'}))

# define iterator
training_corpus = (
    dataset[i : i + 1000]["text"]
    for i in range(0, len(dataset), 1000)
)

#train the new tokenizer for dataset
tokenizer = tokenizer_base.train_new_from_iterator(training_corpus, 5000)
#test trained tokenizer for sample text
text = dataset['text'][123]
print(text)
```

![](img/c66a51e3ac4dad4f8e2c5244908f2549.png)

```py
# let's check tokenization process
input_ids = tokenizer(text).input_ids
subword_view = [tokenizer.convert_ids_to_tokens(id) for id in input_ids]
np.array(subword_view)
```

![](img/a9a95cf24c4b15bff37110a33f2e4635.png)

你可以看到像“cricketer”这样的词被分解为*cricket*和*##er*，这表明标记器已经经过充分训练。然而，可以尝试不同的词汇大小；我的词汇量是 5000，相对较小，但适合这个玩具示例。

最终，我们可以将训练好的标记器保存到我们的目录中。

```py
tokenizer.save_pretrained("tokenizer/sinhala-wordpiece-yt-comments")
```

# 定义数据整理器并标记化数据集。

让我们定义一个用于 MLM 任务的整理器。在这里，我们将掩盖 15%的标记。无论如何，我们可以设置不同的掩盖概率。

```py
from transformers import DataCollatorForLanguageModeling

data_collator = DataCollatorForLanguageModeling(
    tokenizer=tokenizer, mlm=True, mlm_probability=0.15
)
```

让我们使用之前创建的标记器对数据集进行标记化。我用自定义类替代了原始的*LineByLineTextDataset*，利用了 Hugging Face accelerate。

```py
import torch
from torch.utils.data import Dataset
from accelerate import Accelerator, DistributedType

class LineByLineTextDataset(Dataset):
    def __init__(self, tokenizer, raw_datasets, max_length: int):
        self.padding = "max_length"
        self.text_column_name = 'text'
        self.max_length = max_length
        self.accelerator = Accelerator(gradient_accumulation_steps=1)
        self.tokenizer = tokenizer

        with self.accelerator.main_process_first():
            self.tokenized_datasets = raw_datasets.map(
                self.tokenize_function,
                batched=True,
                num_proc=4,
                remove_columns=[self.text_column_name],
                desc="Running tokenizer on dataset line_by_line",
            )
            self.tokenized_datasets.set_format('torch',columns=['input_ids'],dtype=torch.long)

    def tokenize_function(self,examples):
        examples[self.text_column_name] = [
            line for line in examples[self.text_column_name] if len(line[0]) > 0 and not line[0].isspace()
        ]
        return self.tokenizer(
            examples[self.text_column_name],
            padding=self.padding,
            truncation=True,
            max_length=self.max_length,
            return_special_tokens_mask=True,
        )
    def __len__(self):
        return len(self.tokenized_datasets)

    def __getitem__(self, i):
        return self.tokenized_datasets[i]
```

让我们对数据集进行标记化。

```py
tokenized_dataset_train = LineByLineTextDataset(
    tokenizer= tokenizer,
    raw_datasets = dataset,
    max_length=256,
)
```

好的，让我们编写训练循环代码。

```py
from transformers import Trainer, TrainingArguments

training_args = TrainingArguments(
    output_dir="./model",
    overwrite_output_dir=True,
    push_to_hub=True,
    hub_model_id="Ransaka/sinhala-bert-yt-comments",
    num_train_epochs=2,
    per_device_train_batch_size=32,
    save_steps=5_000,
    logging_steps = 1000,
    save_total_limit=2,
    use_mps_device = True, # disable this if you're running non-mac env
    hub_private_repo = False, # please set true if you want to save model privetly
    save_safetensors= True,
    learning_rate = 1e-4,
    report_to='wandb'
)

trainer = Trainer(
    model=model,
    args=training_args,
    data_collator=data_collator,
    train_dataset=tokenized_dataset_train
)
```

我们可以使用其*train()*方法调用训练器。

```py
trainer.train()
```

![](img/05feb8f3954884ffa738ba0f551bdaee.png)

在充分训练后，我们的模型可以用于下游任务，如零样本分类和聚类。你可以通过使用这个 Hugging Face 空间的示例获得更多详细信息。

[](https://huggingface.co/spaces/Ransaka/sinhala-embedding-space?source=post_page-----3ccce07c6fdc--------------------------------) [## Sinhala Embedding Space - Ransaka 的 Hugging Face 空间

### 发现由社区制作的惊人 ML 应用

huggingface.co](https://huggingface.co/spaces/Ransaka/sinhala-embedding-space?source=post_page-----3ccce07c6fdc--------------------------------)

# 结论

在资源有限的情况下，预训练模型可能只识别特定的语言模式，但它们在特定的使用案例中仍然很有用。强烈建议在可能的情况下进行微调。

*在本文中，除非另有说明，否则所有图片均为作者提供。*

# **参考文献**

1.  一个可探究的 BERT — [`huggingface.co/spaces/exbert-project/exbert`](https://huggingface.co/spaces/exbert-project/exbert)

1.  BERT 论文 — [`arxiv.org/abs/1810.04805`](https://arxiv.org/abs/1810.04805)

1.  数据集 — [`huggingface.co/datasets/Ransaka/Sinhala-400M`](https://huggingface.co/datasets/Ransaka/Sinhala-400M)
