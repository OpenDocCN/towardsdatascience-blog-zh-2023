# 将文本转化为向量：TSDAE 的无监督方法用于增强嵌入

> 原文：[https://towardsdatascience.com/transforming-text-into-vectors-tsdaes-unsupervised-approach-to-enhanced-embeddings-728eb28ea701?source=collection_archive---------1-----------------------#2023-10-16](https://towardsdatascience.com/transforming-text-into-vectors-tsdaes-unsupervised-approach-to-enhanced-embeddings-728eb28ea701?source=collection_archive---------1-----------------------#2023-10-16)

[](https://medium.com/@silviaonofrei?source=post_page-----728eb28ea701--------------------------------)[![Silvia Onofrei](../Images/198b04b2063b4269eaff52402dc5f8d5.png)](https://medium.com/@silviaonofrei?source=post_page-----728eb28ea701--------------------------------)[](https://towardsdatascience.com/?source=post_page-----728eb28ea701--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----728eb28ea701--------------------------------) [Silvia Onofrei](https://medium.com/@silviaonofrei?source=post_page-----728eb28ea701--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fab562e798558&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransforming-text-into-vectors-tsdaes-unsupervised-approach-to-enhanced-embeddings-728eb28ea701&user=Silvia+Onofrei&userId=ab562e798558&source=post_page-ab562e798558----728eb28ea701---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----728eb28ea701--------------------------------) ·11分钟阅读·2023年10月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F728eb28ea701&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransforming-text-into-vectors-tsdaes-unsupervised-approach-to-enhanced-embeddings-728eb28ea701&user=Silvia+Onofrei&userId=ab562e798558&source=-----728eb28ea701---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F728eb28ea701&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransforming-text-into-vectors-tsdaes-unsupervised-approach-to-enhanced-embeddings-728eb28ea701&source=-----728eb28ea701---------------------bookmark_footer-----------)![](../Images/ce4e9809888d34f2f0ee66fa4f5a7089.png)

[设计来自 Freepik](http://www.freepik.com)

*将 TSDAE 的目标领域预训练与通用语料库上的监督微调相结合，以提高专用领域嵌入的质量。*

## 介绍

嵌入将文本编码为高维向量空间，使用密集向量来表示单词并捕捉其语义关系。生成式 AI 和大型语言模型（LLM）中的最新发展，如上下文搜索和 RAG，严重依赖于其底层嵌入的质量。虽然相似性搜索使用了如余弦相似度这样的基本数学概念，但构建嵌入向量的方法对后续结果有显著影响。

在大多数情况下，预训练的句子变换器可以开箱即用并提供合理的结果。对于这些情况，有许多基于 BERT 的预训练上下文嵌入可供选择，其中一些是领域专用的，可以从如 HuggingFace 等平台下载。

当处理包含许多特定于狭窄领域的技术术语或来源于资源稀缺语言的语料库时，会出现问题。在这些情况下，我们需要处理在预训练或微调期间未见过的*未知词*。

例如，一个在通用文本上预训练的模型将很难将向量准确分配给数学研究论文语料库中的标题。

在这些情况下，由于模型没有接触到领域特定词汇，它难以确定这些词的含义，并将它们准确地放置在相对于语料库中其他词的向量空间中。未知词的数量越多，影响越大，模型的性能越低。

因此，现成的预训练模型在这种情况下表现不佳，而尝试预训练自定义模型由于缺乏标注数据和需要大量计算资源而面临挑战。

## 动机

这项工作受到了一项近期研究[[aviation_article](https://arxiv.org/abs/2305.09556)]的启发，该研究专注于航空领域，其数据具有独特的特征，如技术术语、缩写和非常规语法。

为了解决标注数据不足的问题，作者采用了一种最有效的无监督技术之一，即预训练嵌入（TSDAE），随后进行了使用来自通用语料库的标注数据的微调阶段。经过调整的句子变换器优于通用变换器，展示了该方法在捕捉航空领域数据特征方面的有效性。

## 大纲

[领域适应](https://www.sbert.net/examples/domain_adaptation/README.html)是将文本嵌入调整到特定领域而无需标注训练数据。在本实验中，我使用了一个两步法，根据[[tsdae_article](https://arxiv.org/abs/2104.06979)]，这种方法比仅在目标领域上训练效果更好。

![](../Images/6bb26c7aa0c2ff05cfbd30aa19fcf5c0.png)

图片由作者提供

首先，我开始进行专注于目标领域的预训练，通常称为适应性预训练。此阶段需要从我们的数据集中收集句子。我在这个阶段使用 TSDAE，这是一种在作为预训练任务的领域适应方面表现出色的方法，显著超越了包括掩码语言模型在内的其他方法，正如 [[tsdae_article](https://arxiv.org/abs/2104.06979)] 中所强调的。我正紧密跟随脚本：t[rain_tsdae_from_file.py](https://github.com/UKPLab/sentence-transformers/blob/master/examples/unsupervised_learning/TSDAE/train_tsdae_from_file.py)。

随后，我在通用标记 AllNLI 数据集上对模型进行微调，采用了多重负采样排名损失策略。在此阶段，我使用来自 [training_nli_v2.py](https://github.com/UKPLab/sentence-transformers/blob/master/examples/training/nli/training_nli_v2.py) 的脚本。正如 [[tsdae_article](https://arxiv.org/abs/2104.06979)] 中所记录的，这一步骤不仅能防止过拟合，还能显著提升模型性能。

## TSDAE — 针对目标领域的预训练

TSDAE（*基于变换器的序列去噪自编码器*）是一种无监督的句子嵌入方法，首次由 K. Wang、N. Reimers 和 I. Gurevych 在 [[tsdae_article](https://arxiv.org/abs/2104.06979)] 中提出。

TSDAE 使用了修改过的编码器-解码器变换器设计，其中交叉注意力的键和值被限制在句子嵌入中。我将详细说明原文中突出显示的最佳架构选择 [[tsdae_article](https://arxiv.org/abs/2104.06979)]。

![](../Images/696638076de7ed5792bc391838896cd4.png)

图片由作者提供

+   数据集由未标记的句子组成，在预处理过程中，通过删除 60% 的内容来引入输入噪声，从而使句子受到破坏。

+   编码器通过池化词嵌入将损坏的句子转换为固定大小的向量。根据 [[tsdae_article](https://arxiv.org/abs/2104.06979)]，推荐使用 CLS 池化方法来提取句子向量。

+   解码器需要从损坏的句子嵌入中重构原始输入句子。作者建议在训练过程中将编码器和解码器的参数绑定，以减少模型中的参数数量，从而使训练更加容易，且不易过拟合，同时不会影响性能。

为了保证良好的重构质量，编码器的句子嵌入必须最佳地捕捉语义。预训练的变换器如 `bert-base-uncased` 被用于编码器，而解码器的权重则从中复制。

解码器的注意力机制仅限于编码器生成的句子表示。这种对原始变换器编码器-解码器架构的修改限制了解码器从编码器中检索的信息，并引入了一个瓶颈，迫使编码器生成有意义的句子表示。

在推断时，仅使用编码器来创建句子嵌入。

该模型经过训练，以从损坏的句子中重建干净的句子，这通过最大化目标来完成：

![](../Images/680c448c34b890f8685787af0730455e.png)

图片由作者提供

## AllNLI — 自然语言推断数据集

自然语言推断（NLI）确定两个句子之间的关系。它将假设（第二个句子）的真实性分类为蕴含（基于前提的真实）、矛盾（基于前提的虚假）或中立（既不由前提保证也不被前提反驳）。NLI 数据集是大型标注数据集，其中句子对被标注为它们的关系类别。

对于这项实验，我使用了 AllNLI 数据集，其中包含来自合并的斯坦福自然语言推断（SNLI）和 MultiNLI 数据集的超过 900K 条记录。此数据集可以从以下网址下载：[AllNLI 下载站点](http://sbert.net/datasets)。

## 加载和准备预训练数据

为了构建我们的领域特定数据，我们使用了[Kaggle arXiv 数据集](https://www.kaggle.com/datasets/Cornell-University/arxiv)，大约包含 1.7M 篇学术 STEM 论文，来源于知名电子预印本平台，[arXiv](https://arxiv.org/)。除了标题、摘要和作者之外，每篇文章还附有大量元数据。然而，这里我们只关心标题。

下载后，我将选择数学预印本。鉴于 Kaggle 文件的庞大，我已将数学论文文件的缩减版本添加到 Github 以便更容易访问。然而，如果你倾向于其他主题，请下载数据集，并在以下代码中将`math`替换为你所需的主题：

```py
 # Collect the papers with subject "math"
def extract_entries_with_math(filename: str) -> List[str]:
    """
    Function to extract the entries that contain the string 'math' in the 'id'.
    """

    # Initialize an empty list to store the extracted entries.
    entries_with_math = []

    with open(filename, 'r') as f:
        for line in f:
            try:
                # Load the JSON object from the line
                data = json.loads(line)
                # Check if the "id" key exists and if it contains "math"
                if "id" in data and "math" in data["id"]:
                    entries_with_math.append(data)

            except json.JSONDecodeError:
                # Print an error message if this line isn't valid JSON
                print(f"Couldn't parse: {line}")

    return entries_with_math

# Extract the mathematics papers
entries = extract_entries_with_math(arxiv_full_dataset)

# Save the dataset as a JSON object
arxiv_dataset_math = file_path + "/data/arxiv_math_dataset.json"

with open(arxiv_dataset_math, 'w') as fout:
    json.dump(entries, fout)
```

我已将数据集加载到 Pandas 数据框 `df` 中。快速检查显示，减少后的数据集包含 55,497 篇预印本——这是我们实验的更实际的大小。虽然[[tsdae_article](https://arxiv.org/abs/2104.06979)]建议约 10K 条目是足够的，我会保留整个减少后的数据集。数学标题可能包含 LaTeX 代码，我将其替换为 ISO 代码以优化处理。

```py
parsed_titles = []

for i,a in df.iterrows():
    """
    Function to replace LaTeX script with ISO code.
    """
    try:
        parsed_titles.append(LatexNodes2Text().latex_to_text(a['title']).replace('\\n', ' ').strip()) 
    except:
        parsed_titles.append(a['title'].replace('\\n', ' ').strip())

# Create a new column with the parsed titles
df['parsed_title'] = parsed_titles
```

我将使用`parsed_title`条目进行训练，所以让我们将它们提取为列表：

```py
# Extract the parsed titles as a list
train_sentences = df.parsed_title.to_list()
```

接下来，让我们通过从每个条目中删除大约 60% 的标记来形成损坏的句子。如果你有兴趣进一步探索或尝试不同的删除比例，请查看[去噪脚本](https://github.com/UKPLab/sentence-transformers/blob/master/sentence_transformers/datasets/DenoisingAutoEncoderDataset.py)。

```py
# Add noise to the data
train_dataset = datasets.DenoisingAutoEncoderDataset(train_sentences)
```

让我们看看处理后的一个条目发生了什么：

```py
print(train_dataset[2010])

initial text: "On solutions of Bethe equations for the XXZ model"
corrupted text: "On solutions of for the XXZ"
```

正如您所注意到的，`Bethe equations` 和 `model` 从初始文本中被移除了。

我们数据处理的最后一步是按批次加载数据集：

```py
train_dataloader = DataLoader(train_dataset, batch_size=8, 
                              shuffle=True, drop_last=True)
```

## TSDAE 训练

虽然我将遵循 t[rain_tsdae_from_file.py](https://github.com/UKPLab/sentence-transformers/blob/master/examples/unsupervised_learning/TSDAE/train_tsdae_from_file.py) 中的方法，但我将一步步构建以便更好地理解。

从选择一个预训练的转换器检查点开始，并坚持使用默认选项：

```py
model_name = 'bert-base-uncased'
word_embedding_model = models.Transformer(model_name)
```

选择 `CLS` 作为池化方法，并指定要构建的向量维度：

```py
pooling_model = models.Pooling(word_embedding_model.get_word_embedding_dimension(),
                               "cls")											 'cls')
```

接下来，通过组合两层来构建句子转换器：

```py
model = SentenceTransformer(modules=[word_embedding_model,
                            pooling_model])													pooling_model])
```

最后，指定损失函数并在训练阶段绑定编码器-解码器参数。

```py
train_loss = losses.DenoisingAutoEncoderLoss(model,
                                             decoder_name_or_path=model_name,
                                             tie_encoder_decoder=True)
```

现在，我们准备调用 fit 方法并训练模型。我还会将其存储以备后续步骤使用。欢迎您调整超参数以优化实验。

```py
model.fit(
    train_objectives=[(train_dataloader, train_loss)],
    epochs=1,
    weight_decay=0,
    scheduler='constantlr',
    optimizer_params={'lr': 3e-5},
    show_progress_bar=True,
    use_amp=True # set to False if GPU does not support FP16 cores
)

pretrained_model_save_path = 'output/tsdae-bert-uncased-math'
model.save(pretrained_model_save_path)
```

*预训练阶段在 Google Colab Pro 实例上使用 A100 GPU 高内存设置，大约花费了 15 分钟。*

## 在 AllNLI 数据集上的微调

让我们开始下载 AllNLI 数据集：

```py
nli_dataset_path = 'data/AllNLI.tsv.gz'

if not os.path.exists(nli_dataset_path):
    util.http_get('<https://sbert.net/datasets/AllNLI.tsv.gz>', 
                  nli_dataset_path)
```

接下来，解压文件并解析数据以进行训练：

```py
def add_to_samples(sent1, sent2, label):
    if sent1 not in train_data:
        train_data[sent1] = {'contradiction': set(),
                             'entailment': set(), 
                             'neutral': set()}
														 'entailment': set												 'neutral': set()}
    train_data[sent1][label].add(sent2)

train_data = {}
with gzip.open(nli_dataset_path, 'rt', encoding='utf8') as fIn:
    reader = csv.DictReader(fIn, delimiter='\\t', 
                            quoting=csv.QUOTE_NONE)
    for row in reader:
        if row['split'] == 'train':
            sent1 = row['sentence1'].strip()
            sent2 = row['sentence2'].strip()

            add_to_samples(sent1, sent2, row['label'])
            add_to_samples(sent2, sent1, row['label'])  # Also add the opposite

train_samples = []
for sent1, others in train_data.items():
    if len(others['entailment']) > 0 and len(others['contradiction']) > 0:
        train_samples.append(InputExample(texts=[sent1, 
                     random.choice(list(others['entailment'])), 
                     random.choice(list(others['contradiction']))]))
        train_samples.append(InputExample(texts=[random.choice(list(others['entailment'])), 
                     sent1, 
                     random.choice(list(others['contradiction']))]))															random.choice(list(others['contradiction']))]))
```

训练数据集大约有 56万3千 个训练样本。最后，使用一个特殊的数据加载器以批量形式加载数据，并避免批量内的重复：

```py
train_dataloader = datasets.NoDuplicatesDataLoader(train_samples,
                                                   batch_size=32)
```

我在这里使用的批量大小比脚本中的默认大小 `128` 要小。虽然更大的批量会给出更好的结果，但它需要更多的 GPU 内存，而由于我的计算资源有限，我选择了较小的批量大小。

最后，使用 MultipleRankingLoss 对 AllNLI 数据集上的预训练模型进行微调。蕴含对是正样本，而矛盾对是困难的负样本。

```py
# Set the model parameters
model_name = 'output/tsdae-bert-uncased-math'
train_batch_size = 32 
max_seq_length = 75
num_epochs = 1

# Load the pre-trained model
local_model = SentenceTransformer(model_name)
# Choose the loss function
train_loss = losses.MultipleNegativesRankingLoss(local_model)
# Use 10% of the train data for warm-up
warmup_steps = math.ceil(len(train_dataloader) * num_epochs * 0.1)

# Train the model
local_model.fit(train_objectives=[(train_dataloader, train_loss)],
          #evaluator=dev_evaluator,
          epochs=num_epochs,
          #evaluation_steps=int(len(train_dataloader)*0.1),
          warmup_steps=warmup_steps,
          output_path=model_save_path,
          use_amp=True  # Set True, if your GPU supports FP16 operations
          )

# Save the model
finetuned_model_save_path = 'output/finetuned-bert-uncased-math'
local_model.save(finetuned_model_save_path)
```

*我在整个 50万 数据集上对模型进行了微调，这大约花了 40 分钟，在 Google Colab Pro 上，进行了 1 个周期，批量大小为 32。*

## 评估 TSDAE 预训练模型和微调模型

我将对 HuggingFace 的 STS（语义文本相似性）数据集进行一些初步评估，使用 `EmbeddingSimilarityEvaluator`，它返回斯皮尔曼等级相关系数。然而，这些评估并没有使用我关注的特定领域，可能未能展示模型的真实表现。详细信息见 [[tsdae_article](https://arxiv.org/abs/2104.06979)] 第4节。

我从 HuggingFace 下载数据集，并指定 `validation` 子集：

```py
import datasets as dts
from datasets import load_dataset

# Import the STS benchmark dataset from HuggingFace
sts = dts.load_dataset('glue', 'stsb', split='validation')
```

这是一个形式为的数据集对象：

```py
Dataset({
    features: ['sentence1', 'sentence2', 'label', 'idx'],
    num_rows: 1379
})
```

为了更好地理解，让我们看一个具体的条目

```py
# Take a peek at one of the entries
sts['idx'][100], sts['sentence1'][100], sts['sentence2'][100], sts['label'][100]

>>>(100,
 'A woman is riding on a horse.',
 'A man is turning over tables in anger.',
 0.0)
```

从这个例子中我们可以看到，每个条目有4个特征，一个是索引，两个句子和一个标签（由人工标注者创建）。标签的值可以在 `0` 和 `5` 之间，并测量两个句子的相似程度（`5` 为最相似）。在这个例子中，这两个句子完全在不同的主题上。

为了评估模型，我们为句子对创建句子嵌入，并计算每对的余弦相似度分数。标签与相似度分数之间的斯皮尔曼等级相关性作为评估分数。

由于我将使用取值范围在 0 到 1 之间的余弦相似度，因此需要对标签进行归一化：

```py
# Normalize the [0, 5] range to [0, 1]
sts = sts.map(lambda x: {'label': x['label'] / 5.0})
```

将数据封装在 HuggingFace 的 `InputExample` 类中：

```py
# Create a list to store the parsed data
samples = []

for sample in sts:
    # Reformat to use InputExample class
    samples.append(InputExample(
        texts=[sample['sentence1'], sample['sentence2']],
        label=sample['label']
    ))
```

创建一个基于 `sentence-transformers` 库中 `EmbeddingSimilarityEvaluator` 类的评估器。

```py
# Instantiate the evaluation module
evaluator = EmbeddingSimilarityEvaluator.from_input_examples(samples)
```

我们计算了 TSDAE 模型、微调模型和几个预训练句子转换器的得分：

![](../Images/c4b1c1e0ea3727afee930f65f6b50c6b.png)

作者提供的图片

因此，在一般范围的数据集上，一些预训练模型，如 `all-mpnet-base-v2`，表现优于 TSDAE 微调模型。然而，通过预训练，初始模型 `bert-base-uncased` 的性能提高了两倍以上。可以想象，通过进一步调整超参数进行微调，可能会获得更好的结果。

## 结论

对于资源有限的领域，将 TSDAE 与微调结合起来是一种相当高效的构建嵌入的策略。考虑到数据量和计算资源，这里的结果相当值得注意。然而，对于那些并不特别异常或领域特定的数据集，考虑到效率和成本，选择一个预训练的嵌入可能会提供可比的性能。

[**Gihub 链接**](https://github.com/SolanaO/Blogs_Content/tree/master/tsdae) **到 Colab 笔记本和示例数据集**。

*因此，我的朋友们，我们在学习旅程中应该始终拥抱好、坏和混乱的东西！*

## 参考文献

[tsdae_article]. K. Wang 等，[TSDAE：使用基于 Transformer 的序列去噪自编码器进行无监督句子嵌入学习](https://arxiv.org/abs/2104.06979) (2021) arXiv:2104.06979

[aviation_article]. L. Wang 等，[为航空领域调整句子转换器](https://arxiv.org/pdf/2305.09556.pdf) (2023) arXiv:2305.09556
