# 使用 LLMs 为你的移动应用提供语音和自然语言输入

> 原文：[https://towardsdatascience.com/speech-and-natural-language-input-for-your-mobile-app-using-llms-e79e23d3c5fd?source=collection_archive---------5-----------------------#2023-07-25](https://towardsdatascience.com/speech-and-natural-language-input-for-your-mobile-app-using-llms-e79e23d3c5fd?source=collection_archive---------5-----------------------#2023-07-25)

## 如何利用 OpenAI GPT-4 功能来导航你的 GUI

[](https://medium.com/@hgwvandam?source=post_page-----e79e23d3c5fd--------------------------------)[![Hans van Dam](../Images/52846b7417b271767597c468edfaec46.png)](https://medium.com/@hgwvandam?source=post_page-----e79e23d3c5fd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e79e23d3c5fd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e79e23d3c5fd--------------------------------) [Hans van Dam](https://medium.com/@hgwvandam?source=post_page-----e79e23d3c5fd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6ce6c6116a37&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fspeech-and-natural-language-input-for-your-mobile-app-using-llms-e79e23d3c5fd&user=Hans+van+Dam&userId=6ce6c6116a37&source=post_page-6ce6c6116a37----e79e23d3c5fd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e79e23d3c5fd--------------------------------) · 14 分钟阅读 · 2023年7月25日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe79e23d3c5fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fspeech-and-natural-language-input-for-your-mobile-app-using-llms-e79e23d3c5fd&user=Hans+van+Dam&userId=6ce6c6116a37&source=-----e79e23d3c5fd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe79e23d3c5fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fspeech-and-natural-language-input-for-your-mobile-app-using-llms-e79e23d3c5fd&source=-----e79e23d3c5fd---------------------bookmark_footer-----------)![](../Images/e9ebbfb4cc7e7ae96821d7ba97ed5661.png)

图片由 [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，发布于 [Unsplash](https://unsplash.com/photos/v9FQR4tbIq8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

大型语言模型（LLM）是一个可以有效处理自然语言的机器学习系统。目前最先进的LLM是GPT-4，它为付费版ChatGPT提供支持。在这篇文章中，你将学习如何通过GPT-4功能调用，为你的应用程序提供高度灵活的语音解释，与应用程序的图形用户界面（GUI）完全协同。这篇文章旨在为产品负责人、用户体验设计师和移动开发者提供指导。

![](../Images/dd3be11e362fdfc3a7b672287aec81ad.png)

# 背景

移动电话（Android和iOS）上的数字助手未能普及，有几个原因，其中包括它们有缺陷、功能有限且使用起来往往很麻烦。LLM，特别是OpenAI GPT-4，拥有更深入理解用户意图的潜力，而不是粗略地模式匹配口语表达，从而有可能带来改变。

Android有Google Assistant的“应用操作”，iOS有SiriKit意图。这些提供了简单的模板来注册你的应用可以处理的语音请求。Google Assistant和Siri在过去几年中已经有了很大改进——甚至超出你的想象。然而，它们的覆盖范围在很大程度上取决于哪些应用实现了对它们的支持。尽管如此，你仍然可以通过语音在Spotify上播放你喜欢的歌曲。然而，这些操作系统提供的服务的自然语言解释早于LLM在这一领域带来的巨大进步——所以是时候迈出下一步：利用LLM的力量使语音输入更可靠和灵活。

尽管我们可以预期操作系统服务（如Siri和Google Assistant）会很快调整策略，以利用LLM，但我们已经可以使我们的应用程序在不受这些服务限制的情况下使用语音。一旦你掌握了本文中的概念，你的应用也将准备好接入[新助手](https://www.axios.com/2023/07/31/google-assistant-artificial-intelligence-news)，一旦它们上线。

你选择的LLM（GPT、PaLM、LLama2、MPT、Falcon等）确实会影响可靠性，但你将在这里学到的核心原理可以应用于任何LLM。我们将让用户通过一句话表达他们的需求，从而访问应用程序的全部功能。LLM将自然语言表达映射到我们应用的导航结构和功能上的函数调用上。这不一定要像机器人一样说出一句完整的句子。**LLM的解释能力允许用户像人类一样说话，使用他们自己的词汇或语言；犹豫、犯错并纠正错误**。用户之所以拒绝语音助手，是因为它们经常无法理解他们的意思，而LLM的灵活性可以让交互变得更加自然和可靠，从而提高用户的接受度。

# 为什么现在在你的应用中使用语音输入？

**优点：**

+   通过一句语音表达来导航到一个界面并提供所有参数

+   浅层学习曲线：用户无需找到数据在应用中的位置或如何操作 GUI

+   免提

+   互补而非不相关（如语音用户界面或 VUI）：语音和 GUI 和谐工作。

+   视力障碍的可及性

+   现在：由于自然语言的解释通过 LLM 达到了一个新水平，回应更加可靠

**缺点：**

+   说话时的隐私

+   准确性/误解

+   仍然相对较慢

+   头脑中的知识与世界中的知识（我能说什么？）：用户不知道系统理解和回答哪些口语表达

受益于语音输入的应用示例包括用于汽车或自行车驾驶辅助的应用。一般来说，当用户不能轻松使用双手时，例如在移动中、戴着手套或忙于用手工作的情况下，他们可能不愿意通过触摸精确导航应用。

购物应用也可以受益于此功能，因为用户可以用自己的话表达需求，而不是通过购物界面和设置过滤器来导航。

当将这种方法应用于提高视力障碍人士的可及性时，您可能考虑加入自然语言输出和文本转语音功能。

# 您的应用

下图展示了一个典型应用的导航结构，以您可能熟悉的火车旅行规划器为例。在顶部，您可以看到触摸导航的默认导航结构。该结构由导航组件控制。所有导航点击都委托给导航组件，后者执行导航操作。底部展示了我们如何利用语音输入来接入这一结构。

![](../Images/20896ca68632952ea9be1a1ee07da5d4.png)

使用 LLM 功能调用来启用您的应用的语音功能

用户说出他们的需求，然后语音识别器将语音转换为文本。系统构建一个包含这些文本的提示并发送给 LLM。LLM 以数据的形式回应应用，告诉它哪个界面需要激活以及使用哪些参数。这个数据对象被转换为深层链接并提供给导航组件。导航组件用正确的参数激活正确的界面：在这个例子中，就是用‘阿姆斯特丹’作为参数的‘外出’界面。请注意，这只是一个简化版。我们将在下面详细说明。

许多现代应用程序在底层有一个集中式导航组件。Android 有 Jetpack Navigation，Flutter 有 Router，而 iOS 有 NavigationStack。集中式导航组件支持深度链接，这是一种技术，允许用户直接导航到移动应用中的特定屏幕，而无需经过应用的主屏幕或菜单。为了使本文中的概念有效，导航组件和集中式深度链接并非必需，但它们使实现这些概念更为简单。

深度链接涉及创建一个独特的 (URI) 路径，该路径指向应用中的特定内容或特定部分。此外，这个路径可以包含控制屏幕上深度链接所指向的 GUI 元素状态的参数。

# 你的应用程序的函数调用

我们通过提示工程技术指示 LLM 将自然语言表达映射到导航功能调用。提示的内容类似于：‘给定以下带参数的函数模板，将以下自然语言问题映射到这些函数模板之一并返回’。

大多数 LLM 都能做到这一点。LangChain 通过 Zero Shot ReAct Agents 有效地利用了这一点，待调用的函数称为 [Tools](https://python.langchain.com/docs/modules/agents/tools/)。OpenAI 已经用特别版本（当前为 gpt-3.5-turbo-0613 和 gpt-4–0613）对其 GPT-3.5 和 GPT-4 模型进行了微调，非常擅长此任务，并为此目的设置了特定的 API 条目。本文将采用 OpenAI 的符号表示，但这些概念可以应用于任何 LLM，例如使用提到的 ReAct 机制。此外，LangChain 有一个特定的代理类型 (AgentType.OPENAI_FUNCTIONS)，在幕后将 Tools 转换为 OpenAI 函数模板。对于 LLama2，你将能够使用 [llama-api](https://www.llama-api.com/) 并使用与 OpenAI 相同的语法。

LLM 的函数调用工作如下：

1.  你将函数模板的 JSON 架构与用户的自然语言表达作为用户消息一起插入到提示中。

1.  LLM 尝试将用户的自然语言表达映射到这些模板之一。

1.  LLM 返回结果 JSON 对象，以便你的代码可以进行函数调用。

在本文中，函数定义是 (移动) 应用程序图形用户界面 (GUI) 的直接映射，其中每个函数对应于一个屏幕，每个参数对应于该屏幕上的一个 GUI 元素。发送到 LLM 的自然语言表达返回一个包含函数名称及其参数的 JSON 对象，你可以用来导航到正确的屏幕并在视图模型中触发正确的函数，以便获取正确的数据。该屏幕上相关 GUI 元素的值根据参数进行设置。

这在下图中进行了说明：

![](../Images/b8ae7bc16f62cd81d969e202d45f53c9.png)

将 LLM 功能映射到你的移动应用程序的 GUI

它展示了添加到LLM提示中的函数模板的精简版本。要查看用户消息‘我在阿姆斯特丹可以做些什么？’的完整提示，[点击这里 (Github Gist)](https://gist.github.com/hansvdam/8b9269390e16fa0bf394d7656bec1ea5)。它包含了你可以从命令行使用或导入到Postman中的完整curl请求。你需要将你自己的O[penAI-key](https://platform.openai.com/account/api-keys)放入占位符中以运行它。

# 没有参数的屏幕

你应用中的一些屏幕没有任何参数，或者至少没有LLM需要了解的参数。为了减少令牌使用和杂乱，我们可以将这些屏幕触发器合并到一个单一的函数中，并使用一个参数：要打开的屏幕。

```py
{
    "name": "show_screen",
    "description": "Determine which screen the user wants to see",
    "parameters": {
        "type": "object",
        "properties": {
            "screen_to_show": {
                "description": "type of screen to show. Either 
                    'account': 'all personal data of the user', 
                    'settings': 'if the user wants to change the settings of 
                                the app'",
                "enum": [
                    "account",
                    "settings"
                ],
                "type": "string"
            }
        },
        "required": [
            "screen_to_show"
        ]
    }
},
```

判断触发函数是否需要参数的标准是用户是否有选择：屏幕上是否进行某种形式的搜索或导航，即是否有可以选择的搜索（类似）字段或标签？

如果没有，那么LLM不需要知道这些信息，并且屏幕触发可以添加到你应用的通用屏幕触发函数中。这主要是一个关于屏幕目的描述的实验问题。如果你需要更长的描述，考虑给它一个自己的函数定义，以便比通用参数的枚举更分开地强调它的描述。

# 提示指令指导和修复：

在你提示的系统消息中，你提供了一般性的引导信息。在我们的示例中，了解当前的日期和时间可能很重要，例如，如果你想为明天计划一个旅行。另一个重要的方面是引导其假设性。我们通常更希望LLM表现得过于自信，而不是因为不确定性而打扰用户。对于我们的示例应用，一个好的系统消息是：

```py
"messages": [
        {
            "role": "system",
            "content": "The current date and time is 2023-07-13T08:21:16+02:00.
                       Be very presumptive when guessing the values of 
                       function parameters."
        },
```

函数参数描述可能需要相当多的调整。例如，在计划火车旅行时，`trip_date_time`就是一个例子。一个合理的参数描述是：

```py
"trip_date_time": {
      "description": "Requested DateTime for the departure or arrival of the 
                      trip in 'YYYY-MM-DDTHH:MM:SS+02:00' format.
                      The user will use a time in a 12 hour system, make an 
                      intelligent guess about what the user is most likely to 
                      mean in terms of a 24 hour system, e.g. not planning 
                      for the past.",
                      "type": "string"
                  },
```

所以如果现在是15:00，而用户说他们想在8点离开，他们实际上指的是20:00，除非他们特别提到一天中的时间。上述指令对GPT-4的效果相当好。但在某些极端情况下，它仍然会失败。我们可以例如添加额外的参数到函数模板中，以便在我们自己的代码中进行进一步修正。例如，我们可以添加：

```py
"explicit_day_part_reference": {
          "description": "Always prefer None! None if the request refers to 
                        the current day, otherwise the part of the day the 
                        request refers to."
          "enum": ["none", "morning", "afternoon", "evening", "night"], 
                           }
```

在你的应用中，你可能会发现一些参数需要后处理以提高其成功率。

# 系统请求澄清

有时，用户的请求缺乏继续处理所需的信息。可能没有适合处理用户请求的函数。在这种情况下，LLM会用自然语言响应，你可以通过例如Toast的方式展示给用户。

也可能存在这样一种情况，即大型语言模型（LLM）确实识别出了一个潜在的函数调用，但缺乏填充所有必需函数参数的信息。在这种情况下，可以考虑将参数设置为可选。但如果这不可行，LLM可能会用用户的语言发送自然语言请求，询问缺失的参数。你应该将这段文本展示给用户，例如通过吐司提示或文本转语音，让他们提供缺失的信息（通过语音）。例如，当用户说“我想去阿姆斯特丹”（而你的应用没有通过系统消息提供默认或当前位置）时，LLM可能会回应“我知道你想要乘火车旅行，你想从哪里出发？”。

这提出了对话历史的问题。我建议你始终在提示中包含用户的最后4条消息，以便信息请求可以分多次进行。为了简化起见，可以省略系统的响应，因为在这种用例中，它们往往弊大于利。

# 语音识别

语音识别是将语音转换为应用中的参数化导航动作的关键部分。当解释的质量较高时，语音识别的质量较差可能会成为最薄弱的环节。手机具有合理质量的内置语音识别，但基于LLM的语音识别如[Whisper](https://openai.com/research/whisper)、谷歌的[Chirp/USM](https://cloud.google.com/speech-to-text/v2/docs/chirp-model)、Meta的[MMS](https://ai.meta.com/blog/multilingual-model-speech-recognition/?utm_source=twitter&utm_medium=organic_social&utm_campaign=blog&utm_content=card)或[DeepGram](https://developers.deepgram.com/reference/streaming)往往会取得更好的结果，特别是当你可以为你的用例[调整](https://cloud.google.com/speech-to-text/docs/adaptation-model)它们时。

# 架构

最好将函数定义存储在服务器上，但它们也可以由应用管理，并随每个请求发送。这两种方式各有利弊。随每个请求发送函数定义更灵活，函数和界面的对齐也可能更容易维护。然而，函数模板不仅包含函数名称和参数，还包含我们可能希望比应用商店更新流程更快更新的描述。这些描述或多或少依赖于LLM，并且根据实际效果进行设计。你可能会想要用更好的或更便宜的LLM来替换当前的LLM，或者甚至在某些时候动态切换。将函数模板存储在服务器上也可能有一个好处，即如果你的应用在iOS和Android上都是原生的，那么可以在一个地方维护它们。如果你同时使用OpenAI的服务进行语音识别和自然语言处理，那么整个流程的技术大图如下：

![](../Images/ca4debb6ef8e0f616fcb21be2a997df9.png)

使用 Whisper 和 OpenAI 函数调用为你的移动应用启用语音的架构

用户说出他们的请求；它被录制到 m4a 缓冲区/文件（如果你愿意，也可以是 mp3），然后发送到你的服务器，服务器将其转发到 [Whisper](https://openai.com/blog/introducing-chatgpt-and-whisper-apis)。Whisper 响应转录内容，你的服务器将其与系统消息和函数模板结合成 LLM 的提示。你的服务器收到原始函数调用 JSON，然后将其处理成应用程序所需的函数调用 JSON 对象。

# 从函数调用到深度链接

为了说明函数调用如何转换为深度链接，我们取初始示例中的函数调用响应：

```py
"function_call": {
                    "name": "outings",
                    "arguments": "{\n  \"area\": \"Amsterdam\"\n}"
                }
```

在不同的平台上，这一过程处理得相当不同，并且随着时间的推移，使用了许多不同的导航机制，并且这些机制仍然在使用中。详细的实现细节超出了本文的范围，但大致来说，这些平台在其最新版本中可以采用如下的深度链接：

在 Android 上：

```py
navController.navigate("outings/?area=Amsterdam")
```

在 Flutter 上：

```py
Navigator.pushNamed(
      context,
      '/outings',
      arguments: ScreenArguments(
        area: 'Amsterdam',
      ),
    );
```

在 iOS 上，事情有些不够标准化，但使用 NavigationStack：

```py
NavigationStack(path: $router.path) {
            ...
}
```

然后发出：

```py
router.path.append("outing?area=Amsterdam")
```

更多关于深度链接的信息可以在这里找到：[Android](https://blog.appcircle.io/article/jetpack-compose-navigation-deeplink)，[Flutter](https://docs.flutter.dev/cookbook/navigation/navigate-with-arguments)，[iOS](https://www.swiftyplace.com/blog/better-navigation-in-swiftui-with-navigation-stack)

# 应用程序的自由文本字段

有两种自由文本输入模式：语音和打字。我们主要讨论了语音，但打字输入的文本字段也是一个选项。自然语言通常相当冗长，因此可能很难与 GUI 交互竞争。然而，GPT-4 通常很擅长从缩写中猜测参数，因此即使是非常简短的缩写打字也常常能被正确解释。

在提示中使用带有参数的函数通常会大大缩小 LLM 的解释上下文。因此，它需要非常少的内容，如果你指示它进行假设则更少。这是一个新的现象，对移动交互具有很大的潜力。在车站到车站规划器的案例中，LLM 在使用本文中示例提示结构时做出了以下解释。你可以使用上述的 [prompt gist](https://gist.github.com/hansvdam/8b9269390e16fa0bf394d7656bec1ea5) 亲自尝试。

**示例：**

‘ams utr’：给我显示从阿姆斯特丹中央车站到乌特勒支中央车站的列车时刻表，从现在起出发。

‘utr ams arr 9’：（假设现在是13:00）。给我显示从乌特勒支中央车站到阿姆斯特丹中央车站的列车时刻表，要求到达时间在21:00之前。

**后续交互**

就像在 ChatGPT 中一样，你可以通过发送一小段交互历史来细化你的查询：

使用历史记录功能，以下内容也非常有效（假设现在是早上9:00）：

输入：‘ams utr’ 并获得上述答案。然后在下一轮输入‘arr 7’。是的，它实际上可以将其翻译为从阿姆斯特丹中央到乌特勒支中央的旅行，预计在19:00之前到达。

我制作了一个关于此的示例网页应用程序，你可以在[这里](https://www.youtube.com/watch?v=XBjiqLD578I)找到相关视频。实际应用程序的链接在描述中。

更新：可以在[这里](https://medium.com/towards-data-science/synergy-of-llm-and-gui-beyond-the-chatbot-c8b0e08c6801)找到这篇文章的继任者，其中包含文本输入，演示视频请见[这里](https://youtu.be/vJy0HI_mH7w)。

# 未来

你可以期待这种深度链接结构处理应用内功能，成为你手机操作系统（Android 或 iOS）的一个重要组成部分。手机上的全球助手将处理语音请求，应用程序可以将其功能暴露给操作系统，以便以深度链接的方式触发。这与 ChatGPT 插件的可用性类似。目前，通过 AndroidManifest 中的意图和 [App Actions](https://developers.google.com/assistant/app) 以及 iOS 上的 [SiriKit intents](https://medium.com/simform-engineering/how-to-integrate-siri-shortcuts-and-design-custom-intents-tutorial-e53285b550cf) 已经可以粗略实现。你对这些功能的控制有限，用户需要像机器人一样说话才能可靠地激活它们。毫无疑问，当[LLM 驱动的助手](https://www.axios.com/2023/07/31/google-assistant-artificial-intelligence-news)接管时，这种情况会随着时间的推移而改善。

VR 和 AR（XR）为语音识别提供了极好的机会，因为用户的双手通常参与其他活动。

可能很快任何人都能运行自己的高质量 LLM。成本将降低，速度在接下来的一年里将迅速增加。LoRA LLMs 很快将出现在智能手机上，这样推理可以在你的手机上进行，从而降低成本并提高速度。此外，竞争也会越来越激烈，包括像 [Llama2](https://ai.meta.com/llama/) 这样的开源项目，以及像 [PaLM](https://developers.generativeai.google/products/palm) 这样的闭源项目。

最后，模态的协同效应可以超越提供对整个应用程序 GUI 的随机访问。LLM（大语言模型）结合多个来源的能力预示着更好的帮助将会出现。一些有趣的文章：[多模态对话](https://masterofcode.com/blog/multimodal-conversation-design-tutorial-part-2-best-practices-use-cases-and-future-outlook)，[谷歌关于 GUI 和 LLM 的博客](https://ai.googleblog.com/2023/05/enabling-conversational-interaction-on.html)，[将 GUI 交互解释为语言](https://pure.tue.nl/ws/files/3655721/200612175.pdf)，[LLM 驱动的助手](https://nickarner.com/notes/llm-powered-assistants-for-complex-interfaces-february-26-2023/)。

# 结论

在本文中，你学会了如何应用函数调用来为你的应用程序启用语音功能。以提供的 [Gist](https://gist.github.com/hansvdam/8b9269390e16fa0bf394d7656bec1ea5) 为出发点，你可以在 Postman 或命令行中进行实验，了解函数调用的强大功能。如果你想在你的应用上运行一个语音启用的POC（概念验证），我建议将架构部分的服务器代码直接集成到你的应用中。整体来说，这归结为 2 次 HTTP 调用，一些提示构建和实现麦克风录音。根据你的技能和代码库，你将在几天内完成 POC 的搭建。

编程愉快！

在 [LinkedIn](https://www.linkedin.com/in/hans-van-dam-71a7866/) 或 [UXX.AI](https://uxx.ai) 上关注我

*除非另有说明，本文中的所有图片均由作者提供。*
