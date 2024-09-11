# Open LLM Falcon-40B 简介：性能、训练数据和架构

> 原文：[`towardsdatascience.com/introduction-to-the-open-llm-falcon-40b-performance-training-data-and-architecture-98388fa40226?source=collection_archive---------4-----------------------#2023-06-07`](https://towardsdatascience.com/introduction-to-the-open-llm-falcon-40b-performance-training-data-and-architecture-98388fa40226?source=collection_archive---------4-----------------------#2023-06-07)

## 开始使用 Falcon-7B、Falcon-40B 及其指令版本

[](https://medium.com/@bnjmn_marie?source=post_page-----98388fa40226--------------------------------)![本杰明·玛丽](https://medium.com/@bnjmn_marie?source=post_page-----98388fa40226--------------------------------)[](https://towardsdatascience.com/?source=post_page-----98388fa40226--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----98388fa40226--------------------------------) [本杰明·玛丽](https://medium.com/@bnjmn_marie?source=post_page-----98388fa40226--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fad2a414578b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-the-open-llm-falcon-40b-performance-training-data-and-architecture-98388fa40226&user=Benjamin+Marie&userId=ad2a414578b3&source=post_page-ad2a414578b3----98388fa40226---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----98388fa40226--------------------------------) ·6 分钟阅读·2023 年 6 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F98388fa40226&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-the-open-llm-falcon-40b-performance-training-data-and-architecture-98388fa40226&user=Benjamin+Marie&userId=ad2a414578b3&source=-----98388fa40226---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F98388fa40226&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-the-open-llm-falcon-40b-performance-training-data-and-architecture-98388fa40226&source=-----98388fa40226---------------------bookmark_footer-----------)![](img/d5fa442c9fa2a7cd9bcf139978783e6e.png)

照片由[Brandon](https://unsplash.com/@greener_30?utm_source=medium&utm_medium=referral)拍摄，发布于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

[Falcon 模型](https://huggingface.co/tiiuae/falcon-40b)自 2023 年 5 月发布以来引起了广泛关注。

它们是因果大语言模型（LLM），或所谓的“仅解码器”模型，非常类似于 GPT。

> 定义：因果语言模型
> 
> 因果语言建模涉及预测一个令牌后面的令牌。在训练期间，模型的注意力仅集中在左侧上下文上。右侧上下文被屏蔽。这些模型通常在数十亿个词上进行训练。

Falcon 模型自 5 月 31 日起[完全免费，甚至用于商业用途（Apache 2.0 许可证）](https://www.tii.ae/news/uaes-falcon-40b-now-royalty-free)。Falcon 模型由[阿布扎比技术创新研究所（TII）](https://www.tii.ae/)开发和训练。

根据初步结果，Falcon-40B，作为 Falcon 模型中最大的一个，超越了所有其他因果 LLM，包括 LLaMa-65B 和 MPT-7B。

在这篇博客文章中，我详细介绍了 Falcon-40B、Falcon-7B 及其指令版本。我们将看到它们与其他模型的表现对比、训练方式以及如何在自己的 GPU 上使用 QLoRa 运行 Falcon7-B。

# OpenLLM 上的表现

Falcon-40B 的指令版本在[OpenLLM 排行榜](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard)中排名第一。标准版本排名第二。

OpenLLM 排行榜评估 LLM 在 4 个任务上的表现：

+   [AI2 Reasoning Challenge](https://arxiv.org/abs/1803.05457) (25-shot)：小学科学问题。

+   [HellaSwag](https://arxiv.org/abs/1905.07830) (10-shot)：一个常识推理基准测试。

+   [MMLU](https://arxiv.org/abs/2009.03300) (5-shot)：涵盖数学、计算机科学和法律等多个领域的 57 个任务。

+   [TruthfulQA](https://arxiv.org/abs/2109.07958) (0-shot)：一个评估模型回答问题时真实性的基准。

Falcon-40B 在所有这些任务上都超越了 Meta AI 的 LLaMa-65B。

# Falcon RefinedWeb

Falcon 模型主要在[Falcon RefinedWeb](https://huggingface.co/datasets/tiiuae/falcon-refinedweb)数据集上进行训练。它也是由 TII 创建的，并在 Apache 2.0 许可证下分发。

RefinedWeb 从 CommonCrawl 中提取，并经过彻底策划。TII 声称它对多模态友好，因为保留了图片的链接和替代文本。

在 Hugging Face Hub 发布的数据集卡片中，TII 写道：“***这个公开提取*** *[…]*”。对我来说，因此不清楚 Falcon 模型是否在这个仅为“提取”的公开版本数据集上进行训练，还是使用了更大的内部版本。

这个提取文件解压需要 2.8Tb 的硬盘空间。

由于它在 Hugging Face Hub 中可用，你只需运行以下代码即可开始使用：

```py
from datasets import load_dataset
rw = load_dataset("tiiuae/falcon-refinedweb")
```

*注意：你需要“datasets”库。如果没有，你可以通过“pip install datasets”安装。*

RefinedWeb 与策划的语料库结合，以训练 Falcon 模型。

这个数据集代表了 Falcon 模型预训练数据的 75%。它仅涵盖英语。为了增加更多语言，他们还准备了“RefinedWeb-Europe”，涵盖了多种欧洲语言：德语、西班牙语、法语、意大利语、葡萄牙语、波兰语、荷兰语、罗马尼亚语、捷克语和瑞典语。

最后，为了覆盖更多的体裁和领域，他们添加了书籍、对话（例如，来自 Reddit）、代码、技术报告和科学论文（例如，来自 arXiv）的语料库。*注意：他们没有透露“代码”的来源。也不清楚他们编制的数据集的许可证是什么。*

总共，这相当于 1500 亿个标记用于预训练 Falcon 模型。

# Falcon-40B 和 Falcon-7B 的预训练

对于预训练，他们使用了：

+   旋转位置嵌入 ([Su et al., 2021](https://arxiv.org/abs/2104.09864))

+   Multiquery ([Shazeer et al., 2019](https://arxiv.org/abs/1911.02150)) 和 FlashAttention ([Dao et al., 2022](https://arxiv.org/abs/2205.14135))

+   并行注意力/MLP 与双层规范

Falcon-40B 具有以下架构：

+   层数：60

+   嵌入维度：8,192

+   头数：64

+   词汇表大小：65,024

+   序列长度：2,048

这与 LLaMa 的架构非常相似，只是词汇表大了一倍。

在我看来，序列长度在我们看到 LLM 接受超过 10,000 个标记的情况下，如 GPT-4 和[Claude](https://www.anthropic.com/index/introducing-claude)，显得相当短。

Falcon-7B 有一个较小的架构，使其能够在消费者硬件上进行微调。与 40B 版本唯一的区别是层数和嵌入维度减少了一半：

+   层数：60

+   嵌入维度：4,544

两个版本都使用了 bfloat16 精度和 AdamW。它们使用了 AWS SageMaker 和 384 个 A100 40GB GPU 的 P4d 实例，但尚未透露训练持续了多长时间。

# Falcon-40B/7B 的指令版本

[Falcon-40B](https://huggingface.co/tiiuae/falcon-40b-instruct) 和 [7B](https://huggingface.co/tiiuae/falcon-7b-instruct) 的指令版本表现更好。

Falcon-40B-Instruct 在 AWS SageMaker 上训练，使用了配备 64 个 A100 40GB GPU 的 P4d 实例。对于 Falcon-7B-Instruct，他们仅使用了 32 个 A100。

它们在 250 百万个标记的聊天/指令数据集混合数据上进行了微调，这些数据来源于[Bai ze](https://github.com/project-baize/baize-chatbot)、[GPT4all](https://github.com/nomic-ai/gpt4all)、[GPTeacher](https://github.com/teknium1/GPTeacher)和 1300 万标记的 RefinedWeb 语料库。

Bai ze 是由 ChatGPT 生成的数据集。我会对在商业应用中使用 Falcon 模型的指令版本保持谨慎。根据 OpenAI 的[使用条款](https://openai.com/policies/terms-of-use)：

> “限制。你不得 […] (iii) 使用服务的输出开发与 OpenAI 竞争的模型”

“服务”包括 ChatGPT。而 Falcon-40B 是一个可以与 OpenAI 的 GPT 模型“竞争”的模型。

# 如何在你的 GPU 上使用 Falcon-7B 和 QLoRa

在上一篇文章中，我介绍了 QLoRa 在消费级硬件上微调 LLMs 的方法：

## QLoRa: 在你的 GPU 上微调大型语言模型

### 现在可以在消费级硬件上对具有数十亿参数的模型进行微调。

towardsdatascience.com

你可以对 Falcon-7B 按相同的步骤操作，但它不会在 Google Colab 的免费实例上运行。模型需要过多的 CPU 内存。

如果你的计算机有 32 Gb 的 RAM，这应该可以工作。如果没有这么多 RAM，你将需要选择云计算或 Google Colab Pro 等方式。

一旦你有了支持 Falcon-7B 的环境，还有一些小修改需要对我的 QLoRa 教程进行调整。

首先，你必须安装“einops”：

```py
pip install -q einops
```

然后，按如下方式修改模型的加载：

```py
model = AutoModelForCausalLM.from_pretrained(model_id, quantization_config=bnb_config, device_map="auto", trust_remote_code=True)
```

在这一行中，“trust_remote_code=True” 是必要的。这是 Hugging Face 获得你同意一些代码由模型直接在你的机器上执行的方式。在这里，Falcon 运行一个配置脚本。

除此之外，其它方面应该与我的教程相同。

如果你不想使用 QLoRa 并且有 GPU 集群访问权限，标准的加载和运行 Falcon-7B/[Falcon-40B 的方法可以参考 Hugging Face 模型卡片](https://huggingface.co/tiiuae/falcon-40b)。

```py
from transformers import AutoTokenizer, AutoModelForCausalLM
import transformers
import torch

model = "tiiuae/falcon-40b"

tokenizer = AutoTokenizer.from_pretrained(model)
pipeline = transformers.pipeline(
    "text-generation",
    model=model,
    tokenizer=tokenizer,
    torch_dtype=torch.bfloat16,
    trust_remote_code=True,
    device_map="auto",
)
sequences = pipeline(
   "Girafatron is obsessed with giraffes, the most glorious animal on the face of this Earth. Giraftron believes all other animals are irrelevant when compared to the glorious majesty of the giraffe.\nDaniel: Hello, Girafatron!\nGirafatron:",
    max_length=200,
    do_sample=True,
    top_k=10,
    num_return_sequences=1,
    eos_token_id=tokenizer.eos_token_id,
)
for seq in sequences:
    print(f"Result: {seq['generated_text']}")
```

# 结论

Falcon 模型是预训练的 LLMs。如果你有数据进行微调，你可以将它们用于任何自然语言处理任务。请注意，即使不进行微调，标准（非指令型）版本在许多任务中已经表现得非常好，如在 [OpenLLM 排行榜](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard)上所示，能够回答来自各个领域的问题并进行常识推理。

Falcon 模型的“指令”版本已经过微调。它们表现得像 ChatGPT，即具有一般知识的聊天机器人。

Falcon 模型也是流行的 LLaMa 模型的非常有趣的替代品。Falcon-40B 是：

+   **更小**：LLaMa 有 65 亿参数，而 Falcon-40B 只有 40 亿参数，因此需要的内存较少。

+   **更好**：在 OpenLLM 排行榜上，Falcon-40B 排名第一。

+   **免费**：Falcon 模型在 Apache 2.0 许可证下分发，允许商业使用，而 LLaMa 只能用于研究目的。

如果你对这些模型感兴趣，关注这篇博客文章。TII 将发布一篇科学论文/技术论文，更详细地描述他们的工作。一旦上线，我会在这里提供链接。

*如果你喜欢这篇文章并希望阅读接下来的文章，支持我的最佳方式是使用这个链接成为 Medium 会员：*

[](https://medium.com/@bnjmn_marie/membership?source=post_page-----98388fa40226--------------------------------) [## 通过我的推荐链接加入 Medium - 本杰明·玛丽

### 加入我们的人工智能社区，获取前沿研究的访问权限。这个博客旨在揭示人工智能领域的最新进展。

medium.com](https://medium.com/@bnjmn_marie/membership?source=post_page-----98388fa40226--------------------------------)

*如果你已经是会员并希望支持这项工作，* [*请在 Medium 上关注我*](https://medium.com/@bnjmn_marie)*。*
