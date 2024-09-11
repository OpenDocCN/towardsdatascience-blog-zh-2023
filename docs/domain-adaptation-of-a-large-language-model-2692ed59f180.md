# 大型语言模型的领域适配

> 原文：[https://towardsdatascience.com/domain-adaptation-of-a-large-language-model-2692ed59f180?source=collection_archive---------5-----------------------#2023-11-14](https://towardsdatascience.com/domain-adaptation-of-a-large-language-model-2692ed59f180?source=collection_archive---------5-----------------------#2023-11-14)

## 使用HuggingFace将预训练模型适配到新领域

[](https://medium.com/@mina.ghashami?source=post_page-----2692ed59f180--------------------------------)[![Mina Ghashami](../Images/745f53b94f5667a485299b49913c7a21.png)](https://medium.com/@mina.ghashami?source=post_page-----2692ed59f180--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2692ed59f180--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2692ed59f180--------------------------------) [Mina Ghashami](https://medium.com/@mina.ghashami?source=post_page-----2692ed59f180--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc99ed9ed7b9a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdomain-adaptation-of-a-large-language-model-2692ed59f180&user=Mina+Ghashami&userId=c99ed9ed7b9a&source=post_page-c99ed9ed7b9a----2692ed59f180---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----2692ed59f180--------------------------------) · 13分钟阅读 · 2023年11月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2692ed59f180&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdomain-adaptation-of-a-large-language-model-2692ed59f180&user=Mina+Ghashami&userId=c99ed9ed7b9a&source=-----2692ed59f180---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2692ed59f180&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdomain-adaptation-of-a-large-language-model-2692ed59f180&source=-----2692ed59f180---------------------bookmark_footer-----------)![](../Images/dcd659ece26bd7d235dacf241d80c574.png)

图片来自[unsplash](https://unsplash.com/photos/a-pink-flower-in-the-middle-of-a-cactus-TyRP_UH7I_0)

大型语言模型（LLMs），如BERT，通常是在类似维基百科和BookCorpus的通用领域语料库上进行预训练的。如果将它们应用于医疗等更专业的领域，与*适配*这些领域的模型相比，性能往往会下降。

> 在本文中，我们将探讨如何使用HuggingFace Transformers库将预训练的LLM（如Deberta base）适配到医学领域。具体而言，我们将介绍一种称为中间预训练的有效技术，在这种技术中，我们在目标领域的数据上进一步预训练LLM。这使模型适应新领域，并提高其性能。

这是一个简单却有效的技术，可以将LLMs调整到你的领域，并在下游任务性能上获得显著提升。

让我们开始吧。

# 第一步：数据

任何项目的第一步是准备数据。由于我们的数据集属于医学领域，它包含以下字段以及更多：

![](../Images/153b7422d396eaf71d450853c1da8540.png)

图片作者

在这里列出所有字段是不可能的，因为字段众多。但即使是这些现有字段的简要介绍，也有助于我们为LLM形成输入序列。

> 需要记住的第一点是，输入必须是序列，因为LLMs将输入视为文本序列。

为了将其形成序列，我们可以注入特殊标签，以告知LLM接下来会出现什么信息。考虑以下示例：`<patient>name:John, surname: Doer, patientID:1234, age:34</patient>`，`<patient>` 是一个特殊标签，告知LLM接下来的是关于患者的信息。

所以我们将输入序列形成如下：

![](../Images/15080a76be38bffaa30e1a17f24bd446.png)

图片作者

如你所见，我们注入了四个标签：

1.  `<patient> </patient>`：包含关于患者的信息

1.  `<hospital> </hospital>`：包含关于医院的信息

1.  `<event> </event>` ：包含关于 `patient` 在 `hospital` 中所做的各个事件的信息。

1.  `<visits> </visits>`：用于封装患者在医院中经历的所有事件。

在每个标签块中，我们包含的属性是 *key:value* 对。

> 请注意，对于给定的患者和医院，我们按时间戳排序事件并将它们连接在一起。这形成了患者在医院中经历的按时间排序的访问序列。

特殊标签的优点在于，在训练LLM后，如果我想要一个患者的嵌入，可以通过检索 `<patient>` 标签的嵌入来获取。同样，如果我们想要一个作为患者档案的嵌入，可以检索 `<visits>` 标签的嵌入，因为这个标签包含了患者在医院中经历的所有事件。

假设我们的数据存储在s3中；其中数据模式只有一列叫做“text”，每条记录是“text”列中上述格式的序列。我们使用以下代码从s3加载数据：

```py
import s3fs
import random

files = {}
fs = s3fs.S3FileSystem()

train_path = "s3://bucket/train/*.parquet"
s3_files = ["s3://" + p for p in fs.glob(train_path)]
random.shuffle(s3_files)
files["train"] = s3_files

validation_path = "s3://bucket/test/*.parquet"
s3_files = ["s3://" + p for p in fs.glob(validation_path)]
random.shuffle(s3_files)
files["validation"] = s3_files

from datasets import load_dataset
raw_datasets = load_dataset("parquet", data_files=files, streaming=False, use_auth_token = True)
print(raw_datasets)
```

和 `raw_datasets` 看起来如下：

![](../Images/711fe4d19dac23adebfe6d751f30d809.png)

图片作者

# 第二步：编码

首先，通过以下命令安装要求：

```py
!pip install -r requirements.txt 
```

requirements.txt 文件如下：

```py
pytest==7.4.2
pytest-cov==4.1.0
datasets==2.13.0
huggingface-hub==0.16.4
tensorboard==2.14.0
networkx==2.6.3
numpy==1.22.4
pandas==2.0.3
s3fs==2023.5.0
tokenizers==0.13.3
tqdm==4.66.1
transformers==4.31.0
evaluate==0.4.0
accelerate==0.23.0
bitsandbytes==0.41.1
trl==0.5.0
peft==0.4.0
pyarrow==13.0.0
pydantic==1.10.6
deepspeed==0.9.0
```

编写代码时，我们需要定义模型参数、数据参数和训练参数。然后我们需要定义模型，并将其置于PEFT（参数高效）设置中，如果我们想通过PEFT进行训练。

首先，我们定义数据、模型和训练的输入参数。

## 模型参数

> 模型参数是指定我们将要训练或微调的模型/分词器的参数。下面的类将这些实现为`dataclass`。我们稍后将从中获取一个实例来传递我们的选择。

这里最重要的字段是`model_name_or_path`，但为了完整性，我们保留所有参数。

```py
@dataclass
class ModelArguments:
    """
    Arguments pertaining to which model/config/tokenizer we are going to use
    """

    model_name_or_path: Optional[str] = field(
        default=None,
        metadata={
            "help": (
                "The model checkpoint for weights initialization. Don't set if you want to train a model from scratch."
            )
        },
    )
    model_type: Optional[str] = field(
        default=None,
        metadata={"help": "If training from scratch, pass a model type from the list: " + ", ".join(MODEL_TYPES)},
    )
    config_overrides: Optional[str] = field(
        default=None,
        metadata={
            "help": (
                "Override some existing default config settings when a model is trained from scratch. Example: "
                "n_embd=10,resid_pdrop=0.2,scale_attn_weights=false,summary_type=cls_index"
            )
        },
    )
    config_name: Optional[str] = field(
        default=None, metadata={"help": "Pretrained config name or path if not the same as model_name"}
    )
    tokenizer_name: Optional[str] = field(
        default=None, metadata={"help": "Pretrained tokenizer name or path if not the same as model_name"}
    )
    cache_dir: Optional[str] = field(
        default=None,
        metadata={"help": "Where do you want to store the pretrained models downloaded from huggingface.co"},
    )
    use_fast_tokenizer: bool = field(
        default=True,
        metadata={"help": "Whether to use one of the fast tokenizer (backed by the tokenizers library) or not."},
    )
    model_revision: str = field(
        default="main",
        metadata={"help": "The specific model version to use (can be a branch name, tag name or commit id)."},
    )
    use_auth_token: bool = field(
        default=None,
        metadata={
            "help": "The `use_auth_token` argument is deprecated and will be removed in v4.34\. Please use `token`."
        },
    )
    trust_remote_code: bool = field(
        default=False,
        metadata={
            "help": (
                "Whether or not to allow for custom models defined on the Hub in their own modeling files. This option"
                "should only be set to `True` for repositories you trust and in which you have read the code, as it will "
                "execute code present on the Hub on your local machine."
            )
        },
    )
    low_cpu_mem_usage: bool = field(
        default=False,
        metadata={
            "help": (
                "It is an option to create the model as an empty shell, then only materialize its parameters when the pretrained weights are loaded. "
                "set True will benefit LLM loading time and RAM consumption."
            )
        },
    )

    def __post_init__(self):
        if self.config_overrides is not None and (self.config_name is not None or self.model_name_or_path is not None):
            raise ValueError(
                "--config_overrides can't be used in combination with --config_name or --model_name_or_path"
            )
```

## PEFT参数

下面是与参数高效训练相关的参数。这使用了lora包进行低秩适应。

```py
@dataclass
class PEFTArguments:
    """
    Arguments pertaining to what training arguments we pass to trainer.
    """
    lora_r: Optional[int] = field(
        default=0, metadata={"help": "LoRA bottleneck dim. This value must be > 0 to utilize LoRA."}
    )

    lora_alpha: Optional[int] = field(
        default=32, metadata={"help": "LoRA alpha"}
    )

    lora_dropout: Optional[float] = field(
        default=0.1, metadata={"help": "LoRA dropout probability"}
    )

    target_modules: Optional[str] = field(
        default="", metadata={
            "help": "Target modules to use for LoRA adaptation (must be input as a comma delimited string)"
        }
    )
```

## 数据参数

这些是与我们将要输入模型进行训练和评估的数据相关的参数。

```py
@dataclass
class DataTrainingArguments:
    """
    Arguments pertaining to what data we are going to input our model for training and eval.
    """

    dataset_name: Optional[str] = field(
        default=None, metadata={"help": "The name of the dataset to use (via the datasets library)."}
    )
    dataset_config_name: Optional[str] = field(
        default=None, metadata={"help": "The configuration name of the dataset to use (via the datasets library)."}
    )
    train_file: Optional[str] = field(default=None, metadata={"help": "The input training data file (a text file)."})
    validation_file: Optional[str] = field(
        default=None,
        metadata={"help": "An optional input evaluation data file to evaluate the perplexity on (a text file)."},
    )
    overwrite_cache: bool = field(
        default=False, metadata={"help": "Overwrite the cached training and evaluation sets"}
    )
    validation_split_percentage: Optional[int] = field(
        default=5,
        metadata={
            "help": "The percentage of the train set used as validation set in case there's no validation split"
        },
    )
    max_seq_length: Optional[int] = field(
        default=None,
        metadata={
            "help": (
                "The maximum total input sequence length after tokenization. Sequences longer "
                "than this will be truncated."
            )
        },
    )
    preprocessing_num_workers: Optional[int] = field(
        default=None,
        metadata={"help": "The number of processes to use for the preprocessing."},
    )
    mlm_probability: float = field(
        default=0.15, metadata={"help": "Ratio of tokens to mask for masked language modeling loss"}
    )
    line_by_line: bool = field(
        default=False,
        metadata={"help": "Whether distinct lines of text in the dataset are to be handled as distinct sequences."},
    )
    pad_to_max_length: bool = field(
        default=False,
        metadata={
            "help": (
                "Whether to pad all samples to `max_seq_length`. "
                "If False, will pad the samples dynamically when batching to the maximum length in the batch."
            )
        },
    )
    max_train_samples: Optional[int] = field(
        default=None,
        metadata={
            "help": (
                "For debugging purposes or quicker training, truncate the number of training examples to this "
                "value if set."
            )
        },
    )
    max_eval_samples: Optional[int] = field(
        default=None,
        metadata={
            "help": (
                "For debugging purposes or quicker training, truncate the number of evaluation examples to this "
                "value if set."
            )
        },
    )

    streaming: bool = field(default=False, metadata={"help": "Enable streaming mode"})
    additional_special_tokens: Optional[str] = field(
        default="<datasource>,</datasource>,<prefix>,</prefix>,<username>,</username>,<accountId>,</accountId>,<session>,</session>,<event>,</event>", 
        metadata={"help": "Comma seperated list of special tokens to add to tokenizer."}
    )

    additional_tokens: Optional[str] = field(
        default=None, metadata={"help": "Comma seperated list of additional tokens to add to tokenizer."}
    )

    masking_strategy: Optional[str] = field(
        default="word", metadata={
            "help": (
                "Type of masking strategy used for MLM. "
                "Note that white_space strategy only supports BPE tokenizer (e.g., gpt2, roberta)."
            ),
            "choices": ["word", "token", "span", "white_space", "token_sep"]
        }
    )

    masking_span_p: Optional[float] = field(
        default=0.2, metadata={"help": "The masking span length follows a geometric distribution, p is the parameter."}
    )

    masking_sep_token: Optional[str] = field(
        default=":", metadata={"help": "Token used to divide input into different spans for MLM."}
    )

    masking_prefix_flag: Optional[bool] = field(
        default=False, metadata={"help": "Mask entities in the prefix as whole."}
    )

    entity_sep: Optional[str] = field(
        default="<datasource>,</datasource>,<username>,</username>,<accountId>,</accountId>", 
        metadata={
            "help": (
                "Comma seperated list of separator tokens, used when masking_prefix_flag = True."
                "The format is: 'start_token_1,end_token_1,start_token_2,end_token_2'"
            )
        }
    )

    def __post_init__(self):
        if self.streaming:
            require_version("datasets>=2.0.0", "The streaming feature requires `datasets>=2.0.0`")
```

## 初始化参数

接下来，我们在上述类中传递输入参数并初始化所有参数：

```py
model_args = ModelArguments(model_name_or_path='microsoft/deberta-base')

data_args = DataTrainingArguments(masking_strategy = 'token_sep', 
                                masking_span_p = 0.2,
                                masking_sep_token = ',',
                                masking_prefix_flag= True,
                                streaming = False,
                                mlm_probability = 0.15,
                                pad_to_max_length = False,
                                line_by_line = True,
                                additional_tokens = None,
                                 )

training_args = TrainingArguments(output_dir = './output', 
                                  max_steps= 2000,
                                  eval_steps= 200,
                                  logging_steps=200,
                                  do_train= True,
                                  do_eval= True,
                                  evaluation_strategy='steps',
                                  remove_unused_columns = False,
                                  label_names = ["labels"],
                                  per_device_train_batch_size = 4,
                                  per_device_eval_batch_size = 4,
                                  overwrite_output_dir = True 
                                 )
```

如你所见，我们正在加载deberta-base模型，因此我们将把这个模型适应于医学领域。

## 数据分词

在这一部分，我们进行数据的分词、整理和分组。

分词部分加载了与我们模型相关的预训练分词器，并添加了特殊标记。

```py
tokenizer_kwargs = {
        "cache_dir": model_args.cache_dir,
        "use_fast": model_args.use_fast_tokenizer,
        "revision": model_args.model_revision,
        "use_auth_token": model_args.use_auth_token,
        "trust_remote_code": model_args.trust_remote_code,
    }

tokenizer = AutoTokenizer.from_pretrained(model_args.model_name_or_path, **tokenizer_kwargs)

tokenizer.add_special_tokens({'additional_special_tokens': data_args.additional_special_tokens.split(",")})

if data_args.additional_tokens:
    tokenizer.add_tokens(data_args.additional_tokens.split(",")) 
```

然后我们加载模型，并使用分词器词汇中的令牌数量更新模型的嵌入层：

```py
config_kwargs = {
        "cache_dir": model_args.cache_dir,
        "revision": model_args.model_revision,
        "use_auth_token": model_args.use_auth_token,
        "trust_remote_code": model_args.trust_remote_code,
    }

config = AutoConfig.from_pretrained(model_args.model_name_or_path, **config_kwargs)

model = AutoModelForMaskedLM.from_pretrained(
            model_args.model_name_or_path,
            from_tf=bool(".ckpt" in model_args.model_name_or_path),
            config=config,
            cache_dir=model_args.cache_dir,
            revision=model_args.model_revision,
            use_auth_token=model_args.use_auth_token,
            trust_remote_code=model_args.trust_remote_code,
            low_cpu_mem_usage=model_args.low_cpu_mem_usage,
        )

embedding_size = model.get_input_embeddings().weight.shape[0]
if len(tokenizer) > embedding_size:
    model.resize_token_embeddings(len(tokenizer))
```

我们还在分词器中更新模型的上下文长度：

```py
# Check if tokenizer.model_max_length is undefined
if tokenizer.model_max_length > 1e9:
    tokenizer.model_max_length = model.config.max_position_embeddings
```

然后我们编写分词函数并将其应用于数据集。我们的数据有一列称为“text”。我们对这一列进行分词并从输出中移除它：

```py
 tokenized_datasets = raw_datasets.map(
      lambda example: tokenizer(example['text']),
      batched=True,
      remove_columns=["text"],
  ) 
```

`tokenized_datasets`如下所示：

![](../Images/8cd8e23fa2768b624a2d89b565840132.png)

作者提供的图片

如果我们检查`train`段中每个记录的`input_ids`的长度，我们会看到记录的`input_ids`长度不同。

```py
l = []
for item in tokenized_datasets['train']:
    l.append(len(item['input_ids']))
print(set(l))
```

它打印出以下长列表：

```py
2243,1204, 2310, 2402, 645, 2319, ....
```

要点是每个记录有不同的序列长度。我们可以填充它们、截断它们或将它们分组为上下文长度的序列，以确保它们大小相同。

```py
 data_args.max_seq_length = tokenizer.model_max_length
max_seq_length = data_args.max_seq_length

# Main data processing function that will concatenate all texts from our dataset and generate chunks of
# max_seq_length.
def group_texts(examples):
    # Concatenate all texts.
    concatenated_examples = {k: list(chain(*examples[k])) for k in examples.keys()}
    total_length = len(concatenated_examples[list(examples.keys())[0]])
    # We drop the small remainder, and if the total_length < max_seq_length  we exclude this batch and return an empty dict.
    # We could add padding if the model supported it instead of this drop, you can customize this part to your needs.
    total_length = (total_length // max_seq_length) * max_seq_length
    # Split by chunks of max_len.
    result = {
        k: [t[i : i + max_seq_length] for i in range(0, total_length, max_seq_length)]
        for k, t in concatenated_examples.items()
    }
    return result

tokenized_datasets = tokenized_datasets.map(
    group_texts,
    batched=True,
)
```

现在，如果你重复这个练习：

```py
l = []
for item in tokenized_datasets['train']:
    l.append(len(item['input_ids']))
print(set(l))
```

它只打印`{512}`，因为所有序列的长度都是512。

接下来我们定义数据整理器：

```py
@dataclass    
class MaskingDataCollator:
    tokenizer: PreTrainedTokenizerBase
    wwm_probability: Optional[float] = 0.2

    def __call__(self, features: List[Dict[str, Any]]) -> Dict[str, Any]:
        for i, feature in enumerate(features):
            mask_ids = feature.pop("mask_ids")

            # word_id to token index mapping
            mapping = self.word_mapping(mask_ids)
            # Randomly mask words
            if "labels" not in feature.keys():
                labels = feature["input_ids"].copy()
            else:
                labels = feature["labels"]
            feature["labels"], _ = self.random_masking_whole_word(mapping, feature["input_ids"], labels, self.tokenizer.mask_token_id)

        batch = default_data_collator(features)
        return batch

    def word_mapping(self, mask_ids):
        # Create a map between words and corresponding token start and end inds
        mapping = defaultdict(list)
        current_word_index = -1
        current_word = None
        for i, word_id in enumerate(mask_ids):
            if word_id is not None:
                if word_id != current_word:
                    current_word = word_id
                    current_word_index += 1
                mapping[current_word_index].append(i)

        return mapping

    def random_masking_whole_word(self, mapping, input_ids, labels, mask_token_id):

        mask = np.random.binomial(1, self.wwm_probability, size=len(mapping))

        # masked at least one mask_id
        if sum(mask) == 0:
            rn_i = random.choice(range(len(mask)))
            mask[rn_i] = 1

        new_labels = [-100] * len(labels)

        for word_id in np.where(mask)[0]:
            word_id = word_id.item()
            for idx in mapping[word_id]:
                new_labels[idx] = labels[idx]
                input_ids[idx] = mask_token_id
        return new_labels, input_ids

data_collator = MaskingDataCollator(
        tokenizer, 
        wwm_probability=data_args.mlm_probability
    )
```

## 训练模型

如果我们要在参数高效模式下进行训练，我们使用lora包，如下所示：

```py
peft_args = PEFTArguments()

peft_config = None
if peft_args.lora_r > 0:
    logger.info("Using LoRA for model adaptation...")
    peft_config = LoraConfig(
        r=peft_args.lora_r,
        lora_alpha=peft_args.lora_alpha,
        lora_dropout=peft_args.lora_dropout,
        target_modules=peft_args.target_modules.split(",") if peft_args.target_modules 
        else TRANSFORMERS_MODELS_TO_LORA_TARGET_MODULES_MAPPING[model.config.model_type]
    )

    model = get_peft_model(model, peft_config)
```

然后我们继续编写用于计算选择指标的`compute_metrics`函数。这里我们使用*准确率*。

```py
train_dataset = tokenized_datasets["train"]
eval_dataset = tokenized_datasets["validation"]

def compute_metrics(eval_preds):
    preds, labels = eval_preds
    labels = labels.reshape(-1)
    preds = preds.reshape(-1)
    # this is to ensure we compute loss on masked entities
    mask = labels != -100
    labels = labels[mask]
    preds = preds[mask]
    return metric.compute(predictions=preds, references=labels)
```

注意`mask = labels != -100`是为了确保我们在被掩盖的实体上计算损失。对于被掩盖的实体，其对应的标签是一个正ID（即原始标记在该位置的输入ID）。对于没有被掩盖的实体，因此我们不想计算模型在这些实体上的表现，其对应的标签被设置为`-100`。

定义`mask = labels != -100`产生`mask`作为布尔向量，只有在实体被掩盖时它才为True。

**Logit 处理**：然后，我们对logits进行预处理。以下函数返回最大logit出现的索引。这将是模型的预测。

```py
def preprocess_logits_for_metrics(logits, labels):
    if isinstance(logits, tuple):
        # Depending on the model and config, logits may contain extra tensors,
        # like past_key_values, but logits always come first
        logits = logits[0]
    return logits.argmax(dim=-1)
```

**Trainer**：

这是我们初始化`trainer`对象的地方。我们很快通过`trainer.train()`开始训练。

```py
# Initialize our Trainer
trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=train_dataset,
    eval_dataset=eval_dataset,
    tokenizer=tokenizer,
    data_collator=data_collator,
    compute_metrics=compute_metrics if training_args.do_eval and not is_torch_tpu_available() else None,
    preprocess_logits_for_metrics=preprocess_logits_for_metrics
    if training_args.do_eval and not is_torch_tpu_available()
    else None,
)
```

让我们通过`trainer.train()`启动训练并保存工件：

```py
train_result = trainer.train()
trainer.save_model()  # Saves the tokenizer too for easy upload
metrics = train_result.metrics

if not data_args.streaming:
    max_train_samples = (
        data_args.max_train_samples if data_args.max_train_samples is not None else len(train_dataset)
    )
    metrics["train_samples"] = min(max_train_samples, len(train_dataset))
else:
    metrics["max_steps"] = training_args.max_steps

trainer.log_metrics("train", metrics)
trainer.save_metrics("train", metrics)
trainer.save_state()
```

它打印出以下输出：

![](../Images/627f970f24d0601cd081d26e49947868.png)

作者提供的图片

首先，注意`trainer.train()`返回一个名为`train_result`的`TrainOutput`类型对象。该对象如下所示：

```py
TrainOutput(global_step=2000, training_loss=3.754445343017578, metrics={'train_runtime': 794.2916, 'train_samples_per_second': 10.072, 'train_steps_per_second': 2.518, 'total_flos': 2454016352256000.0, 'train_loss': 3.754445343017578, 'epoch': 0.78, 'train_samples': 10261})
```

并且注意使用`metrics = train_result.metrics`我们正在访问*metrics*字典。稍后我们将把这个字典传递给`trainer.log_metrics()`和`trainer.save_metrics()`。

`trainer.log_metrics()`打印出的报告如下所示：

![](../Images/037c6316aef7914599c45733639deb1d.png)

作者提供的图片

其次，注意我们保存了一些东西：

+   `trainer.save_model()`：这将保存模型及其tokenizer。我们可以稍后使用`from_pretrained()`重新加载它。

+   `trainer.save_state()`：这将保存训练器的状态，因为`trainer.save_model()`不会保存状态。此语句会创建一个*trainer_state.json*文件，如下所示：

![](../Images/ef0ac5e2cbb59d6021982a0a3052215b.png)

作者提供的图片

+   `trainer.save_metrics("train", metrics)`：这将把指标保存到该训练分割的json文件中，例如`train_results.json`。这个文件如下所示：

![](../Images/858a5c6fea6d1685699160197fcf196a.png)

作者提供的图片

# 结论

在这篇文章中，我们回顾了如何将预训练的LLM适应到新的领域，如医学、金融等。我们从huggingFace获取了一个预训练的deberta基础模型，并继续在医学数据上进行再训练。我们将训练好的模型保存在一个目录中以便进行定制化评估。

如果你有任何问题或建议，请随时联系我：

Email: mina.ghashami@gmail.com

LinkedIn: [https://www.linkedin.com/in/minaghashami/](https://www.linkedin.com/in/minaghashami/)
