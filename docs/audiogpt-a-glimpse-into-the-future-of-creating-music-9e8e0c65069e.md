# AudioGPT — 探索未来音乐创作的前景

> 原文：[`towardsdatascience.com/audiogpt-a-glimpse-into-the-future-of-creating-music-9e8e0c65069e`](https://towardsdatascience.com/audiogpt-a-glimpse-into-the-future-of-creating-music-9e8e0c65069e)

## 研究论文分析和解释

[](https://medium.com/@maxhilsdorf?source=post_page-----9e8e0c65069e--------------------------------)![Max Hilsdorf](https://medium.com/@maxhilsdorf?source=post_page-----9e8e0c65069e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9e8e0c65069e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9e8e0c65069e--------------------------------) [Max Hilsdorf](https://medium.com/@maxhilsdorf?source=post_page-----9e8e0c65069e--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9e8e0c65069e--------------------------------) ·阅读时间 10 分钟·2023 年 5 月 2 日

--

![](img/7c61267fd6c17c0ecfb42939253873df.png)

图片来自 [Pixabay](https://www.pexels.com/de-de/foto/low-angle-view-von-beleuchtungsgeraten-im-regal-257904/)。

随着 [MusicLM](https://google-research.github.io/seanet/musiclm/examples/) 于 2023 年 1 月发布，音乐家和数据科学家都清楚了：AI 正在颠覆我们制作音乐的方式。就在几天前，下一代大型音频 AI 模型发布了。在这篇文章中，我们将探讨为什么我认为 **这个模型可能是音乐制作重大技术变革的基础**。

本文解决了以下问题：

+   什么是 AudioGPT？

+   它是如何工作的？

+   它的能力和限制是什么？

+   这对未来的音乐制作意味着什么？

# 什么是 AudioGPT？

AudioGPT 是由一群中美研究人员于 2023 年 4 月发布的研究项目 **[1]**。但它实际做了什么，它与 GPT 模型有什么关系？好吧，让我们问问它吧！

![](img/e7bbdf3f190c060c8b60a5e39a454a8a.png)

AudioGPT 回答了“什么是 AudioGPT？”的问题。图像由作者提供。

## AudioGPT 是一个对话助手

从截图中可以看出，AudioGPT 可以在类似于 ChatGPT 的聊天界面中使用。实际上，对于大多数对话应用，它的工作方式与 ChatGPT 完全相同。AudioGPT 的一个独特功能是，除了文本外，聊天机器人还可以处理语音输入，先将音频转录为文本。因此，这是真正的对话助手，你可以根据需求与之对话或书写。

## AudioGPT 能够执行各种音频任务

AudioGPT 的对话能力仅仅是一个支持功能。它的真正目的在于提供一个统一的体验来解决音频分析和生成领域中的多种任务。以下是它可以处理的一些任务的精选：

+   **音频标题：** 使用文本描述音频信号的内容

+   **源分离：** 将音频信号拆分成不同的事件（声音、噪音等）

+   **图像转音频：** 生成符合图像内容的音频

+   **分数转音频：** 根据文本、音符和音符时值生成歌声

+   **更多任务** 我们稍后会讨论！

有趣的是，与 ChatGPT 相比，AudioGPT 能够接收和发送音频文件。例如，当我要求 AudioGPT 为我生成特定的声音时，它创建了这些声音，将其导出为 wav 文件，并将导出文件的位置发送给我。

![](img/62aaf175cdf853d3ea775325c0371fce.png)

AudioGPT 被要求生成“狮子吼叫的声音，背景有赛车声”。图片由作者提供。

在我们能够理解这项技术对未来音乐创作的意义之前，让我首先向你展示 AudioGPT 实际上是如何工作的，以及它的优势和局限性。

# AudioGPT 是如何实现的？

尽管 AudioGPT 对用户来说可能像一个典型的 AI 聊天机器人，但实际上在幕后还有更多复杂的工作。事实上，聊天机器人 AI（ChatGPT）仅作为用户请求与其他 AI 模型之间的翻译器。这种方法在图像（[TaskMatrix](https://github.com/microsoft/TaskMatrix)）或文本（[LangChain](https://github.com/hwchase17/langchain)）等其他领域已经存在。让我们看看作者在他们的论文中提供的 AudioGPT 工作流程的插图 **[1]**。

![](img/1e2355352532863074311bf69920068d.png)

AudioGPT 的内部工作流程。图片来源于 Huang, Li, Yang, Shit 等（2023）**[1]**。

如你所见，工作流程被分为四个不同的步骤。让我们简要地了解一下所有步骤。

## 步骤 1：模态转换

AudioGPT 被设计用于处理语音和文本输入。因此，第一步是检查用户是通过文本还是语音与系统互动。如果输入是语音，则由类似于 Alexa 或 Siri 的语音识别系统将其转录并转换为文本。对用户而言，这个转换步骤应该是无缝的。

## 步骤 2：任务分析

在这个文本输入下，ChatGPT 接管并尝试理解用户的请求。无论你说“生成一个雷声效果的 wav 文件”还是“给我一个雷声”：ChatGPT 擅长理解相同问题的不同表述，并将请求映射到特定任务——在这种情况下是文本到音频的声音生成。

## 步骤 3：模型分配

一旦 ChatGPT 理解了请求，它会从系统中目前包含的 17 个模型中选择一个合适的模型。这 17 个模型中的每一个都以非常特定的方式处理特定任务。因此，ChatGPT 理解请求、找到正确模型并以模型可以处理的方式呈现用户请求至关重要。

## 步骤 4：响应生成

一旦找到并运行了合适的模型，它将生成一个输出。这个输出可以有各种不同的形式（音频、文本、图像、视频）。这时 ChatGPT 再次发挥作用。它收集模型输出并以用户可以理解和解释的方式展示给用户。例如，文本输出可能直接传递给用户，而音频输出将被导出，用户将收到一个指向导出音频的文件路径。

## 内存与聊天历史

解决一个任务是很棒的。然而，这种聊天机器人方法真正突出的地方在于，AudioGPT 可以查看整个对话历史。这意味着你总是可以引用之前对话中的请求、问题或输出，并让 AudioGPT 对其进行操作。在某种意义上，它感觉就像 ChatGPT，但具有接收和发送音频文件的能力。

# AudioGPT 能做些什么？

在这一部分，我想给你一些来自论文的例子，展示 AudioGPT 能做什么。当然，这不是一个全面的列表，而只是一些有趣的亮点。

## 图像到音频生成

![](img/4272971c06721ab8d1fd67d09a2c5f73.png)

图像到音频生成示例来自 Huang, Li, Yang, Shit 等（2023）[1]。

在这个例子中，AudioGPT 被要求生成与猫的图像相匹配的音频。系统随后会响应导出音频文件的位置和音频波形的可视化。我们不能听到这篇论文中的示例，但响应很可能是类似于猫的嘶嘶声或咕噜声。在后台，首先对图像进行描述，然后将图像描述合成成音频信号。这对于音乐家来说可能非常有帮助，只需输入他们所寻找的图像，就可以为音乐创建样本。

## 歌唱声音生成

![](img/401d50f22b98b666f04bd771b834ca36.png)

歌唱声音生成示例来自 Huang, Li, Yang, Shit 等（2023）[1]。

现在，这对于音乐家来说非常相关！如果我们给模型提供文本以及音符和音符时长的信息，它会合成一个歌声并将音频发送给你。在后台，应用了最先进的语音合成模型（DiffSinger **[2]**，VISinger **[3]**）。很容易想象，这种技术可以直接应用在数字音频工作站（DAW）中，例如，为嘻哈节拍或甚至背景人声创建歌唱样本。

## 声音提取

![](img/aa8d99f0e2ebf1bfffe5c27bbfb46275.png)

声音提取示例来自 Huang, Li, Yang, Shit 等（2023）[1]。

基于文本提示，AudioGPT 识别音频信号中发生特定事件的位置，并为用户剪切掉无关的音频部分。仅使用语言提示来剪切样本或声音可能对音乐家极为有用。我们可能很快就能告诉我们的 DAW“提取这个样本中最具情感的部分并将其剪切为一小节”，而无需自己做任何机械工作。

## 来源分离

![](img/118bac72ba99073edaac2653d83bc4fd.png)

来源分离示例来自 Huang, Li, Yang, Shit 等（2023）[1]。

在这里，要求 AudioGPT 从音频信号中分离出两个说话者，并分别返回这两个提取出的说话者。目前，该系统中没有包括音乐源分离工具。然而，可以很容易地想象，我们可以通过聊天机器人界面在我们的 DAW 中直接提取特定的乐器或乐器组。

# AudioGPT 的局限性是什么？

尽管这些例子展示了 AudioGPT 如何为未来的突破性技术奠定基础，但它仍然有很多局限性。

## 它不是为音乐而构建的

在本帖的背景下，需要注意的是，AudioGPT 尚不是一个很好的音乐分析或生成工具。目前唯一真正专用的音乐模型是歌唱声音合成模型。其他一些模型能够产生音乐声音，但主要是为语音和声音而非音乐构建的。

然而，这并不是系统本身的局限性。而主要是因为开发人员没有决定在该工具中包含更多专用音乐 AI 模型。在当前 AudioGPT 作为基础的状态下，将更多音频模型纳入该系统或构建一个独立的音乐专用系统是可行的。

## 这是一个正在进行中的工作

根据我使用 AudioGPT 的有限经验，我已经可以看出任务分配过程的效果不如我所希望的那样。许多时候，我的请求被误解，调用了错误的模型，导致完全无用的输出。似乎仍需要进行一些优化，以使该系统在理解用户需求方面越来越有能力。

此外，整体音频 AI 的现状仍远远落后于文本 AI。例如，AudioGPT 中包含的 17 个模型大多数运作尚可，但存在明显的局限性。因此，即使 AudioGPT 的任务分配完美，系统仍将受制于底层模型的能力。

# 这对音乐意味着什么？

## AI 作曲/制作助手

目前，AudioGPT 对音乐家的工作和生活并没有真正的影响。然而，我估计这种情况在不久的将来会发生变化。要使这些系统在这个领域真正具有变革性，需要采取以下两个步骤：

1.  扩展 AudioGPT 的音乐模型（源分离、标记、去噪、音频效果等）或构建一个专注于这些特定任务的独立 MusicGPT 模型。

1.  开发插件，使音乐家能够通过聊天界面在他们的 DAW 中访问 AudioGPT（或 MusicGPT）。

显然，这不是一项容易的任务。特别是第二步可能是一个巨大的障碍。然而，在 DAW 中实现这样的聊天机器人可能是像苹果（Logic）、Ableton 或 Image-Line（FL Studio）这样的公司巨大的竞争优势。一个完善、领域特定且良好集成的 AudiGPT 版本可以显著提高创建音乐的效率、创造自由度和乐趣。

如果系统将当前项目中的所有音频和 MIDI 事件都存储在内存中，你可以随时使用简单的语音或文本命令来移动、编辑、组合、删除或增强这些事件。此外，创作过程中的一些事情很容易用语言表达，但手动执行却很困难。假设你想让你的歌曲听起来更加“放松”和“悠闲”。你是调整乐器选择、混音器、效果链和母带设置中的数百个旋钮，还是简单地告诉你的 AI 助手为你调整旋钮？我梦想有一个 DAW 插件，它允许我们在一个直观的聊天机器人界面内生成声音、应用效果、混音/母带处理我们的曲目、分离乐器、分析和标记音乐，以及更多。这种系统无疑能使我们在音乐创作流程中更加高效和富有创造力。

## 增强，而非替代

在过去几个月中，像 MusicLM 这样的生成模型引发了许多艺术家的生存恐惧。虽然无可争议的是 AI 将显著颠覆音乐产业，但我认为 AudioGPT 是一个很好的例子，说明这些技术可以增强而不是取代我们的音乐工作。聊天机器人界面对创建、编辑和重新安排声音非常有帮助，但它不作为一个自主的代理。作曲家或制作人的创造力、意图和情感才是赋予最终产品价值和意义的关键。

从长远来看，人类创作的音乐总是比 AI 创作的音乐对人类更有价值。这是因为音乐本质上是社会性的，是人们之间的一种沟通方式。对我们大多数人来说，音乐是否是用仅仅是声学乐器“手工制作”的并不重要。真正重要的是，无论在创作过程中使用了什么技术，都是由人类以受控的方式和作为个人创意表达的手段来利用这些工具。从这个角度看，扩展 AudioGPT 的功能并将其集成到音乐创作工具中，让我们能够从加速或提升创意的技术中获益，同时保持作为作曲家的价值和目的。

# 我该如何使用 AudioGPT？

## 作为程序员

作为程序员，你可以简单地克隆 [AudioGPT GitHub 仓库](https://github.com/AIGC-Audio/AudioGPT)，安装所有使用的模型，输入你的 OpenAI API 密钥，然后开始使用。这将允许你使用论文中展示的所有功能。

## 作为一个非技术人员

如果你不是程序员，你仍然可以在这个 [HuggingFace 网络应用](https://huggingface.co/spaces/AIGC-Audio/AudioGPT)中有限度地使用 AudioGPT。要使用该系统，你需要一个 OpenAI API 密钥。[这里](https://www.howtogeek.com/885918/how-to-get-an-openai-api-key/) 是如何获取密钥的教程。根据 OpenAI 目前的使用条款，你可能需要输入你的信用卡信息才能使用该令牌。这个密钥是必需的，因为 AudioGPT 在后台使用 ChatGPT。使用 ChatGPT 并不昂贵（截至 4 月 23 日，每 ~700 字约 0.002 美分。见 [文档](https://openai.com/pricing)）。不过，如果你决定将此密钥用于 AudioGPT，建议在你的 OpenAI 账户中监控系统产生的费用。

不幸的是，这个 HuggingFace 网络应用对我来说一直运行不太好。当我上传文件时，通常会出现错误。音频输出有时完全错误，尽管我的请求似乎被理解了……如果你已经有了 OpenAI API 密钥，绝对应该尝试一下。如果没有，我不确定这个网络应用是否值得你去创建账户和密钥的努力。

# 参考文献

**[1] 黄、李、杨、石等（2023）**。《AudioGPT：理解与生成语音、音乐、声音和谈话头像》。[arXiv:2304.12995v1](https://arxiv.org/abs/2304.12995)

**[2] 刘等（2021）**。《DiffSinger：通过浅层扩散机制进行歌声合成》。[arXiv:2105.02446](https://arxiv.org/abs/2105.02446)

**[3] 张等（2021）**。《VISinger：使用对抗学习进行端到端歌声合成的变分推断》。[arXiv:2110.08813](https://arxiv.org/abs/2110.08813)

如果你喜欢这篇文章，可以看看我关于 MusicLM 的文章，即 Google 的突破性音乐生成模型：[**MusicLM：Google 解决了 AI 音乐生成问题吗？**](https://medium.com/towards-data-science/musiclm-has-google-solved-ai-music-generation-c6859e76bc3c)
