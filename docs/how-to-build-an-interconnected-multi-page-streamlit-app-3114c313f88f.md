# 如何构建一个互联的多页面 Streamlit 应用

> 原文：[`towardsdatascience.com/how-to-build-an-interconnected-multi-page-streamlit-app-3114c313f88f?source=collection_archive---------8-----------------------#2023-07-24`](https://towardsdatascience.com/how-to-build-an-interconnected-multi-page-streamlit-app-3114c313f88f?source=collection_archive---------8-----------------------#2023-07-24)

## 从规划到执行——我是如何构建 GPT 实验室的

[](https://medium.com/@dclin?source=post_page-----3114c313f88f--------------------------------)![Dave Lin](https://medium.com/@dclin?source=post_page-----3114c313f88f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3114c313f88f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3114c313f88f--------------------------------) [Dave Lin](https://medium.com/@dclin?source=post_page-----3114c313f88f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6b1d830863a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-an-interconnected-multi-page-streamlit-app-3114c313f88f&user=Dave+Lin&userId=6b1d830863a3&source=post_page-6b1d830863a3----3114c313f88f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3114c313f88f--------------------------------) ·9 分钟阅读·2023 年 7 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3114c313f88f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-an-interconnected-multi-page-streamlit-app-3114c313f88f&user=Dave+Lin&userId=6b1d830863a3&source=-----3114c313f88f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3114c313f88f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-an-interconnected-multi-page-streamlit-app-3114c313f88f&source=-----3114c313f88f---------------------bookmark_footer-----------)![](img/e492690c6892273adc45a13ebb488b40.png)

照片由 [Clément Hélardot](https://unsplash.com/@clemhlrdt?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上提供

*注意：本文最初发布于* [*Streamlit 博客*](https://blog.streamlit.io/how-to-build-an-interconnected-multi-page-streamlit-app/)*。我希望在这里与 Medium 社区分享。*

哇！自从我首次发布关于 [构建 GPT 实验室的经验教训](https://www.notion.so/Building-GPT-Lab-with-Streamlit-2b8e8694a1384373a0bf176506d79444?pvs=21) 的博客文章以来，这三个月真是令人难以置信！ 🚀

感谢你们的巨大支持，GPT Lab 已获得超过 9K 的应用查看次数、1150+ 的独特登录用户、900+ 次与助手的会话、650+ 个测试过的提示以及 180+ 个创建的助手。该应用还在 Streamlit 应用画廊中与其他优秀应用一起展示。

[`gptlab.streamlit.app/?embed=true`](http://gptlab.streamlit.app/?embed=true)

你们中的许多人问我，“你是如何规划和构建如此大型的 Streamlit 应用的？”迫不及待地想回答，我决定将 GPT Lab 开源。

在这篇文章中，我将分享关于这个雄心勃勃项目的策略和思维过程的见解。我希望这能激励你们将 Streamlit 推向极限，并将你们雄心勃勃的应用变为现实。

*💡 想跳到下一部分？查看* [*应用程序*](https://gptlab.streamlit.app/) *和* [*代码*](https://github.com/dclin/gptlab-streamlit)*。*

# 规划一个大型的 Streamlit 应用

构建大型 Streamlit 应用，例如 GPT Lab，需要仔细的规划，而不仅仅是将代码拼凑在一起。对于 GPT Lab，我专注于规划这四个关键方面：

1.  **功能和用户体验。** 应用程序将做什么？我们旨在提供什么样的用户体验？

1.  **数据模型。** 数据将如何持久化？应该存储在数据库中的内容与会话状态变量中的内容应该如何区分？

1.  **代码结构。** 应该如何构建应用以确保模块化、可维护性和可扩展性？

1.  **会话状态。** 需要哪些会话状态变量来链接用户界面？

理解这些方面提供了我所尝试构建的更清晰的视图，并提供了一个系统性处理复杂任务的框架。

让我们更详细地探讨每个方面。

# 功能和用户体验：创建初始规范和低保真用户体验模型

首先，我创建了一个简单的规范文档（或称为“规范”），概述了整体范围和方法。我还包括了一个站点地图，详细说明了我想支持的用例。该规范为我提供了明确的路线图以及衡量进展的手段。

这是原始规范的摘录：

> ***范围***
> 
> *构建一个平台，允许生成式 AI (GA) 机器人爱好者为他们的朋友和家人创建自己的基于 GPT-3 的聊天机器人。目标是验证足够多的 GA 机器人爱好者是否愿意构建他们的利基领域机器人。*
> 
> ***方法***
> 
> *一个公共的 Streamlit 网站，允许用户与四个预训练的教练机器人中的一个互动，或创建并与他们的机器人互动。*

与大多数开发项目一样，我做了一些修改。但原始的站点地图大部分保持不变，因为我能够实现大多数计划中的功能。

这是站点地图的最终版本：

```py
GPT Lab
│
├── Home
│
├── Lounge
│
├── Assistant
│   ├── Search for assistant
│   ├── Assistant details
│   ├── Active chat
│   └── Chat recap
│
├── Lab
│   ├── Step 1: initial prompt + model config
│   ├── Step 2: test chat
│   ├── Step 3: other configs
│   └── Step 4: confirmation
│
├── FAQ
│
└── Legal
    ├── Terms
    └── Privacy policy
```

我不能过分强调功能规划的重要性。它提供了一个路线图、衡量进展的方法以及思考数据模型的起点。

# 数据模型：确定模式

从一开始，我就认识到后端数据存储对于持久化用户、助手和会话记录至关重要。在考虑了我的选择后，我决定使用 Google Firestore，因为它具有可扩展性、实时能力和慷慨的免费层。为了支持未来的扩展，我战略性地设计了数据模型。尽管当前应用程序仅使用了其潜力的一部分，但可以在 GPT Lab 中添加提示版本控制。这将使用户能够编辑或恢复他们的助手。

*💡 注意：在应用程序后台和数据模型中，助手被称为机器人，尽管我之前坚持不在用户界面中称其为机器人 😅。*

现在，让我们深入探讨 GPT Lab 中的四个主要 Firestore 集合：users、user_hash、bots 和 sessions。

**用户和 user_hash**

用户集合是应用程序存储有关用户信息的地方。为了保护用户隐私，应用程序不会存储任何个人可识别信息（PII）。相反，每个用户仅与其 OpenAI API 密钥的单向哈希值相关联。每当用户创建助手或开始/结束与助手的会话时，指标字段会递增。这允许在应用程序内进行基本的分析收集。

```py
Users Collection
   |
   | - id: (Firestore auto-ID)
   | - user_hash: string (one-way hash value of OpenAI API key)
   | - created_date: datetime
   | - last_modified_date: datetime
   | - sessions_started: number
   | - sessions_ended: number
   | - bots_created: number
```

Google Firestore 不提供确保集合中文档字段值唯一性的方法，因此我创建了一个名为 user_hash 的单独集合。这确保每个唯一的 API 密钥只有一个关联的用户记录。每个用户文档唯一地关联到一个 user_hash 文档，每个 user_hash 文档可能与一个用户文档相关联。数据模型足够灵活，以适应未来更改 API 密钥的用户（用户可以使用旧的 API 密钥登录，然后更换为新的密钥）。

```py
User_hash Collection
   |
   | - id = one-way hash value of OpenAI API key
   | - user_hash_type: string (open_ai_key)
   | - created_date: datetime
```

**机器人**

机器人集合存储 AI 助手的配置。每个 AI 助手的核心是其大型语言模型（LLM）、模型配置和提示。为了在未来实现提示和模型配置的适当版本控制，model_configs 和 prompts 被建模为子集合（GPT Lab 的愿景之一是成为你提示的存储库）。

为了最小化子集合读取（以便无需不断查询子集合以获取活动记录），活动子集合的文档 ID 也在文档级别存储。session_type 字段指示助手是否处于头脑风暴或辅导会话中，这会影响会话消息的截断技术。

最后，当用户开始或结束与助手的会话时，指标字段会递增。

```py
Bots Collection
   |
   | - id: (Firestore auto-ID)
   | - name: string
   | - tag_line: string
   | - description: string
   | - session_type: number
   | - creator_user_id: string
   | - created_date: datetime
   | - last_modified_date: datetime
   | - active_initial_prompt_id: string
   | - active_model_config_id: string
   | - active_summary_prompt_id: string
   | - showcased: boolean
   | - is_active: boolean
   |
   v
   |--> Model_configs subcollection
   |     |
   |     | - config: map
   |     |     | - model: string 
   |     |     | - max_tokens: number 
   |     |     | - temperature: number 
   |     |     | - top_p: number 
   |     |     | - frequency_penalty: number 
   |     |     | - presence_penalty: number 
   |     | - created_date: datetime
   |     | - is_active: boolean
   |
   v
   |--> Prompts subcollection
         |
         | - message_type: string
         | - message: string
         | - created_date: datetime
         | - is_active: boolean
         | - sessions_started: number
         | - sessions_ended: number
```

S**essions**

sessions 集合存储会话数据。它包含两种类型的会话：实验室会话（用于测试提示）和助手会话（用于与创建的助手聊天）。为了减少对机器人文档频繁检索的需要，它的信息会在会话文档中缓存。这是有道理的，因为如果实现编辑助手的用例，机器人文档可能会出现偏差。

`messages_str` 字段存储发送到 OpenAI LLM 的最新有效负载。此功能允许用户恢复之前的助手会话。`messages` 子集合存储实际的聊天消息。注意，实验室会话聊天消息不会被存储。

为了确保用户的机密性和隐私，OpenAI 请求有效负载和会话消息在保存到数据库之前会被加密。这种数据模型允许用户重新启动以前的会话并继续与助手聊天。

```py
Sessions Collection
   |
   | - id: (Firestore auto-ID)
   | - user_id: string
   | - bot_id: string
   | - bot_initial_prompt_msg: string
   |
   | - bot_model_config: map
   |     | - model: string 
   |     | - max_tokens: number 
   |     | - temperature: number 
   |     | - top_p: number 
   |     | - frequency_penalty: number 
   |     | - presence_penalty: number 
   |
   | - bot_session_type: number
   | - bot_summary_prompt_msg: string
   | - created_date: datetime
   | - session_schema_version: number
   | - status: number
   | - message_count: number
   | - messages_str: string (encrypted)
   |
   v
   |--> Messages subcollection
         |
         | - created_date: datetime
         | - message: string (encrypted)
         | - role: string
```

通过从一开始就仔细考虑所有潜在的用例，我创建了一个未来-proof 的数据模型，能够满足应用程序不断发展的需求和功能。在接下来的部分中，我们将深入了解后端应用程序代码的结构，以了解它如何支持和实现这个强大的数据模型。

# 代码结构：为了可扩展性和模块化进行结构化

我创建 GPT Lab 是为了赋能那些技术水平低或没有技术技能的用户，使他们能够构建自己的基于提示的 LLM AI 应用，而无需担心底层基础设施。我的目标是最终提供后端 API，将用户的自定义前端应用（无论是否使用 Streamlit）与他们的 AI 助手连接。这激励我设计了一个解耦架构，将前端 Streamlit 应用与后端逻辑分开。

后端代码的结构如下：

```py
+----------------+     +-------------------+     +-------------------+     +------------+
|                |     |                   |     |                   |     |            |
|  Streamlit App |<--->| util_collections  |<--->| api_util_firebase |<--->|  Firestore |
|                |     | (users, sessions, |     |                   |     |            |
|                |     |  bots)            |     |                   |     |            |
+----------------+     +-------------------+     +-------------------+     +------------+
                             |
                             |
                             v
                     +-----------------+     +------------+
                     |                 |     |            |
                     | api_util_openai |<--->|   OpenAI   |
                     |                 |     |            |
                     +-----------------+     +------------+
```

模块如下：

+   **api_util_firebase** 处理与 Firestore 数据库的 CRUD 操作。

+   **api_util_openai** 与 OpenAI 的模型进行交互，向上游模型提供统一的聊天模型，修剪聊天消息，并尝试检测和防止提示注入攻击。

+   **api_util_users**、**api_util_sessions** 和 **api_util_bots** 是它们各自 Firestore 集合的接口。它们与 api_util_firebase 和 api_util_openai 进行交互，并实现 GPT Lab 特定的业务逻辑。

这种设计使得代码的不同部分可以独立开发、测试和扩展。它还建立了一个更简单的迁移路径，将后端 util_collections 模块转换为 Google Cloud Functions，这些函数可以通过 API Gateways 公开。

# 会话状态：管理 UI 和用户流程

正如在 [第一篇博客文章](https://blog.streamlit.io/building-gpt-lab-with-streamlit/#2-developing-advanced-uis-with-ui-functions-rendered-by-session-states) 中解释的，我使用了会话状态变量来控制和管理 Streamlit 页面上的功能。以下说明了这些变量在整个应用中的使用方式：

*home.py*

+   **用户** 控制是否渲染 OpenAI API 密钥模块

pages/1_lounge.py

+   **用户** 控制是否渲染 OpenAI API 密钥模块，启用助手选择，并显示我的助手标签页。

+   用户选择与助手互动后，助手详细信息会存储在 **bot_info** 中。

pages/2_assistant.py

+   **用户** 控制是否渲染 OpenAI API 密钥模块。

+   **bot_info**、**session_id** 和 **session_ended** 决定显示哪个屏幕变体。

+   **bot_info** 不存在：检查 assistant_id 是否在 URL 参数中。如果没有，则提示用户搜索助手。

+   **bot_info** 和 **session_id** 存在，并且 **session_ended** 为 false：显示聊天会话屏幕。

+   **bot_info** 和 **session_id** 存在，并且 **session_ended** 为 true：显示聊天会话回顾屏幕。

+   在聊天会话中，**session_msg_list** 存储对话内容。

pages/3_lab.py

+   **用户** 控制是否渲染 OpenAI API 密钥模块以及是否允许用户在实验室中开始创建助手。

+   **lab_active_step** 控制渲染哪个实验室会话状态：

+   如果为 1：渲染步骤 1 UI 以设置助手的初始提示和模型。

+   如果为 2：渲染步骤 2 UI 测试与助手的聊天。

+   如果为 3：渲染步骤 3 UI 完成助手详细信息。创建时，机器人记录会在 Firestore DB 中创建，并将文档 ID 保存到 lab_bot_id。

+   如果为 4 且 lab_bot_id 已设置：渲染步骤 4 UI 显示助手创建确认。

+   在测试聊天会话期间，**lab_msg_list** 存储测试消息。通过使用单独的 **lab_bot_id** 和 **bot_info**，我可以让用户在休息室/助手和实验室之间来回跳转，而不会丢失每个部分的进度。

在完成前期规划后，接下来的执行变得更加可控。

# 总结

在这篇文章中，我涵盖了创建 GPT 实验室所需的前期规划，包括功能、数据模型、代码和会话状态。我希望这能激励你构建自己的雄心勃勃的 Streamlit 应用程序。

通过 [Twitter](https://twitter.com/dclin) 或 [Linkedin](https://www.linkedin.com/in/d2clin/) 与我联系。我很期待你的反馈。

祝你使用 Streamlit 愉快！ 🎈
