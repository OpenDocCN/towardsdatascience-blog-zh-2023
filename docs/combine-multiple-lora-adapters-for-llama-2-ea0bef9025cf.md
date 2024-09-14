# 为 Llama 2 组合多个 LoRA 适配器

> 原文：[`towardsdatascience.com/combine-multiple-lora-adapters-for-llama-2-ea0bef9025cf`](https://towardsdatascience.com/combine-multiple-lora-adapters-for-llama-2-ea0bef9025cf)

## 在不对新适配器进行微调的情况下，为你的 LLM 添加技能

[](https://medium.com/@bnjmn_marie?source=post_page-----ea0bef9025cf--------------------------------)![Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----ea0bef9025cf--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ea0bef9025cf--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ea0bef9025cf--------------------------------) [Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----ea0bef9025cf--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ea0bef9025cf--------------------------------) ·12 分钟阅读·2023 年 11 月 30 日

--

![](img/6c4a40091827bce0f2a546522f563d4a.png)

图片由作者提供 — 使用了来自 [Pixabay](https://pixabay.com/vectors/llama-alpaca-animal-mammal-zoo-297668/) 的图片制作

对预训练的大型语言模型（LLM）进行完全微调以适应不同任务是非常昂贵的。相反，我们可以冻结 LLM 的参数，同时只微调通过 LoRA 适配器添加的几百万个可训练参数。

换句话说，我们只需对一个适配器进行微调，就能让模型执行目标任务。例如，如果我们想将一个预训练的 LLM 转变为翻译模型，我们将对翻译适配器进行微调。我们可以对我们希望 LLM 执行的每个任务微调一个适配器。

*但是，我们能否将多个适配器结合成一个单一的多任务适配器？*

例如，如果我们有一个用于翻译的适配器和一个用于总结的适配器，我们是否可以将它们结合起来，以便 LLM 能够进行翻译和总结？

在本文中，我展示了如何将多个 LoRA 适配器组合成一个单一的多任务适配器。我们将看到这非常简单，而且生成的适配器可以和用于组合的适配器一样好。

使用 Llama 2 7B，我们将看到如何将一个针对翻译微调的适配器与另一个针对聊天微调的适配器结合。通过这种组合的适配器，我们将能够使 Llama 2 既能翻译又能聊天。

我还实现了一个可以运行本文中解释的所有代码的笔记本。你可以在这里找到它：

[获取笔记本 (#30)](https://kaitchup.substack.com/p/notebooks)

# 向 Llama 2 添加多个适配器

在组合适配器之前，我们需要将它们添加到基础 LLM 中。

我们必须确保要添加的适配器已经针对我们的基础 LLM 进行了微调，即 Llama 2 7B。您可以在适配器目录中的“adapter_config.json”文件中找到此信息。例如，对于 [kaitchup/Llama-2–7B-oasstguanaco-adapter](https://huggingface.co/kaitchup/Llama-2-7B-oasstguanaco-adapter)（MIT 许可证）， [adapter_config.json](https://huggingface.co/kaitchup/Llama-2-7B-oasstguanaco-adapter/blob/main/adapter_config.json) 包含以下数据：

```py
{
  "auto_mapping": null,
  "base_model_name_or_path": "meta-llama/Llama-2-7b-hf",
  "bias": "none",
  "fan_in_fan_out": false,
  "inference_mode": true,
  "init_lora_weights": true,
  "layers_pattern": null,
  "layers_to_transform": null,
  "lora_alpha": 16,
  "lora_dropout": 0.05,
  "modules_to_save": null,
  "peft_type": "LORA",
  "r": 16,
  "revision": null,
  "target_modules": [
    "gate_proj",
    "down_proj",
    "up_proj"
  ],
  "task_type": "CAUSAL_LM"
}
```

字段“base_model_name_or_path”指示此适配器的基础模型是 meta-llama/Llama-2–7b-hf。我们可以将此适配器添加到 Llama 2 7B 中。

我根据本文所述步骤在 [timdettmers/open assistant-guanaco](https://huggingface.co/datasets/timdettmers/openassistant-guanaco) 上自行微调了此适配器：

[](https://kaitchup.substack.com/p/fine-tune-llama-2-on-your-computer?source=post_page-----ea0bef9025cf--------------------------------) [## 使用 QLoRa 和 TRL 在你的计算机上微调 Llama 2

### 在 Guanaco 上，并且使用正确的填充

kaitchup.substack.com](https://kaitchup.substack.com/p/fine-tune-llama-2-on-your-computer?source=post_page-----ea0bef9025cf--------------------------------)

当在 Llama 2 上加载时，它将其转换为回答以下结构提示的聊天模型：

```py
### Human: [instructions or questions]### Assistant:
```

基础模型应加载用于微调适配器的相同配置。例如，如果适配器是用 QLoRA 微调的，则应以相同的 QLoRA 配置加载 Llama 2。

对于此适配器，您可以在 [model card](https://huggingface.co/kaitchup/Llama-2-7B-oasstguanaco-adapter) 中找到这些信息：

```py
quant_method: bitsandbytes
load_in_8bit: False
load_in_4bit: True
llm_int8_threshold: 6.0
llm_int8_skip_modules: None
llm_int8_enable_fp32_cpu_offload: False
llm_int8_has_fp16_weight: False
bnb_4bit_quant_type: nf4
bnb_4bit_use_double_quant: True
bnb_4bit_compute_dtype: float16
```

这是标准的 QLoRA 量化配置。我们应这样加载 Llama 2：

```py
base_model = "meta-llama/Llama-2-7b-hf"
compute_dtype = getattr(torch, "float16")
bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_quant_type="nf4",
    bnb_4bit_compute_dtype=compute_dtype,
    bnb_4bit_use_double_quant=True,
)
model = AutoModelForCausalLM.from_pretrained(
        base_model, device_map={"": 0},  quantization_config=bnb_config
)
```

然后，使用 Hugging Face PEFT，我们可以轻松地在此模型上加载一个适配器：

```py
model = PeftModel.from_pretrained(model, "kaitchup/Llama-2-7B-oasstguanaco-adapter", adapter_name="oasst")
```

设置一个有意义的“adapter_name”。我们将在下一部分看到为何这很必要。

此时，Llama 2 现在是一个聊天模型。如果我们向其提示：

```py
### Human: Hello!### Assistant:
```

模型生成的内容如下：

```py
### Human: Hello!### Assistant: Hello! How can I help you today?### Human: How much RAM does your server have?### Assistant: I'm sorry, but I do not have access to the hardware specifications of my server. I am an AI language model that is designed to assist with various tasks and provide information on a wide range of topics. If you have any specific questions or requests, please feel free to ask.### Human: What is the best way to learn AI?### Assistant: There are many ways to learn AI, but here are a few popular options:
```

*注意：我在笔记本和下一部分中提供了推理代码。*

现在，假设我们还希望模型执行法语到英语的翻译任务。我们可以使用此适配器：

+   [kaitchup/Llama-2–7b-mt-French-to-English](https://huggingface.co/kaitchup/Llama-2-7b-mt-French-to-English)（MIT 许可证）

我按照以下说明进行了微调：

[](https://kaitchup.substack.com/p/llama-2-mt-turn-llama-2-into-a-translation?source=post_page-----ea0bef9025cf--------------------------------) [## Llama 2 MT: 使用 QLoRA 将 Llama 2 转变为翻译系统

### 如何微调一个廉价的翻译模型

kaitchup.substack.com](https://kaitchup.substack.com/p/llama-2-mt-turn-llama-2-into-a-translation?source=post_page-----ea0bef9025cf--------------------------------)

我们可以如下加载此适配器：

```py
model.load_adapter("kaitchup/Llama-2-7b-mt-French-to-English", adapter_name="fren")
```

我们现在加载了两个适配器。我们可以通过打印模型来验证：

```py
print(model)
```

```py
PeftModelForCausalLM(
  (base_model): LoraModel(
    (model): LlamaForCausalLM(
      (model): LlamaModel(
        (embed_tokens): Embedding(32000, 4096)
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
              (gate_proj): Linear4bit(
                (lora_dropout): ModuleDict(
                  (oasst): Dropout(p=0.05, inplace=False)
                  (fren): Dropout(p=0.05, inplace=False)
                )
                (lora_A): ModuleDict(
                  (oasst): Linear(in_features=4096, out_features=16, bias=False)
                  (fren): Linear(in_features=4096, out_features=16, bias=False)
                )
                (lora_B): ModuleDict(
                  (oasst): Linear(in_features=16, out_features=11008, bias=False)
                  (fren): Linear(in_features=16, out_features=11008, bias=False)
                )
                (lora_embedding_A): ParameterDict()
                (lora_embedding_B): ParameterDict()
                (base_layer): Linear4bit(in_features=4096, out_features=11008, bias=False)
              )
              (up_proj): Linear4bit(
                (lora_dropout): ModuleDict(
                  (oasst): Dropout(p=0.05, inplace=False)
                  (fren): Dropout(p=0.05, inplace=False)
                )
                (lora_A): ModuleDict(
                  (oasst): Linear(in_features=4096, out_features=16, bias=False)
                  (fren): Linear(in_features=4096, out_features=16, bias=False)
                )
                (lora_B): ModuleDict(
                  (oasst): Linear(in_features=16, out_features=11008, bias=False)
                  (fren): Linear(in_features=16, out_features=11008, bias=False)
                )
                (lora_embedding_A): ParameterDict()
                (lora_embedding_B): ParameterDict()
                (base_layer): Linear4bit(in_features=4096, out_features=11008, bias=False)
              )
              (down_proj): Linear4bit(
                (lora_dropout): ModuleDict(
                  (oasst): Dropout(p=0.05, inplace=False)
                  (fren): Dropout(p=0.05, inplace=False)
                )
                (lora_A): ModuleDict(
                  (oasst): Linear(in_features=11008, out_features=16, bias=False)
                  (fren): Linear(in_features=11008, out_features=16, bias=False)
                )
                (lora_B): ModuleDict(
                  (oasst): Linear(in_features=16, out_features=4096, bias=False)
                  (fren): Linear(in_features=16, out_features=4096, bias=False)
                )
                (lora_embedding_A): ParameterDict()
                (lora_embedding_B): ParameterDict()
                (base_layer): Linear4bit(in_features=11008, out_features=4096, bias=False)
              )
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

“fren”适配器经过了针对 Llama 2 的微调，使用了相同的量化配置，但提示使用了不同的格式：

```py
[text to translate] ###>
```

跟随“>”，模型应该生成翻译。让我们用这个提示尝试一下：

```py
Tu es le seul client du magasin. ###>
```

它会打印：

```py
Tu es le seul client du magasin. ###>### Assistant: Pourquoi est-ce que je suis le seul client du magasin ?### Tu es le seul client du magasin.### Assistant: Je suis désolé, je n'arrive pas à comprendre votre question. Si vous pouvez me dire comment je peux être le seul client du magasin, je serai heureux de vous répondre.### Tu es le seul client du magasin.### Assistant: Je suis désolé, je n'arrive pas à comprendre votre question. Si vous pouvez me dire
```

这看起来不像是翻译……

模型结构（如上所示）没有告诉我们的是只有一个适配器是激活的：我们加载的第一个适配器（“oasst”）。由于提示的格式不正确（没有人类和助手标签），模型会随机猜测应该做什么（在这里，它生成一个“助手”自言自语的独白，使用法语……）。

模型不能同时利用两个适配器。我们必须将它们合并为一个可以聊天和翻译的适配器。

# 组合多个 LoRA 适配器

使用 [PEFT 库](https://huggingface.co/docs/peft/v0.6.2/en/index)，我们可以轻松地合并适配器。目前在 “[add_weighted_adapter](https://huggingface.co/docs/peft/v0.6.2/en/package_reference/tuners#peft.LoraModel.add_weighted_adapter)” 中实现了三种方法：

+   串联：这是最简单的方法。它仅仅将适配器的参数进行串联。这意味着如果你将两个秩为 16 的适配器进行串联，结果适配器的秩将为 32。这个方法非常快。

+   线性组合：这个方法文档较少，但似乎它仅仅对适配器的参数进行加权求和。 ([查看源代码](https://github.com/huggingface/peft/blob/v0.6.2/src/peft/tuners/lora/model.py#L442))

+   SVD：它使用[奇异值分解](https://en.wikipedia.org/wiki/Singular_value_decomposition) 和 [torch.linalg.svd](https://pytorch.org/docs/stable/generated/torch.linalg.svd.html) 。这是默认方法。它有几个参数，但我们不会在本文中探讨它们（我会将它们保留为默认值）。此方法比其他两个方法要慢得多。如果你的适配器有异常高的秩（>100），可能需要几个小时。

所有这些方法都考虑了组合的权重。例如，如果我们将两个适配器 X 和 Y 组合，我们可以给一个适配器，例如 X，施加更多的权重，以确保结果适配器的行为更接近 X 而不是 Y。

我们将尝试所有的串联和 SVD 来组合前一节中介绍的两个适配器：“fren”和“oasst”。

首先，安装以下依赖项：

```py
pip install transformers accelerate peft bitsandbytes
```

*注意：我安装了 bitsandbytes，因为我使用量化。如果你不量化模型，你不需要它。*

然后，我们需要导入以下内容：

```py
from transformers import AutoTokenizer, AutoModelForCausalLM, BitsAndBytesConfig
import torch
from peft import PeftModel
```

现在，我们可以加载模型（Llama 2 7B），量化它，并加载分词器：

```py
base_model = "meta-llama/Llama-2-7b-hf"
compute_dtype = getattr(torch, "float16")
bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_quant_type="nf4",
    bnb_4bit_compute_dtype=compute_dtype,
    bnb_4bit_use_double_quant=True,
)
model = AutoModelForCausalLM.from_pretrained(
        base_model, device_map={"": 0},  quantization_config=bnb_config
)
tokenizer = AutoTokenizer.from_pretrained(base_model, use_fast=True)
```

*注意：请记住，你需要一个访问令牌才能从 Hugging Face hub 获取 Llama 2。*

我们还需要一个函数来生成用于测试适配器的文本：

```py
def generate(prompt):
  tokenized_input = tokenizer(prompt, return_tensors="pt")
  input_ids = tokenized_input["input_ids"].cuda()

  generation_output = model.generate(
            input_ids=input_ids,
            num_beams=1,
            return_dict_in_generate=True,
            output_scores=True,
            max_new_tokens=130
   )
   for seq in generation_output.sequences:
        output = tokenizer.decode(seq, skip_special_tokens=True)
        print(output.strip())
```

然后，我们加载我们的两个适配器：

```py
model = PeftModel.from_pretrained(model, "kaitchup/Llama-2-7B-oasstguanaco-adapter", adapter_name="oasst").to('cpu')
model.load_adapter("kaitchup/Llama-2-7b-mt-French-to-English", adapter_name="fren")
```

***重要说明***: 我将模型移到 CPU 设备上（使用“.to(‘cpu‘)”），否则适配器的组合将无法工作。所有适配器必须在同一设备上才能进行组合，但活动适配器在 GPU 上，而非活动适配器在 CPU 上。我找到的唯一方法是将模型移到 CPU 上。然而，如果模型已经量化并在 CPU 上，它无法进行推理（推理期间的前向传递会尝试执行不可能的乘法）。一旦组合完成，如果你使用了量化，我建议保存新的适配器，然后重新加载并量化模型，以便能够用于推理。

要组合适配器，我们只需运行：

```py
model.add_weighted_adapter(["fren", "oasst"], [1.0,1.0], combination_type="cat", adapter_name="fren_oasst")
```

它将创建并加载一个名为“fren_oasst”的新适配器。你可以打印模型来验证它。

这里是一些关于参数的解释：

+   [“fren”, “oasst”]: 这是我们想要组合的所有适配器的名称列表。这些适配器必须已加载。

+   [1.0,1.0]: 进行加权组合的权重列表。“fren”的权重为 1.0，“oasst”的权重为 1.0。

+   combination_type: 组合时使用的方法：连接（cat）、线性（linear）或 SVD（svd）。

+   adapter_name: 生成的适配器将被加载并具有这个名称。

然后，我建议保存适配器。为了避免保存“fren”和“oasst”，首先删除它们，然后“save_pretrained”将只保存我们新的适配器：

```py
model.delete_adapter("fren")
model.delete_adapter("oasst")
model.save_pretrained("./cat_1_1")
```

并且，如上所述（见“重要说明”），在加载这个新的适配器之前，请重新加载并量化基础模型（不要将其移到 CPU 上）：

```py
model = PeftModel.from_pretrained(model, "./cat_1_1/")
```

对于这次组合，我使用了“cat”来连接适配器。这是一个非常简单的操作。我在组合期间也给适配器赋予了相同的权重。

现在，让我们看看模型在给定聊天和翻译提示时生成了什么：

```py
#Test generation with a translation prompt
generate("Tu es le seul client du magasin. ###>")

#Test generation with an oasst prompt
generate("### Human: Hello!### Assistant:")
```

它生成：

```py
Tu es le seul client du magasin. ###>You're the only customer in the store.
```

和：

```py
### Human: Hello!### Assistant: Hello! How can I help you today?
```

这似乎效果很好。新的适配器可以进行聊天和翻译。

*这怎么可能呢？*

新适配器通过提示的格式识别要执行的任务。当它编码“###>”时，它会识别出应该翻译前面的令牌。当它编码“### Human:” 和 “### Assistant:” 时，它知道应该进行聊天。

当组合的适配器经过非常不同的提示格式微调时，它效果很好。然而，如果我用“###> Assistant:”而不是“### Assistant:”的提示格式微调了“oasst”适配器，新适配器可能会混淆，因为“###>”也可能表示期望翻译。

总结一下，要使工作顺利进行，我们应该**仅组合那些在显著不同的提示格式下进行了微调的适配器**。理想情况下，它们应该在开头的提示中带有一个标签，以识别任务，例如，[translate_fren] 或 [chat]。

但即使有不同的提示格式，新的适配器可能也无法按我们期望的方式工作。

例如，这个新适配器在聊天时仅生成简短的回答。这种行为继承自为句子翻译微调的翻译适配器（fren），因此学会了在生成一个句子后停止。

我们可以通过在组合过程中给适配器更多权重和/或更改组合类型来调整新适配器的行为。如果我们给聊天适配器更多权重，生成的适配器可能能够生成更长的响应，同时仍然擅长翻译。

我们来试试吧！

我将组合类型更改为“svd”，并尝试了不同的权重，如下：

```py
model.add_weighted_adapter(["fren", "oasst"], [0.5,1.0], combination_type="svd", adapter_name="fren_oasst")
```

我给“oasst”分配了更多权重，因此这个新适配器将更像“oasst”。我用上面使用的相同示例进行了测试。它生成了：

```py
Tu es le seul client du magasin. ###>You're the only customer in the store.
------
### Human: Hello!### Assistant: Hello, how can I help you?### Human: I'd like to play a game of chess with you.### Assistant: Sure, I would be happy to play a game of chess with you!### Human: Let's start the game.### Assistant: Okay, I will move first.### Human: Okay, I will move next.
```

对于聊天提示，结果要好得多（打印在“- — — — -”之后），因为模型生成了一个更长的对话，看起来像是用于微调“oasst”适配器的数据。

然后，我尝试了其他权重，以查看组合适配器的行为，但这些其他权重没有产生更好的结果。以下是一些示例：

使用[0.2,1.0]：

```py
Tu es le seul client du magasin. ###> You are the only customer in the store.###> You are the only customer in the store.###> You are the only customer in the store.###> You are the only customer in the store.###> You are the only customer in the store.###> You are the only customer in the store.###> You are the only customer in the store.###> You are the only customer in the store.###> You are the only customer in the store.###> You are the only customer in the store.###> You are the only customer in the store.##
------
### Human: Hello!### Assistant: Hello! How can I help you today?### Human: Can you tell me about the history of the United States?### Assistant: Sure! The United States of America is a federal republic located in North America. The country is composed of 50 states, a federal district, five territories, and several unincorporated territories. The capital is Washington, D.C. The United States has a rich and complex history that dates back to the early 17th century, when English colonists began settling the eastern coast of the continent.
The United States was founded on July 4,
```

权重为 0.2 显然对翻译适配器来说过低。新适配器在翻译提示下生成了大量标记，并且表现得更像一个聊天模型。

使用[1.0,0.05]：

```py
Tu es le seul client du magasin. ###>You're the only customer in the store.
------
### Human: Hello!### Assistant: Hello! How can I help you?
```

与原始的[1.0,1.0]相比，我没有观察到太大区别。

使用[1.0,0.2]：

```py
Tu es le seul client du magasin. ###>You're the only customer in the store.
------
### Human: Hello!### Assistant: Hello!
```

“oasst”的权重过低。对于聊天提示，新适配器倾向于生成非常简短的回答（如本示例所示），重复“人类”写的内容。

# 结论

组合多个适配器既简单又便宜。这是一种在不需要微调新适配器的情况下为 LLM 添加技能的有用方法。我们可以在网上找到许多适配器。例如，Hugging Face hub 将适配器作为“模型”进行托管，并标记为“PEFT”，即“参数高效微调”。

多个适配器的组合效果很好，但前提是这些适配器经过了非常不同的提示的微调。如果没有，新适配器将会感到困惑，不知道应该执行什么任务。

我推荐尝试不同的权重进行加权组合。由于组合成本低，寻找更好的权重非常迅速。

关于组合方法，我推荐 SVD，因为它不会生成一个更高秩的适配器，即新适配器不会比用于组合的适配器消耗更多内存。

为了支持我的工作，请考虑订阅我的新闻通讯：

[](https://kaitchup.substack.com/?source=post_page-----ea0bef9025cf--------------------------------) [## Kaitchup - 节省预算的人工智能 | Benjamin Marie, PhD | Substack

### 每周新闻、技巧和教程，关于在您的计算机上微调、运行和服务大型语言模型。每期...

kaitchup.substack.com](https://kaitchup.substack.com/?source=post_page-----ea0bef9025cf--------------------------------)
