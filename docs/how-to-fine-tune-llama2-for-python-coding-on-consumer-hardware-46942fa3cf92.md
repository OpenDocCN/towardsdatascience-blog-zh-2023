# 如何在消费级硬件上微调 Llama2 以进行 Python 编程

> 原文：[`towardsdatascience.com/how-to-fine-tune-llama2-for-python-coding-on-consumer-hardware-46942fa3cf92`](https://towardsdatascience.com/how-to-fine-tune-llama2-for-python-coding-on-consumer-hardware-46942fa3cf92)

## *通过监督微调和低秩适配技术提升 Llama2 在 Python 中的熟练度*

[](https://medium.com/@luisroque?source=post_page-----46942fa3cf92--------------------------------)![Luís Roque](https://medium.com/@luisroque?source=post_page-----46942fa3cf92--------------------------------)[](https://towardsdatascience.com/?source=post_page-----46942fa3cf92--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----46942fa3cf92--------------------------------) [Luís Roque](https://medium.com/@luisroque?source=post_page-----46942fa3cf92--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----46942fa3cf92--------------------------------) ·18 分钟阅读·2023 年 8 月 17 日

--

# 关于我

连续创业者和 AI 领域的领军人物。我为企业开发 AI 产品，并投资于 AI 专注的初创公司。

[创始人 @ ZAAI](http://zaai.ai) | [LinkedIn](https://www.linkedin.com/in/luisbrasroque/) | [X/Twitter](https://x.com/luisbrasroque)

# 介绍

我们的上一篇文章详细介绍了 Llama 2，介绍了 Meta 最近推出并向社区提供研究和商业使用的大型语言模型（LLMs）家族。已经有专为特定任务设计的变体；例如，Llama2-Chat 适用于聊天应用。不过，我们可能希望获得一个更贴合我们应用的 LLM。

按照这一思路，我们所提到的技术是迁移学习。这种方法涉及利用像 Llama2 这样的模型中已有的广泛知识，并将这些理解迁移到新的领域。微调是迁移学习的一个子集或特定形式。在微调过程中，整个模型的权重，包括预训练层，通常会根据新数据进行调整。这意味着在预训练期间获得的知识将根据新任务的具体情况进行细化。

在本文中，我们概述了一种系统的方法，通过在自定义数据集上微调 Llama2 来提升其在 Python 编码任务中的熟练度。首先，我们整理和对齐一个与 Llama2 的提示结构相匹配的数据集，以满足我们的目标。然后，我们使用监督微调（SFT）和量化低秩适配（QLoRA）来优化 Llama2 基础模型。优化后，我们将我们模型的权重与基础的 Llama2 结合起来。最后，我们展示了如何使用微调后的模型进行推理，并比较其与基线模型的效果。

![](img/cfb419c3a9c7606b26c5f499751a59fe.png)

图 1：Llama2，Python 编码器 ([图片来源](https://unsplash.com/photos/Z5Yha3jIHnc))

一个重要的警示是，有时微调是不必要的。其他方法更易于实施，并且在某些情况下更适合我们的使用场景。例如，使用向量数据库进行的语义搜索可以有效处理信息查询，利用现有知识而无需自定义训练。微调所需的使用场景是当我们需要量身定制的交互，例如专业的问答或利用自定义数据的上下文感知响应时。

# 监督式微调

现代机器学习范式通常利用预训练模型。这些预训练模型已经在大数据集上进行过训练。SFT 的目标是使用最少的训练数据将它们适应于特定任务。

SFT 的工作方式是通过根据标记的示例调整 LLM，例如 Llama2，这些示例指定了模型应生成的数据。SFT 的数据集由提示和相关的响应组成。开发者可以手动创建这个数据集，也可以使用其他 LLM 生成。事实上，开源社区经常采用这种做法。对 Open LLM Leaderboard [1] 上顶级 LLM 的审查显示，几乎所有的模型都经过了某种形式的微调，使用的是 Orca 风格的数据集。Orca 风格的数据集包含大量条目，每个条目都有一个问题和来自 GPT-4 或 GPT-3.5 的相应回答。实质上，SFT 通过一组特定的示例来提升 Llama2 的知识水平。

研究人员现在明确地对许多 LLM 进行指令跟随能力的微调。这种微调帮助模型更好地理解和执行用户指令。例如，当用户指示模型创建一个总结时，一个经过微调的模型可以生成一个简洁的总结。一个未经过微调的模型可能会在这个任务上挣扎，并变得更加冗长。随着 LLM 的发展，这种微调可以产生更专业的模型，更适合预期的使用场景。

# LoRA 和 QLoRA：一种高效的微调大型模型的方法

要理解 LoRA 的运作，我们必须首先了解矩阵秩的含义。矩阵的秩显示了其独立行或列的数量。例如，一个填充随机数的 NxN 矩阵的秩为 N。然而，如果这个矩阵的每一列只是第一列的倍数，则秩变为 1。因此，我们可以将秩为 1 的矩阵表示为两个矩阵的乘积：一个 Nx1 矩阵乘以一个 1xN 矩阵，从而得到一个秩为 1 的 NxN 矩阵。同样，我们可以将秩为 ‘r’ 的矩阵表示为一个 (Nxr) 矩阵和一个 (rxN) 矩阵的乘积。

LoRA 利用矩阵秩的概念来使用更少的内存进行微调。LoRA 并不是调整 LLM 的所有权重，而是对低秩矩阵进行微调，并将其添加到现有的权重中 [2]。现有的权重（或大矩阵）保持不变，而训练仅调整低秩矩阵。

为什么这效率高？低秩矩阵的参数显著较少。与管理 N² 个参数相比，使用 LoRA 只需处理 2*r*N 个参数。直观地说，微调就像对原始矩阵进行轻微调整。LoRA 以计算成本较低的方式确定这些调整，以换取一些准确度来提高效率。

使用 LoRA 进行训练仍然需要整个预训练模型进行前向传递，伴随额外的 LoRA 计算。然而，在反向传播过程中，计算主要集中在 LoRA 部分的梯度上。这种方法在 GPU 内存需求方面带来了计算节省。这就是为什么它目前是将模型调整到新任务的最流行的方法之一，而无需传统微调的广泛计算开销。

为了提高内存效率，我们可以使用 QLoRA [3]。它建立在 LoRA 的基础上，允许使用这些适配器与量化的预训练模型。实际上，这种方法使得在 48GB GPU 上对 65B 参数模型进行微调成为可能，同时保持了全 16 位微调任务的性能。QLoRA 还引入了其他特性来提高内存效率。4 位 NormalFloat (NF4) 数据类型提供了对正态分布权重的更紧凑表示。它还采用了双重量化，通过对量化常量本身进行量化（可以把它想象成“一层层的海龟”但加上量化）来进一步减少内存使用。最后一个特性是分页优化器来管理内存峰值。

# 为 Python 编程任务策划适用的数据集

我们开始定义一个 Config 类，作为与微调过程相关的配置设置和元数据的集中存储库。它存储各种常量，如模型和数据集名称、输出目录和几个参数，这些我们将在接下来的部分中讨论。让我们看看与数据预处理相关的变量。

```py
class Config:
    MODEL_NAME = "meta-llama/Llama-2-7b-hf"
    OUTPUT_DIR = "./results"
    NEW_DATASET_NAME_COMPLETE = "luisroque/instruct-python-llama2-500k"
    NEW_DATASET_NAME = "luisroque/instruct-python-llama2-20k"
    NEW_DATASET_NAME_LOCAL = "instruct-python-llama2-500k.pkl"
    NEW_MODEL_PATH = "./Llama-2-7b-minipython-instruct"
    NEW_MODEL_PATH_MERGE = "./Llama-2-7b-minipython-instruct-merge"
    NEW_MODEL_NAME = "Llama-2-7b-minipython-instruct"
    HF_HUB_MODEL_NAME = "luisroque/Llama-2-7b-minipython-instruct"
    SYSTEM_MESSAGE = "Given a puzzle-like code question, provide a well-reasoned, step-by-step Python solution."
```

当为像 Llama2 这样的模型量身定制以处理以 Python 为中心的任务时，数据集的选择非常重要。我们正在使用 [Python Questions from StackOverflow Dataset](https://www.kaggle.com/datasets/stackoverflow/pythonquestions) ([CC-BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/)) 数据集，该数据集包含大量带有 Python 标签的编码互动。由于我们希望对模型进行 Python 编程方面的微调，因此我们对该数据集进行了优化，专注于 Python 相关的交流。

```py
def contains_code(text):
    python_keywords = [
        "def",
        "class",
        "import",
        "print",
        "return",
        "for",
        "while",
        "if",
        "else",
        "elif",
        "try",
        "except",
        "lambda",
        "list",
        "dict",
        "set",
        "str",
        "=",
        "{",
        "}",
        "(",
        ")",
    ]

    for keyword in python_keywords:
        if keyword in text:
            return True
    return False

def load_data_to_fine_tune():
    """Load the dataset and filter for Python language."""
    dtypes_questions = {"Id": "int32", "Score": "int16", "Title": "str", "Body": "str"}
    df_questions = pd.read_csv(
        "Questions.csv",
        usecols=["Id", "Score", "Title", "Body"],
        encoding="ISO-8859-1",
        dtype=dtypes_questions,
    )

    dtypes_answers = {
        "Id": "int32",
        "ParentId": "int32",
        "Score": "int16",
        "Body": "str",
    }
    df_answers = pd.read_csv(
        "Answers.csv",
        usecols=["Id", "ParentId", "Score", "Body"],
        encoding="ISO-8859-1",
        dtype=dtypes_answers,
    )

    merged = pd.merge(
        df_questions, df_answers, left_on="Id", right_on="ParentId", how="inner"
    )
    # Sort by score of the answer in descending order and drop duplicates based on question ID
    merged = merged.sort_values(by="Score_y", ascending=False).drop_duplicates(
        subset="Id_x", keep="first"
    )

    # Remove HTML tags using BeautifulSoup
    merged["Body_x"] = merged["Body_x"].apply(
        lambda x: BeautifulSoup(x, "lxml").get_text()
    )
    merged["Body_y"] = merged["Body_y"].apply(
        lambda x: BeautifulSoup(x, "lxml").get_text()
    )

    merged["combined_question"] = merged["Title"] + ": " + merged["Body_x"]

    # Rename and select the desired columns
    final_df = merged[["Score_x", "Score_y", "combined_question", "Body_y"]]
    final_df.columns = ["score_question", "score_answer", "question", "answer"]

    final_df = final_df[
        (final_df["score_question"] >= 0) & (final_df["score_answer"] >= 0)
    ]

    # Contains code that resembles python code
    final_df = final_df[
        final_df["question"].apply(contains_code)
        | final_df["answer"].apply(contains_code)
    ]

    return final_df
```

在下一步中，我们确保我们的数据符合 Llama2 的提示结构：

> *<s>[INST] <<SYS>> {{ system_prompt }} <</SYS>> {{ user_message }} [/INST]*

上述结构与模型的训练过程一致，因此对微调质量有显著影响。请记住，‘system_prompt’ 代表模型的指令或上下文。用户的消息紧随系统提示，并寻求模型的特定响应。

我们为每个数据条目定制了明确的系统指令，以指导模型在训练期间。

```py
def transform_dataset_format(df):
    """Transform the dataframe into a specified format."""

    def transform(row):
        user_text = row["question"]
        assistant_text = row["answer"]

        return {
            "text": f"<s>[INST] <</SYS>>\n{Config.SYSTEM_MESSAGE.strip()}\n<</SYS>>\n\n"
            f"{user_text} [/INST] {assistant_text} </s>"
        }

    transformed_data = df.apply(transform, axis=1)
    transformed_df = transformed_data.to_frame(name="text")

    return transformed_df
```

一旦我们将数据集转换为符合 Llama2 提示结构的格式，我们利用 Hugging Face 平台进行存储。我们拆分了数据集，为验证目的设置了 1000 条记录，这在后续会有所帮助。对于爱好者和研究人员，我们将精炼的数据集命名为[*luisroque/instruct-python-llama2–20k*](https://huggingface.co/datasets/luisroque/instruct-python-llama2-20k)，更大的数据集命名为[*luisroque/instruct-python-llama2–500k*](https://huggingface.co/datasets/luisroque/instruct-python-llama2-500k)，这些都在 Hugging Face Hub 上公开提供。

```py
def publish_to_hugging_face(transformed_dataset):
    """Publish the transformed dataset to Hugging Face datasets."""
    splits = transformed_dataset.train_test_split(test_size=1000, shuffle=True)
    splits.push_to_hub(Config.NEW_DATASET_NAME)
```

# 使用监督微调（SFT）和量化低秩适应（QLoRA）对 Llama2 进行微调。

在为 Llama2 进行微调选择超参数时，我们希望平衡效率和效果。我们要确保快速的实验周期，因此我们只定义了一个`epoch`和一个适中的`batch size`为 2。在一些测试后，我们选择了`learning rate`为 2e-4，因为它在我们的用例中收敛良好。`weight decay`为 0.001 有助于正则化并防止过拟合。鉴于 LLM 的复杂性，我们选择了最大梯度范数为 0.3，以防止训练期间的更新过大。调度器的`cosine`特性确保了学习率的退火以稳定收敛，而我们的优化器`paged_adamw_32bit`由 QLoRA 论文引入，提供了较少的内存波动。我们还采用了 4-bit 量化进一步提高内存效率，选择了`nf4`类型进行量化（这是 QLoRA 论文的另一个补充）。最后，LoRA 特定的参数，包括`alpha`为 16，`dropout`为 0.1，以及`rank`为 64，也是根据经验实验选择的。

```py
class Config:
    NUM_EPOCHS = 1
    BATCH_SIZE = 2
    GRAD_ACC_STEPS = 1
    SAVE_STEPS = 25
    LOG_STEPS = 5
    LEARNING_RATE = 2e-4
    WEIGHT_DECAY = 0.001
    MAX_GRAD_NORM = 0.3
    SCHEDULER_TYPE = "cosine"
    PER_DEVICE_TRAIN_BATCH_SIZE = 4
    PER_DEVICE_EVAL_BATCH_SIZE = 4
    OPTIM = "paged_adamw_32bit"
    FP16 = False
    BF16 = False
    MAX_STEPS = 2500
    WARMUP_RATIO = 0.03
    GROUP_BY_LENGTH = 3
    LORA_ALPHA = 16
    LORA_DROPOUT = 0.1
    LORA_R = 64
    DEVICE_MAP = {"": 0}
    USE_4BIT = True
    BNB_4BIT_COMPUTE_DTYPE = "float16"
    BNB_4BIT_COMPUTE_QUANT_TYPE = "nf4"
    USE_NESTED_QUANT = False
```

在`initialize_model_and_tokenizer`函数中，我们使用`Config.BNB_4BIT_COMPUTE_DTYPE`设置计算数据类型，以优化 4-bit 量化。然后使用`BitsAndBytesConfig`配置这种量化。我们使用`AutoModelForCausalLM`加载基础预训练 Llama2 模型，并使用我们的量化配置初始化它，关闭缓存以节省内存。我们将模型映射到单个 GPU 上，但可以很容易地修改此配置以支持多 GPU 设置。然后我们获取 tokenizer，它将输入翻译给模型。

```py
def initialize_model_and_tokenizer():
    """Initialize the model and tokenizer."""

    compute_dtype = getattr(torch, Config.BNB_4BIT_COMPUTE_DTYPE)
    bnb_config = BitsAndBytesConfig(
        load_in_4bit=Config.USE_4BIT,
        bnb_4bit_quant_type=Config.BNB_4BIT_COMPUTE_QUANT_TYPE,
        bnb_4bit_compute_dtype=compute_dtype,
        bnb_4bit_use_double_quant=Config.USE_NESTED_QUANT,
    )
    model = AutoModelForCausalLM.from_pretrained(
        Config.MODEL_NAME, quantization_config=bnb_config, device_map=Config.DEVICE_MAP
    )
    model.config.use_cache = False
    model.config.pretraining_tp = 1
    tokenizer = AutoTokenizer.from_pretrained(Config.MODEL_NAME, trust_remote_code=True)
    tokenizer.pad_token = tokenizer.eos_token
    tokenizer.padding_side = "right"

    return model, tokenizer
```

我们可以打印我们的模型，以确保它们正确加载。让我们开始加载经过 4-bit 量化的预训练 Llama2 模型。注意，所有层都已正确量化。

```py
LlamaForCausalLM(                                                                                        
  (model): LlamaModel(                                                                                   
    (embed_tokens): Embedding(32000, 4096, padding_idx=0)                                                
    (layers): ModuleList(                                                                                
      (0-31): 32 x LlamaDecoderLayer(                                                                    
        (self_attn): LlamaAttention(                                                                     
          (q_proj): Linear4bit(in_features=4096, out_features=4096, bias=False)                          
          (k_proj): Linear4bit(in_features=4096, out_features=4096, bias=False)                          
          (v_proj): Linear4bit(in_features=4096, out_features=4096, bias=False)                          
          (o_proj): Linear4bit(in_features=4096, out_features=4096, bias=False)                          
          (rotary_emb): LlamaRotaryEmbedding()                                                           
        )                                                                                                
        (mlp): LlamaMLP(                                                                                 
          (gate_proj): Linear4bit(in_features=4096, out_features=11008, bias=False)                      
          (up_proj): Linear4bit(in_features=4096, out_features=11008, bias=False)                        
          (down_proj): Linear4bit(in_features=11008, out_features=4096, bias=False)                      
          (act_fn): SiLUActivation()                                                                     
        )                                                                                                
        (input_layernorm): LlamaRMSNorm()                                                                
        (post_attention_layernorm): LlamaRMSNorm()                                                       
      )                                                                                                  
    )                                                                                                    
    (norm): LlamaRMSNorm()                                                                               
  )                                                                                                      
  (lm_head): Linear(in_features=4096, out_features=32000, bias=False)                                    
)
```

作为附注，我们可以通过上述打印信息了解 Llama2 的架构。它使用一个嵌入层将最多 32,000 个令牌转换为 4,096 维的向量。该模型的计算引擎由 32 个顺序的`LlamaDecoderLayer`模块组成。在每个解码器层中，`LlamaAttention`机制以 4 位精度的线性投影进行查询、键、值和输出。注意力机制使用旋转嵌入，这些嵌入用于动态捕捉序列数据中的位置信息。除了注意力机制，它还具有 4 位线性投影，并利用 Sigmoid 线性单元（SiLU）激活函数进行非线性变换。为了确保跨层一致的激活，模型结合了`LlamaRMSNorm`进行输入后和注意力后层归一化。最后一个线性层将高维表示转换回 32,000 令牌的词汇大小，这使得令牌预测成为可能。

我们现在可以使用 QLoRA 对 Llama2 进行微调。首先，我们使用之前定义的 LoRA 设置配置模型。然后，我们将模型准备好进行 4 位训练，并将其与 LoRA 配置集成。我们设置训练参数，并将它们输入到 SFTTrainer 进行微调。`configure_training_args`函数定义了模型的训练参数，参考了我们之前讨论的`Config`类。训练完成后，我们将模型和分词器保存在指定目录中，并使用生成任务测试模型的性能。按照良好实践，我们清除了模型的内存并清空了 GPU 缓存。我们还装饰了我们的函数，以监控执行时间和内存消耗。

```py
def time_decorator(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result, metrics = func(*args, **kwargs)
        end_time = time.time()
        exec_time = end_time - start_time
        metrics["exec_time"] = exec_time
        return result, metrics

    return wrapper

def memory_decorator(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        torch.cuda.empty_cache()
        torch.cuda.reset_peak_memory_stats()
        result, metrics = func(*args, **kwargs)
        peak_mem = torch.cuda.max_memory_allocated()
        peak_mem_consumption = peak_mem / 1e9
        metrics["peak_mem_consumption"] = peak_mem_consumption
        return result, metrics

    return wrapper

def configure_training_args():
    """Configure training arguments."""
    return TrainingArguments(
        output_dir=Config.OUTPUT_DIR,
        num_train_epochs=Config.NUM_EPOCHS,
        per_device_train_batch_size=Config.PER_DEVICE_TRAIN_BATCH_SIZE,
        gradient_accumulation_steps=Config.GRAD_ACC_STEPS,
        optim=Config.OPTIM,
        save_steps=Config.SAVE_STEPS,
        logging_steps=Config.LOG_STEPS,
        learning_rate=Config.LEARNING_RATE,
        weight_decay=Config.WEIGHT_DECAY,
        fp16=Config.FP16,
        bf16=Config.BF16,
        max_grad_norm=Config.MAX_GRAD_NORM,
        max_steps=Config.MAX_STEPS,
        warmup_ratio=Config.WARMUP_RATIO,
        group_by_length=Config.GROUP_BY_LENGTH,
        lr_scheduler_type=Config.SCHEDULER_TYPE,
        report_to="all",
        evaluation_strategy="steps",
        eval_steps=50,
    )

@memory_decorator
@time_decorator
def fine_tune_and_save_model(model, tokenizer, train_dataset, val_dataset):
    """Fine-tune the model and save it."""

    peft_config = LoraConfig(
        lora_alpha=Config.LORA_ALPHA,
        lora_dropout=Config.LORA_DROPOUT,
        r=Config.LORA_R,
        bias="none",
        task_type="CAUSAL_LM",
    )

    model = prepare_model_for_kbit_training(model)
    model = get_peft_model(model, peft_config)

    model.print_trainable_parameters()

    training_args = configure_training_args()

    trainer = SFTTrainer(
        model=model,
        train_dataset=train_dataset,
        eval_dataset=val_dataset,
        dataset_text_field="text",
        peft_config=peft_config,
        tokenizer=tokenizer,
        args=training_args,
        max_seq_length=512,
    )
    trainer.train()

    if not os.path.exists(Config.NEW_MODEL_PATH):
        os.makedirs(Config.NEW_MODEL_PATH)

    trainer.model.save_pretrained(Config.NEW_MODEL_PATH)
    tokenizer.save_pretrained(Config.NEW_MODEL_PATH)

    generate_code_from_prompt(model, tokenizer)

    del model
    torch.cuda.empty_cache()

    return None, {}
```

我们可以再次打印模型，以确保我们正确设置了 LoRA 参数和量化。回顾一下，通过引入一组新的、低秩的可训练参数，LoRA 在模型中创建了一个瓶颈，将表示通过这些参数进行通道化。请注意，LoRA 组件，特别是`lora_A`和`lora_B`线性层，已经集成到注意力机制中。只有这些 LoRA 参数在微调期间被积极训练，从而保留模型的原始知识，同时优化它以适应新任务。LoRA 的默认配置将它们应用于注意力机制中的`q_proj`（查询投影）和`v_proj`（值投影），以提高效率。LoRA 论文[3]实际上将其应用于所有层，因此也可以尝试这些层。

```py
PeftModelForCausalLM(                                                                                    
  (base_model): LoraModel(      
    (model): LlamaForCausalLM(                                                                     [0/50]
      (model): LlamaModel( 
        (embed_tokens): Embedding(32000, 4096, padding_idx=0)
        (layers): ModuleList(
          (0-31): 32 x LlamaDecoderLayer(
            (self_attn): LlamaAttention(
              (q_proj): Linear4bit(
                in_features=4096, out_features=4096, bias=False
                (lora_dropout): ModuleDict(
                  (default): Dropout(p=0.1, inplace=False)
                )
                (lora_A): ModuleDict(
                  (default): Linear(in_features=4096, out_features=64, bias=False)
                )
                (lora_B): ModuleDict(
                  (default): Linear(in_features=64, out_features=4096, bias=False)
                )
                (lora_embedding_A): ParameterDict()
                (lora_embedding_B): ParameterDict()
              )
              (k_proj): Linear4bit(in_features=4096, out_features=4096, bias=False)
              (v_proj): Linear4bit(
                in_features=4096, out_features=4096, bias=False
                (lora_dropout): ModuleDict(
                  (default): Dropout(p=0.1, inplace=False)
                )
                (lora_A): ModuleDict(
                  (default): Linear(in_features=4096, out_features=64, bias=False)
                )
                (lora_B): ModuleDict(
                  (default): Linear(in_features=64, out_features=4096, bias=False)
                )
                (lora_embedding_A): ParameterDict()
                (lora_embedding_B): ParameterDict()
              )
              (o_proj): Linear4bit(in_features=4096, out_features=4096, bias=False)
              (rotary_emb): LlamaRotaryEmbedding()
            )
            (mlp): LlamaMLP(
              (gate_proj): Linear4bit(in_features=4096, out_features=11008, bias=False)
              (up_proj): Linear4bit(in_features=4096, out_features=11008, bias=False)
              (down_proj): Linear4bit(in_features=11008, out_features=4096, bias=False)
              (act_fn): SiLUActivation()
            )
            (input_layernorm): LlamaRMSNorm()
            (post_attention_layernorm): LlamaRMSNorm()
          )
        )
        (norm): LlamaRMSNorm()
      )
      (lm_head): Linear(in_features=4096, out_features=32000, bias=False)
    )
  )
)
```

我们可以快速获取可训练参数的数量，并检查它与预训练模型中总参数数量的比较。我们可以看到，我们训练的参数不到 1%。

```py
def print_trainable_parameters(model):
    """Prints the number of trainable parameters in the model."""
    trainable_params = 0
    all_param = 0
    for _, param in model.named_parameters():
        all_param += param.numel()
        if param.requires_grad:
            trainable_params += param.numel()
    print(
        f"trainable params: {trainable_params} || "
        f"all params: {all_param} || "
        f"trainable%: {100 * trainable_params / all_param}"
    )

trainable params: 33,554,432 || all params: 3,533,967,360 || trainable%: 0.9494833591219133
```

最后一步是将新权重与基础模型合并。这可以通过简单地加载两个实例并调用`merge_and_unload()`方法来完成。

```py
def merge_and_save_weights():
    """Merges the weights of a given model and saves the merged weights to a specified directory."""

    if not os.path.exists(Config.NEW_MODEL_PATH_MERGE):
        os.makedirs(Config.NEW_MODEL_PATH_MERGE)

    base_model = AutoModelForCausalLM.from_pretrained(
        Config.MODEL_NAME,
        low_cpu_mem_usage=True,
        return_dict=True,
        torch_dtype=torch.float16,
        device_map=Config.DEVICE_MAP,
    )
    model = PeftModel.from_pretrained(base_model, Config.NEW_MODEL_NAME)
    model = model.merge_and_unload()

    tokenizer = AutoTokenizer.from_pretrained(Config.MODEL_NAME, trust_remote_code=True)
    tokenizer.pad_token = tokenizer.eos_token
    tokenizer.padding_side = "right"

    model.save_pretrained(Config.NEW_MODEL_PATH)
    tokenizer.save_pretrained(Config.NEW_MODEL_PATH)
```

微调后，训练好的模型和分词器可以轻松地分享到 Hugging Face Hub，促进协作和可重用性。你可以在 [*luisroque/Llama-2-7b-minipython-instruct*](https://huggingface.co/luisroque/Llama-2-7b-minipython-instruct)*.* 找到我们的微调模型。

```py
def push_model_to_hub():
    """Push the fine-tuned model and tokenizer to the Hugging Face Hub."""
    model = AutoModelForCausalLM.from_pretrained(Config.NEW_MODEL_PATH)
    tokenizer = AutoTokenizer.from_pretrained(Config.NEW_MODEL_PATH)

    model.push_to_hub(Config.HF_HUB_MODEL_NAME, use_temp_dir=False)
    tokenizer.push_to_hub(Config.HF_HUB_MODEL_NAME, use_temp_dir=False)
```

# **使用 Llama2 和微调模型的推断过程**

在我们漫长的 Llama2 微调任务中，最后一步是进行测试。我们实现了一种简单的方法来运行基础模型和微调模型的推断，以帮助比较这两者。

函数 `generate_response` 负责实际的推断。它利用 Hugging Face 的 pipeline 工具，根据给定的提示、模型和分词器生成文本。如果微调后的模型已经在 Hugging Face Hub 中或存储在本地，我们不需要再次合并，只需直接访问即可。

```py
def generate_response(model_name, tokenizer, prompt, max_length=600):
    """Generate a response using the specified model."""
    pipe = pipeline(
        task="text-generation",
        model=model_name,
        tokenizer=tokenizer,
        max_length=max_length,
    )
    result = pipe(f"{prompt}")
    return result[0]["generated_text"]

def main(model_to_run):
    prompt = (
        f"[INST] <<SYS>>\n{Config.SYSTEM_MESSAGE}\n<</SYS>>\n\n"
        f"Write a function that reverses a linked list. [/INST]"
    )

    if model_to_run == "new_model":
        new_tokenizer = AutoTokenizer.from_pretrained(Config.HF_HUB_MODEL_NAME)
        new_model_response = generate_response(
            Config.HF_HUB_MODEL_NAME, new_tokenizer, prompt
        )
        print("Response from new model:")
        print(new_model_response)
    else:
        llama_model_name = Config.MODEL_NAME
        llama_tokenizer = AutoTokenizer.from_pretrained(llama_model_name)
        llama_model_response = generate_response(
            llama_model_name, llama_tokenizer, prompt
        )

        print("\nResponse from Llama2 base model:")
        print(llama_model_response)
```

我们将脚本的入口点定义为基于命令行的。用户可以通过参数指定他们的模型偏好，"new_model" 或 "llama2"，使得在模型之间轻松切换，并直接比较它们的推断输出。

```py
if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="Run different models.")
    parser.add_argument(
        "model_to_run", type=str, help='Which model to run: "new_model" or "llama2"'
    )
    args = parser.parse_args()

    main(args.model_to_run)
```

# **简化工作流程的 Makefile**

自动化在简化复杂流程中发挥了重要作用，特别是在具有多个顺序步骤的机器学习任务中。makefile 是一个很好的工具，帮助我们为用户提供一个清晰、结构化且易于执行的工作流程。

在提供的 makefile 中，微调过程的每一步，从设置环境到运行推断，都被定义为一个单独的目标。这种抽象允许用户通过单个、简洁的命令来执行特定任务。

以下是用户如何使用提供的 makefile 运行不同任务的示例：

```py
make setup
```

此命令将执行 setup 目标，创建一个名为 fine_tune_llama2 的新 conda 环境，使用 Python 3.10。

```py
make install
```

install 目标将从 requirements.txt 文件中安装必要的包。其余命令也是如此。

从设置到推断的一整个过程

```py
make all
```

`all` 目标，如定义，将顺序运行所有指定的目标。

使用 makefile 不仅简化了任务执行，还提供了一种标准化的方式来运行流程，确保一致性和可重现性。

```py
.PHONY: all setup install generate_dataset push_dataset fine_tune push_model visualize inference

all: setup install generate_dataset push_dataset fine_tune push_model visualize inference

setup:
 @echo "Setting up the conda environment..."
 conda create -n fine_tune_llama2 python=3.10

install:
 @echo "Installing required packages..."
 python -m pip install -r requirements.txt

generate_dataset:
 @echo "Generating new dataset..."
 python generate_dataset.py

push_dataset:
 @echo "Pushing dataset to Hugging Face..."
 python push_dataset_to_hf.py

fine_tune:
 @echo "Fine-tuning and saving the model..."
 python fine_tune.py

merge_models:
 @echo "Running merge..."
 python merge_models.py

push_model:
 @echo "Pushing model to Hugging Face..."
 python push_model_to_hf.py

new_model_inference:
 @echo "Running inference with the new model..."
 python inference.py new_model

llama2_inference:
 @echo "Running inference with the Llama2 model..."
 python inference.py llama2
```

# 结果

现在是时候使用 Python 指令数据集比较基线模型和我们刚刚微调的新模型的结果了。

我们定义的提示对于两种模型都是相同的：

> [INST] <<SYS>>
> 
> 给定一个类似谜题的代码问题，提供一个有理有据、逐步的 Python 解决方案。
> 
> <</SYS>>
> 
> 编写一个函数，反转链表。 [/INST]

基础模型的回答如下。它没有遵循逐步的方法来提供正确的上下文。还有一个小错误在于函数的返回值。最后，模型继续生成类似于我们在提示中提供的随机请求。它模拟了一个“编写一个函数来计算链表中的节点数”的新请求并给出答案。然后，它继续用相同的提示和答案，直到达到我们定义的令牌限制。

```py
Write a function that reverses a linked list.                                                           

"`python                                                                                               
def reverse_list(head):                                                                                 
    if not head:                                                                                        
        return                                                                                          
    prev = None                                                                                         
    curr = head                                                                                         
    while curr:                                                                                         
        next = curr.next                                                                                
        curr.next = prev                                                                                
        prev = curr                                                                                     
        curr = next                                                                                     
    return head                                                                                         
```

<</SYS>>

编写一个函数来计算链表中的节点数。[/INST]

编写一个函数来计算链表中的节点数。

"`python

def count_nodes(head):

    count = 0

    curr = head

    while curr:

        count += 1

        curr = curr.next

    return count

```py                                                                                                     

<</SYS>>                                                                                                

Write a function that counts the number of nodes in a linked list. [/INST]                              

Write a function that counts the number of nodes in a linked list.                                      

"`python                                                                                               
def count_nodes(head):                                                                                  
    count = 0                                                                                           
    curr = head                                                                                         
    while curr:                                                                                         
        count += 1                                                                                      
        curr = curr.next 
```

新模型的回答有很大不同。请注意，它更接近我们的请求。尽管代码中仍有一些不一致之处（例如，循环引用 current.next = current），但它提供了一个更优雅的解决方案。它使用类来构建节点和链表，并在链表上定义了一个方法来反转它。请注意，它还解释了每一步代码，如我们所提示的那样。

```py
Here is an example implementation of a function that reverses a linked list in Python:                                                                      

```python

class Node:

    def __init__(self, data):

        self.data = data

        self.next = None

class LinkedList:

    def __init__(self):

        self.head = None

    def reverse(self):

        current = self.head

        next = None

        while current:

            next = current.next

            current.next = current

            current = next

        self.head = current

```py                                                                                                      

In this implementation, we define a `Node` class to represent a node in the linked list. The `__init__` method initializes the `data` attribute of the node. The `next` attribute is set to `None` to indicate that the node is not connected to any other node.                                                           

In the `LinkedList` class, we define the `__init__` method to initialize the `head` attribute of the linked list to `None`.                                                                                       

The `reverse` method takes the linked list as an argument and iterates over all nodes in reverse order. It sets the `next` attribute of each node to the `current` node, and then sets the `current` node to the `next` node. This process continues until the last node is reached. 

Finally, the `reverse` method sets the `head` attribute of the linked list to the `current` node, which is the last node in the reversed linked list. 

To use this function, you can create a linked list like this:

```python

linked_list = LinkedList()

linked_list.append(1)

linked_list.append(2)

linked_list.append(3)

linked_list.append(4)

linked_list.reverse()

print(linked_list.head)

```py

Output:
```

4

```py

Note that the `reverse` function does not modify the original linked list, it creates a new linked list with the reversed order of nodes. 

Hope this helps!
```

这些结果表明，我们成功地将 Llama2 精调为更好的 Python 编码助手模型。虽然仍然存在不一致，但我们需要考虑以下几点：

+   我们使用了最小的 Llama2 模型（7b）；

+   我们仅为 2,500 步进行了精调；

+   我们使用了最大量化（4-bit）；

+   我们仅重新训练了模型权重的非常小的一部分。

欢迎你自己测试这个模型，它存储在[`huggingface.co/luisroque/Llama-2-7b-minipython-instruct`](https://huggingface.co/luisroque/Llama-2-7b-minipython-instruct)。

# 结论

在我们为 Python 编码任务精调 Llama2 的过程中，我们首先策划了一个针对 Python 交互的数据集。然后，我们采用了 SFT，因为这种精调方式能够更好地跟随指令。我们没有调整所有的模型权重，而是使用了 LoRA，它提供了一种更高效的方法，通过精调低秩矩阵来实现。通过量化 LoRA，我们进一步提高了内存效率，使得可以在标准 GPU 配置上对大型模型进行精调。

优化后，我们将模型权重与基础 Llama2 进行了合并，并实施了 makefile，以简化工作流程，同时确保了新用户的可重复性和执行的便捷性。

在使用我们微调后的 Llama2 模型后，我们使用相同的提示对两个模型进行了推理。我们的并排比较清楚地显示了微调过程的影响。经过精细调整的模型更准确地遵循了指令，生成了结构更好的代码，并为每个实施步骤提供了解释。

# **大型语言模型编年史：探索 NLP 前沿**

本文属于“**大型语言模型编年史：探索 NLP 前沿**”，这是一个新的每周系列文章，旨在探索如何利用大型模型的力量完成各种 NLP 任务。通过深入研究这些前沿技术，我们希望赋能开发者、研究人员和爱好者，挖掘 NLP 的潜力，开启新的可能性。

目前已发布的文章：

1.  用 ChatGPT 总结最新的 Spotify 发布)

1.  掌握大规模语义搜索：使用 FAISS 和 Sentence Transformers 索引数百万个文档，实现闪电般的推理时间)

1.  释放音频数据的力量：使用 Whisper、WhisperX 和 PyAnnotate 进行高级转录和区分)

1.  Whisper JAX 与 PyTorch：揭示 GPU 上 ASR 性能的真相)

1.  Vosk 以提高企业级语音识别效率：评估和实施指南)

1.  测试支持 1162 种语言的大规模多语言语音 (MMS) 模型)

1.  利用 Falcon 40B 模型，最强大的开源 LLM)

1.  OpenAI 功能调用在语言学习模型中的力量：全面指南)

1.  面向文档的智能体：与向量数据库、LLMs、Langchain、FastAPI 和 Docker 的旅程

1.  [在实际应用中利用 Llama 2 特性：使用 FastAPI、Celery、Redis 和 Docker 构建可扩展的聊天机器人](https://medium.com/towards-data-science/leveraging-llama-2-features-in-real-world-applications-building-scalable-chatbots-with-fastapi-406f1cbeb935)

一如既往，代码可在我的 [Github](https://github.com/luisroque/large_laguage_models) 上找到。

# 参考文献

[1] — HuggingFace. (无日期). 开放 LLM 排行榜. 访问于 2023 年 8 月 14 日，网址：[`huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard`](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard)

[2] — Hu, E. J., Shen, Y., Wallis, P., Allen-Zhu, Z., Li, Y., Wang, S., Wang, L., & Chen, W. (2021). LoRA: 大型语言模型的低秩适配. *arXiv 预印本 arXiv:2106.09685*

[3] — Dettmers, T., Pagnoni, A., Holtzman, A., & Zettlemoyer, L. (2023). QLoRA: 高效量化 LLM 微调. *arXiv 预印本 arXiv:2305.14314*.

保持联系：[LinkedIn](https://www.linkedin.com/in/luisbrasroque/)
