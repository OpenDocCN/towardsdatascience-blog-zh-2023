# 大型语言模型加载简介

> 原文：[`towardsdatascience.com/an-introduction-to-loading-large-language-models-e7488a7352ed`](https://towardsdatascience.com/an-introduction-to-loading-large-language-models-e7488a7352ed)

## 掌握巨型模型：Llama2 和 HuggingFace 大型语言模型加载简介

[](https://medium.com/@angelamarieteng?source=post_page-----e7488a7352ed--------------------------------)![Amber Teng](https://medium.com/@angelamarieteng?source=post_page-----e7488a7352ed--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e7488a7352ed--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e7488a7352ed--------------------------------) [Amber Teng](https://medium.com/@angelamarieteng?source=post_page-----e7488a7352ed--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e7488a7352ed--------------------------------) ·15 分钟阅读·2023 年 10 月 12 日

--

![](img/ab1b2fdbc7396ec29bedfbd8c64ed35f.png)

图片由 [Possessed Photography](https://unsplash.com/@possessedphotography?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，发布在 [Unsplash](https://unsplash.com/photos/jIBMSMs4_kA?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 上

在人工智能巨头的时代，模型通过数 TB 的数据和数十亿的参数训练而成的领域，自然语言处理变得更加易于接触——不仅对工程师、数据科学家和机器学习研究人员，对爱好者、商人和学生也是如此。我们正站在技术革命的十字路口——由庞大的语言模型驱动。

这场革命影响的不仅仅是我们中的一小部分人，*而是所有人*。因此，深入了解这些大型语言模型（LLMs）的定义和能力，以及如何使用这些 LLMs，变得越来越重要。*那么，为什么工程师理解如何加载这些 LLMs 是至关重要的呢？*

这些新的 LLMs 几乎渗透到当今技术领域的每一个角落——数据科学家和自然语言处理（NLP）工程师越来越需要将基于 LLM 的解决方案整合到他们的产品和系统中，无论是在学术界还是工业界。显而易见，了解 LLMs 的基本知识对于做出有关使用哪种模型、何时使用某些模型以及这些模型对特定项目或应用程序的好处等明智决策至关重要。如果没有对 LLMs 的这种基础理解，工程师可能会错过利用最先进（SOTA）LLM 能力构建产品的有影响力的机会。

利用和理解这些 LLMs 的第一步是加载模型。实际来说，为了有效地使用 LLMs，工程师必须首先了解如何加载它们。为什么加载 LLMs 会有挑战？

# 加载 LLMs 的挑战

由于其庞大的规模以及潜在的硬件要求和软件配置，加载 LLMs 尤其具有挑战性。许多 NLP 工程师在 LLMs 的加载步骤上“不出意外地陷入困境”，这可能会阻碍他们尝试这些模型并真正发挥其能力。能够掌握加载 LLMs 艺术的工程师无疑在利用这些 LLMs 时获得了竞争优势。

这些模型中庞大的参数数量意味着无与伦比的表达能力，使得这些 LLMs 能够有效地捕捉复杂的语言模式和上下文。像 Llama2 和 GPT 这样的 LLMs 的大规模也允许在专业任务和行业上进行微调——这些将在本文稍后更详细地讨论，并将在我未来的文章中继续探讨。毫不意外，这些 LLMs 的庞大参数数量带来了优越的性能，使这些模型在各种语言相关应用中表现出色——从文本生成、问答、摘要、翻译、零样本学习及其他。

在本文中，我们将探讨如何在不同配置下以及使用不同大小的模型来加载今天最受欢迎的模型之一——Meta 的 Llama2。无论你是 NLP 工程师、研究人员，还是对探索人工智能的未来（和现在）充满热情的爱好者，本文将指导你了解加载 LLMs 的基础知识。

![](img/29a535e5c06be6b564ccfb3b42238ab5.png)

图片由 [Paz Arando](https://unsplash.com/@pazarando?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，来源于 [Unsplash](https://unsplash.com/photos/ZHmvek0kJW8?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

# 什么是 Llama2？

由 Meta 开发，作为 Llama1 的改进版，并于 2023 年 7 月发布，[Llama2](https://ai.meta.com/llama/#inside-the-model)是一套[“预训练和微调的大型语言模型（LLM），规模从 70 亿到 700 亿参数不等”](https://scontent.fmnl25-1.fna.fbcdn.net/v/t39.2365-6/10000000_662098952474184_2584067087619170692_n.pdf?_nc_cat=105&ccb=1-7&_nc_sid=3c67a6&_nc_ohc=TjG8FsFfH8cAX_uux2_&_nc_ht=scontent.fmnl25-1.fna&oh=00_AfBhA6S3oJWa52QjMx99BmgN3gg06l2jsH2-P2MNIxSmAw&oe=6529307F)。今年，它因其与其他 SOTA 模型（包括开源和闭源模型）相比的卓越表现以及惊人的规模——从 27 亿参数到 700 亿参数不等——而备受关注。Llama2 已经显示出以非凡的熟练度理解、生成和操控人类语言的能力。其定制化能力也激励了工程师将其适应于广泛的应用领域。特别是，其在微调方面的灵活性使 Llama2 能够针对特定任务、行业和数据集进行调整——赋予工程师在行业和学术界探索其应用的能力。Llama2 还配有 Llama2-Chat，它经过优化用于对话用例，我们将在本文后续部分探讨。根据 Llama2 论文，Llama2-Chat 利用了对齐技术的迭代应用，如指令调优和来自人类反馈的强化学习。有关模型架构和微调过程的更多细节，以及有关安全性的精彩讨论，请参见 Meta 的官方论文[这里](https://scontent.fmnl25-1.fna.fbcdn.net/v/t39.2365-6/10000000_662098952474184_2584067087619170692_n.pdf?_nc_cat=105&ccb=1-7&_nc_sid=3c67a6&_nc_ohc=TjG8FsFfH8cAX_uux2_&_nc_ht=scontent.fmnl25-1.fna&oh=00_AfBhA6S3oJWa52QjMx99BmgN3gg06l2jsH2-P2MNIxSmAw&oe=6529307F)。

# 使用 HuggingFace 库

在这个项目中，我们将使用 HuggingFace 库来加载 Llama2。[HuggingFace](https://huggingface.co/)是一个开源数据科学和机器学习（ML）平台，致力于使 AI 领域民主化——这是工程师可以找到模型、数据集和应用程序（在[HuggingFace Spaces](https://huggingface.co/spaces)上）的地方。

具体而言，我们将使用 HuggingFace (HF) Transformers 库，该库包含 [不同尺寸的 Meta Llama2 模型](https://huggingface.co/models?library=transformers&sort=trending&search=meta)。HF 的 Transformers 库最初作为一个开源项目构建，现在与 PyTorch 和 TensorFlow 都能很好地兼容。在我的 NLP 旅程中，我从使用 HF 的 Transformers 库中受益匪浅，因为它使得访问最先进的 NLP 模型，包括 Llama2，变得更加容易。通过 HF 的 API（这是我在这个项目中使用的），我们将看到如何开始加载 LLM 和其他预训练模型。在未来的文章中，我还将分享如何使用 HF 的 API 来微调和部署这些 LLM。

![](img/7172d6a6dfebca40f6fb00b437aa544b.png)

[凯特·列维茨卡娅](https://unsplash.com/@katereyy?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)拍摄于 [Unsplash](https://unsplash.com/photos/CtFF1YUD8vE?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

# 开始使用

在本文中，我们将探索如何加载不同尺寸的 LLM（特别是 Llama2），这对于不同的计算设置将很有帮助。*本文作为 Suhas Pai 的书籍* [*Designing Large Language Model Applications*](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fwww.oreilly.com%2Flibrary%2Fview%2Fdesigning-large-language%2F9781098150495%2F)* 的附属和扩展。*

对于这个项目，我最初在 Google Colab 上使用了 Colab 免费版进行代码测试，尽管在加载大型模型和进行推理时遇到了许多问题。因此，对于一些较大的模型（例如 130 亿参数的 Llama2 Hugging Face 模型），假设使用的是 Google Colab Pro 的基本订阅（截至撰写时）。

# 资源分配

要开始使用，我在我的 Colab 笔记本上进行了以下资源分配——完整代码可以在此查看。

+   Python 3 Google Compute Engine 后端（GPU）

+   系统 RAM：51.0 GB

+   GPU RAM: 40.0 GB

+   磁盘空间：166.8 GB

+   托管运行时类型：A100

请注意，为了提高效率并考虑到 GPU RAM 限制和磁盘空间限制，我们在每次模型运行后清空了 GPU 内存，同时也清理了磁盘空间。

![](img/4202ce23b001c3830a1af17c7b5faf11.png)

Google Colab 资源。*图片来自作者。*

# HuggingFace 登录

Llama2 是一个受限模型，这意味着我们需要在 [Meta 网站](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fai.meta.com%2Fresources%2Fmodels-and-libraries%2Fllama-downloads%2F) 上注册一个许可证才能使用它。这通常需要一两天时间，并且您的注册密钥将与您的 HuggingFace 令牌关联。要创建和使用自己的 HuggingFace 令牌，请参阅 [此](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fhuggingface.co%2Fdocs%2Fhub%2Fsecurity-tokens) HuggingFace 官方网站上的指南。

由于我们使用的是 Google Colab，我们可以使用以下代码登录。

```py
from huggingface_hub import notebook_login
notebook_login()
```

![](img/0438ead04de6a2308a6f3a1c6e07ee68.png)

作者提供的图片

# 加载库

接下来，我们将加载所需的库，以开始加载 2.7 亿参数的 Llama2 聊天模型、2.7 亿参数的 Llama2 模型和 130 亿参数的 Llama2 模型。在未来的文章中，我们还将展示如何使用 AWS 加载 700 亿参数的 Llama2 模型，请继续关注。

```py
from transformers import AutoTokenizer, pipeline, AutoModelForCausalLM, BitsAndBytesConfig
from torch import cuda, bfloat16
```

我们将使用`bitsandbytes`进行分片。有关模型量化的更多信息，请参见 HuggingFace 文档[这里](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fhuggingface.co%2Fdocs%2Ftransformers%2Fmain_classes%2Fquantization)。

# 通过 HF 加载 Llama2 2.7 亿参数聊天模型

接下来，我们将从最小的 Llama2 模型开始。我们可以根据 HF Transformers 库和相关的[模型卡](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fhuggingface.co%2Fmeta-llama%2FLlama-2-7b-chat-hf)来指定模型名称。然后，我们将指定分词器——但对于这个演示，我们可以直接使用`AutoTokenizer`和相关的 Llama2 预训练分词器。然后，我们可以使用`transformers.pipeline`函数来指定我们的任务、模型和 PyTorch 数据类型，再设置模型特定的参数，如`top_k`、`do_sample`（告知模型从 logits 中采样，而不是使用贪婪解码）、`max_new_tokens`（指定返回的最大 token 数量）。请注意，Llama2 聊天模型更适合对话。

```py
model = "meta-llama/Llama-2-7b-chat-hf"

tokenizer = AutoTokenizer.from_pretrained(model)
pipeline = transformers.pipeline(
    "text-generation",
    model=model,
    torch_dtype=torch.float16,
    device_map="auto",
)
```

对于这个项目，我们将询问不同变体的“*概述加载大型语言模型的步骤*”这个问题。

```py
sequences = pipeline(
    'Outline the steps to load a large language model using Python.\n',
    do_sample=True,
    top_k=10,
    num_return_sequences=1,
    eos_token_id=tokenizer.eos_token_id,
    # max_length=200,
    max_new_tokens = 1000
)
```

然后，我们将输出生成的文本。

```py
for seq in sequences:
    print(f"Result: {seq['generated_text']}")
```

结果如下：

> 结果：概述使用 Python 加载大型语言模型的步骤。
> 
> 介绍：由于大型语言模型在各种自然语言处理（NLP）任务中的出色表现，近年来受到了广泛关注。这些模型需要大量的计算资源和内存，使得将它们加载到本地机器上变得具有挑战性。本文将概述使用 Python 加载大型语言模型的步骤。
> 
> 步骤 1：选择语言模型 第一步是选择一个你想加载的大型语言模型。当前有几个流行的模型，包括 BERT、RoBERTa 和 XLNet。你可以选择这些模型中的一个，或尝试不同的变体。
> 
> 步骤 2：下载模型 一旦选择了语言模型，你需要下载它。你可以从模型的官方网站或信誉良好的数据仓库下载预训练模型。确保下载正确版本的模型，这可能会根据你正在处理的任务而有所不同。
> 
> 步骤 3：准备模型 下载模型后，你需要准备模型以供使用。这涉及将模型加载到内存中，并按照所需的格式进行格式化。你可以使用 Python 中的 `torch` 库来加载模型并执行其他自然语言处理任务。
> 
> 步骤 4：加载模型 要将模型加载到内存中，你可以使用 `torch.load()` 函数。这个函数接受模型文件的路径并将其加载到内存中。以下是一个示例：```py import torch model = torch.load(“path/to/model.pth”) ```
> 
> 步骤 5：微调模型 在加载模型后，你可能希望对其进行微调以适应你的特定任务。这涉及调整模型的权重以提高其在任务上的表现。你可以使用 `torch.optim` 模块来优化模型的权重。以下是一个示例：```py import torch model = torch.load(“path/to/model.pth”) optimizer = torch.optim.Adam(model.parameters(), lr=0.001) ```
> 
> 步骤 6：使用模型 一旦模型被加载并微调，你就可以用它来完成你的自然语言处理任务。你可以使用 `model()` 函数来进行预测或执行其他自然语言处理任务。以下是一个示例：```py import torch model = torch.load(“path/to/model.pth”) input_text = “This is a sample input text.” output = model(input_text) ```
> 
> 结论：在本文中，我们概述了使用 Python 加载大型语言模型的步骤。这些步骤包括选择语言模型、下载模型、准备模型、将模型加载到内存中、微调模型以及将模型用于自然语言处理任务。通过遵循这些步骤，你可以利用大型语言模型的强大功能来完成你的自然语言处理任务。

![](img/3ade20c5d7841a1728349a372a2014d3.png)

照片由 [Dušan veverkolog](https://unsplash.com/@veverkolog?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 拍摄，来源于 [Unsplash](https://unsplash.com/photos/Hc9G7owDs1s?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

# 通过 HF 加载 Llama2 27 亿参数模型

接下来，我们将尝试使用 Base Llama2 模型。

```py
model_id = "meta-llama/Llama-2-7b-hf"
bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_use_double_quant=True,
    bnb_4bit_quant_type="nf4",
    bnb_4bit_compute_dtype=torch.bfloat16
)

tokenizer = AutoTokenizer.from_pretrained(model_id)
model_4bit = AutoModelForCausalLM.from_pretrained(model_id, quantization_config=bnb_config, device_map="auto")
```

我们还设置了上面的 `bitsandbytes` 配置。有关模型量化的更多信息，请参阅 HuuggingFace 文档 [这里](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fhuggingface.co%2Fdocs%2Ftransformers%2Fmain_classes%2Fquantization)。

```py
text = """
You are a helpful, respectful and honest assistant. Always answer as helpfully as possible, while being safe.  Your answers should not include any harmful, unethical, racist, sexist, toxic, dangerous, or illegal content. Please ensure that your responses are socially unbiased and positive in nature.

If a question does not make any sense, or is not factually coherent, explain why instead of answering something not correct. If you don't know the answer to a question, please don't share false information.

Question: What are the steps to load a large language model using Python? \n

Answer: """

device = "cuda:0"
inputs = tokenizer(text, return_tensors="pt").to(device)

outputs = model_4bit.generate(**inputs, max_new_tokens=1000)
print(tokenizer.decode(outputs[0], skip_special_tokens=True))
```

为了测试，我们得到：

> 你是一个乐于助人、尊重他人且诚实的助手。始终尽可能提供有帮助的回答，同时确保安全。你的回答不应包含任何有害的、不道德的、种族歧视的、性别歧视的、毒性的、危险的或非法的内容。请确保你的回答在社会上是无偏见且积极的。如果一个问题没有任何意义，或在事实上不连贯，请解释原因，而不是回答不正确的内容。如果你不知道一个问题的答案，请不要分享虚假的信息。
> 
> **问题：** 使用 Python 加载大型语言模型的步骤是什么？
> 
> **回答：**
> 
> 你可以使用以下步骤通过 Python 加载大型语言模型：
> 
> 1.下载模型：你可以从互联网下载模型并将其保存到你的计算机上。
> 
> 2.导入必要的库：你需要导入加载模型所需的库，如 numpy、pandas 和 tensorflow。
> 
> 3. 定义模型：你需要通过指定词汇表和维度数量来定义模型。
> 
> 4. 加载模型：你需要通过指定词汇表、维度数量以及模型保存的文件路径来加载模型。
> 
> 5. 训练模型：你需要通过提供输入文本和期望输出文本来训练模型。
> 
> 6. 评估模型：你需要通过提供输入文本和期望输出文本来评估模型，并将输出与期望输出进行比较。
> 
> 7. 保存模型：你需要通过指定词汇表、维度数量以及模型保存的文件路径来保存模型。
> 
> 8. 加载模型：你需要通过指定词汇表、维度数量以及模型保存的文件路径来加载模型。
> 
> 9. 评估模型：你需要通过提供输入文本和期望输出文本来评估模型，并将输出与期望输出进行比较。
> 
> 10. 保存模型：你需要通过指定词汇表、维度数量以及模型保存的文件路径来保存模型。
> 
> 11. 加载模型：你需要通过指定词汇表、维度数量以及模型保存的文件路径来加载模型。
> 
> 12. 评估模型：你需要通过提供输入文本和期望输出文本来评估模型，并将输出与期望输出进行比较。
> 
> 13. 保存模型：你需要通过指定词汇表、维度数量以及模型保存的文件路径来保存模型。
> 
> 14. 加载模型：你需要通过指定词汇表、维度数量以及模型保存的文件路径来加载模型。
> 
> 15. 评估模型：你需要通过提供输入文本和期望输出文本来评估模型，并将输出与期望输出进行比较。
> 
> 16. 保存模型：你需要通过指定词汇表、维度数量以及模型保存的文件路径来保存模型。
> 
> 17. 加载模型：你需要通过指定词汇表、维度数量以及模型保存的文件路径来加载模型。
> 
> 18. 评估模型：你需要通过提供输入文本和期望输出文本来评估模型，并将输出与期望输出进行比较。
> 
> 19. 保存模型：你需要通过指定词汇表、维度数量以及模型保存的文件路径来保存模型。
> 
> 20. 加载模型：你需要通过指定词汇表、维度数量以及模型保存的文件路径来加载模型。
> 
> 21. 评估模型：你需要通过提供输入文本和期望输出文本来评估模型，并将输出与期望输出进行比较。

# 通过 HF 加载 Llama2 130 亿参数模型

接下来，我们还将尝试一个更大的模型——Llama2 的 130 亿参数模型。有关该模型的更多信息，请查看 [模型卡](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fhuggingface.co%2Fmeta-llama%2FLlama-2-13b-hf)。

```py
model_id = "meta-llama/Llama-2-13b-hf"
bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_use_double_quant=True,
    bnb_4bit_quant_type="nf4",
    bnb_4bit_compute_dtype=torch.bfloat16
)

tokenizer = AutoTokenizer.from_pretrained(model_id)
model_4bit = AutoModelForCausalLM.from_pretrained(model_id, quantization_config=bnb_config, device_map="auto")
```

就像我们在 27 亿参数模型中所做的那样，我们还将使用 `bitsandbytes`。

让我们试试吧！

```py
text = """
You are a helpful, respectful and honest assistant. Always answer as helpfully as possible, while being safe.  Your answers should not include any harmful, unethical, racist, sexist, toxic, dangerous, or illegal content. Please ensure that your responses are socially unbiased and positive in nature.

If a question does not make any sense, or is not factually coherent, explain why instead of answering something not correct. If you don't know the answer to a question, please don't share false information.

Q: What are the steps to load a large language model using Python? Please explain the answer to me in terms that a 5th grader can understand. Thank you. \n

A: """

device = "cuda:0"
inputs = tokenizer(text, return_tensors="pt").to(device)

outputs = model_4bit.generate(**inputs, max_new_tokens=1000)
print(tokenizer.decode(outputs[0], skip_special_tokens=True))
```

使用类似之前的问题，我们得到了一个有趣的结果：

> 你是一个有帮助、尊重和诚实的助手。始终尽可能提供有帮助的回答，同时确保安全。你的回答不应包含任何有害、不道德、种族歧视、性别歧视、有毒、危险或非法的内容。请确保你的回答在社会上没有偏见，并且性质积极。如果问题没有任何意义，或不符合事实，请解释原因，而不是回答不正确的内容。如果你不知道问题的答案，请不要提供虚假的信息。
> 
> **Q:** 使用 Python 加载大型语言模型的步骤是什么？请用五年级学生能理解的术语解释答案。谢谢。
> 
> **A:**
> 
> 1\. 下载语言模型。
> 
> 2\. 导入语言模型。
> 
> 3\. 加载语言模型。
> 
> 4\. 使用语言模型。

# 结果比较及简要的提示工程讨论

在本文中，我们看到 Llama2 不同大小和版本如何在一个简单的问答提示上表现。请注意，我们将提示分为三部分。首先，我们从 **指令文本** 开始，指明模型应该如何回答问题及其相关的限制；其次，包括实际问题作为 **输入文本**，然后我们可以询问 Llama2 我们想要的具体问题——这可以包括前缀“Question”或“Q:”，以便让模型更明确。最后，我们还包括 **输出文本**，这是模型将基于我们的问题生成令牌的地方。在这里，我们也可以包括“Answer”或“A:”以便让模型更明确——在我的测试中，我发现这对于给 Llama2 一些关于答案格式的示例特别有用。我们不会在本文中深入探讨提示工程，但有关更多信息，请随时阅读这篇 [博客文章](https://www.promptingguide.ai/)。

你觉得上面三个结果中哪些方面比较有趣？我个人非常喜欢 Llama2 如何基于“用五年级学生能理解的术语解释答案”这一提示生成答案。此外，Llama2 在 27 亿参数的结果中看起来像是在循环打印加载 LLM 的步骤——虽然这在概念上是有意义的，但是否有其他更直观、更人性化的方式来回答这个问题呢？

# 其他考虑事项

加载大型语言模型只是了解和创建有用应用的初步步骤。提升 LLM 性能有许多方法，例如并行处理、混合精度训练、模型量化（我们在这里使用了这种方法）、缓存、批处理和分布式系统。

所有这些都是令人兴奋的探索方向，我希望这篇博客文章以及随附的笔记本能为你的 LLM 之旅提供一个良好的起点！

> 完整代码可以在[这里](https://github.com/piesauce/llm-playbooks/blob/loading_llms/ateng/notebooks/Loading_LLMs_Basic.ipynb)查看。
> 
> 这篇文章作为 Suhas Pai 的书籍[《设计大型语言模型应用》](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fwww.oreilly.com%2Flibrary%2Fview%2Fdesigning-large-language%2F9781098150495%2F)的补充和扩展，并且是书籍 LLM 仓库的一部分。
> 
> 购买[我的书](https://www.amazon.com/Data-Resource-Emerging-Countries-Landscape/dp/1641372524)，如有任何错误或建议，请通过[电子邮件](http://at2507@nyu.edu)报告。也欢迎就合作事宜与我联系！我目前对新项目持开放态度。
> 
> 通过[LinkedIn](https://www.linkedin.com/in/angelavteng/)、[Twitter](https://twitter.com/ambervteng)或[TikTok](https://www.tiktok.com/@angelaworkout)与我联系。
> 
> 在[Medium](https://medium.com/@angelamarieteng)上关注我。

## *参考文献：*

+   [`ai.meta.com/llama/`](https://ai.meta.com/llama/)

+   [`about.fb.com/news/2023/07/llama-2/`](https://about.fb.com/news/2023/07/llama-2/)

+   [`ai.meta.com/research/publications/llama-2-open-foundation-and-fine-tuned-chat-models/`](https://ai.meta.com/research/publications/llama-2-open-foundation-and-fine-tuned-chat-models/)

+   [`scontent.fmnl25-1.fna.fbcdn.net/v/t39.2365-6/10000000_662098952474184_2584067087619170692_n.pdf?_nc_cat=105&ccb=1-7&_nc_sid=3c67a6&_nc_ohc=TjG8FsFfH8cAX_uux2_&_nc_ht=scontent.fmnl25-1.fna&oh=00_AfBhA6S3oJWa52QjMx99BmgN3gg06l2jsH2-P2MNIxSmAw&oe=6529307F`](https://scontent.fmnl25-1.fna.fbcdn.net/v/t39.2365-6/10000000_662098952474184_2584067087619170692_n.pdf?_nc_cat=105&ccb=1-7&_nc_sid=3c67a6&_nc_ohc=TjG8FsFfH8cAX_uux2_&_nc_ht=scontent.fmnl25-1.fna&oh=00_AfBhA6S3oJWa52QjMx99BmgN3gg06l2jsH2-P2MNIxSmAw&oe=6529307F)

+   [`huggingface.co/`](https://huggingface.co/)

+   [`huggingface.co/transformers/v4.10.1/main_classes/pipelines.html`](https://huggingface.co/transformers/v4.10.1/main_classes/pipelines.html)

+   [`huggingface.co/docs/transformers/main_classes/pipelines#transformers.Text2TextGenerationPipeline`](https://huggingface.co/docs/transformers/main_classes/pipelines#transformers.Text2TextGenerationPipeline)

+   [`huggingface.co/docs/transformers/main/llm_tutorial`](https://huggingface.co/docs/transformers/main/llm_tutorial)

+   [`www.promptingguide.ai/`](https://www.promptingguide.ai/)

+   [`huggingface.co/docs/transformers/main_classes/quantization`](https://huggingface.co/docs/transformers/main_classes/quantization)

+   [`huggingface.co/models?library=transformers&sort=trending&search=meta`](https://huggingface.co/models?library=transformers&sort=trending&search=meta)

+   [`huggingface.co/meta-llama/Llama-2-7b-chat-hf`](https://huggingface.co/meta-llama/Llama-2-7b-chat-hf)

+   [`huggingface.co/blog/4bit-transformers-bitsandbytes`](https://huggingface.co/blog/4bit-transformers-bitsandbytes)
