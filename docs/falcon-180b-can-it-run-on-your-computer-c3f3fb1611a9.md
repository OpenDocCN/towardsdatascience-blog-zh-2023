# Falcon 180B：它能在你的计算机上运行吗？

> 原文：[`towardsdatascience.com/falcon-180b-can-it-run-on-your-computer-c3f3fb1611a9`](https://towardsdatascience.com/falcon-180b-can-it-run-on-your-computer-c3f3fb1611a9)

## 是的，如果你有足够的 CPU 内存

[](https://medium.com/@bnjmn_marie?source=post_page-----c3f3fb1611a9--------------------------------)![Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----c3f3fb1611a9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c3f3fb1611a9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c3f3fb1611a9--------------------------------) [Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----c3f3fb1611a9--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c3f3fb1611a9--------------------------------) ·阅读时间 7 分钟·2023 年 9 月 12 日

--

![](img/8f3997b4586e11465fe44235b7675fb0.png)

图片由作者制作，插图来源于 Pixabay ([1](https://pixabay.com/vectors/computer-technology-internet-web-312476/),[2](https://pixabay.com/illustrations/falcon-birds-of-prey-bird-2928724/))

2023 年 5 月，阿布扎比技术创新研究所（TII）发布了两个预训练的 LLM：Falcon-7B 和 Falcon-40B 及其聊天版本。这两个模型表现非常出色，并在 [OpenLLM 排行榜](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard) 上排名第一。

TII 发布的第三个模型刚刚加入了 Falcon 家族：**Falcon 180B**，一个具有 1800 亿参数的模型。它比 Llama 2 70B 多了 2.5 倍的参数，比 Falcon-40B 多了 4.5 倍。

以下是关于 Falcon 180B 的一些事实（来源：[Falcon 180B 模型卡](https://huggingface.co/tiiuae/falcon-180B)）：

+   经过 3.5 万亿个标记的预训练 ([RefinedWeb](https://huggingface.co/datasets/tiiuae/falcon-refinedweb))

+   以 Apache 2.0 许可证分发

+   大小为 360 GB

+   在 [OpenLLM 排行榜](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard) 上排名第一（截至 2023 年 9 月 11 日）：

![](img/2ae19cc41090ae37059620b860053c96.png)

OpenLLM 排行榜截图（2023 年 9 月 11 日） — 作者提供的图片

还有一个聊天版本。模型可以在 Hugging Face hub 上获取：

+   [Falcon 180B](https://huggingface.co/tiiuae/falcon-180B)

+   [Falcon 180B Chat](https://huggingface.co/tiiuae/falcon-180B-chat)

**Falcon 180B** 完全免费且技术先进。但它也是一个庞大的模型。

*它能在你的计算机上运行吗？*

除非你的计算机准备好进行非常高强度的计算，否则无法开箱即用运行 Falcon 180B。你需要升级计算机并使用该模型的量化版本。

在这篇文章中，我解释了如何在消费级硬件上运行 Falcon-180B。我们将看到在现代计算机上运行一个 1800 亿参数的模型是相对负担得起的。我还讨论了几种有助于减少硬件要求的技术。

# 在你的计算机上加载 Falcon 180B：你需要什么？

首先你需要知道的是，Falcon 180B 具有 1800 亿个参数，存储为 bfloat16。一个 (b)float16 参数在内存中占用 2 字节。

当你加载一个模型时，标准的 Pytorch 流程如下：

1.  一个空模型被创建：180B 参数 * 2 字节 = 360 GB

1.  将权重加载到内存中：180B 参数 * 2 字节 = 360 GB

1.  将第 2 步加载的权重加载到第 1 步创建的空模型中。

1.  将第 3 步获得的模型移动到用于推理的设备上，例如 GPU。

第 1 步和第 2 步是消耗内存的步骤。总的来说，你需要 720 GB 的可用内存。这可以是 CPU RAM，但为了快速推理，你可能想使用 GPU，例如 9 个带有 80 GB VRAM 的 A100。

无论是 CPU RAM 还是 VRAM，这都是大量的内存。幸运的是，这些要求可以很容易地减少。

在 Hugging Face Hub 上，Falcon 180B 采用 safetensors 格式分发。这个格式相较于标准的 Pytorch 格式有几个优势。它（几乎）没有复制，因此模型直接加载到第 1 步创建的空模型中。这节省了大量内存。

> ***关于 safetensors***
> 
> [*safetensors*](https://huggingface.co/docs/safetensors/index) *节省内存，但它还使模型运行更安全，因为在这种格式中无法存储任意代码。safetensors 模型的加载速度也更快。当你从 hub 下载模型时，使用这种格式代替“.bin”格式，可以实现更快、更安全且节省内存的加载。*

尽管看起来我们跳过了第 2 步，但仍然会有一些内存开销需要预期。TII 在[模型卡](https://huggingface.co/tiiuae/falcon-180B)上写道，400 GB 的内存是可行的。这仍然很多，但比使用标准的 Pytorch 格式少 220 GB。

我们需要一个具有 400 GB 存储空间的设备，例如，5 个带有 80 GB VRAM 的 A100 GPU。我们距离“消费级”配置还有很大差距。

# 将 Falcon 180B 分割到多个内存设备上。

你可能没有一个 400 GB 的单一内存设备，但如果你结合所有以下设备的内存，你的计算机可能有超过 400 GB 的内存：

+   GPU 的 VRAM：如果你有一块 NVIDIA RTX 3090 或 4090，那已经有 24 GB。

+   CPU RAM：大多数现代计算机至少有 12 GB 的 CPU RAM。扩展 CPU RAM 也非常便宜。

+   硬盘（或 SSD）：这可以是几个 TB 的空闲内存。请注意，如果你计划使用 SSD（NVMe M2 类型）来运行 LLM，它将比典型的硬盘快得多。

为了利用可用设备，我们可以将 Falcon 180B 切分，以便它按照优先顺序使用设备的最大内存：GPU、CPU RAM 和硬盘。

通过[Accelerate](https://huggingface.co/docs/accelerate/index)的 device_map 可以轻松实现这一点。

device_map 将模型的整个层分配到你拥有的不同设备上。

![](img/346ada05e45868b2a6a42513bf97173a.png)

device_map — 作者提供的图片

如果你想查看一些 device_map 使用的例子，[查看我的笔记本](https://kaitchup.substack.com/p/notebooks)。我在大多数笔记本中使用了 device_map。

[device_map 非常方便可以避免 CUDA 内存不足错误](https://kaitchup.substack.com/p/device-map-avoid-out-of-memory-errors-when-running-large-language-models-af7de5076f9d)。但如果你打算在消费级硬件上使用 Falcon 180B，这仍然远非理想。即使配置高端，配备 24 GB 的 VRAM 和 32 GB 的 CPU RAM，也会在硬盘上留下几百 GB 的空间。

这是一个问题，原因有两个：

+   硬盘和 SSD 的**速度比 VRAM 和 CPU RAM**慢得多。从硬盘加载和运行 Falcon 180B 会花费非常长的时间。

+   消费级硬盘和 SSD**并未设计和测试用于这种密集使用**。如果模型的许多部分被卸载到硬盘上，系统将不得不在推理过程中多次访问和读取这些巨大的模型分片。这是一个长期的巨大读写操作数量。如果你进行几天的推理，例如生成一些合成数据集，这可能会损坏你的硬盘，或者至少显著减少其寿命。

为了避免过度使用硬盘，我们没有太多解决方案：

+   添加一张 GPU：大多数高端主板可以容纳两张 RTX 3090/4090。这将为你提供 48 GB 的 VRAM。

+   扩展 CPU RAM：大多数主板有 4 个可用于 CPU RAM 套件的插槽。虽然有 4*128GB 的 CPU RAM 套件出售，但不容易找到，而且仍然很昂贵。*注意：操作系统对 CPU RAM 的总支持量也有限制。对于 Windows 10，它是 2 TB。如果你使用的是旧版本操作系统，在购买更多 RAM 之前应查看其文档。*

+   量化 Falcon 180B 并扩展 CPU RAM。

Falcon 180B 的量化是减少内存消耗的最佳选择之一。

# 通过量化减少 Falcon 180B 的大小

量化非常大的语言模型到较低精度现在已成为一种常见做法。[GPTQ](https://arxiv.org/abs/2210.17323)和[bitsandbytes nf4](https://arxiv.org/abs/2305.14314)是将 LLM 量化到 4 位精度的两种流行方法。

Falcon 180B 使用 bfloat16。我们看到它是 360 GB。

一旦量化到 4 位精度，它仅为 90 GB（1800 亿参数 * 0.5 字节）。我们可以使用 100 GB 的内存（90GB + 一些内存开销）来加载 4 位 Falcon 180B。

如果你有 24 GB 的 VRAM，你“仅需”75 GB 的 CPU RAM。这仍然很多，但比加载原始模型要实惠得多，而且在推理过程中不会将模型的层卸载到硬盘上。*注意：你仍然需要在硬盘上留出 100 GB 的自由空间以存储模型。*

你甚至不需要 GPU。拥有 128GB 的 CPU 内存，你可以仅使用 CPU 进行推理。

量化本身是非常昂贵的。幸运的是，我们已经可以在网上找到量化版本。[TheBloke](https://huggingface.co/TheBloke)发布了使用 GPTQ 制作的 4-bit 版本：

+   [4-bit Falcon 180B](https://huggingface.co/TheBloke/Falcon-180B-GPTQ)

+   [4-bit Falcon 180B Chat](https://huggingface.co/TheBloke/Falcon-180B-Chat-GPTQ)

*注意：还有 3-bit 模型作为这些模型的“分支”提供。请遵循 4-bit 模型卡上的说明来获取这些模型。*

尽管模型的精度有所降低，但根据[Hugging Face 的实验](https://huggingface.co/blog/falcon-180b)，模型的性能保持相似。

GPTQ 模型推理速度快，你可以使用 LoRA 适配器进行微调。例如，我在这篇文章中展示了如何微调使用 GPTQ 量化的 Llama 2。

[## 使用 Transformers 和 TRL 进行量化和微调 LLMs](https://kaitchup.substack.com/p/quantize-and-fine-tune-llms-with?source=post_page-----c3f3fb1611a9--------------------------------)

### GPTQ 现在使用起来容易多了

[kaitchup.substack.com](https://kaitchup.substack.com/p/quantize-and-fine-tune-llms-with?source=post_page-----c3f3fb1611a9--------------------------------)

虽然可以对 GPTQ 模型进行微调，但我不推荐这样做。使用 QLoRA 进行微调虽然内存消耗相似，但由于 nf4 量化的改进，能获得更好的模型，正如[QLoRA 论文](https://arxiv.org/abs/2305.14314)中所示。

# 结论

总结一下，你需要量化和 100GB 内存来在一个相对实惠的电脑上运行 Falcon 180B。

对于快速推理或微调，你需要一个 GPU。RTX 4090（或更便宜但较慢的 RTX 3090 24GB）足以加载量化模型的 1/4。如果你的电脑机箱有足够的空间，你甚至可以放入两张 RTX 显卡。

如果你想在仅 CPU 配置上运行 Falcon-180B，即没有 GPU，忘掉微调吧，这会太慢。推理也会很慢，但使用近期的高端 CPU 和优化过的软件，如 llama.cpp，运行 Falcon 180B 还是可能的。

如果你对使用[llama.cpp](https://github.com/ggerganov/llama.cpp)感兴趣，可以看看我关于如何仅使用 CPU 运行 Vicuna 的文章：

[## 高速推理：使用 llama.cpp 和 Vicuna 在 CPU 上](https://kaitchup.substack.com/p/high-speed-inference-with-llama-cpp-and-vicuna-on-cpu-136d28e7887b?source=post_page-----c3f3fb1611a9--------------------------------)

### 快速推理不需要 GPU

[kaitchup.substack.com](https://kaitchup.substack.com/p/high-speed-inference-with-llama-cpp-and-vicuna-on-cpu-136d28e7887b?source=post_page-----c3f3fb1611a9--------------------------------)
