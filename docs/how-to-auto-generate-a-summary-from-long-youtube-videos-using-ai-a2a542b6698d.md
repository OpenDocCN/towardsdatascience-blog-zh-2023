# 如何使用 AI 自动生成长时间 YouTube 视频的摘要

> 原文：[`towardsdatascience.com/how-to-auto-generate-a-summary-from-long-youtube-videos-using-ai-a2a542b6698d`](https://towardsdatascience.com/how-to-auto-generate-a-summary-from-long-youtube-videos-using-ai-a2a542b6698d)

## 使用 Whisper 和 BART 模型在本地 PC 上总结斯蒂芬·沃尔夫勒姆的演讲的逐步指南

[](https://medium.com/@anna.bildea?source=post_page-----a2a542b6698d--------------------------------)![Ana Bildea, PhD](https://medium.com/@anna.bildea?source=post_page-----a2a542b6698d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a2a542b6698d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a2a542b6698d--------------------------------) [Ana Bildea, PhD](https://medium.com/@anna.bildea?source=post_page-----a2a542b6698d--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----a2a542b6698d--------------------------------) ·7 分钟阅读·2023 年 4 月 14 日

--

![](img/73936710bcb5eeef18ac5b7cbe3fa600.png)

作者生成的图像

# 动机

在当今快速变化的世界中，保持信息更新和获得灵感可能是一项挑战，尤其是当时间紧迫时。就个人而言，我非常喜欢 YouTube 上的播客和演讲。这些播客和演讲是知识的宝藏，充满了来自各个领域顶尖人才的见解。然而，由于时间限制，我无法观看每一个有趣的视频，因为它们通常超过一小时。这让我开始思考：如果我能创建一个端到端的解决方案来自动提取主要亮点呢？因此，我开始探索 AI 生成解决方案，帮助我获取一些错过的播客/演讲的自动摘要。

在本文中，我讨论了在本地 PC 上进行端到端解决方案。首先，我将介绍如何使用开源的[Whisper 模型](https://huggingface.co/openai/whisper-medium)，对[斯蒂芬·沃尔夫勒姆关于 ChatGPT、AI 和 AGI 的演讲](https://www.youtube.com/watch?v=szxiPMyuMGY)进行转录，该演讲在 YouTube 上可用。接着，我将演示如何使用开源的[BART](https://arxiv.org/abs/1910.13461)模型总结长文本。

让我们看看如何实现这个目标。

> 请记住，确保在下载内容之前核实版权/许可是否允许下载是至关重要的。

# 一点背景

> [**Whisper**](https://cdn.openai.com/papers/whisper.pdf)是一个开源自动语音识别模型，基于从互联网收集的 680,000 小时多语言数据进行训练。它依赖于端到端的编码器-解码器[Transformer](https://arxiv.org/abs/1706.03762)架构。
> 
> [**BART**](https://arxiv.org/abs/1910.13461) 是一个基于 Transformer 的 seq2seq 模型，结合了双向（BERT 风格）编码器和自回归（GPT 风格）解码器。它通过随机添加噪声并学习重建原始内容进行预训练，在总结和翻译等任务上表现良好。
> 
> [**HuggingFace transformers**](https://huggingface.co/docs/transformers/index) 库提供了一个用户友好的解决方案来使用和自定义模型。此外，它还提供了可以用于微调模型以更好地适应数据的 API。
> 
> [**PyTube**](https://github.com/pytube/pytube) 是一个无需依赖的 Python 库，用于下载和流式传输 YouTube 视频。
> 
> [**NLTK**](https://www.nltk.org/)是一个标准的自然语言处理（NLP）任务的 Python 库。

# 端到端过程

过程包含四个主要步骤：

> 1\. 设置环境
> 
> 2\. 下载 YouTube 视频：PyTube
> 
> 3\. 转录音频：Whisper
> 
> 4\. 总结生成的文本：BART

![](img/89c729e6ad301d77e5fd22684596eeaa.png)

图片由作者提供

# 1\. 设置环境

我的环境设置如下：

+   **Jupyter Notebook** 在一个使用 Python 3.10 的虚拟环境中运行

+   **模型**： [OpenAI Whisper](https://github.com/openai/whisper), [BART](https://huggingface.co/facebook/bart-large-cnn)

+   **库**： [pytube](https://pypi.org/project/pytube/), [transformers](https://huggingface.co/docs/transformers/index), [unstructured](https://pypi.org/project/unstructured/), ffmpeg-python

## 1.1 安装库

一些备注：

**👉** 请注意，只有在从笔记本单元格安装库时才需要 `!`。

**👉** 直接从 GitHub 安装 Whisper 模型的最新更新。

**👉 解决 PyTube 问题。** 如果遇到以下错误 `"pytube: AttributeError: ‘NoneType’ object has no attribute ‘span’ cipher.p"`，请前往 `{home}/.local/lib/{your_pythonversion: ex. python3.10}/site-packages/pytube/cipher.py Line 411` 并将 `transform_plan_raw` 变量的值替换如下：

## 1.2 导入库

# 1\. 下载 YouTube 视频

让我们获取以下演讲的总结“[*ChatGPT, AI, and AGI with Stephen Wolfram*](https://www.youtube.com/watch?v=szxiPMyuMGY) *(Wolfram Research 创始人兼首席执行官)*” *在* YouTube 上可用 ([创作共享许可（允许重用）](https://www.youtube.com/t/creative_commons))。

要将视频本地下载为音频文件，我们使用 PyTube 库的 YouTube 类。确保提供有效的 URL。

# 2\. 转录音频

一旦我们将音频下载到本地，我们应该会看到一个名为 `demo.mp3` 的文件。要转录音频，我们加载 `medium Whisper multilingual model`，它具有 7.69 亿个参数，并提供英语或多语言格式。您可以查看[可用语言模型列表](https://github.com/openai/whisper#available-models-and-languages)，选择最适合您设置的模型。为了更高的准确性，您可以使用 `large Whisper multilingual model`。

合并后的字符串将存储在 `result[‘text’]` 变量中，并保存在本地的 `demo.txt` 文件中。

❗️ 需要注意的是，转录过程可能需要超过一个小时，具体取决于您的电脑配置。要测试演示，您可以选择一个较短的视频。

# 3\. 总结生成的文本

由于模型无法一次处理多个标记，因此重要的是将文本分割成较小的段落，每个段落最多包含 4000 个标记。为此，我们可以使用 `punkt` 预训练句子分割模型，它是自然语言工具包（NLTK）库的一部分，能够有效处理自然语言。一旦我们将文本分割成较小的句子块，就可以将它们存储在 `text_chunks` 变量中以供进一步使用。

我们使用句子分割以防止信息丢失

## 3.1 将大文本分割成块

这里是可以用来完成工作的代码。

代码包括两个函数：`read_file()` 用于读取 `demo.txt` 文件，`split_text_into_chunks()` 用于将文本分割成块。

## 3.2 使用 BART 进行文本摘要

为了总结文本，我们使用 `HuggingFace Transformers` 库和预训练的多语言 BART-large 模型，`[facebook/bart-large-cnn](https://huggingface.co/facebook/bart-large-cnn)`，该模型在 CNN Daily Mail 数据集上进行了微调。Hugging Face 的 Transformers 库提供了许多用于文本、图像或声音等各种任务的现成模型。例如，它提供了一个易于使用的 BART 模型文本摘要管道：`pipeline("summarization", model="facebook/bart-large-cnn")`。这使得使用起来简单而友好。

以下是执行摘要生成的代码。

总体而言，代码创建了 BART 摘要生成器的一个实例，为给定的文本块生成摘要，并仅在成功生成摘要时将其保存到 `summary_demo.txt` 文件中。如果摘要超过 5000 个字符，我们将再次使用 BART 摘要生成器。输出将保存在 `short_summary_demo.txt` 文件中。

**这是总结：**

> Wolfram 语言可能成为更系统探索大型语言模型本质和深度的基础。这是一种精确的计算语言，但它谈论的是现实世界。LLM 中没有太多的模板代码。我认为 Chat GPT 向我们展示了一个重要的科学片段。我们已经自动化了模板代码。我的猜测是，随着人们越来越多地实际使用它，他们只会编辑代码。而它将完成大量的初始五行代码的工作。描述意义的规律性更多了。这实际上是一个问题，即 LLM 可以生成的内容与我们自然语言理解系统可以捕捉的内容之间的界限在哪里。我们已经有数十亿年的进化时间来处理自然的方式。微软研究院发布了一份关于 GPT-4 的 154 页分析报告，在报告中他们得出结论，并且这是他们论文标题中的内容，他们看到了 AGI 的一瞥。你可以做的计算宇宙的可能性是非常大的。我们人类只关心其中的一小部分。问题在于将计算宇宙中的事物与我们人类感兴趣的事物连接起来。在 1900 年，人们不会感到惊讶地认为空间是离散的。我希望在不久的将来，我们实际上能找到一种类似于空间布朗运动的现象，我们将能够看到，我们可以确定它是离散的。

## 主要收获

该教程是一个个人副项目的一部分，专注于探索生成性 AI 工具。

总结一下，Whisper 模型在所有测试的视频中都表现出色。尽管它偶尔会误识别产品或人物名称，但我对结果相当满意，并且会继续使用它。

另一方面，BART 模型提供了一个值得信赖的开源摘要选项。它的摘要效果相当好。我将它与 [谷歌研究的 T5 模型](https://huggingface.co/docs/transformers/model_doc/t5) 进行了比较，BART 的摘要更优。的确，它可能并不总是捕捉到所有关键事实，但它的结果很好，所以我会继续使用它来处理我的个人总结任务。

总体而言，像 Whisper 和 BART 这样的 AI 生成解决方案帮助我高效提取长时间播客和演讲中的重要见解。这样，即使在我没有剩余时间的时候，我也能保持信息更新。

我希望你喜欢这篇文章。

# 感谢阅读！

如果你想将来在收件箱中收到我的故事，不要忘记 [订阅](https://medium.com/subscribe/@anna.bildea)。

*如果你喜欢阅读我的故事并希望支持我作为作家，可以考虑注册成为 Medium 会员，访问数千篇数据工程和数据科学文章。*

[](https://medium.com/@anna.bildea/membership?source=post_page-----a2a542b6698d--------------------------------) [## 使用我的推荐链接加入 Medium — Bildea Ana

### 作为 Medium 会员，您会员费的一部分将分配给您阅读的作者，您将获得对每篇故事的完整访问权限…

medium.com](https://medium.com/@anna.bildea/membership?source=post_page-----a2a542b6698d--------------------------------)

*在* [*LinkedIn*](https://www.linkedin.com/in/ana-bildea-phd-2339b728/) *和* [Twitter](https://twitter.com/AnaBildea) *上找到我*!

查看我关于生成性 AI、MLOps 和负责任 AI 的文章合集

![Ana Bildea 博士](img/acaa243e5f1e9f9254c32b65042c822b.png)

[Ana Bildea 博士](https://medium.com/@anna.bildea?source=post_page-----a2a542b6698d--------------------------------)

## 生成性 AI

[查看列表](https://medium.com/@anna.bildea/list/generative-ai-30d313b29b80?source=post_page-----a2a542b6698d--------------------------------)11 个故事![](img/df56a4e1a4785f73007a1ba8d1191b78.png)![](img/b6a7ab27a61a2cd49de8c07ee38f5999.png)![](img/8c3c51cf26b3db2c54205da85ad9fe2e.png)![Ana Bildea 博士](img/acaa243e5f1e9f9254c32b65042c822b.png)

[Ana Bildea 博士](https://medium.com/@anna.bildea?source=post_page-----a2a542b6698d--------------------------------)

## 负责任的 AI

[查看列表](https://medium.com/@anna.bildea/list/responsible-ai-10009e82f412?source=post_page-----a2a542b6698d--------------------------------)1 个故事![](img/46a362cef2c3ddcc9e9a1134400f8a6d.png)![Ana Bildea 博士](img/acaa243e5f1e9f9254c32b65042c822b.png)

[Ana Bildea 博士](https://medium.com/@anna.bildea?source=post_page-----a2a542b6698d--------------------------------)

## MLOps - AI 在生产中的应用

[查看列表](https://medium.com/@anna.bildea/list/mlops-ai-in-production-04b6c81c50c8?source=post_page-----a2a542b6698d--------------------------------)4 个故事![](img/8fbedcb9f3f75894caff649172adece1.png)![](img/d5014b3b3843fc4b2172bef517cccaa4.png)![](img/2dba051abf51711268415c3f1e055a60.png)
