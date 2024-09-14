# 学习 Transformers 代码优先：第一部分 — 设置

> 原文：[`towardsdatascience.com/nanogpt-learning-transformers-code-first-part-1-f2044cf5bca0`](https://towardsdatascience.com/nanogpt-learning-transformers-code-first-part-1-f2044cf5bca0)

## 使用 nanoGPT 作为起点的 4 部分 Transformers 探索

[](https://medium.com/@oaguy1?source=post_page-----f2044cf5bca0--------------------------------)![Lily Hughes-Robinson](https://medium.com/@oaguy1?source=post_page-----f2044cf5bca0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f2044cf5bca0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f2044cf5bca0--------------------------------) [Lily Hughes-Robinson](https://medium.com/@oaguy1?source=post_page-----f2044cf5bca0--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f2044cf5bca0--------------------------------) ·阅读时间 8 分钟·2023 年 7 月 7 日

--

![](img/dfca366786f1d736d9f6aebbeedc4b65.png)

图片由 [Josh Riemer](https://unsplash.com/@joshriemer?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

不知道你是否和我一样，有时查看代码比阅读论文更简单。当我在开发 [AdventureGPT](https://github.com/oaguy1/AdventureGPT) 时，我首先阅读了 [BabyAGI](https://github.com/yoheinakajima/babyagi) 的源代码，这是一种用大约 600 行 Python 实现的 [ReAct 论文](https://arxiv.org/abs/2210.03629)。

最近，我通过优秀的 [Cognitive Revolution Podcast](https://www.cognitiverevolution.ai/) 的 [第 33 集](https://www.cognitiverevolution.ai/e33-the-tiny-model-revolution-with-ronen-eldan-and-yuanzhi-li-of-microsoft-research/) 了解到了一篇名为 [TinyStories](https://arxiv.org/abs/2305.07759) 的最新论文。TinyStories 尝试展示经过数百万（而不是数十亿）参数训练的模型，在足够高质量的数据下也能有效。在论文中的微软研究人员的案例中，他们使用了从 GPT-3.5 和 GPT-4 生成的合成数据，这些数据生成的零售成本大约为 $10k。数据集和模型可以从作者的 [HuggingFace repo](https://huggingface.co/roneneldan) 获取。

听到一个模型可以在 30M 及更少参数上进行训练，我感到很着迷。作为参考，我在搭载 GTX 1660 Ti 的 Lenovo Legion 5 笔记本上进行所有模型训练和推理。即使仅仅是推理，大多数拥有超过 3B 参数的模型也太大，无法在我的机器上运行。我知道有付费的云计算资源，但我是在空闲时间学习这些内容，实际上只能负担得起通过 API 调用产生的适度 OpenAI 账单。因此，能够在我的普通硬件上训练模型的想法让我立刻振奋起来。

我开始阅读 TinyStories 论文，很快意识到他们在模型训练中使用了已经停用的[GPT Neo](https://github.com/EleutherAI/gpt-neo)模型。我开始深入研究代码，看看是否能理解它，并意识到我需要从更小的东西开始。为了提供背景，我主要是一名后端软件工程师，具有足够的机器学习经验以便在听到别人谈论神经网络时不会完全迷失。我离一个真正的 ML 工程师还很远，这使我在搜索引擎中输入了“gpt from scratch”以寻找更温和的入门介绍。我找到下面的视频，一切都发生了变化。

这是我一直在寻找的。除了视频中链接的基本代码库，还有一个名为[nanoGPT](https://github.com/karpathy/nanoGPT)的精简版本，目前仍在积极开发中。更重要的是，**训练代码和模型代码每个大约有 300 行 python**。对我来说，这比视频更令人兴奋。我关闭了视频，开始仔细研究源代码。nanoGPT 利用了我从未使用过的 PyTorch。它还涉及了足够的数学和机器学习术语，让我这位新手感到紧张。这将是一个比我预期更大的工程。

理解某件事的最佳方法之一是写下来。因此，我计划深入研究 nanoGPT 代码库中的代码，阅读著名的“[Attention is All You Need](https://arxiv.org/abs/1706.03762)”论文，并以自下而上的实践方式学习变换器。无论我在这个过程中学到什么，我都希望在这个系列中写下来。如果你想跟随学习，可以将 nanoGPT 代码库克隆到你的机器上（模型甚至可以在 CPU 上训练，所以没有硬件借口），并进行跟随。

克隆代码库后，我做的第一件事是按照 README 的指示训练最简单的模型，即使用[tiny_shakespeare 数据集](https://huggingface.co/datasets/tiny_shakespeare)的字符级生成模型。这里有一个脚本用于准备训练数据，一个脚本用于实际训练，还有一个采样脚本用于输出生成的文本。通过几个终端命令和一个多小时的训练，我得到一个能够输出类似莎士比亚风格文本的简单模型。

遵循说明是好的，但我直到将其修改为适合自己的用例后才真正理解某些东西。我的目标是使用 TinyStories 数据集训练一个类似的字符级模型。这需要创建我自己的数据准备脚本，以使数据集准备好进行训练。让我们*深入探讨*一下这个问题。

nanoGPT 有两种类型的数据准备脚本：一种用于 GPT-2 风格模型，一种用于字符级模型。我从 HuggingFace 仓库下载的 GPT-2 模型中提取了一些代码，其余的都来自 tiny_shakespeare 字符级脚本。这里一个重要的点是，tiny_shakespeare 仅有 1MB 多一点，仅包含 40k 行莎士比亚的作品。而 TinyStories 压缩后超过 3GB，包含 39.7M 个故事。对 tiny_shakespeare 进行分词和切片的方法不能直接转移，至少在我的笔记本电脑上（拥有 32GB RAM）是这样。我在尝试 pythonic、易读的数据准备方法时多次崩溃了我的机器。最终脚本使用了一些技巧，我将在下面详细说明。

首先，我处理数据列表的首选方案是 [列表推导式](https://realpython.com/list-comprehension-python/)，这是一种从现有列表生成新列表并进行修改的语法。在这种情况下，列表推导式的问题是，3GB 的压缩文本在 RAM 中变得接近 10GB。现在，列表推导式需要在 RAM 中多次复制列表。对于小数据不是问题，但对 TinyStories 来说不可行。

数据准备脚本的输出是一个压缩的 NumPy 数组，包含训练和验证数据的字符级编码，以及一个包含唯一字符完整列表和编码/解码映射的元数据 pickle，以将这些字符转换为数字。以此为参考，一旦找到并映射到数字，我们不需要其他任何东西，除了最终的编码数字数组。最有效的内存使用方法是通过简单的 for 循环迭代数据，同时分段构建这些输出。为此，在循环前初始化一个变量，然后在每次交互中更新。这样可以防止在 RAM 中保存数据集的多个版本，只输出我们需要的内容。最终的词汇生成代码如下：

```py
chars_dataset = set([])
len_dataset = 0

# get all the unique characters that occur in this text as well as total length for training data
desc = "Enumerate characters in training set"
for story in tqdm(dataset['train']['text'], desc):
    chars = list(set(story))

    for char in chars:
        chars_dataset.add(char)

    len_dataset += len(story)
```

也就是说，将 30.7M 个故事（超过 40 亿字符）编码为数字的数组仍然占用大量 RAM，因为 Python 是动态存储整数的。这里引入 NumPy，它具有更高效的数组存储，可以指定整数的确切大小。除了高效的存储，NumPy 还有内存高效的数组连接，可以用于逐步构建最终的编码数组，而不是一次性完成。

我对脚本的最后修饰是使用 [tqdm](https://pypi.org/project/tqdm/) 为每一步添加一个进度条，最后我准备好运行脚本。所以，我让它过夜运行，早上回来时，脚本仍在运行，估计剩余计算时间超过 100 小时。

这时我真正意识到：30.7M 的故事对于语言模型来说虽然小巧，但绝对不是一个可以在单线程上处理的玩具数据集。是时候引入大招：并行化了。并行化带来了许多复杂性和开销，但性能提升是值得的。幸运的是，有许多方法可以并行化 Python 代码。许多解决方案需要对串行执行的脚本进行重大重写或复杂的抽象。经过一番挖掘，我找到了一种方法，让我可以保持大部分脚本不变，但仍然运行多个进程以利用所有线程。

[Ray](https://www.ray.io) 是一个可以轻松地将 Python 方法并行化的库，可以在本地或集群中运行。它处理任务队列中的任务并启动工作进程来处理队列。如果这引起了你的兴趣，下面有一个很好的 ray 指南。

[](/modern-parallel-and-distributed-python-a-quick-tutorial-on-ray-99f8d70369b8?source=post_page-----f2044cf5bca0--------------------------------) ## 现代并行和分布式 Python：Ray 的快速教程

### Ray 是一个用于并行和分布式 Python 的开源项目。

towardsdatascience.com

在选择并行化内容时，encode 函数似乎是一个不错的候选者。它有明确的输入和输出，对这些输入没有副作用，而且是计算时间中最大的一部分之一。将现有代码适配 ray 变得非常简单：通过装饰器使函数对 ray 可用，功能调用稍微更改以添加远程属性，并有一个函数来启动所有数据的执行。以下是最初在我的代码库中呈现的样子：

```py
import ray

ray.init()

…

# given all the unique characters within a dataset, 
# create a unique mapping of characters to ints
stoi = { ch:i for i,ch in enumerate(chars_dataset) }

@ray.remote
def encode(s):
    return [stoi[c] for c in s]

…

encoded_stories = []
for story in dataset[‘train’][‘text’]:
    encoded_stories.append(encode.remote(story))

ray.get(encoded_stories)

…
```

拥有了所有 CPU 的力量，我继续前进，却立即导致了我的笔记本电脑崩溃。由于 ray 使用的本地分布式调用栈，整个数据集在内存中存在了几次。仅仅将整个数据集放入队列就导致了内存不足错误。我感到恼火，借此机会买了更多的 RAM（64GB，来了！），但在 RAM 发货期间继续调整代码。

下一个合乎逻辑的步骤是将 ray 处理的请求批量化成适合合理内存的大小。添加批量逻辑相对简单，并且在最终的代码库中有体现，链接将在文章末尾提供。实际上，令人感兴趣的是实验批量大小。一开始，我选择了一个随机的批量大小（5000），效果不错，但我很快意识到在每个批次中，单线程代码占用了相当多的时间。

实际上，在观察我首选的系统监视器时，我看到一个核心被占用数分钟，最后我所有笔记本的核心都亮起了几秒钟，然后又回到只有一个核心被使用。这使我尝试调整批量大小，希望能更快地喂养那些饥饿的 CPU 核心，并让它们保持更长时间的活动。减少批量大小没有帮助，因为每批中的同步代码用于从完整数据集中切分和准备一个批次。这些代码无法并行化，因此每个批次生成数据块的启动成本时间较大。这使我尝试了相反的方法，即增加数据块大小，以使核心保持更长时间的活跃。这有效，因为数据块生成的时间不论数据块大小如何都相同，但每个数据块处理了更多的数据。结合将编码后处理移到 ray 函数中，我能够在短短几个小时内处理 30%的训练数据集，全部在一台笔记本电脑上完成。

最终，在再过几个小时后，我拥有了一个完全准备好的自定义数据集，以供字符级模型使用。我很高兴我不需要求助于昂贵的云计算来处理训练集，如果 RAM 增加不起作用，这将是我的下一个步骤。更重要的是，我深入了解了为字符级模型创建/处理数据集的含义。

在本系列的下一篇文章中，我将检查实际的模型代码，尽我所能地进行解释，并链接大量外部资源以提供额外的信息，以弥补我的知识不足。一旦文章写好，我将回来在这里提供一个链接。与此同时，我已经在下面链接了我数据集准备脚本的最终版本，您可以跟随并了解在有限计算平台上处理较大数据集所需的内容。

[](https://github.com/oaguy1/nanoGPT/blob/master/data/tinystories_char/prepare.py?source=post_page-----f2044cf5bca0--------------------------------) [## nanoGPT/data/tinystories_char/prepare.py at master · oaguy1/nanoGPT

### 最简单、最快速的中型 GPT 训练/微调的代码库。 - nanoGPT/data/tinystories_char/prepare.py…

github.com](https://github.com/oaguy1/nanoGPT/blob/master/data/tinystories_char/prepare.py?source=post_page-----f2044cf5bca0--------------------------------)

此外，系列的第二部分已经上线！点击下面阅读！

[](/learning-transformers-code-first-part-2-gpt-up-close-and-personal-1635b52ae0d7?source=post_page-----f2044cf5bca0--------------------------------) ## 学习变换器代码第一部分 — GPT 近距离了解

### 深入研究通过 nanoGPT 生成预训练变换器

[towardsdatascience.com
