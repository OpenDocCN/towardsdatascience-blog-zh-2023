# 使用 GPT 和 Whisper 创建个性化语音助手

> 原文：[`towardsdatascience.com/creating-your-personalized-voice-assistant-with-gpt-and-whisper-ddb9bd8c419`](https://towardsdatascience.com/creating-your-personalized-voice-assistant-with-gpt-and-whisper-ddb9bd8c419)

## 分步指南

[](https://donatoriccio.medium.com/?source=post_page-----ddb9bd8c419--------------------------------)![Donato Riccio](https://donatoriccio.medium.com/?source=post_page-----ddb9bd8c419--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ddb9bd8c419--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ddb9bd8c419--------------------------------) [Donato Riccio](https://donatoriccio.medium.com/?source=post_page-----ddb9bd8c419--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ddb9bd8c419--------------------------------) ·6 分钟阅读·2023 年 5 月 18 日

--

![](img/91c085fb9d42bb3ace220b9baec4f77a.png)

照片由 [Ivan Bandura](https://unsplash.com/@unstable_affliction?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本文旨在指导你创建一个简单而强大的语音助手，按照你的偏好定制。我们将使用两个强大的工具——Whisper 和 GPT，来实现这一目标。你可能已经知道 GPT 及其强大之处，但*Whisper 是什么？*

Whisper 是 OpenAI 提供的先进语音识别模型，能够提供准确的音频转文本服务。

我们将逐步指导你完成每个步骤，并包含编码指令。最终，你将拥有一个运行良好的语音助手。

# 在你开始之前

## OpenAI API 密钥

如果你已经有了 OpenAI API 密钥，可以跳过这一部分。

Whisper 和 GPT APIs 都需要 OpenAI API 密钥才能访问。与 ChatGPT 的固定费用订阅不同，API 密钥的费用是根据你使用服务的多少来计算的。

价格合理。撰写本文时，Whisper 的价格为 $0.006 / 分钟，GPT（使用模型 gpt-3.5-turbo）的价格为 $0.002 / 1K tokens（一个 token 大约是 0.75 个单词）。

![](img/90f919b32509289cf43eb40b7d2ef4a2.png)

OpenAI 网站。图片来源于作者。

要获取你的密钥，首先在 OpenAI 网站上创建一个账户。登录后，点击右上角的你的名字，然后选择*查看 API 密钥*。点击*创建新的密钥*后，你的密钥将会显示出来。务必保存它，因为你之后将无法再次查看。

## 包

代码块显示了项目所需的库。该项目涉及使用 OpenAI 的 Python 库进行 AI 任务，**pyttsx3**用于生成语音，**SoundDevice**用于录音和播放音频，**numpy**和**scipy**用于数学运算。像往常一样，在开始新项目时，你应该创建一个新的虚拟环境，然后安装包。

# 代码结构

我们的代码将围绕一个类进行结构化，总共约 90 行代码。假设你对 Python 类有基本了解。

![](img/1471474c58d6a02c0eb23166249a635c.png)

由作者提供的图片。

`listen`方法捕获用户的语音输入，并使用 Whisper 将其转换为文本。`think`方法将文本发送到 GPT，生成自然语言响应。`speak`方法将响应文本转换为音频并播放。这个过程会重复：用户可以通过发出另一个请求来继续对话。

![](img/bb587d1395678babdd36f258fffa8f0a.png)

代码结构。由作者提供的图片。

# __init__

此函数负责初始化历史记录和设置 API 密钥。

我们需要一个历史记录来跟踪之前的消息。它基本上是我们助手的短期记忆，允许它记住你在对话中早些时候说过的话。

# listen

![](img/220a3e3def47c2b30b9682c48feef6fb.png)

listen 函数。由作者提供的图片。

这个方法是我们助手的“耳朵”。

`listen`函数允许接收用户的输入。此函数从你的麦克风录制音频，并将其转录为文本。

它的功能如下：

+   录制音频时打印*Listening…*。

+   使用 sounddevice 在 44100 Hz 的采样率下录制 3 秒（或任何你想要的时间）的音频。

+   将录制的音频作为 NumPy 数组保存到临时 WAV 文件中。

+   使用 OpenAI API 的`transcribe`方法将音频发送到 Whisper 进行转录。

+   打印转录的文本到控制台，以确认转录是否成功。

+   返回转录的文本作为字符串。

在示例中，助手监听了 3 秒，但你可以根据需要更改时间。

# think

![](img/b88809c7d91c07d6363a1d3b71e75427.png)

think 函数。由作者提供的图片。

我们助手的大脑由 GPT 提供支持。think 函数接收助手听到的内容并提出响应。*怎么做到的？*

响应不是在你的计算机上生成的。文本需要发送到 OpenAI 的服务器，通过 API 进行处理。然后将响应保存在响应变量中，用户消息和响应都添加到历史记录中，这是助手的短期记忆，为 GPT 模型生成响应提供上下文。

# speak

![](img/38616ab8a0bee7bf85d420f72f7dd8b2.png)

speak 函数。由作者提供的图片。

**speak**函数负责将文本转换为语音并播放给用户。这个函数接受一个参数：text。它应该是一个表示要转换为语音的文本的字符串。

当函数被调用并传入一个文本字符串作为参数时，它会用命令`engine = pyttsx3.init()`初始化 pyttsx3 语音引擎。这个对象`engine`是将文本转换为语音的主要接口。

然后，函数指示语音引擎使用命令`engine.say(text)`将提供的文本转换为语音。这会将提供的文本排队等待朗读。命令`engine.runAndWait`告诉引擎处理排队的命令。

Pyttsx3 在本地处理所有文本到语音的转换，这在延迟方面可以是一个显著的优势。

# 最终修饰

助手现在已经准备好。我们只需创建一个助手对象，开始对话即可。

对话是一个无限循环，直到用户说出包含*Goodbye*的句子时才结束。

# 个性化体验的小贴士

自定义你的 GPT 助手非常简单！我们构建的代码非常模块化，可以通过添加各种功能来进行自定义。这里有一些想法可以帮助你入门：

+   **为助手赋予角色**：更改初始提示，让你的助手扮演英语老师、励志演讲者或其他任何你能想到的角色！查看[Awesome ChatGPT Prompts](https://github.com/f/awesome-chatgpt-prompts)获取更多创意。

+   **更改语言：** 想使用其他语言？没问题！只需将代码中的*english*更改为你想要的语言即可。

+   **构建应用程序：** 你可以轻松地将助手集成到任何应用程序中。

+   **添加个性：**通过添加自定义回应或使用不同的语调和语言风格，赋予你的助手独特的个性。

+   **与其他 API 集成：** 将你的助手与其他 API 集成，以提供更高级的功能，如天气预报或新闻更新。

# 结论

在这篇文章中，我们解释了如何检索你的 OpenAI API 密钥，并提供了用于捕获用户输入、生成回应和将文本转换为语音以进行播放的 listen、think 和 speak 函数的代码示例。

有了这些知识，你可以开始创建适合你特定需求的独特语音助手。从创建一个帮助日常任务的个人助手，到构建一个语音控制的自动化系统，可能性是无限的。你可以在链接的[GitHub repo](https://github.com/reese3222/nanoassistant)中获取所有代码。

*喜欢这篇文章吗？订阅我的新闻通讯，[*The Data Interview*](https://thedatainterview.substack.com/)，每周将数据科学面试问题发送到你的邮箱。*

*此外，你还可以在* [*LinkedIn*](https://www.linkedin.com/in/driccio/)*上找到我*。
