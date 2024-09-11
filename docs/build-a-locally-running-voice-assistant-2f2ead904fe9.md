# 构建一个本地运行的语音助手

> 原文：[https://towardsdatascience.com/build-a-locally-running-voice-assistant-2f2ead904fe9?source=collection_archive---------1-----------------------#2023-12-29](https://towardsdatascience.com/build-a-locally-running-voice-assistant-2f2ead904fe9?source=collection_archive---------1-----------------------#2023-12-29)

## 向LLM提问而不泄露私人信息

[](https://sebastiengilbert.medium.com/?source=post_page-----2f2ead904fe9--------------------------------)[![Sébastien Gilbert](../Images/380f6588c3ef718947bcf82061f190eb.png)](https://sebastiengilbert.medium.com/?source=post_page-----2f2ead904fe9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2f2ead904fe9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2f2ead904fe9--------------------------------) [Sébastien Gilbert](https://sebastiengilbert.medium.com/?source=post_page-----2f2ead904fe9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F975aef8c496a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-locally-running-voice-assistant-2f2ead904fe9&user=S%C3%A9bastien+Gilbert&userId=975aef8c496a&source=post_page-975aef8c496a----2f2ead904fe9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2f2ead904fe9--------------------------------) ·7分钟阅读·2023年12月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2f2ead904fe9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-locally-running-voice-assistant-2f2ead904fe9&user=S%C3%A9bastien+Gilbert&userId=975aef8c496a&source=-----2f2ead904fe9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2f2ead904fe9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-locally-running-voice-assistant-2f2ead904fe9&source=-----2f2ead904fe9---------------------bookmark_footer-----------)![](../Images/431d431bc36ee8d489e091189e0b7371.png)

图片由作者生成，并由openart.ai协助

我不得不承认，我最初对大型语言模型（LLM）生成实际有效的代码片段的能力持怀疑态度。我带着最坏的预期尝试了，结果却感到惊喜。像与任何聊天机器人互动一样，问题的格式很重要，但随着时间的推移，你会学会如何明确你需要帮助的问题的边界。

我习惯了在编写代码时有一个始终可用的在线聊天机器人服务，但当我的雇主发布了禁止员工使用它的公司政策时，我不得不寻找其他方案。我可以回到以前的谷歌习惯，但我决定构建一个本地运行的LLM服务，这样我可以在不泄露公司外部信息的情况下提问。感谢[HuggingFace](https://huggingface.co)上的开源LLM提供以及[chainlit项目](https://docs.chainlit.io/get-started/overview)，我能够组建一个满足编码辅助需求的服务。

下一步是添加一些语音交互。尽管语音不适合编码辅助（你希望看到生成的代码片段，而不是听到它们），但在某些情况下，你需要在创意项目上获得灵感。讲故事的感觉为体验增添了价值。另一方面，你可能不愿意使用在线服务，因为你希望保持工作的隐私。

在这个项目中，我将带你了解构建一个允许你通过语音与开源LLM交互的助手的步骤。所有组件都在你的计算机上本地运行。

# 架构

架构包括三个独立的组件：

+   一个唤醒词检测服务

+   一个语音助手服务

+   一个聊天服务

![](../Images/b645360bc06d8cab1f0dd141376d991e.png)

三个组件的流程图。图片由作者提供。

这三个组件是独立的项目，每个项目都有自己的github仓库。让我们逐一了解每个组件及其如何交互。

## 聊天服务

聊天服务运行开源LLM，名为[*HuggingFaceH4/zephyr-7b-alpha*](https://huggingface.co/HuggingFaceH4/zephyr-7b-alpha)。该服务通过POST调用接收提示，将提示传递给LLM，并将输出作为调用响应返回。

你可以在[这里](https://github.com/sebastiengilbert73/chat_service)找到代码。

在…/chat_service/server/中，将**chat_server_config.xml.example**重命名为**chat_server_config.xml**。

然后你可以使用以下命令启动聊天服务器：

```py
python .\chat_server.py
```

> 当服务第一次运行时，由于从[HuggingFace](https://huggingface.co/)网站下载大型文件并存储在本地缓存目录中，启动需要几分钟时间。

你会从终端收到服务正在运行的确认：

![](../Images/3a9c8aab18256f8324d1b98a08cb387d.png)

聊天服务运行的确认。图片由作者提供。

如果你想测试与LLM的交互，请前往…/chat_service/chainlit_interface/。

将**app_config.xml.example**重命名为**app_config.xml**。使用以下命令启动网络聊天服务：

```py
.\start_interface.sh
```

浏览到本地地址**localhost:8000**

你应该能够通过文本接口与本地运行的LLM进行交互：

![](../Images/33d813385a193a71ec43e16e4b7694b0.png)

与本地运行的LLM的文本交互。图片由作者提供。

## 语音助手服务

语音助手服务是进行语音转文本和文本转语音转换的地方。你可以在[这里](https://github.com/sebastiengilbert73/voice_assistant)找到代码。

转到…/voice_assistant/server/。

将**voice_assistant_service_config.xml.example**重命名为**voice_assistant_service_config.xml**。

助理通过播放问候语开始，以表明它正在监听用户。问候语的文本在**voice_assistant_config.xml**中配置，位于*<welcome_message>*元素下：

![](../Images/89c8cc195d28f9f5a7a83ae94e2f9e42.png)

voice_assistant_config.xml文件。图片由作者提供。

允许程序将文本转换为你可以通过音频输出设备听到的语音的文本转语音引擎是[pyttsx3](https://pypi.org/project/pyttsx3/)。根据我的经验，这个引擎用英语和法语说话都非常自然。与其他依赖API调用的软件包不同，它是在本地运行的。

一个名为[*facebook/seamless-m4t-v2-large*](https://huggingface.co/facebook/seamless-m4t-v2-large)的模型执行语音转文本推断。**voice_assistant_service.py**首次运行时会下载模型权重。

**voice_assistant_service.main()**中的主要循环执行以下任务：

+   从麦克风获取句子。使用语音转文本模型将其转换为文本。

+   检查用户是否说出了配置文件中的*<end_of_conversation_text>*元素定义的消息。在这种情况下，谈话结束，程序在播放再见消息后终止。

+   检查句子是否是胡言乱语。语音转文本引擎即使我什么也没说，通常也会输出有效的英语句子。偶尔，这些不必要的输出会重复出现。例如，胡言乱语的句子有时会以“[”或“i’m going to”开头。我在配置文件的*<gibberish_prefix_list>*元素中收集了通常与胡言乱语句子相关的前缀列表（这个列表可能会根据其他语音转文本模型而有所变化）。每当音频输入以列表中的一个前缀开头时，句子就会被忽略。

+   如果句子看起来不是胡言乱语，请向聊天服务发送请求。播放响应。

**voice_assistant_service.main()**中的主要循环。代码由作者提供。

## 唤醒词服务

最后一个组件是一个持续监听用户麦克风的服务。当用户说出唤醒词时，系统调用会启动语音助手服务。唤醒词服务运行的模型比语音助手服务模型要小。因此，持续运行唤醒词服务是有意义的，而语音助手服务则只有在需要时才会启动。

你可以在[这里](https://github.com/sebastiengilbert73/wakeword_service)找到唤醒词服务的代码。

克隆项目后，转到…/wakeword_service/server。

将**wakeword_service_gui_config.xml.example**重命名为**wakeword_service_gui_config.xml**。

将**command.bat.example**重命名为**command.bat**。你需要编辑**command.bat**，使虚拟环境激活和对**voice_assistant_service.py**的调用与您的目录结构相匹配。

你可以通过以下调用来启动服务：

```py
python gui.py
```

唤醒词检测服务的核心是[openwakeword](https://github.com/dscripka/openWakeWord)项目。在几个唤醒词模型中，我选择了“hey jarvis”模型。我发现简单地说“Jarvis?”就会触发检测。

每当检测到唤醒词时，都会调用一个命令文件，正如配置文件的*<command_on_wakeword>*元素中所指定的。在我们的情况下，**command.bat**文件激活虚拟环境并启动语音助手服务。

![](../Images/a10b684d726fc922f4169fcf2d404486.png)

唤醒词检测服务GUI的配置文件。图片来源于作者。

如果你想将唤醒词检测服务用于其他用途，你可以编辑**command.bat**文件，以启动你想要的任何程序。

当检测到唤醒词时，唤醒词检测服务会发出两声哔声以指示已触发。这一快速反馈告诉用户，他们必须等待，因为文本到语音和语音到文本模型正在加载到计算机的GPU中，这可能需要几秒钟。当两个模型都加载完毕后，语音助手服务会播放问候语，交谈可以开始。对话结束后，语音助手服务终止，唤醒词检测服务会发出另一声两声哔声信号。

当三个组件安装并测试完成后，我们可以启动聊天和唤醒词检测服务。唤醒词检测服务将不断监听麦克风。当检测到唤醒词（“Jarvis?”）时，它将启动语音助手服务。然后你可以大声提问并收到语音回答。当检测到对话结束短语（“谢谢，再见”）时，语音助手服务结束。

# 与助手的交互示例

这是与语音助手交互的一个示例：

> “Jarvis?” [哔—哔]
> 
> [在语音助手模型加载期间的几秒钟沉默]
> 
> “你好！我能帮你什么？”
> 
> “土星的环由什么组成？”
> 
> “土星的环主要由冰粒组成，其中大部分是水冰。然而，环中也有少量其他冰类，例如氨冰和岩石碎片。环的确切成分因距离土星的远近而异，内圈的密度较高，颗粒较小，而外圈则分布较广，含有较大的冰粒。”
> 
> “告诉我电影‘回到未来’的情节。”
> 
> “电影《回到未来》讲述了高中生马蒂·麦克弗莱的故事，他意外地被传送回1955年，乘坐由他那古怪的科学家朋友布朗博士创造的时光旅行德洛林车。马蒂必须在修复对现在造成的损害的同时应对过去的挑战，并防止他的青少年母亲爱上他的青少年父亲。在这个过程中，马蒂结识了年轻版的布朗博士，并帮助他克服个人悲剧。这部电影探讨了命运、家庭和改变过去的后果等主题。”
> 
> “谢谢，再见”
> 
> “再见” [嘟嘟声]

# 结论

在这个项目中，我们实现了一个由唤醒词检测服务、语音助手服务和聊天服务组成的语音助手系统。这些组件都是独立的模块，可以在其他项目中重复使用。

在我开发这个语音助手的过程中，有许多惊喜，其中最让我印象深刻的是语音转文本的质量。如果你像我一样，可能会遇到过无法准确转录简单命令的自动语音识别系统，比如*“调低音量”*！我原本以为语音转文本会是整个流程的主要障碍。在尝试了几个不令人满意的模型后，我最终选择了[*facebook/seamless-m4t-v2-large*](https://huggingface.co/facebook/seamless-m4t-v2-large)，对其结果的质量印象深刻。我甚至可以用法语说一句话，神经网络会自动将其翻译成英语。这简直令人惊叹！

我希望你能尝试这个有趣的项目，并告诉我你是如何使用它的！
