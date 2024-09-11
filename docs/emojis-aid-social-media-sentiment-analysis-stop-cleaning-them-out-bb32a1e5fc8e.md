# 表情符号有助于社交媒体情感分析：不要再清除它们了！

> 原文：[https://towardsdatascience.com/emojis-aid-social-media-sentiment-analysis-stop-cleaning-them-out-bb32a1e5fc8e?source=collection_archive---------5-----------------------#2023-01-31](https://towardsdatascience.com/emojis-aid-social-media-sentiment-analysis-stop-cleaning-them-out-bb32a1e5fc8e?source=collection_archive---------5-----------------------#2023-01-31)

## 在社交媒体情感分析中利用表情符号来提高准确性。

[](https://medium.com/@bc3088?source=post_page-----bb32a1e5fc8e--------------------------------)[![Bale Chen](../Images/de6276419f1f93de346387a5ea2cc5b8.png)](https://medium.com/@bc3088?source=post_page-----bb32a1e5fc8e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bb32a1e5fc8e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bb32a1e5fc8e--------------------------------) [Bale Chen](https://medium.com/@bc3088?source=post_page-----bb32a1e5fc8e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8695cd8317da&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Femojis-aid-social-media-sentiment-analysis-stop-cleaning-them-out-bb32a1e5fc8e&user=Bale+Chen&userId=8695cd8317da&source=post_page-8695cd8317da----bb32a1e5fc8e---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb32a1e5fc8e--------------------------------) ·14 分钟阅读·2023年1月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbb32a1e5fc8e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Femojis-aid-social-media-sentiment-analysis-stop-cleaning-them-out-bb32a1e5fc8e&user=Bale+Chen&userId=8695cd8317da&source=-----bb32a1e5fc8e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbb32a1e5fc8e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Femojis-aid-social-media-sentiment-analysis-stop-cleaning-them-out-bb32a1e5fc8e&source=-----bb32a1e5fc8e---------------------bookmark_footer-----------)![](../Images/37923fbf0be08bd4aef8544f5266e515.png)

图片由 [Denis Cherkashin](https://unsplash.com/@denic?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/emojis?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**简明扼要：**

+   无论你使用什么模型，或如何在分析中纳入表情符号，将表情符号包括在社交媒体情感分析中都能显著提高情感分类的准确性

+   超过一半的流行 BERT 基础编码器不支持表情符号

+   Twitter-RoBERTa编码器在情感分析中表现最佳，并且与表情符号协调良好

+   与其清除表情符号，不如将它们转换为文本描述，这可以帮助提高情感分类的准确性并处理词汇表外的问题。

随着社交媒体成为人们生活的重要组成部分，人们在互联网上分享的内容对许多方面都极具价值。许多现代自然语言处理（NLP）技术被应用于理解公众的社交媒体帖子。情感分析是最受欢迎和最关键的NLP话题之一，侧重于计算性地分析对书面文本中实体的意见、情感、情绪或态度[[1](#1a95)]。因此，社交媒体情感分析（SMSA）是一个理解和学习在短社交媒体帖子中表达情感的领域。

这个项目的另一个重要特征是可爱的文内图形——表情符号😄。这些图形符号在社交媒体通讯中越来越受到欢迎。根据[Emojipedia的统计数据](https://emojipedia.org/stats/)（2021年），一个著名的表情符号参考网站，现在超过五分之一的推文包含表情符号（21.54%），而超过一半的Instagram评论中包含表情符号。表情符号是一种方便且简洁的表达情感和传达意义的方式，这可能解释了它们的巨大受欢迎程度。

尽管表情符号在网络通信中无处不在，但它们在NLP和SMSA领域并不受青睐。在数据预处理阶段，表情符号通常会与其他非结构化信息如URL、停用词、特殊字符和图片一起被删除[[2](#c7e8)]。虽然一些研究人员近年来开始研究在SMSA中包含表情符号的潜力，但这仍然是一种小众方法，等待进一步研究。这个项目旨在**检验流行的BERT编码器对表情符号的兼容性**并**探索将表情符号融入SMSA以提高准确性的方法**。

## 目录

**1** [**背景知识**](#776f)

1.1 [SMSA究竟是什么？](#4843)

1.2 [情感分析方法的发展](#d8db)

2[**实验**](#cfe0)2.1 [模型设计](#40f1)

2.2 [数据准备阶段的经验教训（一个悲伤的故事）](#2f88)

2.3 [BERT家族的表情符号兼容性测试](#666a)

2.4 [实验方法来预处理表情符号](#bc42)

**3** [**结果讨论**](#df89) **4** [**结论**](#fa8d)[**致谢**](#5d30)[**参考文献**](#e8ad)

# 1 背景知识

## 1.1 SMSA究竟是什么？

这里是一些关于SMSA的背景知识，你可能希望在深入实际实验之前了解。到目前为止，不需要技术背景/数学知识。让我首先解释最典型的SMSA任务的直觉：

![](../Images/08d91363c13fa841f5deb5f015f9ded1.png)

SMSA示例

如上图所示，给定一个社交媒体帖子，模型（由灰色机器人表示）将输出其情感标签的预测。在这个例子中，模型回应该帖子 57.60% 可能表达正面情感，12.38% 可能是负面，30.02% 可能是中立的。一些研究将帖子分类为二元方式，即正面/负面，但其他研究也考虑了“中立”作为选项。本项目遵循后者。

## 1.2 情感分析方法的发展

据我了解，研究社交媒体情感的首个定量方法是使用**词汇表方法**。该模型有一个预定义的词汇表，将每个词元映射到情感分数。因此，给定一个句子，模型查询词汇表，汇总每个词的情感分数，并输出总体情感分数。非常直观，对吧？

![](../Images/a25d24f3eec39ee769107336e506031b.png)

基于词汇表的情感分析模型基本图

[SentiWordNet](https://github.com/aesuli/SentiWordNet) 和 [VADER](https://github.com/cjhutto/vaderSentiment) 是这种方法的两个范例，受到了业界和学术界的青睐。

随着**机器学习**的发展，像 SVM、随机森林、多层感知器等分类器在情感分析中获得了立足之地。然而，这些模型不适用于文本输入，因此这些分类器与**词嵌入模型**结合以执行情感分析任务。词嵌入模型将词语转换为机器可以操作的数值向量。[Google 的 word2vec](https://arxiv.org/abs/1301.3781) 嵌入模型是文本数据表示学习的重大突破，其后是[Pennington 等人的 GloVe](https://nlp.stanford.edu/projects/glove/)和[Facebook 的 fasttext](https://fasttext.cc/)。

由于自然语言的顺序特性和深度学习的极大普及，**递归神经网络（RNN）** 成为了“流行的小子”。RNN 按顺序解码或“读取”词嵌入序列，在循环中保留顺序结构，而词汇表模型和传统机器学习模型未能实现这一点。

![](../Images/c4ff03168aa444fe5c62fe3842d61634.png)

现代 SMSA 的典型工作流程

上图解释了演变后的工作流程。词嵌入被传入 RNN 模型，模型输出最后一个隐藏状态（如果你不清楚最后的隐藏状态是什么，直观上它是 RNN 在“读取”完所有文本后“总结”的内容）。最后，我们使用前馈全连接神经网络将高维隐藏状态映射到情感标签上。

我们快完成了！最后一块拼图是**Transformer模型**。即使你还没有学习NLP，你可能也听说过“Attention is All You Need”[[3](#1c64)]。在这篇论文中，他们提出了自注意力技术，并开发了Transformer模型。这些模型非常强大，以至于在几乎所有NLP的子任务中超越了之前的模型。如果你不熟悉Transformer模型，我强烈推荐你阅读Giuliano Giacaglia的[这篇入门文章](/transformers-141e32e69591)。

由于预训练Transformer模型具有无与伦比的性能，业界和学术界已经开始大规模使用这些模型。多亏了[Hugging Face](http://huggingface.co/models)的*transformer*包，开发者现在可以轻松导入和部署这些大型预训练模型。BERT，即双向编码器表示的Transformer，是最著名的基于Transformer的编码器模型，它为文本学习出色的表示。之后，基于BERT开发了RoBERTa、BERTweet、DeBERTa等模型。

# 2 实验

有了这些背景知识，我们现在可以深入到实验和编程部分了！如果你对NLP机制感到不自信，我建议你跳过技术细节或阅读一些关于NLP的入门博客，例如在Towards Data Science上。首先，我们要明确实验目标。我们想知道：

+   目前流行的基于预训练BERT模型的兼容性如何与emoji数据。

+   如果我们在SMSA过程中加入emoji，性能会受到什么影响。

+   在数据处理阶段，我们应该如何处理以包含emoji。

## 2.1 模型设计

我们的模型遵循上述的神经网络范式，由预训练的基于BERT的编码器、Bi-LSTM层和前馈全连接网络组成。图示如下：

![](../Images/1fe39eea86b8886152593fa59c468784.png)

模型图

为了清楚，预处理的推文首先通过预训练编码器，变成一个表示向量序列。然后，表示向量通过Bi-LSTM层。LSTM的两个方向的两个最后隐藏状态将由前馈层处理，以输出推文情感的最终预测。

我们改变编码器模型和emoji预处理方法，以观察性能的变化。为了控制变量，Bi-LSTM和前馈层在所有实验中配置相同。在训练过程中，我们只训练Bi-LSTM和前馈层。预训练编码器模型的参数被冻结。

该模型的PyTorch实现及其他技术细节可以在我的[GitHub Repo](https://github.com/BaleChen/emoji-setiment-analysis)中找到。

## 2.2 数据准备阶段的经验教训

数据的可用性是每个数据科学研究者的痛点。起初，我想找一个基准的Twitter情感分析数据集，以便将结果与之前的模型进行比较，但我遇到了以下障碍：

1.  大多数数据集只有“推文ID”作为查询键来查找原始内容。要访问带有ID的原始推文，我需要**Twitter API访问权限**。我和我的教授导师都尝试申请过，但我们都没有获得批准（我们仍然不知道原因）。

1.  嗯……另一个问题是**大部分推文已经消失**！这意味着它们要么被作者删除，要么因为某种原因被Twitter服务器删除了。

1.  尽管有很少的数据集直接存储推文内容，但存储在***.csv或*.tsv格式的数据集无法保留表情符号**。也就是说，原始推文中有表情符号，但我从网上下载的编译数据集完全丢失了所有表情符号。

因此，无论何时你想进行Twitter情感分析，确保首先验证数据集是否通过Tweet ID存储推文，这需要你额外的努力来检索原始文本。如果数据集是几年前的，推文很容易消失。此外，不要对申请Twitter API抱有太高的期望。我的导师是一位美国著名大学的助理教授，但也无法满足他们的要求（原因不明）。最后，为了保留表情符号，千万不要将其保存为csv或tsv格式。Pickle、xlsx或json可能是你的好选择。

无论如何，找到一个保留表情符号、具有情感标签且大小合适的数据集对我来说非常困难。最终，我发现Novak等人的[数据集](https://figshare.com/articles/dataset/Emoji_Sentiment_Ranking/1600931)符合所有标准。

## 2.3 BERT家族的表情符号兼容性测试

在实现基于BERT的编码器之前，我们需要了解它们是否与表情符号兼容，即是否能为表情符号标记生成独特的表示。更具体地说，在将推文传递给编码器之前，它会首先由模型分词器进行***分词***，这种分词器是特定于编码器的（例如，RoBERTa-base使用RoBERTa-base分词器，而BERT-base使用BERT-base分词器）。分词器的作用是将长字符串的文本输入拆分为词汇表中的单个词标记（如下图所示）。

![](../Images/46fd0a58138ad2eeca109b73da2a8f1b.png)

spaCy的基于规则的分词器 ([source](https://github.com/explosion/spaCy))

在我们的案例中，如果表情符号不在分词器词汇表中，它们将被全部标记为未知标记（例如“<UNK>”）。因此，编码器模型将对所有这些未知标记产生相同的向量表示，在这种情况下，清理或不清理表情符号在模型性能上技术上不会有任何区别。

我选择了以下常见的基于BERT的编码器列表。

+   ALBERT-base-v2

+   BERT-base, BERT-large

+   BERTweet-base, BERTweet-large

+   DeBERTa-base, DeBERTa-large

+   DistilBERT

+   RoBERTa-base, RoBERTa-large

+   Twitter-RoBERTa

+   XLMRoBERTa-base, XLMRoBERTa-large

可以使用HuggingFace的[*transformers*](https://pypi.org/project/transformers/)包和[*emoji*](https://pypi.org/project/emoji/)包轻松完成测试。我们首先导入它们：

```py
from transformers import AutoTokenizer
import emoji
```

AutoTokenizer是一个非常有用的功能，你可以使用模型名称来加载相应的分词器，比如下面这行代码，其中我导入了BERT-base分词器。

```py
tokenizer = AutoTokenizer.from_pretrained('bert-base-uncased') 
```

然后，我们使用*emoji*包来获取完整的表情符号列表，并使用编码和解码功能来检测兼容性。

```py
emoji_list = list(emoji.EMOJI_DATA.keys())
cnt = 0
for e in emoji_list:
    tokenized = tokenizer.decode(tokenizer.encode(e)).strip("</s>").strip()
    if e not in tokenized:
        cnt += 1
print(f"{cnt/len(emoji_list)} of the emojis are not identified by this tokenizer.")
```

## 2.4 实验方法来预处理表情符号

我们提出了5种数据预处理方法，以利用表情符号信息，而不是从原始推文中删除表情符号 (rm)。

**直接编码 (dir)** 使用支持表情符号的预训练编码器模型直接将表情符号向量化。这样，表情符号就会被视为普通的词令。这是最直接的方法。

**用描述替换表情符号 (emoji2desc)** 预训练的编码器并没有专门针对表情符号创建表示。相反，它们是在大量文本上进行训练的。我们推测编码器可能对词语的表示要优于表情符号，因此将表情符号转换为其官方描述可能有助于更好地提取语义信息。例如，“我爱动物 😍”将变成“我爱动物 笑脸带心眼”。下面的Python实现展示了如何使用*emoji*包：

```py
def emoji2description(text):
  return emoji.replace_emoji(text, replace=lambda chars, data_dict: ' '.join(data_dict['en'].split('_')).strip(':'))
```

**连接表情符号 (concat-emoji)** 本质上，我们将表情符号重新定位到句子的末尾，并执行直接编码方法。由于表情符号不属于句子的语法结构，我们想知道重新定位它们是否有助于更好地区分文本和表情符号信息。例如，“天气太冷了🧊。 不想再工作了😡😭。” 变成 “天气太冷了。 不想再工作了。 🧊😡😭”

```py
def emoji2concat_emoji(text):
    emoji_list = emoji.emoji_list(text)
    ret = emoji.replace_emoji(text, replace='').strip()
    for json in emoji_list:
        this_emoji = json['emoji']
        ret += ' ' + this_emoji
    return ret
```

**连接描述 (concat-desc)** 此外，我们还测试了用文本描述替换那些重新定位的表情符号。

```py
def emoji2concat_description(text):
    emoji_list = emoji.emoji_list(text)
    ret = emoji.replace_emoji(text, replace='').strip()
    for json in emoji_list:
        this_desc = ' '.join(emoji.EMOJI_DATA[json['emoji']]['en'].split('_')).strip(':')
        ret += ' ' + this_desc
    return ret
```

**元特征（meta）** 除了将表情符号视为句子的一部分外，我们还可以将它们视为高级特征。我们使用表情符号情感排名 [[4](#de51)] 词典来获取积极性、中立性、消极性和情感分数特征。然后，我们将这些特征与表情符号的向量表示连接起来，形成推文的表情符号元特征向量。这个向量包含了推文的表情符号情感信息。纯文本将照常通过编码器和BiLSTM层，然后将元特征向量与BiLSTM层的最后隐藏状态连接，作为前馈层的输入。这个过程本质上是将表情符号从句子中隔离开来，将它们视为推文的元数据。

# **结果讨论**

经过所有这些技术设计，我们最终进入了结果部分。首先，让我们看看这些常见的BERT基础编码器模型的表情符号兼容性。

![](../Images/bae1c8a4f2a8a36d4375f2199ed2ccb7.png)

BERT基础的表情符号模型的表情符号兼容性

**超过一半的这些模型不能识别所有表情符号！** RoBERTa（基础版和大版）、**DeBERTa**（基础版和大版）、**BERTweet-large** 和 **Twitter-RoBERTa** 支持所有表情符号。然而，像 **BERT**（基础版和大版）、**DistilBERT** 和 **ALBERT** 这样的常见编码器几乎不支持任何表情符号。

现在，让我们比较不同表情符号兼容编码器和不同方法下的模型性能。下图中的百分比表示情感分类的准确率。每个单元格代表某种预处理方法下的编码器模型的准确率。

（请注意 *emoji2vec* 是一个于2015年开发的基准模型。它不是BERT基础的，而是一个预定义的表情符号嵌入模型，也可以生成表情符号的向量表示。它可以看作是Google的Word2vec模型的扩展）

![](../Images/9603acca512c29461bcc59bc9041739b.png)

情感分类准确率

为了比较将表情符号融入SMSA过程中的不同方法，我们还展示了不同方法下的准确率及置信区间。

![](../Images/564a0d218182070e34434676c064f680.png)

每种预处理方法的平均准确率（带置信区间）

一个重要的发现是，**无论如何包含表情符号，都能提升** SMSA **模型的性能。** 移除表情符号平均降低了1.202%的准确率。对于包含表情符号的方法，重叠的置信区间表示出相对模糊的区分。没有“通常最佳”的方法来利用表情符号。

![](../Images/9e8a528a73b2c060276b63a66e7b3861.png)

每个编码器模型的平均准确率（带置信区间）

在所有编码模型中进行比较的结果如上方柱状图所示。柱状图顶部也标注了置信区间。小的置信区间表示**对排名的统计信心较高**。**Twitter-RoBERTa在所有模型中表现最佳**，这很可能是由**训练领域**造成的。***emoji2vec***，该模型开发于2015年并早于变压器模型的兴起，在当前标准下对表情符号的**表示相对较差**。

既然没有找到“普遍最佳”的方法，我们深入探讨不同模型如何从各种预处理方法中获得不同的收益。下图展示了使用某种预处理方法与一开始移除表情符号相比的百分比改进。

![](../Images/e41ec99904cff81302c9a289b4d89a1f.png)

百分比改进热图

首先，所有的改进指标都是正数，这强有力地证明了表情符号在SMSA中的有用性。在数据中包含表情符号将提高SMSA模型的性能。

**通常，对于基于BERT的模型，直接编码表情符号似乎是一种足够且有时是最好的方法。** 令人惊讶的是，最简单的方法效果与复杂的方法一样好，甚至更好。

**表情符号表示学习模型较差的情况可能会从将表情符号转换为文本描述中受益更多。** emoji2vec模型的改进既出现最大值也出现最小值。很可能emoji2vec对表情符号的向量表示较差，但将表情符号转换为文本描述有助于捕捉社交媒体帖子的情感意义。

**RoBERTa-large无论预处理方法如何显示出意外的小改进**，这表明它不像其他基于BERT的模型那样受益于表情符号。这个结果可能是因为RoBERTa-large的架构可能更适合学习纯文本的表示而不是表情符号，但仍需要更严格的论证。

# 4 结论

从这个项目中，关键的收获是**无论你使用什么模型或预处理方法，在SMSA过程中包含表情符号都会提高情感分类的准确性。因此，当你面对社交媒体情感分析任务时，*一定要仔细考虑*是否清除它们！**

处理SMSA任务并协调表情符号的最佳模型是[Twitter-RoBERTa](https://huggingface.co/cardiffnlp/twitter-roberta-base-sentiment)编码器！如果你正在处理Twitter数据和分析推文情感，请使用它。

关于如何具体融入表情符号，方法之间并没有显著差异，因此一种直接的方法——将表情符号直接视为普通单词标记——可以完美解决问题。然而，考虑到我们研究中一半的常见BERT编码器不支持表情符号，我们建议使用[emoji2desc](#ff68)方法。也就是说，使用之前提到的[一行简单代码](#57b7)将表情符号转换为其官方文本描述，这可以轻松处理词汇表外的表情符号标记。

如果你使用传统的词嵌入如 *word2vec*，而且你也不想浪费可爱的表情符号，可以考虑使用[emoji2desc](#ff68)或[concat-emoji](#afb8)方法，而不是使用 *emoji2vec* 模型。

希望我们的项目能够指导SMSA研究人员和行业工作者如何在过程中包含表情符号。更重要的是，这个项目提供了一种提高SMSA准确性的全新视角。深入研究技术细节并不是唯一的进展方式，例如，这些简单却强大的表情符号也可以提供帮助。

脚本、学术报告及更多内容可以在我的[GitHub Repo](https://github.com/BaleChen/emoji-setiment-analysis)中找到。

关于文章中的图片，除非另有说明，否则均为作者提供。

# **致谢**

我衷心感谢我的研究导师和指导教授[马修·劳里埃](https://mlauriere.github.io/)。他为我提供了深刻的建议，并在这个夏季研究过程中给予指导。能与他一起完成这项研究，并在我生日时收到他的邮件问候，我感到非常荣幸和高兴。

本工作还部分得到了纽约大学IT高性能计算资源、服务和员工专业知识的支持。

此外，我真诚地感谢纽约大学及纽约大学上海校区提供的[DURF](https://shanghai.nyu.edu/academics/undergraduate-research)研究机会。

感谢所有在这个夏天帮助我的朋友和家人。没有你们的支持，这项研究是不可能完成的。

# 参考文献

[1] Liu, B. [Sentiment Analysis: Mining Opinions, Sentiments, and Emotions.](https://doi.org/10.1017/CBO9781139084789) (2015)，剑桥大学出版社，剑桥。

[2] Chakriswaran, P., Vincent, D. R., Srinivasan, K., Sharma, V., Chang, C.-Y., 和 Reina, D. G. [Emotion AI-Driven Sentiment Analysis: A Survey, Future Research Directions, and Open Issues.](https://doi.org/10.3390/app9245462) (2019)，应用科学。

[3] Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A., Kaiser, u., & Polosukhin, I. [Attention is All you Need.](https://papers.nips.cc/paper/2017/file/3f5ee243547dee91fbd053c1c4a845aa-Paper.pdf) (2017), 在 *Advances in Neural Information Processing Systems* 中。Curran Associates, Inc.

[4] Kralj Novak, P., Smailović, J., Sluban, B., & Mozetič, I. [Sentiment of Emojis.](https://doi.org/10.1371/journal.pone.0144296) (2015)，*PLOS ONE*，*10*(12)，e0144296。
