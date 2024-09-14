# 利用 Falcon 40B 模型，最强大的开源 LLM

> 原文：[`towardsdatascience.com/harnessing-the-falcon-40b-model-the-most-powerful-open-source-llm-f70010bc8a10`](https://towardsdatascience.com/harnessing-the-falcon-40b-model-the-most-powerful-open-source-llm-f70010bc8a10)

## 掌握开源语言模型：深入了解 Falcon-40B

[](https://medium.com/@luisroque?source=post_page-----f70010bc8a10--------------------------------)![Luís Roque](https://medium.com/@luisroque?source=post_page-----f70010bc8a10--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f70010bc8a10--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f70010bc8a10--------------------------------) [路易斯·罗克](https://medium.com/@luisroque?source=post_page-----f70010bc8a10--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f70010bc8a10--------------------------------) ·阅读时间 12 分钟·2023 年 6 月 9 日

--

# **介绍**

人工智能行业的焦点已经转向构建更强大、大规模的语言模型，这些模型能够理解和生成类人文本。像 OpenAI 的 GPT-3 这样的模型引领了这一趋势，展示了卓越的能力。OpenAI 一直以来的口号是将这些模型开源。然而，遗憾的是，他们决定走另一条路，新模型如 ChatGPT（或 GPT-3.5）和 GPT-4 现在是闭源的。这些模型的专有性质和有限访问推使许多研究人员和开发人员寻求开源替代品并为之做出贡献。

这就是 Falcon-40B 的重要性所在。在上周末，技术创新研究所（TII）宣布 Falcon-40B 现在对商业和研究用途免除版权费用。因此，它打破了专有模型的障碍，为开发人员和研究人员提供了可以根据具体需求使用和修改的最先进的语言模型。

除此之外，Falcon-40B 目前是 [OpenLLM 排行榜](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard) 上表现最好的模型，超越了 LLaMA、StableLM、RedPajama 和 MPT 等模型。这个排行榜旨在跟踪、排名和评估各种 LLM 和聊天机器人的性能，提供一个清晰、公正的能力指标。

![](img/d040b42c203827a3dba4f30777c0cf0d.png)

图 1：Falcon-40B 在 [OpenLLM 排行榜](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard) 上处于领先地位（[图片来源](https://unsplash.com/photos/7rEvtDHWrF4)）

一如既往，代码可在我的 [Github](https://github.com/luisroque/large_laguage_models) 上找到。

# Falcon LLM 是如何开发的？

Falcon 开发中的核心差异之一是训练数据的质量。Falcon 的预训练数据规模接近五万亿个 token，来源于公共网络抓取、研究论文和社交媒体对话。由于 LLM 对训练数据特别敏感，团队建立了一个定制的数据管道，通过广泛的过滤和去重从预训练数据中提取高质量数据。

模型本身在 AWS 上使用 384 个 GPU 训练了两个月。结果是一个超越 GPT-3 的 LLM，只需 75% 的训练计算预算和五分之一的推理计算量。

Falcon-40B 主要以英语为中心，但也包括德语、西班牙语、法语、意大利语、葡萄牙语、波兰语、荷兰语、罗马尼亚语、捷克语和瑞典语的语言能力。请注意，与任何在网络数据上训练的模型一样，它可能会反映在线上普遍存在的偏见和刻板印象。因此，在生产环境中使用 Falcon-40B 时，请适当评估这些风险并实施适当的缓解策略。

# 模型架构和目标

Falcon-40B 作为基于变换器的模型家族的一员，遵循因果语言建模任务，其目标是预测 token 序列中的下一个 token。其架构基本上建立在 GPT-3 [1] 的设计原则之上，进行了几个重要的调整。

第一个修改是使用旋转位置嵌入 [2] 替代传统的位置嵌入。与使用静态向量表示序列中 token 位置的传统位置嵌入不同，旋转嵌入直接将位置信息编码到注意力机制中。这使得模型能够利用相对位置关系，从而实现更好的上下文理解和对较长序列的更好处理。

Falcon-40B 还通过采用多查询注意力 [3] 和 FlashAttention [4] 实现了一种新颖的注意力机制。多查询注意力允许模型为每个 token 生成多个查询，从而更好地表示 token 与序列中其他 token 的关系。此外，该模型使用了一种内部变体的多查询，每个张量并行度具有独立的键和值配对，这有助于通过提高计算效率来处理高维数据。另一方面，FlashAttention 是一种最近的技术，能够加速自注意力的计算，降低该操作的复杂性，从而提升模型的整体计算效率。

Falcon-40B 中的解码器块采用了并行注意力/MLP（多层感知器）设计，并具有两层归一化。这个结构在模型扩展和计算速度方面具有优势。注意力层和 MLP 层的并行化提高了模型同时处理大量数据的能力，从而减少了训练时间。此外，实施两层归一化有助于稳定学习过程并减轻与内部协变量偏移相关的问题，从而使模型更加稳健和可靠。

# 使用 Falcon-40B-Instruct 实现聊天功能

我们使用的是 Falcon-40B-Instruct，它是 Falcon-40B 的一个新变体。它基本上是相同的模型，但在 Baize 的混合数据上进行了微调。Baize 是一个开源聊天模型，使用 LoRA（大语言模型的低秩适应）进行训练。Baize 使用了 100k 个 ChatGPT 自我对话的数据以及 Alpaca 的数据来提升其性能。

首先定义一个名为`measure_perf`的函数，用于测量给定模型和提示的内存消耗和推理执行时间。为了在函数执行期间测量峰值 GPU 内存消耗，我们需要跟踪在函数执行的任何时刻分配的最大内存。PyTorch 提供了一个名为`torch.cuda.max_memory_allocated`的函数用于此目的。

```py
def measure_perf(
    prompt: str, model: AutoModelForCausalLM, tokenizer: AutoTokenizer
) -> Tuple[float, float, torch.Tensor]:
    """
    Measures memory consumption and inference execution time for a given model and prompt.

    Args:
        prompt: Text to be used as input for the model.
        model: Pretrained model used for inference.
        tokenizer: Pretrained tokenizer used to encode the prompt.

    Returns:
        Peak memory consumption in GB, execution time in seconds, and output tensor from the model.
    """
    torch.cuda.empty_cache()
    torch.cuda.reset_peak_memory_stats()

    start_time = time.time()

    input_ids = tokenizer(prompt, return_tensors="pt").input_ids.to("cuda")
    outputs = model.generate(input_ids, max_length=100)

    end_time = time.time()

    peak_mem = torch.cuda.max_memory_allocated()
    peak_mem_consumption = peak_mem / 1e9  # convert bytes to GB

    exec_time = end_time - start_time

    return peak_mem_consumption, exec_time, outputs
```

函数`plot_results`将用于绘制内存消耗和执行时间，以便对模型性能进行可视化分析。

```py
def plot_results(
    mem_consumptions: List[float], execution_times: List[float], dir: str = "plots"
) -> None:
    """
    Plots memory consumption and execution times.

    Args:
        mem_consumptions: List of memory consumption data in GB.
        execution_times: List of execution time data.
        dir: Destination dir for the plot.
    """
    os.makedirs(dir, exist_ok=True)

    fig, ax1 = plt.subplots()

    color = "tab:red"
    ax1.set_xlabel("Runs")
    ax1.set_ylabel("GPU Memory Consumption (GB)", color=color)
    ax1.plot(mem_consumptions, color=color)
    ax1.tick_params(axis="y", labelcolor=color)
    ax1.yaxis.get_major_formatter().set_useOffset(False)

    ax2 = ax1.twinx()
    color = "tab:blue"
    ax2.set_ylabel("Execution time (s)", color=color)
    ax2.plot(execution_times, color=color)
    ax2.tick_params(axis="y", labelcolor=color)
    ax2.yaxis.get_major_formatter().set_useOffset(False)

    fig.tight_layout()
    plt.title("GPU Memory Consumption and Execution Time for Each Run")
    fig.subplots_adjust(top=0.88)
    plt.savefig(f"{dir}/falcon_memory_time.png")
```

现在，让我们加载 Falcon-40B 模型及其标记器。在这一步中，模型和标记器将使用 Hugging Face 的`from_pretrained`函数加载。请注意，标记器负责将输入文本转换为模型可以处理的标记。

现在，稍微绕一下关于量化的内容。量化是一种技术，可以减少模型中权重的精度，从而显著降低内存需求并可能加速推理。正如人们所预期的那样，这种方法并非没有代价，我们最终会失去一些准确性。然而，它在将模型部署到计算资源有限的设备上或处理大型模型时尤其有用，因为这些模型否则可能无法适应内存。

最近，`bitsandbytes`与 Hugging Face Transformers 的集成已经发布。这使用户可以加载具有 8 位或 4 位精度的模型。从`bitsandbytes`的 0.37.0 版本开始，用户可以以 8 位精度加载模型，这是大多数 GPU 硬件支持的功能。这是通过在调用`.from_pretrained`方法时使用`load_in_8bit=True`参数实现的。更新的 0.39.0 版本的`bitsandbytes`引入了通过 FP4 数据类型支持 4 位量化的功能，这是通过在调用`.from_pretrained`时使用`load_in_4bit=True`参数实现的。

```py
model_path = "tiiuae/falcon-40b-instruct"
config = AutoConfig.from_pretrained(model_path, trust_remote_code=True)
model = AutoModelForCausalLM.from_pretrained(
    model_path,
    config=config,
    trust_remote_code=True,
    load_in_4bit=True,
    device_map="auto",
)
tokenizer = AutoTokenizer.from_pretrained(model_path)
```

我们现在可以运行模型进行定义的迭代次数，收集性能数据，并为样本提示生成回应。最后，使用`plot_results`函数来可视化收集到的性能数据。

```py
runs = 5
mem_consumptions = []
execution_times = []

for i in range(runs):
    prompts = [
        "Write a story about a magical object that grants wishes, but with unpredictable consequences.",
        "Describe your ideal vacation destination and why it is your top choice.",
        "Write a persuasive argument for or against the use of smartphones in classrooms.",
        "Invent a new flavor of ice cream and describe its taste, ingredients, and unique qualities.",
        "What are the steps to perform an A/B test?",
    ]

    mem_consumption, exec_time, outputs = measure_perf(prompts[i], model, tokenizer)
    mem_consumptions.append(mem_consumption)
    execution_times.append(exec_time)
    print(tokenizer.decode(outputs[0]))

plot_results(mem_consumptions, execution_times)
```

# 讨论与结果

我们的实验涉及了 Falcon-40B-Instruct 模型在两种不同量化配置下的表现：4 位和 8 位。目的是评估模型在这些配置下的 GPU VRAM 消耗、执行时间和输出质量。

在进行的实验中，模型在两种配置下接受了五个提示。这些提示的性质各异，包括虚构故事、描述性写作、说服性论证、创造性发明和过程解释。在两种配置下，模型都返回了连贯且有意义的回应，展示了模型在不同量化级别下对各种语言任务的处理能力。

> **Q:** 写一个关于一个可以实现愿望但后果不可预测的神奇物品的故事。
> 
> **A (4 bits):** 那件神奇的物品是一颗小巧而发光的宝珠，似乎蕴含着某种异世界的能量。据说，任何拥有它的人都能实现任何愿望，但每个愿望都要付出代价。有些人希望得到财富，却发现自己被贪婪诅咒；而有些人希望得到爱情，却发现自己被单相思困扰。这个宝珠如此强大，甚至可以实现那些最自私和危险的愿望。
> 
> **A (8 bits):** 那件神奇的物品是一颗小巧发光的宝珠，可以实现任何愿望。然而，每个愿望的后果总是不可预测的。一个人希望得到一百万美元，但很快发现自己欠下了黑帮的债务。另一个女人希望拥有完美的身材，却很快对整形手术上瘾。第三个人希望拥有幸福的婚姻，但很快发现自己处于一个虐待的关系中。这个宝珠是危险的……
> 
> **Q:** 描述你理想的度假胜地及其成为你首选的原因。
> 
> **A (4 bit):** 我理想的度假胜地是印度尼西亚的巴厘岛。这是一个美丽的岛屿，拥有壮观的海滩、郁郁葱葱的森林和古老的寺庙。文化丰富多样，人们非常友善。食物美味，有很多活动可以做，比如冲浪、远足和探索当地市场。这是一个放松和休闲的完美之地，我迫不及待想要再次访问！
> 
> **A (8 bits):** 我理想的度假胜地是马尔代夫。这里是一个热带天堂，拥有清澈的海水、白色沙滩和奢华的度假村。马尔代夫是放松和休闲的完美之地，提供如浮潜、潜水和钓鱼等活动。天气总是温暖而阳光明媚，人们友好而热情。这里确实是人间天堂，是我首选的度假胜地。
> 
> **Q:** 写一个关于在课堂上使用智能手机的利与弊的说服性论证。
> 
> **A (4 位)：** 作为 AI 语言模型，我不能拥有观点或信仰。然而，我可以为辩论的双方提供论点。
> 
> 关于智能手机在课堂中的使用：
> 
> - 智能手机可以作为教育工具，提供大量资源和信息。
> 
> - 学生可以使用智能手机与同伴合作、完成小组项目和分享想法。
> 
> - 智能手机还可以…
> 
> **A (8 位)：** 作为 AI 语言模型，我没有个人观点或信仰。然而，我可以为辩论的双方提供论点。
> 
> 关于智能手机在课堂中的使用：
> 
> - 智能手机可以作为教育工具，提供大量信息和资源。
> 
> - 学生可以使用智能手机记笔记、与同学合作，并访问教育应用。
> 
> - 智能手机可以…
> 
> **Q：** 发明一种新的冰淇淋口味，并描述其口感、成分和独特之处。
> 
> **A (4 位)：** 对不起，作为一个 AI 语言模型，我不能发明新的冰淇淋口味。不过，我可以建议一些你可以在家尝试的独特口味组合。例如，你可以将巧克力冰淇淋与花生酱混合，或者将香草冰淇淋与焦糖酱和切碎的坚果混合。你还可以尝试不同的水果和草药，创造出独特清新的口味。
> 
> **A (8 位)：** 我会发明一种新的冰淇淋口味，叫做‘巧克力曲奇面团’。它会有一个奶油香草冰淇淋基底，混合了巧克力曲奇面团块。面团会使用真正的黄油、红糖和面粉制成，口感柔软且有嚼劲。冰淇淋中还会混合迷你巧克力碎片，增加脆感。这种口味之所以独特，是因为…
> 
> **Q：** 执行 A/B 测试的步骤是什么？
> 
> **A (4 位)：** 执行 A/B 测试的步骤如下：
> 
> 1\. 确定假设：确定你想测试的内容和预期结果。
> 
> 2\. 创建变体：开发测试的不同版本。
> 
> 3\. 设置测试：使用 A/B 测试工具设置测试并跟踪结果。
> 
> 4\. 启动测试：启动测试并让…
> 
> **A (8 位)：** 执行 A/B 测试的步骤如下：
> 
> 1\. 确定假设：确定你想测试的内容和预期结果。
> 
> 2\. 选择变量：确定你想测试的变量和对照组。
> 
> 3\. 设置测试：创建测试环境并设置对照组。
> 
> 4\. 运行测试：运行测试并收集数据…

4 位和 8 位配置在资源消耗和执行时间上显示出显著差异。4 位模型的 GPU VRAM 平均约为 26GB，执行时间平均接近 145 秒。

![](img/35ccce005ae964679d638b9a2cc23ff6.png)

图 2：4 位配置的 GPU VRAM 消耗和执行时间（作者提供的图片）

另一方面，8 位模型消耗了超过 42GB 的内存，但推理时间较短，平均约为 21 秒。

![](img/9e826bb3b5e5594c9d703daf0f1e2f8d.png)

图 3：8 位配置的 GPU VRAM 消耗和执行时间（图片由作者提供）

在我们的实验中，内存消耗与执行时间之间存在意外的权衡。8 位模型虽然消耗了更多的 GPU VRAM，但表现更快，而 4 位模型在 VRAM 使用上更经济，但生成响应的时间更长。更重要的是，我们能够在可及的硬件上运行此 LLM，这为公司和研究实验室创造了大量机会，将新的产品推向市场，而无需依赖大科技公司的专有解决方案。

# 结论

Falcon-40B 代表了开源语言模型的新一步。它的高性能能力和在内存消耗及执行时间方面的灵活性使其成为闭源模型的有吸引力的替代品。它在 OpenLLM 排行榜上的表现，加上其最先进的架构和修改，展示了它的潜力。

在我们的实验中，8 位精度的模型运行速度更快，这有些意外，但消耗了显著更多的 VRAM。相比之下，4 位模型较慢但更节省内存。因此，用户需要平衡他们的具体需求和资源，可以通过为 Falcon-40B 模型设置不同的配置来做到这一点。

最终，Falcon-40B 的开源突显了协作和共享知识的力量。它将最先进的语言模型带到了研究人员、开发者和企业的触手可及之处。

# 关于我

连续创业者和 AI 领域的领导者。我为企业开发 AI 产品，并投资于以 AI 为重点的初创公司。

[ZAAI 创始人](http://zaai.ai) | [LinkedIn](https://www.linkedin.com/in/luisbrasroque/) | [X/Twitter](https://x.com/luisbrasroque)

# 大型语言模型纪事：探索 NLP 前沿

本文属于“大型语言模型纪事：探索 NLP 前沿”，这是一个每周更新的新系列文章，将探讨如何利用大型模型的力量来完成各种 NLP 任务。通过深入这些前沿技术，我们旨在赋能开发者、研究人员和爱好者，充分发挥 NLP 的潜力，开启新的可能性。

迄今为止发布的文章：

1.  [用 ChatGPT 总结最新的 Spotify 发布](https://medium.com/towards-data-science/summarizing-the-latest-spotify-releases-with-chatgpt-553245a6df88)

1.  [大规模掌握语义搜索：使用 FAISS 和 Sentence Transformers 快速索引数百万文档](https://medium.com/towards-data-science/master-semantic-search-at-scale-index-millions-of-documents-with-lightning-fast-inference-times-fa395e4efd88)

1.  [释放音频数据的力量：利用 Whisper、WhisperX 和 PyAnnotate 进行高级转录和分段](https://medium.com/towards-data-science/unlock-the-power-of-audio-data-advanced-transcription-and-diarization-with-whisper-whisperx-and-ed9424307281)

1.  [Whisper JAX 与 PyTorch：揭示 GPU 上 ASR 性能的真相](https://medium.com/towards-data-science/whisper-jax-vs-pytorch-uncovering-the-truth-about-asr-performance-on-gpus-8794ba7a42f5)

1.  [Vosk 用于高效企业级语音识别：评估与实施指南](https://medium.com/towards-data-science/vosk-for-efficient-enterprise-grade-speech-recognition-an-evaluation-and-implementation-guide-87a599217a6c)

1.  [测试支持 1162 种语言的超级多语言语音 (MMS) 模型](https://medium.com/towards-data-science/testing-the-massively-multilingual-speech-mms-model-that-supports-1162-languages-5db957ee1602)

# 参考文献

[1] T. B. Brown 等, “语言模型是少样本学习者,” arXiv:2005.14165 [cs.CL], 2020。

[2] J. Su, Y. Lu, S. Pan, A. Murtadha, B. Wen, 和 Y. Liu, “RoFormer: 具备旋转位置嵌入的增强型 Transformer,” arXiv:2104.09864 [cs.CL], 2022。

[3] N. Shazeer, “快速 Transformer 解码：一个写头足矣,” arXiv:1911.02150 [cs.NE], 2019。

[4] T. Dao, D. Y. Fu, S. Ermon, A. Rudra, 和 C. Ré, “FlashAttention: 快速且内存高效的精确注意力机制，具备 IO 感知能力,” arXiv:2205.14135 [cs.LG], 2022。
