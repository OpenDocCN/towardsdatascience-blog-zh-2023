# 使用 Hugging Face 的 Transformer 模型构建评论毒性排序器

> 原文：[`towardsdatascience.com/building-a-comment-toxicity-ranker-using-hugging-faces-transformer-models-aa5b4201d7c6`](https://towardsdatascience.com/building-a-comment-toxicity-ranker-using-hugging-faces-transformer-models-aa5b4201d7c6)

## 赶上 NLP 和 LLM（第一部分）

[](https://medium.com/@jacky.kaub?source=post_page-----aa5b4201d7c6--------------------------------)![Jacky Kaub](https://medium.com/@jacky.kaub?source=post_page-----aa5b4201d7c6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----aa5b4201d7c6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----aa5b4201d7c6--------------------------------) [Jacky Kaub](https://medium.com/@jacky.kaub?source=post_page-----aa5b4201d7c6--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----aa5b4201d7c6--------------------------------) ·18 分钟阅读·2023 年 8 月 6 日

--

![](img/138fd036a0427c78ad23c3b834c93d74.png)

图片来源：[Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral)于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

作为一名数据科学家，我从未有机会深入探索自然语言处理的最新进展。随着夏季和今年初大语言模型的新热潮，我决定是时候深入这个领域并开始一些小项目。毕竟，没有比实践更好的学习方法了。

在我的旅程开始时，我意识到很难找到能够手把手引导读者、一步一步深入理解新 NLP 模型并通过具体项目进行的内容。因此，我决定开始这一新系列的文章。

## 使用 HuggingFace 的 Transformer 模型构建评论毒性排序器

在这篇文章中，我们将深入探讨构建评论毒性排序器。这个项目灵感来源于去年在 Kaggle 上举办的[“Jigsaw 毒性评论严重性评估”竞赛](https://www.kaggle.com/competitions/jigsaw-toxic-severity-rating)。

竞赛的目标是构建一个能够判断哪个评论（在给定的两个评论中）最具毒性的模型。

为此，模型会为每个输入的评论分配一个分数，以确定其相对毒性。

## 本文涵盖内容

在这篇文章中，我们将使用 Pytorch 和 Hugging Face transformers 训练我们的第一个 NLP 分类器。我不会深入讲解 transformers 的工作原理，而是更多地关注实际细节和实现，并引入一些对系列后续文章有用的概念。

具体来说，我们将看到：

+   如何从 Hugging Face Hub 下载模型

+   如何自定义和使用编码器

+   从 Hugging Face 模型中构建并训练一个 Pytorch 排名器

本文直接面向希望从实际角度提升其自然语言处理技能的数据科学家。我不会详细讲解变换器的理论，即使我会详细编写代码，也希望你已经对 PyTorch 有一些了解。

# 探索与架构

## 训练数据集

我们将处理一个将评论配对并将其分类为“较少毒性”和“更多毒性”的数据集。

相对毒性的选择是由一组标注者做出的。

下图显示了来自训练集的数据样本。工人字段表示进行分类的标注者的 id。

![](img/14c039579153181c775d353a0ce968ef.png)

训练集样本，作者插图

注意：数据集在开源许可证下提供，遵循[Kaggle 竞赛规则](https://www.kaggle.com/competitions/jigsaw-toxic-severity-rating/rules)。

## 排名系统

在任何机器学习项目中，理解任务具有至关重要的意义，因为它显著影响了合适模型和策略的选择。这种理解应从项目启动时就建立起来。

在这个具体的项目中，我们的目标是构建一个排名系统。与其预测一个具体的目标，我们的重点是确定一个任意值，以便在样本对之间进行有效比较。

让我们首先绘制一个基本的图示来表示这个概念，知道我们稍后会更深入地探讨“模型”的工作原理。

![](img/22b9222296a975270c41dc9347545820.png)

我们想要实现的一个非常基本的视图

以这种方式可视化任务至关重要，因为它表明项目的目标不仅仅是基于训练数据训练一个简单的二分类器。与其仅仅预测 0 或 1 来识别最有毒的评论，排名系统旨在分配任意值，从而有效地比较评论。

## 模型训练与边际排名损失

考虑到“模型”仍然是一个黑箱神经网络，我们需要建立一种利用这个系统并利用我们的训练数据来更新模型权重的方法。为此，我们需要一个合适的损失函数。

鉴于我们的目标是构建一个排名系统，[边际排名损失](https://pytorch.org/docs/stable/generated/torch.nn.MarginRankingLoss.html)是一个相关的选择。这个损失函数受到铰链损失的启发，后者通常用于优化样本之间的最大边际。

边际排名损失对样本对进行操作。对于每一对样本，它比较“模型”对两个样本产生的分数，并强制它们之间有一个边际。这个边际表示正确排序的样本之间期望的分数差异。

![](img/ea8968cc4c66377b1036e886f2383d8a.png)

Margin Ranking Loss 函数公式，作者插图

在上述公式中，x1 和 x2 是两个样本的排名得分，y 是一个系数，如果 x1 应该排名高于 x2，则 y 等于 1，否则为 -1。 “margin” 是公式的超参数，设置了需要达到的最小间隔。

让我们看看这个损失函数是如何工作的：

假设 y=1，这意味着与 x1 相关的样本应该比与 x2 相关的样本排名更高：

1.  如果 (x1 — x2) > margin，样本 1 的得分比样本 2 的得分高出足够的间隔，则 max() 的右侧项为负数。返回的损失将等于 0，并且这两个排名之间没有惩罚。

1.  如果 (x1 — x2) < margin，这意味着 x1 和 x2 之间的间隔不足，或者更糟的是，x2 的得分高于 x1 的得分。在这种情况下，损失会更高，因为样本 2 的得分高于样本 1 的得分，这会惩罚模型。

鉴于此，我们现在可以按照如下修订我们的训练方法：

对于训练集中的一个样本（或一个批次）：

1.  将 more_toxic 消息传递给模型，得到 Rank_Score1 (x1)

1.  将 less_toxic 消息传递给模型，得到 Rank_Score2 (x2)

1.  计算 y = 1 时的 MarginRankingLoss

1.  根据计算出的损失更新模型的权重（反向传播步骤）

![](img/64ef5386783c2c1de22401c8f01997c4.png)

使用 Margin Ranking Loss 的模型训练步骤，作者插图

## 从文本到特征表示：编码器块

我们的训练程序现在已设置完成。是时候深入了解‘模型’组件本身了。在 NLP 的世界里，你会经常遇到三种主要类型的模型：编码器、解码器和编码器-解码器组合。在这一系列文章中，我们将更详细地研究这些类型的模型。

对于本特定文章的目的，我们需要一个可以将消息转换为特征向量的模型。这个向量作为输入生成最终的排名得分。这个特征向量将直接从变换器架构的编码器中派生。

我不会在这里深入理论，因为其他人已经解释得很好（我推荐 Hugging Face 的[入门课程](https://huggingface.co/learn/nlp-course/chapter1/1)，写得非常好）。只需记住这个过程的关键部分叫做注意力机制。它通过查看其他相关词，即使它们相隔很远，也帮助变换器理解文本。

有了这种架构，我们将能够调整权重，以生成我们文本的最佳向量表示，从而识别出对任务最重要的特征，并将最终层从变换器连接到一个最终节点（称为“头”），该节点将生成最终的排名得分。

让我们相应地更新我们的图示：

![](img/4c4db09f498c5f162c1b9e069d0a994f.png)

我们训练流水线的更新视图，作者插图

## 分词器

正如你从上述图表中看到的，模型内部出现了一个我们尚未提及的组件：预处理步骤。

这个预处理步骤旨在将原始文本转换为可以通过神经网络处理的内容（数字），而这就是分词器的作用。

分词器的主要功能有两个：分割（即将文本切割成片段，这些片段可以是单词、单词的一部分或字母）和索引（即将每个文本片段映射到一个唯一的值，该值在字典中引用，以便可以反向操作）。

需要记住的一件非常重要的事情是，文本的分词方式有多种，但如果你使用预训练模型，你需要使用相同的分词器，否则预训练权重将毫无意义（由于不同的分割和索引）。

另一个重要的事情是要记住，编码器只是一个神经网络。因此，它的输入需要是固定大小的，但你的输入文本不一定符合这一点。分词器允许你通过两个操作来控制你的词向量的大小：填充和截断。这也是一个重要的参数，因为一些预训练模型会使用更小或更大的输入空间。

在下面的图中，我们添加了分词器，并展示了消息如何从模块到模块进行转换。

![](img/10ba9651d96992fd16ec09b76f2c6f4f.png)

最终训练示意图，作者插图

就这样，我们已经揭示了所有需要了解的组件，以便有效地处理我们的“评论毒性排名”任务。总结上述图表：每对消息（较少毒性和较多毒性）将分别传递给模型流水线。它们将首先经过分词器、编码器和排名层，以产生一对分数。这对分数将用于计算边际排名损失，这将用于反向传播步骤中，更新编码器和最终排名层的权重，并优化它们以完成任务。

在下一部分，我们将亲自动手编写代码，使用 Hugging Face transformers 模块和 Pytorch 构建上述流水线。

# 构建、训练和评估模型

我们在前面的部分中涵盖了理论，现在是时候亲自动手，开始处理我们的模型了。

虽然过去构建和训练复杂的深度学习模型可能很复杂，但新的现代框架使其变得更简单。

## Hugging Face 是你所需的一切

Hugging Face 是一家了不起的公司，致力于使复杂的深度学习模型民主化。

它们构建了帮助你构建、加载、微调和共享复杂变换器模型的抽象。

在接下来的部分中，我们将使用他们的**transformers**包，该包提供了构建预训练 NLP 模型并用于自己任务所需的所有工具。在接下来的几周内，我们将更详细地探索该包提供的不同可能性

该包与 TensorFlow 和 PyTorch 库兼容。

首先，让我们安装 transformers 包

```py
pip install transformers
```

从 Hugging Face 获取的模型可以在他们的[Model Hub](https://huggingface.co/models)网站上找到。你可以找到各种类型的模型以及描述，以了解模型的功能、参数数量、训练数据集等。

在本文中，我们将使用架构[roberta-base](https://huggingface.co/roberta-base)，这是一个相对轻量的编码器，经过多个英文语料库的训练。

模型描述提供了大量与我们的任务相关的非常重要的信息：

+   该模型具有 125M 个参数

+   该模型已在多个英文语料库上进行过训练，这一点很重要，因为我们的评论数据集是英文的

+   该模型已经在掩蔽语言模型的目标上进行过训练，这一目标是尝试预测文本中被掩蔽的单词，并使用前后的文本进行预测，这并非总是如此（例如，GPT 等模型只使用单词前的上下文来进行预测，因为它们在推断新文本时无法看到句子的未来）。

+   该模型对大小写敏感，这意味着它会区分“WORD”和“word”。这在毒性检测器中尤为重要，因为字母的大小写是判断毒性的一个重要线索。

Hugging Face 可以为每个模型提供使用的分词器以及不同配置的基本神经网络（你可能不希望所有的权重：有时你只想限制在编码器部分，解码器部分，停留在隐藏层等）。

从 Hugging Face hub 获取的模型可以在本地克隆（这样运行会更快）或直接在代码中加载，通过使用其 repo id（例如我们案例中的 roberta-base）

## 加载和测试分词器

要加载分词器，我们可以简单地使用 transformers 包中的 AutoTokenizer 类，并指定我们想要使用的分词器

```py
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained('roberta-base')
```

为了对文本进行分词，我们可以简单地调用“encode”或“encode_plus”方法。“encode_plus”不仅会提供你文本的分词版本，还会提供一个注意力掩码，用于忽略纯填充部分的编码。

```py
text = "hello world"

tokenizer.encode_plus(
    text,
    truncation=True,
    add_special_tokens=True,
    max_length=10,
    padding='max_length'
    )
```

将返回一个字典，其中“input_ids”是编码序列，“attention_mask”用于允许变换器忽略填充的标记：

```py
{
  'input_ids': [0, 42891, 232, 2, 1, 1, 1, 1, 1, 1], 
  'attention_mask': [1, 1, 1, 1, 0, 0, 0, 0, 0, 0]
}
```

在我们使用的参数中，有：

+   max_length: 指定编码序列的最大长度

+   add_special_tokens: 向文本中添加<start>和<end>标记

+   truncation: 如果文本不适合 max_length，则会截断文本

+   padding: 添加填充标记直到 max_length

## 加载预训练模型

要加载预训练模型，Hugging Face 提供了多个类，具体取决于你的需求（你是在使用 TensorFlow 还是 Pytorch？你尝试实现什么类型的任务）。

在我们的案例中，我们将使用 AutoModel，它允许你直接加载模型架构及预训练权重。请注意，如果你使用 TensorFlow，你可以通过使用 TFAutoModel 类而不是 AutoModel 类来实现相同的功能。

AutoModel 类将直接从 [RobertaModel](https://huggingface.co/docs/transformers/model_doc/roberta#robertamodel) 加载模型架构，并加载与 Hugging Face 中的 “roberta-base” 仓库相关联的预训练权重。

至于 Tokenizer，我们可以直接从 repo-id 或本地仓库路径加载模型，通过使用 AutoModel 的 from_pretrained 方法：

```py
from transformers import AutoModel

robertaBase = AutoModel.from_pretrained("roberta-base")
```

请注意，编码器没有在特定任务上进行训练，我们不能简单地使用模型。相反，我们需要用我们的数据集进行微调。

我们可以再三检查 robertaBase 是否是 pytorch.nn.Module 的实例，并且可以集成到更复杂的 PyTorch 架构中：

```py
import pytorch.nn as torch

isinstance(robertaBase, nn.Module)
>> True
```

你也可以通过简单地打印它来检查其架构，就像你对待标准 PyTorch 模块一样：

```py
print(robertaBase)

>> RobertaModel(
  (embeddings): RobertaEmbeddings(
    (word_embeddings): Embedding(50265, 768, padding_idx=1)
    (position_embeddings): Embedding(514, 768, padding_idx=1)
    (token_type_embeddings): Embedding(1, 768)
    (LayerNorm): LayerNorm((768,), eps=1e-05, elementwise_affine=True)
    (dropout): Dropout(p=0.1, inplace=False)
  )
  (encoder): RobertaEncoder(
    (layer): ModuleList(
      (0-11): 12 x RobertaLayer(
        (attention): RobertaAttention(
          (self): RobertaSelfAttention(
            (query): Linear(in_features=768, out_features=768, bias=True)
            (key): Linear(in_features=768, out_features=768, bias=True)
            (value): Linear(in_features=768, out_features=768, bias=True)
            (dropout): Dropout(p=0.1, inplace=False)
          )
          (output): RobertaSelfOutput(
            (dense): Linear(in_features=768, out_features=768, bias=True)
            (LayerNorm): LayerNorm((768,), eps=1e-05, elementwise_affine=True)
            (dropout): Dropout(p=0.1, inplace=False)
          )
        )
        (intermediate): RobertaIntermediate(
          (dense): Linear(in_features=768, out_features=3072, bias=True)
          (intermediate_act_fn): GELUActivation()
        )
        (output): RobertaOutput(
          (dense): Linear(in_features=3072, out_features=768, bias=True)
          (LayerNorm): LayerNorm((768,), eps=1e-05, elementwise_affine=True)
          (dropout): Dropout(p=0.1, inplace=False)
        )
      )
    )
  )
  (pooler): RobertaPooler(
    (dense): Linear(in_features=768, out_features=768, bias=True)
    (activation): Tanh()
  )
)
```

## 构建自定义神经网络

这个最后的层实际上是我们在本文第一部分讨论的整个文本的向量表示，我们只需将其连接到用于排序的最终节点，以完成我们的神经网络架构。

为此，我们将通过封装 nn.Module 来简单地构建自己的自定义模块，就像我们用 PyTorch 构建经典神经网络一样。

```py
model_name = "roberta-base"
last_hidden_layer_size = 768
final_node_size = 1

class ToxicRankModel(nn.Module):

    def __init__(self, model_name, last_hidden_layer_size):
        super(ToxicRankModel, self).__init__()
        self.robertaBase = AutoModel.from_pretrained(model_name)
        self.dropout = nn.Dropout(p=0.1)
        self.rank_head = nn.Linear(last_hidden_layer_size, 1)

    def forward(self, ids, mask):        
        output = self.robertaBase(input_ids=ids,attention_mask=mask,
                         output_hidden_states=False)
        output = self.dropout(output[1])
        score= self.fc(output)
        return score

#This line check if the GPU is available, else it goes with the CPU
device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")

#After initiation, we send the model to the device
toxicRankModel = ToxicRankModel(model_name, last_hidden_layer_size)
toxicRankModel = toxicRankModel.to(device)
```

在 forward() 方法中需要注意几点：

1.  我们将两个主要输入传递给 robertBase 模型，input_ids 和 attention_mask。它们都是由 Tokenizer 生成的。

1.  AutoModel 具有参数（如 output_hidden_states）。根据你选择的参数，你可以让模型作为编码器或解码器运行，并将模型定制用于不同的 NLP 任务。

1.  你是否注意到我们在 dropout 中传递了 output[1]？这是因为基本模型提供了两个输入：

+   首先，最后的隐藏状态，它包含每个标记的上下文表示（或上下文嵌入），可以用于实体识别等任务。

+   其次，来自 Pooler 的输出，它包含整个文本的向量表示，就是我们在这里寻找的。

## 构建自定义数据集

使用 Pytorch，我们还需要创建自己的 Dataset 类，用于存储原始数据，以及 DataLoader，用于在训练过程中按批次馈送神经网络。

在使用 Pytorch 构建自定义数据集时，你必须实现两个强制性方法：

+   __len__，它给出训练数据的大小（对数据加载器来说是重要信息）

+   __getitem__，它接受原始输入（来自第“i”行）并进行预处理，以便神经网络（作为张量）可以处理

如果你记得之前部分的图示，我们实际上是在计算损失之前并行传递两个输入到模型中：less_toxic 和 more_toxic。

__getitem__ 方法将处理消息的分词，并为转换器准备输入，将分词后的输入转换为张量。

```py
class CustomDataset(Dataset):
    def __init__(self, train_df, tokenizer, max_length):

        #token list standard size
        self.length = max_length

        #Here the tokenizer will be an instance of the tokenizer
        #shown previously
        self.tokenizer = tokenizer

        #df is the training df shown in the beginning of the article
        self.more_toxic = train_df['more_toxic'].values
        self.less_toxic = train_df['less_toxic'].values

    def __len__(self):
        return len(self.more_toxic)

    def __getitem__(self, i):
        # get both messages at index i
        message_more_toxic = self.more_toxic[i]
        message_less_toxic = self.less_toxic[i]

        #tokenize the messages
        dic_more_toxic = self.tokenizer.encode_plus(
                                message_more_toxic,
                                truncation=True,
                                add_special_tokens=True,
                                max_length=self.length,
                                padding='max_length'
                            )
        dic_less_toxic = self.tokenizer.encode_plus(
                                message_less_toxic,
                                truncation=True,
                                add_special_tokens=True,
                                max_length=self.length,
                                padding='max_length'
                            )

        #extract tokens and masks
        tokens_more_toxic = dic_more_toxic['input_ids']
        mask_more_toxic = dic_more_toxic['attention_mask']

        tokens_less_toxic = dic_less_toxic['input_ids']
        mask_less_toxic = dic_less_toxic['attention_mask']

        #return a dictionnary of tensors
        return {
            'tokens_more_toxic': torch.tensor(tokens_more_toxic, dtype=torch.long),
            'mask_more_toxic': torch.tensor(mask_more_toxic, dtype=torch.long),
            'tokens_less_toxic': torch.tensor(tokens_less_toxic, dtype=torch.long),
            'mask_less_toxic': torch.tensor(mask_less_toxic, dtype=torch.long),
        }
```

我们现在可以生成 DataLoader，用于模型的批量训练。

```py
def get_loader(df, tokenizer, max_length, batch_size):

    dataset = CustomDataset(
        df, 
        tokenizer=tokenizer, 
        max_length=max_length
    )

    return DataLoader(
        dataset, 
        batch_size=batch_size, 
        shuffle=True,
        drop_last=True)

max_length = 128
batch_size = 32
train_loader = get_loader(train_df, tokenizer, max_length, batch_size=batch_size)
```

+   batch_size 指定了用于前向传递/反向传播的样本数量

+   shuffle = True 意味着数据集在两个 epoch 之间会被打乱

+   drop_last 意味着如果最后一个 batch 没有正确数量的样本，它将被丢弃。这一点很重要，因为 batch normalization 对于不完整的 batch 处理效果不好。

## 训练模型

我们快完成了，现在是时候为一个 epoch 准备训练流程了。

**自定义损失**

首先，让我们定义一个自定义损失函数。 Pytorch 已经提供了 MarginRankingLoss，我们只是将其封装为 y = 1（因为我们将始终将 more_toxic 作为 x1，less_toxic 作为 x2）。

```py
from torch.nn import MarginRankingLoss

#Custom implementation of the MarginRankingLoss with y = 1
class CustomMarginRankingLoss(nn.Module):
    def __init__(self, margin=0):
        super(CustomMarginRankingLoss, self).__init__()
        self.margin = margin

    def forward(self, x1, x2):
        #with y=1 this is how looks the loss
        loss = torch.relu(x2 - x1 + self.margin)
        return loss.mean()

def criterion(x1, x2):
    return CustomMarginRankingLoss()(x1, x2)
```

**优化器**

对于这个实验，我们将使用经典的 AdamW，它目前是最先进的，并解决了原始 Adam 实现的一些问题。

```py
optimizer_lr = 1e-4
optimizer_weight_decay = 1e-6
optimizer = AdamW(toxicRankModel.parameters(), 
                  lr=optimizer_lr, 
                  weight_decay=optimizer_weight_decay)
```

**调度器**

调度器有助于调整学习率。在开始时，我们希望较高的学习率以更快地收敛到最佳解，而在训练结束时，我们希望较小的学习率以真正微调权重。

```py
scheduler_T_max = 500
scheduler_eta_min = 1e-6
scheduler = lr_scheduler.CosineAnnealingLR(optimizer,T_max=scheduler_T_max, eta_min=scheduler_eta_min)
```

**训练例程**

我们现在准备好训练我们的 NLP 模型以进行毒性评论排序。

使用 Pytorch 训练一个 epoch 非常简单：

1.  我们迭代通过我们的数据加载器，它会从数据集中打乱并选择预处理的数据

1.  我们从数据加载器中提取 tokens 和 masks

1.  我们通过对模型进行前向传递来计算每条消息的排名

1.  当两个排名都计算完毕后，我们可以计算 MarginRankingLoss（用于反向传播），以及一个准确率分数，表示正确分类的对数百分比（仅供参考）

1.  我们更新我们的系统（反向传播、优化器和调度器）

1.  我们迭代直到数据加载器中的所有数据都被使用完。

```py
def train_one_epoch(model, optimizer, scheduler, dataloader, device):

    #Setup train mode, this is important as some layers behave differently
    # during train and inference (like batch norm)
    model.train()

    #Initialisation of some loss
    dataset_size = 0
    running_loss = 0.0
    running_accuracy = 0.0

    progress_bar = tqdm(enumerate(dataloader), total=len(dataloader), desc="Training")

    for i, data in progress_bar:
        more_toxic_ids = data['tokens_more_toxic'].to(device, dtype = torch.long)
        more_toxic_mask = data['mask_more_toxic'].to(device, dtype = torch.long)
        less_toxic_ids = data['tokens_less_toxic'].to(device, dtype = torch.long)
        less_toxic_mask = data['mask_less_toxic'].to(device, dtype = torch.long)

        batch_size = more_toxic_ids.size(0)

        #Forward pass both inputs in the model
        x1 = model(more_toxic_ids, more_toxic_mask)
        x2 = model(less_toxic_ids, less_toxic_mask)

        #Compute margin ranking loss
        loss = criterion(x1, x2)
        accuracy_measure = (x1 > x2).float().mean().item()

        #apply backpropagation, increment optimizer
        loss.backward()
        optimizer.step()
        scheduler.step()

        optimizer.zero_grad()

        #Update cumulative loss for monitoring
        running_loss += (loss.item() * batch_size)
        dataset_size += batch_size

        epoch_loss = running_loss / dataset_size

        running_accuracy += (accuracy_measure * batch_size)
        epoch_accuracy = running_accuracy / dataset_size

        progress_bar.set_postfix({'loss': epoch_loss, 'accuracy': epoch_accuracy}, refresh=True)        

    #Garbage collector
    gc.collect()

    return epoch_loss
```

我在 Kaggle 的 GPU T4 上训练了模型，使我获得了 70% 的评论正确分类的可观成绩。我可能通过调整不同的参数和使用更多的 epochs 提高准确性，但这对于本文的目的来说已经足够了。

## 关于推断的最后一点

我们建立的框架在从预格式化的评论集合中训练时效果很好。

但在“生产”场景下，这种方法就不起作用了，因为你会接收到一堆需要评估毒性评分的消息。

![](img/c98f4de433da1d57aefea5cef4971130.png)

这是一个生产模式下的数据集示例，在这种模式下，我们只接收单条消息，而不是消息对。

对于推断，你将设计另一个 Dataset 类和另一个 DataLoader，这些将与我们之前做的有所不同：

```py
class CustomInferenceDataset(Dataset):
    def __init__(self, messages, tokenizer, max_length):

        #token list standard size
        self.length = max_length

        #Here the tokenizer will be an instance of the tokenizer
        #shown previously
        self.tokenizer = tokenizer

        #df is the training df shown in the beginning of the article
        self.messages = messages

    def __len__(self):
        return len(self.messages)

    def __getitem__(self, i):
        # get a message at index i
        message = self.messages[i]

        #tokenize the message
        dic_messages = self.tokenizer.encode_plus(
                                message,
                                truncation=True,
                                add_special_tokens=True,
                                max_length=self.length,
                                padding='max_length'
                            )

        #extract tokens and masks
        tokens_message = dic_messages['input_ids']
        mask_message = dic_messages['attention_mask']

        #return a dictionnary of tensors
        return {
            'tokens_message': torch.tensor(tokens_message, dtype=torch.long),
            'mask_message': torch.tensor(mask_message, dtype=torch.long),
        }

def get_loader_inference(messages, tokenizer, max_length, batch_size):

    dataset = CustomInferenceDataset(
        messages, 
        tokenizer=tokenizer, 
        max_length=max_length
    )

    return DataLoader(
        dataset, 
        batch_size=batch_size, 
        shuffle=False,
        drop_last=False)
```

变化了什么：

+   我们不再加载消息对，而是单条消息。

+   Loader 没有对数据进行打乱（如果你不想要与原始向量关联的随机分数带来不好的惊喜，这一点非常重要）。

+   由于没有批量归一化计算，并且我们希望对所有数据进行推断，我们将 drop_last 设置为 False，以获取所有批次，即使是未完成的批次。

最后，为了生成排序分数：

```py
@torch.no_grad()
def get_scores(model, test_loader, device):
    model.eval()  # Set the model to evaluation mode
    ranks = []  # List to store the rank scores

    progress_bar = tqdm(enumerate(test_loader), total=len(test_loader), desc="Scoring")

    for i, data in progress_bar:
        tokens_message = data['tokens_message'].to(device, dtype=torch.long)
        mask_message = data['mask_message'].to(device, dtype=torch.long)

        # Forward pass to get the rank scores
        rank = model(tokens_message, mask_message)
        # Convert tensor to NumPy and add to the list
        ranks+=list(rank.cpu().numpy().flatten())

    return ranks
```

这是推断后的前 5 条分类消息。为了保持政治正确，我在这里进行了些许审查…

![](img/cad3789ed07b0b481df419785eeec209.png)

已识别的最具毒性的消息

不太具有建设性… :)

# 结论

在这篇文章中，我们利用了 Hugging Face 预训练模型和 Pytorch 生产了一个能够对消息的毒性等级进行排序的模型。

为此，我们采用了一个“小型”的“Roberta”变换器，并使用 PyTorch 在其编码器末尾连接了一个最终简单的节点。其余部分则更为经典，可能与你之前用 PyTorch 做的其他项目类似。

这个项目是对 NLP 提供的可能性的初步探索，我想简单地介绍一些基础概念，以便进一步研究更具挑战性的任务或更大的模型。

希望你喜欢阅读，如果你想玩玩这个模型，你可以从 [我的 GitHub](https://github.com/jkaub/toxicity-ranker) 下载一个 Notebook。
