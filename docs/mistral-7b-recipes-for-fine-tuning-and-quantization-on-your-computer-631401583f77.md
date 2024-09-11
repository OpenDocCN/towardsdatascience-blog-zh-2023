# Mistral 7B：在您的计算机上进行微调和量化的配方

> 原文：[`towardsdatascience.com/mistral-7b-recipes-for-fine-tuning-and-quantization-on-your-computer-631401583f77?source=collection_archive---------0-----------------------#2023-10-26`](https://towardsdatascience.com/mistral-7b-recipes-for-fine-tuning-and-quantization-on-your-computer-631401583f77?source=collection_archive---------0-----------------------#2023-10-26)

## 便宜的监督微调与令人印象深刻的 LLM

[](https://medium.com/@bnjmn_marie?source=post_page-----631401583f77--------------------------------)![Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----631401583f77--------------------------------)[](https://towardsdatascience.com/?source=post_page-----631401583f77--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----631401583f77--------------------------------) [Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----631401583f77--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fad2a414578b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmistral-7b-recipes-for-fine-tuning-and-quantization-on-your-computer-631401583f77&user=Benjamin+Marie&userId=ad2a414578b3&source=post_page-ad2a414578b3----631401583f77---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----631401583f77--------------------------------) · 9 分钟阅读 · 2023 年 10 月 26 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F631401583f77&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmistral-7b-recipes-for-fine-tuning-and-quantization-on-your-computer-631401583f77&user=Benjamin+Marie&userId=ad2a414578b3&source=-----631401583f77---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F631401583f77&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmistral-7b-recipes-for-fine-tuning-and-quantization-on-your-computer-631401583f77&source=-----631401583f77---------------------bookmark_footer-----------)![](img/bfa5a4ac457b91e1272b5d1cd9f44188.png)

mistral 是一种吹拂在北部地中海的风 —— 图片来自 [Pixabay](https://pixabay.com/illustrations/wind-girl-tree-long-hair-4054954/)

Mistral 7B 是由 Mistral AI 创建的非常受欢迎的大型语言模型（LLM）。它超越了所有 [其他相似规模的预训练 LLM](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard)，甚至优于更大的 LLM，如 Llama 2 13B。

它也非常优化了快速解码，特别是对于长上下文，这要归功于使用滑动窗口计算注意力和分组查询注意力（GQA）。你可以在[介绍 Mistral 7B 的 arXiv 论文](https://arxiv.org/abs/2310.06825)和[Salvatore Raieli](https://medium.com/u/f1a08d9452cd?source=post_page-----631401583f77--------------------------------)的[这篇精彩文章](https://medium.com/gitconnected/mistral-7b-a-new-wind-blowing-other-language-models-b74d7bfe137e)中找到更多细节。

Mistral 7B 表现良好，但足够小，可以在经济实惠的硬件上利用。

在本文中，我将展示如何使用 QLoRA 微调 Mistral 7B。我们将使用我为本文修改的“ultrachat”数据集。Ultrachat 被 Hugging Face 用来创建[Zephyr 7B](https://huggingface.co/HuggingFaceH4/zephyr-7b-alpha)。我们还将看看如何使用 AutoGPTQ 对 Mistral7B 进行量化。

我编写了实现所有部分的笔记本。你可以在这里找到它们：

[获取笔记本（#22，#23）](https://kaitchup.substack.com/p/notebooks)

# 使用 TRL 对 Mistral 7B 进行监督微调

Mistral 7B 是一个拥有 70 亿参数的模型。你大约需要 15 GB 的 VRAM 来加载它到 GPU 上。然后，使用批量进行全面微调将消耗更多的 VRAM。

标准全面微调的替代方案是使用 QLoRA 进行微调。QLoRA 在冻结的量化模型上微调 LoRA 适配器。在之前的文章中，[我已经使用它来微调 Llama 2 7B](https://kaitchup.substack.com/p/fine-tune-llama-2-on-your-computer)。

由于 QLoRA 将量化到 4 位（NF4），我们大约将加载模型的内存消耗减少了 4 倍，即，使用 NF4 量化的 Mistral 7B 消耗约 4 GB 的 VRAM。如果你有一块 12 GB VRAM 的 GPU，这会留出大量空间来增加批量大小并使用 LoRA 针对更多模块。

换句话说，如果你的机器有足够的 VRAM，或者使用配备 T4 GPU 的 Google Colab 免费实例，你可以免费微调 Mistral 7B。

要使用 QLoRA 进行微调，你需要安装以下库：

```py
pip install -q -U bitsandbytes
pip install -q -U transformers
pip install -q -U peft
pip install -q -U accelerate
pip install -q -U datasets
pip install -q -U trl
```

然后，导入以下内容：

```py
import torch
from datasets import load_dataset
from peft import LoraConfig, PeftModel, prepare_model_for_kbit_training
from transformers import (
    AutoModelForCausalLM,
    AutoTokenizer,
    BitsAndBytesConfig,
    AutoTokenizer,
    TrainingArguments,
)
from trl import SFTTrainer
```

接下来，我们加载并配置标记器。与 Llama 2 一样，我们需要定义一个填充标记。像往常一样，我选择了 UNK 标记来填充训练示例。

*注意：Mistral 7B 完全开放。你无需连接到 Hugging Face 或在下载模型和标记器之前签署许可协议。*

```py
model_name = "mistralai/Mistral-7B-v0.1"
#Tokenizer
tokenizer = AutoTokenizer.from_pretrained(model_name, use_fast=True)
tokenizer.pad_token = tokenizer.unk_token
tokenizer.pad_token_id =  tokenizer.unk_token_id
tokenizer.padding_side = 'left'
```

对于微调，我制作了一个自定义版本的[ultrachat](https://huggingface.co/datasets/stingning/ultrachat)（MIT 许可证）。Ultrachat 包含 774k 个 JSON 格式的对话。这对便宜的微调来说太多了，而且该格式对于在消费级硬件上使用 TRL 微调并不理想。*注意：* [*TRL*](https://huggingface.co/docs/trl/index) *是 Hugging Face 开发的一个库，它简化了对指令 LLMs 的微调。*

我随机从 ultrachat 中抽取了 100k 对话，然后将对话展开。例如，在原始数据集中，一个示例如下：

```py
[ "What percentage of the Earth's surface is covered by oceans?", "About 71% of the Earth's surface is covered by oceans.", "Wow, that's a lot of water! No wonder we call it the blue planet.", "Yes, it certainly is! The oceans play a vital role in regulating the Earth's climate and supporting life on our planet. And they're also a great source of food, energy, and recreation for us humans!", "Absolutely! I love visiting the beach and going for a swim in the ocean. It's amazing how vast and powerful the sea can be, yet also so peaceful and calming.", "As an AI language model, I have never gone to the beach or swam in the ocean, but based on what you said, I am sure it's a wonderful experience. The ocean is a place of great beauty and mystery, full of fascinating creatures and hidden treasures waiting to be discovered. It can be both a source of wonder and a reminder of the awesome power of nature." ]
```

一旦展开，我们将所有内容放入一个单一的 token 序列中，并标记为“### Human”和“### Assistant”：

```py
### Human: What percentage of the Earth's surface is covered by oceans?### Assistant: About 71% of the Earth's surface is covered by oceans###Human: Wow, that's a lot of water! No wonder we call it the blue planet### Assistant: Yes, it certainly is! The oceans play a vital role in regulating the Earth's climate and supporting life on our planet. And they're also a great source of food, energy, and recreation for us humans!### Human: Absolutely! I love visiting the beach and going for a swim in the ocean. It's amazing how vast and powerful the sea can be, yet also so peaceful and calming### Assistant: As an AI language model, I have never gone to the beach or swam in the ocean, but based on what you said, I am sure it's a wonderful experience. The ocean is a place of great beauty and mystery, full of fascinating creatures and hidden treasures waiting to be discovered. It can be both a source of wonder and a reminder of the awesome power of nature
```

这与 [timdettmers/openassistant-guanaco](https://huggingface.co/datasets/timdettmers/openassistant-guanaco) 的格式相同。这个格式可以直接被 TRL 使用。你可以在这里找到这个版本的 ultrachat：

+   [kaitchup/ultrachat-100k-flattened](https://huggingface.co/datasets/kaitchup/ultrachat-100k-flattened)

要加载数据集，我们执行：

```py
dataset = load_dataset("kaitchup/ultrachat-100k-flattened")
```

现在，让我们加载模型并为 QLoRA 做准备：

```py
#Quantization configuration
compute_dtype = getattr(torch, "float16")
bnb_config = BitsAndBytesConfig(
        load_in_4bit=True,
        bnb_4bit_quant_type="nf4",
        bnb_4bit_compute_dtype=compute_dtype,
        bnb_4bit_use_double_quant=True,
)
#Load the model and quantize it on the fly
model = AutoModelForCausalLM.from_pretrained(
          model_name, quantization_config=bnb_config, device_map={"": 0}
)

#Cast some modules of the model to fp32 
model = prepare_model_for_kbit_training(model)

#Configure the pad token in the model
model.config.pad_token_id = tokenizer.pad_token_id
model.config.use_cache = False # Gradient checkpointing is used by default but not compatible with caching
```

如果你的 GPU 支持，请将“float16”替换为“bfloat16”。这将使训练更加稳定。

接下来，我们定义 LoRA 的配置。

```py
peft_config = LoraConfig(
        lora_alpha=16,
        lora_dropout=0.05,
        r=16,
        bias="none",
        task_type="CAUSAL_LM",
        target_modules= ['k_proj', 'q_proj', 'v_proj', 'o_proj', "gate_proj", "down_proj", "up_proj"]
)
```

我通常将 r=lora_alpha 设为 16，因为我阅读了 Platypus 论文。

[](https://kaitchup.substack.com/p/platypus-dataset-curation-and-adapters?source=post_page-----631401583f77--------------------------------) [## Platypus: 数据集策划与适配器，用于更好的大型语言模型]

### 在你的目标任务上实现低成本的最先进技术

kaitchup.substack.com](https://kaitchup.substack.com/p/platypus-dataset-curation-and-adapters?source=post_page-----631401583f77--------------------------------)

我瞄准了几个模块。这里，我选择了所有“proj”模块，但还有更多。你可以通过运行“print(model)”找到它们。请注意，添加更多模块可能会提升性能，但也会增加可训练参数，从而消耗更多 VRAM。

对于训练，我选择了以下超参数：

```py
training_arguments = TrainingArguments(
        output_dir="./results",
        #evaluation_strategy="steps",
        #do_eval=True,
        optim="paged_adamw_8bit",
        per_device_train_batch_size=4,
        gradient_accumulation_steps=4,
        per_device_eval_batch_size=4,
        log_level="debug",
        save_steps=20,
        logging_steps=10,
        learning_rate=4e-4,
        #eval_steps=200,
        #num_train_epochs=1,
        max_steps=100,
        warmup_steps=100,
        lr_scheduler_type="linear",
)
```

我仅训练了 100 步。由于样本非常长，使用 T4 GPU 训练一个 epoch 需要超过 200 小时。如果你使用 V100 或 RTX 40xx，你可以将时间减少到 100 小时。

我推荐至少进行 2,000 次训练步骤，以获得一个相当好的模型。使用 T4 训练大约需要两天时间，使用更新的 GPU 只需一天。

我还用“#”注释了与评估相关的超参数，因为评估成本较高。取消注释以记录验证损失。

开始训练：

```py
trainer = SFTTrainer(
        model=model,
        train_dataset=dataset['train'],
        #eval_dataset=dataset['test'],
        peft_config=peft_config,
        dataset_text_field="text",
        max_seq_length=512,
        tokenizer=tokenizer,
        args=training_arguments,
)

trainer.train()
```

然后，一旦训练完成，通过运行以下命令测试训练后的适配器：

```py
from transformers import GenerationConfig
model.config.use_cache = True
model = PeftModel.from_pretrained(model, "./results/checkpoint-100/")
def generate(instruction):
    prompt = "### Human: "+instruction+"### Assistant: "
    inputs = tokenizer(prompt, return_tensors="pt")
    input_ids = inputs["input_ids"].cuda()
    generation_output = model.generate(
            input_ids=input_ids,
            generation_config=GenerationConfig(pad_token_id=tokenizer.pad_token_id, temperature=1.0, top_p=1.0, top_k=50, num_beams=1),
            return_dict_in_generate=True,
            output_scores=True,
            max_new_tokens=256
    )
    for seq in generation_output.sequences:
        output = tokenizer.decode(seq)
        print(output.split("### Assistant: ")[1].strip())
generate("Tell me about gravitation.")
```

所有这些代码都实现于 [这个 notebook #22](https://kaitchup.substack.com/p/notebooks)。

# 使用 AutoGPTQ 和 bitsandbytes NF4 对 Mistral 7B 进行量化

## 使用 bitsandbytes NF4

为了保持内存消耗低，我们希望运行量化模型。

如果你已经用 QLoRA 微调了自己的模型并希望对其进行量化，你最好的选择是加载并量化 Mistral 7B，使用 bitsandbytes nf4，像我们对 QLoRA 所做的那样。然后，将微调的适配器加载到其上。这是我们在上一节的最后一个代码示例中所做的：

```py
model = PeftModel.from_pretrained(model, "./results/checkpoint-100/")
```

我探索了几个其他选项，例如将适配器合并到基础模型中，但没有一个是最优的。

[](https://kaitchup.substack.com/p/lora-adapters-when-a-naive-merge?source=post_page-----631401583f77--------------------------------) [## LoRA Adapters: When a Naive Merge Leads to Poor Performance

### LoRA 适配器微调的情况，使用 QLoRA

kaitchup.substack.com](https://kaitchup.substack.com/p/lora-adapters-when-a-naive-merge?source=post_page-----631401583f77--------------------------------)

我们本可以使用[QA-LoRA 对量化感知的 LoRA 进行微调](https://kaitchup.substack.com/p/qa-lora-quantization-aware-fine-tuning)，但该框架目前尚不支持 Mistral 7B。

Mistral AI 还发布了 Mistral 7B 的指令版。你可以在这里获取，而不是训练自己的指令 Mistral 7B：

+   [mistralai/Mistral-7B-Instruct-v0.1](https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.1)

我使用这个模型进行以下量化示例。

要用 bitsandbytes 加载并量化模型，你首先需要安装以下库：

```py
pip install -q -U bitsandbytes
pip install -q -U transformers
pip install -q -U accelerate
```

然后，运行：

```py
import torch
from transformers import (
    AutoModelForCausalLM,
    AutoTokenizer,
    BitsAndBytesConfig,
    AutoTokenizer,
)

model_name = "mistralai/Mistral-7B-Instruct-v0.1"
#Tokenizer
tokenizer = AutoTokenizer.from_pretrained(model_name, use_fast=True)
tokenizer.pad_token = tokenizer.unk_token
tokenizer.pad_token_id =  tokenizer.unk_token_id
tokenizer.padding_side = 'left'

#Quantization configuration
compute_dtype = getattr(torch, "float16")
bnb_config = BitsAndBytesConfig(
        load_in_4bit=True,
        bnb_4bit_quant_type="nf4",
        bnb_4bit_compute_dtype=compute_dtype,
        bnb_4bit_use_double_quant=True,
)
#Load the model and quantize it on the fly
model = AutoModelForCausalLM.from_pretrained(
          model_name, quantization_config=bnb_config, device_map={"": 0}
)
```

对于生成测试，你可以使用我在前面部分定义的`generate`函数，配合 Mistral Instruct 使用的正确提示格式：

```py
from transformers import GenerationConfig

def generate(instruction):
    prompt = "[INST] "+instruction+" [/INST]\n"
    inputs = tokenizer(prompt, return_tensors="pt")
    input_ids = inputs["input_ids"].cuda()
    generation_output = model.generate(
            input_ids=input_ids,
            generation_config=GenerationConfig(pad_token_id=tokenizer.pad_token_id, temperature=1.0, top_p=1.0, top_k=50, num_beams=1),
            return_dict_in_generate=True,
            output_scores=True,
            max_new_tokens=256
    )
    for seq in generation_output.sequences:
        output = tokenizer.decode(seq)
        print(output.split("[/INST]")[1].strip())
generate("Tell me about gravity.")
```

## 使用 AutoGPTQ

bitsandbytes nf4 量化速度快，但推理速度慢。目前，我们无法序列化 nf4 模型。

AutoGPTQ 的量化速度较慢，但你只需做一次。它也比[nf4 解码速度快](https://kaitchup.substack.com/p/gptq-or-bitsandbytes-which-quantization)，但请注意，用[AutoGPTQ (INT4) 量化的模型略逊色于用 nf4 量化的模型](https://kaitchup.substack.com/p/quantize-and-fine-tune-llms-with)。

我们需要以下库：

```py
pip install -q -U bitsandbytes
pip install -q -U transformers
pip install -q -U accelerate
pip install -q -U optimum
pip install -q -U auto-gptq
```

然后，导入以下包：

```py
import torch
from transformers import (
    AutoModelForCausalLM,
    AutoTokenizer,
    GPTQConfig,
    AutoTokenizer
)
```

然后，当我们用“AutoModelForCausalLM.from_pretrained”加载模型时，它会被量化。

```py
model_name = "mistralai/Mistral-7B-Instruct-v0.1"

tokenizer = AutoTokenizer.from_pretrained(model_name, use_fast=True)
tokenizer.pad_token = tokenizer.unk_token
tokenizer.pad_token_id =  tokenizer.unk_token_id
tokenizer.padding_side = 'left'

quantization_config = GPTQConfig(bits=4, dataset = "c4", tokenizer=tokenizer)

model = AutoModelForCausalLM.from_pretrained(model_name, device_map="auto", quantization_config=quantization_config)
```

量化非常消耗内存。你需要超过 24 GB 的 VRAM，例如，Google Colab Pro 的 A100 会有效。我建议在加载模型后保存它，以确保你不会再需要重复操作。

所有这些量化代码也可以在我的[笔记本 #23](https://kaitchup.substack.com/p/notebooks)中找到。

请注意，TheBloke 在 Hugging Face Hub 上提议了使用 AutoGPTQ 量化的 Mistral Instruct 7B：

+   [TheBloke/Mistral-7B-Instruct-v0.1-GPTQ](https://huggingface.co/TheBloke/Mistral-7B-Instruct-v0.1-GPTQ)

# 结论

Mistral 7B 是一个令人印象深刻的预训练 LLM。你可以很容易地在电脑上用 QLoRA 进行微调。然而，微调依然非常耗时。正如我们所见，你可能需要运行几百小时或更长时间。

如果你不需要在你的数据上微调 Mistral 7B，Mistral AI 提出的 Mistral Instruct 是一个不错的替代方案。要在消费级硬件上运行它，你可以使用 bitsandbytes nf4 或 GPTQ 进行量化。

支持我的工作，请考虑订阅我的新闻通讯：

[](https://kaitchup.substack.com/?source=post_page-----631401583f77--------------------------------) [## Kaitchup - AI on a Budget | Benjamin Marie, PhD | Substack

### 每周提供关于在你的计算机上微调、运行和服务大型语言模型的新闻、技巧和教程。每一个……

[kaitchup.substack.com](https://kaitchup.substack.com/?source=post_page-----631401583f77--------------------------------)
