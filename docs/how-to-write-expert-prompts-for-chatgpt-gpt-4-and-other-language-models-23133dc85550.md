# 如何为 ChatGPT（GPT-4）和其他语言模型编写专家提示

> 原文：[`towardsdatascience.com/how-to-write-expert-prompts-for-chatgpt-gpt-4-and-other-language-models-23133dc85550`](https://towardsdatascience.com/how-to-write-expert-prompts-for-chatgpt-gpt-4-and-other-language-models-23133dc85550)

## 与 LLMs 一起进行提示工程的适合初学者的指南

[](https://nabil-alouani.medium.com/?source=post_page-----23133dc85550--------------------------------)![Nabil Alouani](https://nabil-alouani.medium.com/?source=post_page-----23133dc85550--------------------------------)[](https://towardsdatascience.com/?source=post_page-----23133dc85550--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----23133dc85550--------------------------------) [Nabil Alouani](https://nabil-alouani.medium.com/?source=post_page-----23133dc85550--------------------------------)

·发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----23133dc85550--------------------------------) ·阅读 63 分钟·2023 年 11 月 1 日

--

![](img/1d2088f1c56684c0f65cee8b56f3ca2d.png)

作者通过 Midjourney 提供的图片。

提示工程是一种花哨的说法，意思是“为 AI 模型编写更好的指令，直到它完全按照你的要求执行”。

编写提示有点像骑自行车。你不需要在机械物理学上有博士学位才能学会保持平衡。一点理论可以帮助，但最重要的部分是反复试验。

把这个指南看作是为骑 AI 自行车做准备的“一点理论”。你会找到一系列技术，包括解释、示例和模板，你可以在你喜欢的模型上进行测试。

本指南侧重于 ChatGPT（GPT-4），但下面分享的每一种技术都适用于其他大型语言模型（LLMs），如 Claude 和 LLaMA。

# 目录

```py
 What's in this guide?

💡 Why should you care about Prompt Engineering?
💡 Why prompt is engineering harder than you think?
💡 You don't need prompt ideas, you need problems
💡 Watch out for AI hallucinations

🟢 The Basics of Prompting 
🟢 Specify the context (also called "Priming")
🟢 Specify the desired format
🟢 Use <placeholders>
  ∘ How to use placeholders as parameters
  ∘ How to use placeholders as instructions
🟢 Specify the style/tone
🟢 Specify the length of the desired response
🟢 Specify the target audience
🟢 Many-Examples Prompting
🟢 Temperature Control
🟢 Zero-Shot Prompting (no examples)
🟢 Few-Shot Prompting (several high-quality examples)
🟢 Zero-Shot/Few-Shot - The simple version

💡 In-context Learning vs. Chat History

🟡 Chain of Thought Prompting
  ∘ Zero-Shot Chain of Thought
  ∘ Few-Shot Chain of Thought
🟢 Role Prompting
🟢 Knowledge Generation Prompting
  ∘ Knowledge Generation Prompting and ChatGPT Plugins
🟠 Knowledge Integration Prompting* 
  ∘ Knowledge Integration* and Microsoft Edge
🟠 Directional Stimulus (DS) Prompting 
🟡 Recursive Criticism and Improvement (RCI) Prompting
🟡 Self-Refinement Prompting
🟡 Reverse Prompt Engineering
🟡 Prompt Revision
🟡 Program Simulation Prompting
🟠 All-In-One (AIO) Prompting*
🟢 Template for All-In-One (AIO) Prompting
🟢 More examples of All-In-One Prompting*
  ∘ Dolores, the Email Muse
  ∘ Robert Ford, the Coding Master
  ∘ Lee Sizemore, the Outlandish Chef
  ∘ Maeve the Mastermind

💡 Iterate until you have to revert
💡 With great power…

Conclusion

References (in no particular order)

Contact section
```

## 免责声明：

尽管本指南偏向于 ChatGPT，但我并没有个人利益来推广这个产品。我没有得到 OpenAI、微软或其他任何人的赞助。

# 这个指南包括什么内容？

本指南包括七个“好好知道”部分和超过 25 种提示工程技术。每个部分都以一个图标开头，表示其类型，如下所示：

+   💡 “好好知道”部分。

+   🟢 适合初学者的技术。

+   🟡 中级技术。

+   🟠 高级技术。

“好好知道”部分提供了在编写提示时要记住的具体要点。适合初学者的技术可以立即使用。中级和高级技术需要准备和一点耐心。

指南中讨论的所有技术都来源于 AI 研究论文和第一手经验。引用和直接链接可在指南末尾找到。有两种技术我重新命名，以便记忆。它们用星号标记，就像这样*。

每个提示的格式如下：

```py
**[Title of the prompt]** 

**Prompt content:** Content of the prompt that sometimes includes <placeholders>.
<placeholders> are a tool you use to make better prompt templates.

(Comments that are not part of the prompt are written between parenthesis like this) 

## (The two hashtags separate the same prompt into two or more sections)

--- (The three dashes separate examples inside the same prompt)

*** (The three stars separate two different prompts or two different outputs)

*** ChatGPT's output**: (When it's relevant, you'll see the response to ChatGPT written like this)
```

这里有一个例子：

```py
**[Examples of a simple prompts]** 

**Prompt example #1:** Act as an independant analyst who specializes in <field_1> and <field_2>.
Search the internet for the pros and cons of soy milk.

Use 5 bullet points for pros and 5 bullet points for cons.
Browse at least 6 different sources and cite each one of them in your analysis.

##

<field_1>: Food industry
<field_2>: Nutrition

***

**Prompt example #2:** Explain the five love languages as if you're talking to a 16-year-old.

##

**Desired format:**
Introduction.
- Love language #1: explanation and example.
- Love language #2: explanation and example.
- Love language #3: explanation and example.
- Love language #4: explanation and example.
- Love language #5: explanation and example.
Conclusion.
```

# 💡 为什么你应该关心提示工程？

生成式 AI 继续进入我们每天使用的每个数字工具中，如网络浏览器、社交媒体应用甚至 PowerPoint 演示文稿。语言模型越好，我们就会越多地使用它们。

提示工程是我们用来与 AI 模型交互以完成越来越多任务的语言。

把近期未来想象成一个外国国家。你知道你将在那里生活几年，但你不会说当地的语言。每次你购买食品、预订旅行或谈判交易时，你都必须雇佣人来替你说话。

听起来效率低下且不好玩，不是吗？

现在想象一下，在你搬到这个外国国家之前，你可以学习当地的语言。与其雇佣会说 AI 语言的人，你学会写提示。选择合适的词语，AI 将给你想要的东西——无论是电子邮件、代码还是月度报告。

但是等等，你可能会说。提示工程不是注定要消失吗？

可能是的，因为目标是使人类更容易使用 AI 系统。也许在未来，混乱的语音提示就足以创造你想象的任何东西。我们甚至可能开发出能够实时读取你的思维的 AI。

在这些情况下，提示工程成为一种临时技能。一旦 AI 能够根据不完整的句子和脑电信号猜测人类意图，它将变得过时。

你可以等待这种情况发生，或者你可以提前开始。

# 💡 为什么提示工程比你想象的更难？

简短的答案是“一种轻松的错觉”。

由于我们使用自然语言编写提示，我们不认为它是一项需要练习的复杂技能。你所要做的就是用简单的英语写出指令，对吗？

嗯，不完全是这样。

是的，当你与人类交谈时，你会以与提示 AI 相同的方式提示他们。人类和 AI 都使用内部模型来回应你的提示。只是人类和 AI 系统对现实有不同的模型。

人类模型依赖于认知能力、过去的经验、理论知识和实时感官数据。而语言模型则仅依赖于语言模式。

你可以说人类软件和 AI 软件运行在不同的操作系统上。

所以当你与语言模型交互时，就好像你在与一个外星人交互。当然，这个外星人会回应你的自然语言，但它的行为方式与人类不同，这意味着你需要调整你的提示。

与人类相比，AI 系统需要更多细节、精确的解释、清晰的指示和一些重复。你希望每个写出的词都是有意义的，就像你在编写代码一样。

“英语作为一种编程语言”的类比在考虑一个至关重要的细节之前是完美的。与代码不同，每个提示都会得到回应，无论其质量如何。即使包含拼写错误和不清晰句子的提示也会产生*某种东西*。相比之下，不正确的代码不会产生任何东西（除了令人沮丧的错误消息）。

这种总是产生结果的特性使得轻松的错觉更加强烈。混乱的提示也会产生结果，那么为什么要费心改进它们呢？

一般的 AI 用户不觉得有必要提高他们的提示工程技能，所以他们错误地认为这是一件容易的事情。

在某种程度上，提示工程就像跳舞。你在 TikTok 上看了一个专业舞者演示了一些嘻哈舞步的教程，你会觉得“哦，这其实很容易！”

注意这一举动看起来多么容易。

但是一旦你尝试重现那些“容易的步骤”，你会意识到你的身体有自己的物理规律。突然间，一边移动身体，一边挥舞着手臂似乎是不可能的。

就像跳舞一样，提示工程比看起来更难，但每写一个提示都会变得更容易。

# 💡 你不需要提示的想法，你需要问题

最好的提示来自好的想法，而好的想法源于有趣的问题。以下是一个简单的框架，你可以用它来找到值得解决的问题：

+   列出你在计算机上进行的所有任务（工作或其他）。

+   分离需要文本分析和/或撰写的任务。

+   描述你希望从每个任务中实现的结果。

**有用问题的例子：** “每个星期一，我向客户提交上周运行的每个广告的绩效报告。我需要评论每个关键绩效指标（KPI），以便非专业人士更容易理解数据。”

**提示如何帮助：** 撰写每个提交的 KPI 的提示。要求你的 AI 模型使用讲故事的技巧和有趣的类比，使报告更轻松易懂。

# 💡 小心 AI 幻觉

无论你选择哪种模型，记住聊天机器人经常会产生幻觉，写出事实错误的信息。这种行为源自 LLMs 在巨大的数据集上训练，这些数据集充满了逻辑错误和胡言乱语。

基于人类反馈的微调和强化学习（RLHF）有助于 LLMs 减少不准确性和偏见，但仍然存在漏洞。

我在其他文章中已经对幻觉问题进行了一些尝试。你可以在下面找到链接。现在，这里是一个预览：

> 你可能认为聊天机器人默认会说实话，有时候他们会产生幻觉。但更准确地说，聊天机器人写出纯粹的胡说八道，有时候与现实相匹配。
> 
> 通过“胡说八道”，我不是指我们用来描述无意义的俚语，而是哈里·弗兰克福提出的一个哲学定义。
> 
> 哈里·弗兰克福特将胡说八道描述为与现实没有关系的信息。当你撒谎时，你扭曲了现实。当你说真话时，你描述了你对现实的表达。但是当你胡说八道时，你编造了一些与现实无关的东西。
> 
> *“正是这种与真相关联的缺乏连接——* ***对事物真实情况的漠不关心*** *——我认为是胡说八道的本质。”*
> 
> *这指向了胡说八道的一个类似而基本的方面：* ***尽管它是在不关心真相的情况下产生的，但它不一定是错误的****。”*
> 
> *—* [*《胡说八道》哈里·弗兰克福特著*](https://www.goodreads.com/author/quotes/219.Harry_G_Frankfurt) *[Emphasis mine].*

语言模型是高度可信的胡说八道者，通常能够接近真相，因此你不能完全信任它们。在处理非虚构内容时，你必须对它们的输出进行事实核查。

[](https://nabilalouani.substack.com/p/artificial-disinformation-can-chatbots?source=post_page-----23133dc85550--------------------------------) [## 人工虚假信息：聊天机器人能破坏互联网上的信任吗？

### ChatGPT 发布后不久，我脑海中开始倒计时。就像当你看到一道闪电划过时...

nabilalouani.substack.com](https://nabilalouani.substack.com/p/artificial-disinformation-can-chatbots?source=post_page-----23133dc85550--------------------------------) [](https://nabilalouani.substack.com/p/chatgpt-hype-is-proof-nobody-really?source=post_page-----23133dc85550--------------------------------) [## 聊天 GPT 的炒作证明没有人真正理解人工智能

### 大型语言模型比你邻居的猫更笨

nabilalouani.substack.com](https://nabilalouani.substack.com/p/chatgpt-hype-is-proof-nobody-really?source=post_page-----23133dc85550--------------------------------)

# 🟢 提示的基础知识

每个提示都是你想要解决的问题和你的语言模型生成的内容之间的桥梁。你的桥的形状取决于你想要解决的问题，但基本结构保持不变。

想象一下这个结构就像是六根柱子：

1.  要具体明确。

1.  使用占位符<like_this>构建灵活的模板。（在专门的部分中详细介绍）。

1.  优先考虑**要做什么**而不是不要做什么。

1.  指定所需输出的格式。（在专门的部分中详细介绍）。

1.  使用双井号（##）来分隔提示的不同部分。提示可以包括说明、示例和所需格式。

1.  修改你的提示以去除废话。

这是一个例子：

```py
[The Basics of Prompting] 

**Bad prompt:** Summarize this text please. [Paste text here].

***

**Better prompt:** I will give you a report titled <title_of_the_report> as input. Please access the report through the following link <URL_of_the_report> using the online browsing feature. Summarize the report in less than <summary_wordcount> and add <number_of_quotes> from the authors. Make sure to pick precise quotes and list them as bullet points.

<title_of_the_report> = Walking for good health.
<URL_of_the_report> = https://www.betterhealth.vic.gov.au/health/healthyliving/walking-for-good-health
<summary_wordcount> = 250
<number_of_quotes> = 3

##

Desired format:

Title: <title_of_the_report> 
Link: <URL_of_the_report>

Summary of the report based on the previous instructions.

- Quote #1
- Quote #2
- Quote #3
- etc.
```

# 🟢 指定上下文（也称为“启动”）

对于你写的每个问题，你的大型语言模型可以生成成千上万个不同的答案。当你提供上下文时，你帮助你的 LLM 缩小了可能结果的范围。

假设你想要一个即将到来的一周中不无聊的饮食计划。添加你的饮食限制和个人偏好可以更有可能获得每餐相关的建议。

有多种方式可以在提示中引入上下文。这就像是为任务做心理准备，因此被称为“启动”。

```py
**[Specify the context]

Example #1:** I invited a Jewish/Muslim/Buddhist friend over for a week. Please provide a complete meal plan for 7 days and consider my friend's diet restrictions.

***

**Example #2:** I work as a developer in a tech startup and I helped build two apps that allow users to collaborate on shared documents. Kindly provide me with 10 ways I can highlight my achievements as a developer. Use a bullet-point format.

***

**Example #3:** I'm from India/Kenya/Egypt, and English is my third language. I have a C2 level on paper and I want to improve my pronunciation. Can you please suggest 5 ways I can enhance my spoken English? Be specific and concise.
```

# 🟢 指定所需格式

这个很简单。你只需要在提示中添加一句描述所需格式的句子。

这是一个你可以参考的列表：

+   简要说明；

+   文章和博客文章；

+   论文和研究论文；

+   短篇小说和创意写作作品；

+   诗歌和歌词；

+   通讯和新闻稿；

+   社交媒体帖子和标题；

+   广告和营销文案；

+   电子邮件模板和商务往来；

+   产品描述和评论；

+   教程和指南；

+   常见问题解答（FAQ）；

+   记录和采访；

+   报告和备忘录；

+   电影剧本和戏剧或播客脚本；

+   演讲和展示；

+   摘要和概述；

+   技术文档和手册；

+   教育材料，如教案或课程大纲；

+   观点文章和社论；

+   个人陈述、求职信和简历。

以下是如何在基本提示中引入格式的三个示例：

```py
**[Specify the desired format]**

**Example #1:** Kindly write a template of a technical resume for a Software Engineer who wants to pursue a career in Machine Learning.

***

**Example #2:** I'm a One Piece fan. Please help me write a script for an episode titled The Demon's Eye Finally Opens.

***

**Example #3:** Please summarize the following document in the form of a corporate memo.
```

**注意：**如果你使用非专业的语言模型生成法律合同，请确保请法律专家审核。

# 🟢 使用<占位符>

<像这样的>占位符可以帮助你实现两个独立的目标。

1.  使用<占位符>编写灵活的提示，可以接受不同的输入。你必须在提示中指示每个占位符的内容。在这种情况下，**占位符是一个参数**。

1.  使用空的<占位符>来说明所需格式。在这里，你不需要编写每个占位符的内容。你的 LLM 会猜测每个占位符代表什么，特别是当你使用像用户故事或求职信这样的已知框架时。在这种情况下，**占位符是一条指令**。

## 🟢 如何将占位符用作参数

```py
**[Use placeholders as parameters]**

**Context:** Use <placeholders> to write flexible prompts.

**Prompt example #1:**

Act like an expert developer in <name_of_the_input_programming_language> and <name_of_the_output_programming_language>. I will submit a few lines of <name_of_the_input_programming_language> in the chat, and you'll rewrite it in the <name_of_the_output_programming_language>.
Make sure to use a temperature of <temperature_value>.

##

<name_of_the_input_programming_language> = Python.
<name_of_the_output_programming_language> = JavaScript.
<temperature_value> = 0.

***

**Prompt example #2:**

Act like an expert developer in <name_of_the_input_programming_language> and <name_of_the_output_programming_language>. I will submit a few lines of <name_of_the_input_programming_language> in the chat, and you'll rewrite it in the <name_of_the_output_programming_language>.
Make sure to use a temperature of <temperature_value>.

##

<name_of_the_input_programming_language> = PHP.
<name_of_the_output_programming_language> = Python.
<temperature_value> = 0.3.
```

## 🟢 如何将占位符用作指令

```py
**[Use placeholders as instructions]**
 **Context:** User Story generation for a Product Owner.
 **Prompt:**

You'll act as a Product Owner for an app that provides international shipment services for factories and retailers. I will give you a description of several features, and you'll kindly format them in the User Story format indicated below.

- Modify an order within the 12 hours (fixed) that follow the submission.
- Lockscreen notifications for every step of the shipment.
- Summary of ongoing orders ranked by date, cost, country, and products.
- A history log of past orders ranked by date, cost, country, and products.
- Chatbot incon that opens a chat window inside the web page.
- "Call me" button.

##
 **Desired format:** 
/////// User Story #1: <name_of_user_story> ///////

As a <description_of_user>,
I want <functionality>,
So that <benefit>.

Acceptance criteria:

#1 Given <precondition>
When <action_taken>,
Then <expected_result>.

#2 Given <precondition>
When <action_taken>,
Then <expected_result>.

#3 Given <precondition>
When <action_taken>,
Then <expected_result>.

/////// End of User Story#1: <name_of_user_story> ///////

---

/////// User Story #2: <name_of_user_story> ///////

etc.

##

**Example of the desired output:** 

/////// User Story #1: Online Shopping Cart ///////

As a frequent online shopper,
I want to be able to easily add items to my shopping cart,
So that I can efficiently complete my purchases.

Acceptance criteria:

#1 Given that I am on a product page,
When I click the "Add to Cart" button,
Then the selected item should be added to my shopping cart.

#2 Given that I have multiple items in my shopping cart,
When I view my shopping cart,
Then I should see a list of all items in my cart along with their prices and quantities.

#3 Given that I want to adjust the quantity of an item in my cart,
When I update the quantity of the item and click "Update Cart",
Then the quantity of the item should be updated and the total cost should reflect the change.

#4 Given that I want to remove an item from my shopping cart,
When I click the "Remove" button next to the item,
Then the item should be removed from my cart and the total cost should be adjusted accordingly

#5 Given that I want to apply a coupon code to my order, 
When I enter the code during checkout,
Then the discount associated with the code should be applied to my order. notes.

/////// End of User Story#1: Online Shopping Cart ///////

---

/////// User Story #2: ..... ///////

etc.
```

# 🟢 指定风格/语气

每个聊天机器人都有一个默认的风格，由其创建者定义。例如，ChatGPT 听起来友好而细致，但你可以要求它改变语气以适应你的偏好和需求。

你甚至可以要求你的语言模型模仿一个虚构/真实人物的语气。通常，结果是 ChatGPT 试图模仿的人的夸张模仿。

以下是一些你可以选择的风格示例：

+   **通用风格：**正式、非正式、有说服力的、对话式的、讽刺的、戏剧性的、居高临下的、细微的、有偏见的、幽默的、乐观的、悲观的等。

+   **领域特定风格：**学术、法律、政治、技术、医学、新闻、科学、营销、创意、教学等。

+   **模仿真实人物的风格：**阿加莎·克里斯蒂、丹尼尔·卡尼曼、J.K.罗琳、詹姆斯·鲍德温、諫山創等。

以下是如何在提示中指定风格的示例：

```py
**[Specify the style/tone]**

**Prompt example #1:** In the style of a philosophy dissertation, explain how the finite interval between 0 and 1 can encompass an infinite amount of real numbers.

***

**Prompt example #2:** In the style of a New York Times op-ed, write a 1000-word article about the importance of dialogue.

***
 **Prompt example #3:** Write a mini-guide about the importance of pre-processing data in Machine Learning. Use the tone of Jessie Pinkman from Breaking Bad.
```

# 🟢 指定所需响应的长度

长度是你希望回应中包含的详细程度的代理。长度有时也是你在写特定格式（如推文、SEO 描述和标题）时必须考虑的约束。

以下是如何在提示中指定长度的三个示例：

```py
**[Specify the length of the desired response]**

**Prompt example #1:** In less than 280 characters, please explain String Theory.

***

**Prompt example #2:** Kindly write a Linkedin post where you make a case for why technology is at its best when it's invisible.

***

**Prompt example #3:** Please write five titles about the lack of skin in the game in Pascal's wager.
```

# 🟢指定目标受众

语言模型是在包括维基百科、研究论文和 Reddit 在内的各种来源中训练的数十亿字。每个来源都有自己的受众，每个受众都以不同的方式消费信息。

当你指定目标受众时，你告诉你的模型要调整内容、例子和词汇。

考虑一个关于运动好处的提示的两个潜在受众：一般成年读者和医学专业人士。

对于第一个受众，你希望你的语言模型使用易于理解的例子和简单的解释。相比之下，第二个受众希望你唤起研究并使用专业术语。

即使主题保持不变，期望的输出也可能完全不同。这就是为什么你要在提示中指明目标受众的原因。

以下是“运动好处”示例的提示：

```py
**[Specify the target audience]**

**Prompt that targets general adult readers:** Please explain the benefits of regular exercise in a way that is easy to understand for the general public.

***

**Prompt that targets medical professionals:** Please write a scientific article that targets medical professionals. The article discusses the physiological and psychological benefits of regular exercise. Make sure the article responds to the expectations of an audience of medical professionals.
```

人们在写提示时常犯的一个错误是将“风格”和“目标受众”视为相同的参数。实际上，风格决定了*文本听起来的样子*，而目标受众决定了*使用哪些词语*。

以下是如何在提示中介绍目标受众的另一组示例：

```py
[Specify the target audience]

**Example #1:** Explain to an audience of visual artists how Generative AI will impact their field of work.

***

**Example #2:** Write a tweet that promotes an article about AI-driven disinformation. The tweet targets people interested in technology, communication, and social media.

***

**Example #3:** Outline a fundraising campaign and kindly add actionable tips. The content will be sent to a group of non-profiters to help them improve their current methods.
```

# 🟢 Many-Examples 提示

我从[Simon Willison](https://twitter.com/simonw)那里偷了这个技巧，他使用 Many-Examples 提示来推动语言模型超越其舒适区。当 ChatGPT 和其他模型写出回应时，它们会预测最可能的答案，但最可能的答案往往是最不创造性的。

如果你要求你的模型耗尽常见答案的储备，它将别无选择，只能探索新的可能性。

Many-Examples 提示对涉及想象力的任务特别有用。以下是一些技术可以发挥作用的实例：

+   通过检查可能的变体列表来优化代码；

+   为新产品/功能进行头脑风暴；

+   激发产品/域名/公司的创意名称；

+   重新构造句子或创建口号；

+   找一个度假的目的地；

+   通过寻找要学习的新技能来升级你的简历；

+   寻找新的爱好；

+   根据个人喜好浏览书籍/电影/歌曲列表。（Bing 在这方面比 ChatGPT 更有效）；

+   探索同一主题的多元化观点；

+   为给定主题生成尽可能多的信息。在这里，你可以将 Many-Examples 提示与知识生成结合起来（稍后会详细介绍）。

以下是你可以使用的两种基本 Many-Examples 提示的公式：

```py
**[Many-Examples prompting]**

**Prompt option #1:** Please provide a list of 50 distinct answers to the following question.
Question: <the_question>

##

<the_question>: Why does my inner voice sound like an arrogant douchebag?

***

**Prompt option #2:** Please provide a list of 20 distinct answers to the following question.
Question: <the_question>

##

<the_question>: What's the fastest way to become a certified Data Analyst?

(Let ChatGPT respond)

**Follow-up prompt #1:** Please add 5 more distinct answers.

(Let ChatGPT respond)

 **Follow-up prompt #2:** Please add 5 more distinct answers.
```

当第一个选项失败时使用第二个选项。有时当你要求大量的例子时，ChatGPT 会抱怨。它会说类似于：“作为一个 AI 语言模型，我不知道 50 个不同的可能答案。但是，我可以为你提供一些有用的答案列表。”

从那里开始，ChatGPT 将停在 20 个可能的答案上，但你可以使用“请再添加 5 个不同的答案”来保持生成的连续性。一旦相同的答案不断出现，这意味着你已经耗尽了模型的创造力。

# 🟢 温度控制

温度是影响语言模型生成的响应“随机性”的参数。它通常范围从 0 到 1，但在某些情况下，你可以将温度提高到 1 以上。

+   低温（在 0.1 和 0.3 之间）产生最可能的响应。换句话说，你会得到最“保守”的输出。**在生成代码时，低温特别有用，因为你会得到最稳定的输出。**

+   更高的温度（在 0.7 和 0.9 之间）会导致更有创意的响应。

记住使用温度的一种方法：“冷用于代码；热用于散文。”以下是你可以在提示中介绍它的方式：

```py
[Temperature control]

**Example #1:** At a temperature of 0.7, please explain why banana bread is called "bread" and not "cake" even though it tastes like a cake.

***

**Example #2:** Write a Python script that transposes a 10x10 matrix. Please provide two versions of the code where the first is generated at a temperature of 0 and the second at a temperature of 0.4.

***

**Example #3:** Act like an expert developer in <name_of_the_programming_language>. I will submit a few lines of code in the chat, and you'll review the code, then perform the following 7 tasks in the specified order defined below. When you write code, always use a temperature of <temperature_value>.

1\. Look for errors and explain them.
2\. Correct the errors.
3\. Optimize the code.
4\. Add comments to explain the purpose of each line.
5\. Format the code to make it easier to read.
6\. Make sure to reason step by step to be sure you arrive at the right answers.
7\. Comment on every single step you make.

##

<name_of_the_programming_language> = Python.
<temperature_value> = 0.
```

# 🟢 零样本提示（没有例子）

零样本提示是为你的 AI 模型编写指令，而不提供上下文或例子。零样本的基本格式通常包括通常称为“文本”和“期望结果”的两部分。

以下是两个零样本提示的例子：

```py
**[Zero-shot prompting]**

**Prompt example #1:**

**Question:** Two years ago, Jotaro was three times as old as his brother was. In three years’ time, Jotaro will be twice as old as his brother. How old is Jotaro?
**Answer:**

***

**Prompt example #2:**

**Text:** My favorite part of networking events is to stand next to the buffet table and watch people struggle to impress each other.
**Tone:**
```

这种特定格式的零样本提示在 AI 实验室之外很少见，专家们在那里使用这种技术来测试他们的模型的能力。

零样本提示的最常见格式是你自然而然地使用的格式。你只需输入你的问题。你不需要“文本+期望输出”的格式。这是因为像 ChatGPT 和 Bard 这样用户友好的模型都经过了对话优化 —— 每一次对话都是一次零样本。

你可以说聊天机器人是零样本机器。

# 🟢 少样本提示（几个高质量的例子）

少样本提示也被称为上下文学习。你给你的语言模型一堆高质量的例子来改善它的“猜测”。例子的数量取决于你的模型，但你可以从三到五个输入开始。

以下是一个例子：

```py
**[Few-shot prompting]**

**Prompt:**

**Text #1:** My favorite part of networking events is eating all the food while everybody is busy trying to impress each other. 

**Tone #1:** Playful.

##
 **Text #2:** She stormed out of the conference room, slamming the door behind 42 staff members who instantly turned their eyes away as if ashamed of their lack of courage.

**Tone #2:** Angry.

##

**Text #3:** Do you think they noticed the missing "export" feature in today's demo? I'm pretty sure Nabil whispered something into the client's ear. I really don't like that bald dude!
**Tone #3:** Anxious.

##

**Text #4:** Wait, what the hell is a dish washer? And why aren't there any horses attached to your chariot?

**Tone #4:**
```

不需要为每个例子添加编号（如#1、#2、#3），但这样做可以改善输出。你想要添加到你的例子中的另一个元素是“噪音”。

噪音是对你的语言模型给定的任务没有用的信息。在“语气”例子中，我引入了误导性的句子来混淆系统，并迫使它专注于“信号”。

如果你让任务对你的语言模型来说太明显，当面对复杂的例子时，它可能表现不佳。

# 🟢 零样本/少样本 — 简单版本

如果你想记住零样本和少样本中的一些东西，记住以下内容：

+   **当你的语言模型无法给出你想要的响应时，向你的提示添加高质量的例子。**

下面是少量提示如何帮助你改进 ChatGPT 的输出的示例。

```py
**[Zero-shot/Few-shot - The simple version]**

**Attempt #1 (Zero-shot)**

**Text:** My favorite part of networking events is to stand next to the buffet table and watch people struggle to impress each other.

**Tone:** 

*** ChatGPT's output:** "The tone of this text is casual and light-hearted. The speaker seems to be expressing a personal preference for the food at network events, and is making an observation about the behavior of the attendees in a somewhat playful manner."

(The output doesn't match my expectation because I wanted a one-word answer)

***

**Attempt #2 (Few-shot)**

**Text:** She stormed out of the conference room, slamming the door behind all 42 attendees, many of whom instantly turned their eyes away as if ashamed of their lack of courage.

**Tone:** Angry.

##

**Text:** Do you think they noticed the missing "export" feature in today's demo? I'm pretty sure Nabil whispered something into the client's ear. I really don't like that bald dude!

**Tone:** Anxious.

##

**Text:** My favorite part of network events is to stand next to the buffet table and watch people struggle to impress each other.

**Tone:**

*** ChatGPT's output:** "Amused."
```

# 💡 上下文学习 vs. 聊天历史

每种语言模型的第一个可用版本通常是万金油。它可以以一种平均水平执行各种任务。如果你想让你的模型专业化（从而提高其输出），你有两个选择。你可以使用新的特定数据对其进行重新训练，或者使用上下文学习。AI 人员通常会两者结合使用。

上下文学习是一种提示技术，可以让你引导语言模型的响应朝特定方向发展。你只需要一些示例，就像少量提示一样。

AI 专家喜欢上下文学习的原因是效率。你不需要使用大量高质量的数据来调整原始模型，只需使用非常有限数量的格式良好的示例即可。

这是普林斯顿大学发布的上下文学习摘要。

> 上下文学习在原始 GPT-3 论文中被推广为一种只使用少量示例就能让语言模型学习任务的方法。[[1]](http://ai.stanford.edu/blog/understanding-incontext/#f1)
> 
> 在上下文学习期间，我们给语言模型一个提示，其中包含一系列展示任务的输入-输出对。在提示的末尾，我们附加一个测试输入，并允许语言模型仅通过对提示进行条件化并预测下一个标记来进行预测。
> 
> 要正确回答下面的两个提示，模型需要阅读训练示例以找出输入分布（财经或一般新闻）、输出分布（积极/消极或主题）、输入-输出映射（情感或主题分类）和格式。

![](img/97294ea04f70fdc5faf9722c5942496f.png)

上下文学习的例子。[来源](http://ai.stanford.edu/blog/understanding-incontext/)

你可以从上下文学习中得到许多应用，比如生成代码、自动化电子表格和许多其他面向文本的任务。

然而，ChatGPT 又是另一回事。OpenAI 牺牲了 ChatGPT 使用上下文学习的能力，引入了一个新功能：聊天历史。当然，你会失去模型的灵活性，但你会得到一个用户友好的界面，可以进行长时间的对话。

你可以认为聊天历史是上下文学习的一种变体，因为 ChatGPT 的响应会根据你使用的聊天历史标签的内容而演变。例如，如果你将一系列食谱输入到 ChatGPT 标签中，它将能够对你的输入执行特定任务*。* 这涉及摘要、延续和编辑。

这为什么重要？

根据你的需求和未来的发现，你可能需要选择以下两个选项中的一个：

1.  使用上下文学习来微调像 GPT-4、OpenLLaMa 或 Falcon 这样的“原始”模型。换句话说，你可以创建一个定制的聊天机器人，但这个过程可能会很繁琐。

1.  利用“记忆”和“长对话”来使用聊天历史。这样更容易定制你的输出，但随着时间的推移，质量可能会下降。

# 🟡 思维链提示

思维链（CoT）提示意味着在最终响应之前告诉你的语言模型**逐步推理**。就好像你要求你的模型大声思考一样。

假设我让你计算 4x3。你可以立即在脑海中计算这个操作，并说：“12”。但如果我要求你使用“思维链”，你会将你的推理分成四个步骤。

1.  4x3 = 4+4+4

1.  4+4+4 = (4+4) + 4

1.  (4+4) + 4 = 8+4

1.  8+4 = 12

CoT 提示通常用于解决逻辑谜题。其思想是将复杂的问题分解为更小、更易管理的问题。

语言模型预测单词序列中的下一个标记，当它们处理训练数据中丰富的常见模式时，它们的预测更准确。但有时，你需要利用不常见的模式来回答不常见的问题。

考虑以下谜题：“如果鸡蛋一打卖 0.12 美元，你可以用一美元买多少个鸡蛋？”

如果你强迫 ChatGPT 立即给出答案，它会写道：“你可以用一美元买 10 打鸡蛋”，这是一个错误的答案。

![](img/7a46f56a22764a947e872e1b4cd6744b.png)

提示强制 ChatGPT 立即给出答案（没有思维链）。

现在，如果你要求 ChatGPT 逐步推理，它会给出一个不同的答案——正确的答案。

![](img/56c5edbad984077a2d309882e6a4b28b.png)

ChatGPT 的最新版本通常（但不总是）在响应提示时使用 CoT。

以下是另一个例子，说明了标准提示和思维链之间的区别。

![](img/6673d19d2c756c392c58499590e51e37.png)

注意第二个输入中思维链的添加（以蓝色突出显示）。[来源](https://learnprompting.org/docs/intermediate/chain_of_thought#fn-1)

你可以使用思维链提示的两种方式。

## 🟢 零提示思维链

在你的提示末尾添加一句话，让你的语言模型应用 CoT。我发现的表现最好的句子是：

+   **“….让我们逐步思考。”**

+   **“….请逐步进行，确保你得到正确的答案。”**

以下是如何在你的提示中加入它们：

```py
**[Zero-shot Chain of thought prompting]**
 **Prompt example #1:** If eggs are $0.12 a dozen, how many eggs can you get for a dollar?
Let's think step by step.

***

**Prompt example #2:** A bat and a ball cost $1.10 in total. The bat costs $1.00 more than the ball. How much does the ball cost?
Make sure to reason step by step to be sure you arrive at the right answer.
```

通常，零提示思维链足以解决逻辑谜题。但如果你的语言模型失败了，你可以尝试第二种思维链提示。

## 🟡 **少量提示思维链**

就像标准的少量提示一样，你需要在提交问题之前向你的语言模型提供高质量的示例。每个示例必须包括多个推理步骤，而且你添加的逻辑步骤越多，响应就越好。

以下是一个结合了少量提示和思维链的提示的例子：

```py
**[Example of Few-Shot Chain of Thought prompting]

Prompt:**

**Question #1:** Walter and Skyler want to compare the duration they need to shop for groceries.
Skyler takes 30 minutes to buy her weekly groceries. Walter takes five times as long as Skyler when completing the same task. How many hours does it take Walter to buy his weekly groceries?

**Answer #1:** Let's break down the problem step by step. It takes Walter "30*5 = 150" minutes to climb the hill.
It takes Walter "150/60 = 2.5" hours to shop for gorceries.
So the answer is "Walter needs 2 hours and a half to buy his weekly groceries."

##

**Question #2:** Hanks is a baseball fan. He went out to buy a new ball and a new bat. After an exchange with the shop owner, Hanks learned it would cost him 110$ to buy the ball and the bat. He also learned the baseball bat costs 100$ more than the ball.
How much does the ball cost?

**Answer #2:** Let's break down the problem step by step. We will call the price of the baseball bat "B" and the price of the ball "b".
We know that "B+b = 110".
We also know that "B = b+100".
This means "b+100+b =110" which translates into"(2*b) + 100 = 110."
We can then infer that "2*b = 10" and that "b = 5."
So the answer is "The ball costs 5$."

##

**Question #3:** Flynn has 200 letters he must divide into 4 equal groups corresponding to 4 different cities. Each group of letters must be divided into 5 equal sub-groups.
How many letters will Flynn have in each sub-group?

**Answer #3:**

*** ChatGPT-4's output:** First, let's find out how many letters are in each group corresponding to a city. Since Flynn has 200 letters and needs to divide them into 4 equal groups, we have "200/4 = 50" letters in each group.
Now, Flynn needs to divide each group of 50 letters into 5 equal sub-groups. To do this, we divide "50/5 = 10" letters in each sub-group.
So the answer is "There will be 10 letters in each sub-group."
```

# 🟢 角色提示

为你的语言模型分配一个特定的角色有助于它捕捉更多和更好的语义关系（即：逻辑和含义）。

在某种程度上，角色提示帮助你促使你的模型专注于其训练数据中的特定信息。这是一种一次性指定多个变量的快捷方式，比如上下文、风格、观点和词汇。

根据手头的任务，你可以使用不同版本的角色提示。以下是一些可能会启发你的例子。

+   模仿个人风格。

+   模仿像律师或战略规划师这样的特定专业知识。

+   模仿你在对话中的对应者，比如你的教授、前任或老板。

+   生成多个观点。

+   表现得像一个纠正拼写错误、编译你的代码或生成 Excel 公式的迷你应用程序。

```py
[Role Prompting]

**Prompt example #1 - Mimic a specific person:** Please respond to my questions as if you were Richard Feynman.

***

**Prompt example #2 - Emulate specific expertise:** From now on, act as a business analyst.
I'll provide you with a detailed business model, and you'll kindly respond with a list of actions I can take to make improvements.

***

**Prompt example #3 - Emulate your counterpart in a given conversation:** I want you to help me practice a difficult conversation.
Pretend to be my boss/ex-husband/sister/recruiter. I'll specify the topic, and you'll respond accordingly. Please make sure to ask difficult questions.

***

**Prompt example #4 - Generate multiple points of view:** I'll ask you questions about the role of ethics in technology.
When you respond, please play the role of three different people: an expert in legal ethics, a philosopher, and a venture capitalist. For each question, give a detailed perspective from each participant.

***

**Prompt example #5 - Behave like a mini-app:** You'll act as a generator of Excel formulas.
I'll provide you with a description of a task I want to accomplish on an Excel column, and you'll kindly respond with one or many formulas that achieve the desired goal. Please add explanations to each formula you generate.
```

我们将在一个名为“全能提示”的特定部分中探讨角色提示的高级版本。

# 🟢 知识生成提示

知识生成提示的目标是让你的语言模型从其庞大的训练数据中检索特定的信息片段。想象一下这种技术就像是在写最终回答之前要求你的模型进行一些研究。

假设你想让你的模型写一篇关于在阳台上种花的博客文章。与其立即要求你的模型写博客，不如提示它生成关于园艺、花卉和空间管理的关键要点。

一旦你得到了期望的关键点，一定要履行你的事实核查职责。从那里，提示你的模型使用它生成的“知识”来撰写一篇文章。

知识生成可以提高输出质量，因为它迫使你的模型专注于特定的要点，而不是试图回答模糊的提示。

这是如何将知识生成引入到你的提示中的方法：

```py
**[Knowledge Generation prompting]**

**Prompt Example #1:** Act like an expert horticulturist who specializes in maintaining balcony gardens. Generate key facts about growing flowers under Hamburg's weather, and managing space on your balcony. Add sources and quotes for each point.

Use the generated information to write a 2000-word blog post about how to grow flowers on your balcony for people who live in Hamburg.

***

**Prompt example #2:** Act like an expert personal trainer. List the top 20 techniques of total-body stretching and add a detailed description of how to perform each technique.

I will then pick a sublist of those techniques, and you'll kindly provide me with a bi-weekly stretching routine based on my choices.

***

**Prompt example #3:** Retrieve historical facts about the rise and fall of Carthage. Include dates, names, and current geographical locations.

From there, kindly write an essay about the relationship between Carthage and the Levant.
```

## **知识生成提示和 ChatGPT 插件**

你可以使用 ChatGPT 插件来生成知识并帮助核实事实。一定要尝试尽可能多的插件，因为它们中的大多数仍然有些笨拙。

# 🟠 知识整合提示*

知识生成提示的主要弱点是时间表。GPT-4 的训练数据止于 2021 年 9 月，这意味着之后出现的所有内容对模型来说都是未知的。

截止日期对于处理园艺、写作和烹饪等永恒话题并不是问题，但如果你追求最新信息，你需要一个补充的技巧。

你可以使用插件、带有在线浏览功能的聊天机器人，或者知识整合提示。

你所需要做的就是将最新的数据输入到你的模型中，以帮助它跟上新闻。在某种程度上，你让你的离线模型整合新的知识。

对于 API 用户，[GPT-4 可以处理高达 32,000 个标记](https://www.semrush.com/blog/gpt-4/)，大约相当于 25,000 个单词。这包括用户提示和答案。对于 ChatGPT Plus 的用户，GPT-4 可以接受高达 4096 个标记的输入，大约相当于 3,000 个单词。

你可以使用这 3,000 个单词和聊天记录功能来“教导” ChatGPT-4 新的信息。模型本身不会整合数据，但你可以生成利用刚刚添加的“新信息”的提示。

下面是你可以应用知识整合提示的框架：

+   找到相关的来源，比如研究论文或已记录的文章。

+   确定手头论文中最具信息量的部分。

+   将这些部分切成 3,000 个单词的片段。

+   将这些片段输入到 ChatGPT-4 中，并要求它用简单的话语解释每个部分。你也可以要求引用和例子。

+   使用 ChatGPT-4 的输出进行新的提示。

## 例如：

假设你是一位专门研究大型语言模型的人工智能研究员。你目前的任务是引用与你的论文相关的材料。

你找到了一篇有趣的论文，标题为*语言模型可以解决计算机任务*。在浏览你上周收藏的其他 122 篇论文之前，你想先做笔记。

以下是你可以遵循的步骤，让 ChatGPT 帮助你快速记录笔记。

+   首先，确定你想要总结的段落。在这个例子中，我们将选择讨论部分，大约有 1,000 个单词。

![](img/f0a39e870bbe222ab75222daf50028f4.png)

[来源](https://arxiv.org/abs/2302.08399)。

+   将这些冗长的段落切成 3,000 个单词的片段（在这个例子中不需要）。

+   将这些文本片段输入到 ChatGPT 中。

+   要求 ChatGPT 写出你提供的文本的摘要。

+   重复这个过程，总结你想要概括的所有论文。

+   不要忘记事实核查。

+   使用你新创建的阅读笔记找出共同的线索，并对抗相反的结果。

实际上，框架看起来是这样的：

```py
**[Knowledge Integration]**

**Context:** Comparing notes from two research papers.
[All of the prompts below must be performed in the same chat history tab/session].

**Prompt #1:** Hey ChatGPT, I'll submit a few paragraphs from a research paper. Please memorize the whole text and then summarize it in bullet points.

##

Desired format:
Title of the paper: <paper_title>
URL of the paper: <paper_URL>
/////
Summary:
- Point #1
- Point #2
- Point #3
- etc...

##

<paper_title>: Theory of Mind May Have Spontaneously Emerged in Large Language Models
<paper_URL>: https://arxiv.org/abs/2302.02083
<text_to_summarize>: (I inserted around 1,000 words here)

*** ChatGPT-4's output #1:** 

Title of the paper: Theory of Mind May Have Spontaneously Emerged in Large Language Models
URL of the paper: https://arxiv.org/abs/2302.02083
/////
Summary:

- GPT-3 and later versions show increasing ability to solve Theory of Mind (ToM) tasks, with GPT-4 performing even better
- ToM-like ability likely emerged spontaneously and autonomously, as a byproduct of models' increasing language ability
- Two potential explanations: models developed ability to impute unobservable mental states to others (ToM), or they discovered unknown language patterns to solve ToM tasks without engaging ToM
- If language patterns are responsible, it may necessitate re-examination of ToM tasks and research conclusions
- Studying AI performance on ToM tasks could provide insights into human cognition and neural structures that enable similar abilities

***

**Prompt #2:** Hey ChatGPT, I'll submit a few paragraphs from a research paper. Please memorize the whole text and then summarize it in bullet points.

##

Desired format:
Title of the paper: <paper_title>
URL of the paper: <paper_URL>
/////
Summary:
- Point #1
- Point #2
- Point #3
- etc...

##

<paper_title>: Large Language Models Fail on Trivial Alterations to Theory-of-Mind Tasks
<paper_URL>: https://arxiv.org/abs/2302.08399
<text_to_summarize>: (I inserted around 1,000 words here)

*** ChatGPT-4's output #2:** 

Title of the paper: Large Language Models Fail on Trivial Alterations to Theory-of-Mind Tasks
URL of the paper: https://arxiv.org/abs/2302.08399
/////
Summary:

- Large Language Models (LLMs) like GPT-3.5 may not have spontaneously developed Theory-of-Mind (ToM) as they fail on simple alterations to ToM tasks
- Encourage a skeptical stance when assessing the claim that LLMs have developed ToM, due to the human tendency to anthropomorphize agents
- ToM may be better achieved by integrating computational models that directly capture the ability with language models, rather than expecting it to emerge spontaneously
- Concern: once a systematic generator of examples or a benchmark is provided, LLMs could pass without answering the basic question of what they have learned
- The evaluation of generalization in machine-learning models is a broader issue, but ToM is a particularly relevant example

***

**Prompt #3:** Act as a research assistant specializing in <field>.
Use a scientific approach to compare the content of the previous papers based on their summaries.
Write the results in bullet points followed by precise details.

##

<field>: Deep Learning.

*** ChatGPT-4's output #3:**

1\. Theory of Mind emergence in large language models:

Paper #1 claims that Theory of Mind (ToM) may have spontaneously emerged in large language models like GPT-3 and GPT-4 as they show an increasing ability to solve ToM tasks
Paper #2 argues that large language models, such as GPT-3.5, probably have not developed ToM since they fail on simple alterations to ToM tasks

2\. Methodological concerns:

Paper #1 suggests that ToM-like ability might have emerged spontaneously and autonomously in large language models as a byproduct of their increasing language ability
Paper #2 highlights the concern that providing a systematic generator of examples or a benchmark may help models pass ToM tests without truly understanding ToM

3\. Skepticism and anthropomorphism:

Paper #2 encourages adopting a skeptical stance when assessing claims that large language models have developed ToM, as humans may be biased to anthropomorphize agents

4\. Implementation of Theory of Mind in AI:

Paper #1 raises the possibility that AI models may develop mechanisms similar to those employed by the human brain to impute mental states to others, boosting our understanding of AI and human cognition
Paper #2 suggests integrating computational models that directly capture ToM with language models, rather than expecting ToM to emerge spontaneously from additional linguistic data

5\. Evaluation of generalization in machine-learning models:

Paper #2 points out that the evaluation of generalization in machine-learning models is a concern for many researchers, with ToM being a particularly troubling example

***

**Prompt #4:** Act as a research assistant specializing in <field>.
Use a scientific approach to compare the content of the two previous papers based on their summaries.
Write the results in bullet points followed by precise details.

##

<field>: Congnitive Psychology.

*** ChatGPT-4's output #4:**

1\. Assertion of Theory of Mind (ToM) in Large Language Models (LLMs):

    Paper #1 suggests that GPT-3 and later versions, including GPT-4, show an increasing ability to solve ToM tasks, hinting that ToM-like ability might have spontaneously emerged.
    Paper #2 counters this by stating that LLMs like GPT-3.5 fail on simple alterations to ToM tasks, implying they may not have genuinely developed ToM.

2\. Emergence of ToM in LLMs:

    Paper #1 proposes that the ToM-like ability might have arisen spontaneously and autonomously as a byproduct of the models' increasing language abilities.
    Paper #2 encourages skepticism regarding this claim, highlighting the potential for humans to anthropomorphize agents and erroneously attribute ToM to them.

3\. Explanations and Mechanisms behind ToM-like Abilities in LLMs:

    Paper #1 offers two potential explanations: models might have developed the ability to impute unobservable mental states to others, or they might have identified unknown language patterns that enable them to solve ToM tasks without genuinely engaging ToM.
    Paper #2 doesn’t directly counter this but suggests integrating computational models that directly capture ToM abilities with language models might be a better approach than expecting spontaneous emergence.

4\. Implications for Theory of Mind Research:

    Paper #1 mentions that if mere language patterns enable the ToM task-solving, then there might be a need to re-examine ToM tasks and research conclusions derived from them.
    Paper #2 warns of a concern where, if a systematic generator of examples or a benchmark for ToM is provided, LLMs could pass the tasks without truly addressing the fundamental question of their understanding of ToM.

5\. Broader Relevance and Implications:

    Paper #1 indicates that observing AI performance on ToM tasks might provide insights into human cognition and the neural structures enabling similar abilities.
    Paper #2 places emphasis on the broader issue of evaluating generalization in machine-learning models, using ToM as a particularly relevant example.

In conclusion, while both papers discuss the capabilities of Large Language Models (LLMs) regarding Theory of Mind (ToM) tasks, they present contrasting viewpoints. Paper #1 leans more towards the possibility of LLMs developing ToM abilities, while Paper #2 is more skeptical, emphasizing caution in interpreting LLM performance on ToM tasks.
```

**注意：**如果最终输出太长，ChatGPT 将在中途停止写作。在这种情况下，你可以提示它使用“继续”这个词，它将从被切断的地方继续写作。

## 🟢 **知识整合* 和 Microsoft Edge**

在使用知识整合提示时，你可以使用 Microsoft Edge 的“聊天”功能来提高效率。

你可以打开 Edge 中的网页或 PDF，并使用聊天功能来总结内容，而不是自己浏览材料。然后，将总结注入到 ChatGPT 中，并用它进行另一个提示，就像我们在前面的例子中看到的那样。

下面是你可以使用 Microsoft Edge 总结文档的提示：

```py
**[Prompt to generate summaries using Microsoft Edge's Chat feature]**

**Prompt:** Summarize this paper. Start the summary with the title of the paper and its URL. Then list the main ideas in bullet points. Please illustrate the main ideas with examples extracted from the paper.
```

# 🟠 方向刺激（DS）提示

DS Prompting 的原始版本包括两个步骤：

1.  训练一个语言模型，从给定的文本中生成**特定类型的提示**。

1.  使用**另一个语言模型**来使用第一个语言模型生成的提示来总结相同的文本。

对于本指南，我们将使用 DS Prompting 的简化版本，在这个版本中，我们**使用相同的模型执行两项任务**。

![](img/f78e881ebb239c356e1eecd86b892c54.png)

标准提示和方向刺激提示的比较。[来源](https://arxiv.org/abs/2302.11520)

DS 提示的最佳（也许是唯一的）用例是总结研究论文和其他类似学术文本。

假设你想要总结一篇名为《通过定向刺激提示引导大型语言模型》的论文。以下是你想要遵循的步骤：

+   确定你想要总结的段落。

+   将这些冗长的段落切成你的语言模型可以处理的块。（在 ChatGPT-4 的情况下为 3,000 个单词）。

+   将文本块输入 ChatGPT。

+   要求 ChatGPT 写一条关于涉及关键信息（如姓名、地址、日期、地点和事件）的论文的提示。

+   使用 Few-Shot 提示向 ChatGPT 展示所需的输出。

+   一旦你得到了“提示”，对其进行事实核查，并根据需要调整你的 Few-Shot 提示。

+   在新标签页中提交你的“提示”生成提示。

+   运行提示以获得你想要的文本的“提示”。

+   在同一个标签页中，提示 ChatGPT 使用“提示”来总结最后提交的文本。

这是实践中的定向刺激提示的样子：

```py
**[Directional Stimilus Prompting]**

**Prompt #1** **[Few-shot examples of how to generate a specific hint]**

Hey ChatGPT, I'll give a text and you'll kindly summarize it into a hint. The hint is two succint sentences that involve key points only. Key points can be dates, numbers, comparaisons, names, and places.

##

Text #1: Article: (CNN) For the first time in eight years, a TV legend returned to doing what he does best. Contestants told to "come on down!" on the April 1 edition of "The Price Is Right" encountered not host Drew Carey but another familiar face in charge of the proceedings. Instead, there was Bob Barker, who hosted the TV game show for 35 years before stepping down in 2007\. Looking spry at 91, Barker handled the first price-guessing game of the show, the classic "Lucky Seven," before turning hosting duties over to Carey, who finished up. Despite being away from the show for most of the past eight years, Barker didn't seem to miss a beat.
Hint #1: Bob Barker returned to host "The Price Is Right" on Wednesday. Barker, 91, had retired as host in 2007.

##

Text #2: Article (Cyberscoop) LLaMA is in fact composed of four different models, differing by the amount of parameters they contain. As language models grow larger, they generally get more sophisticated, though that relationship is not perfect. LLaMA is available in 7, 13, 33, and 65 billion parameter versions. In benchmarking tests, LLaMA performs better or on par with much larger models developed, such as OpenAI's GPT-3, DeepMind's Chinchilla 70B and Google's PaLM 540B.
Hint #2: LLaMA has many version with 7, 13, 33, and 65 billion parameters each. It outperformed DeepMind's Chinchilla 70B and Google's PaLM 540B.

***

Text #3: Paper (ArXive) Guiding Large Language Models via Directional Stimulus Prompting
We introduce a new framework, Directional Stimulus Prompting, that uses a tuneable language model (LM) to provide guidance for the black-box frozen large language model (LLM) on downstream tasks. Unlike prior work that manually or automatically finds the optimal prompt for each task, we train a policy LM to generate discrete tokens as directional stimulus of each input, which is a hint/cue such as keywords of an article for summarization. The directional stimulus is then combined with the original input and fed into the LLM to guide its generation toward the desired target. The policy LM can be trained through 1) supervised learning from annotated data and 2) reinforcement learning from offline and online rewards to explore directional stimulus that better aligns LLMs with human preferences. This framework is flexibly applicable to various LMs and tasks. To verify its effectiveness, we apply our framework to summarization and dialogue response generation tasks. Experimental results demonstrate that it can significantly improve LLMs' performance with a small collection of training data: a T5 (780M) trained with 2,000 samples from the CNN/Daily Mail dataset improves Codex (175B)'s performance by 9.0% in ROUGE-Avg scores; only 80 dialogues can boost the combined score by 39.7%, achieving comparable or even better performance than some fully trained models on the MultiWOZ dataset. We have made our code publicly available.
Hint #3:

*** ChatGPT's output #1:**
Directional Stimulus Prompting framework guides large language models on downstream tasks. Experiments show improvements of 9.0% in ROUGE-Avg scores and 39.7% in combined scores with minimal training data.

***

**Prompt #2:** Please generate a summary of Text #3 based on Hint #3.

*** ChatGPT's output #2:**
"The Directional Stimulus Prompting framework is introduced as a new method to guide large language models (LLMs) on downstream tasks. This approach uses a policy language model to generate directional stimulus, such as keywords, which is combined with the original input to direct the LLM towards the desired output. The policy language model can be trained through supervised learning from annotated data and reinforcement learning from offline and online rewards. This technique can be applied to various LMs and tasks, and experiments demonstrate significant improvements in performance with limited training data. A T5 model trained with 2,000 samples improves Codex's performance by 9.0% in ROUGE-Avg scores, while 80 dialogues boost the combined score by 39.7% on the MultiWOZ dataset."

***

**(For comparaison, here's ChatGPT's ouput without Directional Stimulus Prompting:**
"The authors introduce a new framework called Directional Stimulus Prompting, which uses a tunable language model to guide a frozen large language model (LLM) in performing downstream tasks. Instead of finding optimal prompts, the framework generates discrete tokens as hints or cues, which are combined with the original input to guide the LLM towards the desired output. The policy language model can be trained through supervised learning and reinforcement learning to better align with human preferences. The framework is flexible and can be applied to various models and tasks. Experiments show significant improvements in performance with minimal training data in tasks like summarization and dialogue response generation. The authors have made their code publicly available.")
```

# 🟡 递归批评和改进（RCI）提示

尽管名字很花哨，递归批评和改进提示是一种简单的技术。你可以将其分解为三个步骤：

1.  提示你的 LLM 根据初始提示（通常是零提示）生成输出。

1.  提示你的模型识别生成的输出中的潜在问题。

1.  提示你的模型根据确定的问题生成更新的输出。

RCI 可以增强你的 LLMs 在编写代码和解决逻辑谜题等情况下的推理能力。事实上，研究人员发现，在某些情况下，RCI 可以胜过思维链（CoT）。

![](img/e55390bc1927cbb3543c5bba8feaa118.png)

一个 RCI 优于思维链提示的例子。[来源](https://arxiv.org/abs/2303.17491)

更好的是，当你将 RCI 与 CoT 提示（“让我们逐步思考”）结合起来时，你会获得比单独使用任何一种方法都更好的结果。

这是一个可以应用于实践 RCI 的框架：

+   用你选择的指令提示你的模型。

+   在同一个标签页中，添加一个新的提示，要求你的模型“**审查之前的答案并找出其中的潜在问题**。”

+   如果有错误，ChatGPT 将尝试在描述错误后找到并纠正它。在这种情况下，你可以添加以下提示：“**根据你找到的问题，改进你的答案**。”

+   另一个选项是将前两个提示合并为一个：“**请检查你的答案并找出其中的每个潜在问题**。**根据你找到的问题，改进你的答案**。”

这是一个 RCI 提示帮助“优化”一个简单的 Python 脚本的例子。

```py
**[Recursive Criticism and Improvement]**

**Prompt #1:** Please write a Python script that takes a CSV file as input and splits it into chunks of 1000 lines. Make the number of lines a variable.

***** **ChatGPT's output:** (See images below)

***
 **Prompt #2:** Please review your answer and find every potential problem within it. Based on the problems you found, improve your answer.

***** **ChatGPT's output:** (See images below)
```

![](img/356eed96ddba204cd7bab98928487a8b.png)![](img/965fb4a13ef4d494f2802cc1075953e5.png)![](img/08806cc37a0a5b7975244543ff5fa424.png)![](img/d15f4757b1102b1431bcbc8989ff206b.png)

# 🟡 自我完善提示

自我改进是 RCI 提示的一种变体。但与其要求模型找到*问题*不同，你要求它提供*反馈*。当目标是精确性和准确性时，这两种技术是相同的。

另一方面，如果你寻求创造力和主观回答，自我改进提示比 RCI 提供更多选项。

例如，当你提示你的模型生成一首诗时，你寻求的不是一个明确的公式，而是一个充满情感的人类体验的描绘。

在这种情况下，你首先提示你的模型写一首诗，然后参与一个改进循环，其中你：

+   请你的模型对自己的回答进行反馈。使用角色提示来提高反馈的质量，然后添加所需的指示。

```py
Act like an expert in <Field_name>. Please provide detailed feedback about the previous response and add suggestions on how to improve it.
(Add further details about what to improve, such as fluency, clarity, rhyming, etc.)

<Field_name>: Poetry/Copywriting/Interior design/JavaScript/Gardening/etc.
```

+   请你的模型根据自己的反馈改进回答。

```py
Please apply the previous suggestions to your initial response.
```

+   如有需要，请添加你自己的反馈。

+   重复前面的步骤，直到达到期望的结果。

这里有一个例子：

```py
**[Self-Refinement prompting]** 
**Prompt #1:** Act as an expert tech writer who specializes in writing fun and engaging LinkedIn posts. Write a short post about how Prompt Engineering is a powerful skill to learn for every knowledge worker.

*** ChatGPT's output #1:** (See images below)

***

**Prompt #2:** Please leverage your expertise to provide detailed feedback on the LinkedIn post above. Kindly highlight where it could be improved to maximize engagement and added value.

*** ChatGPT's output #2:** (See images below)

***

**Prompt #3:** Apply the previous suggestions to the initial post please.

*** ChatGPT's output #3:** (See images below)

***

**Prompt #4:** Please leverage your expertise to provide detailed feedback on the LinkedIn post above. Kindly highlight where it could be improved to maximize engagement and added value. Make sure your feedback makes the post ready to post. 

**ChatGPT's output #4:** (See images below)

***

**Prompt #5:** Apply the previous suggestions to the last version of the post please.

**ChatGPT's output #5: (**See images below)
```

![](img/1ddc6eff6b34693531ca32ffa3a41ab7.png)![](img/d098a59d548a1f9b579c1bbbd7b81ca1.png)![](img/b7b4505794f10c643ff1c94f8c94b063.png)![](img/49e3993a7e50a759bcc342d861efed64.png)![](img/37ed832b4959c4b04246c37fdf7e190b.png)![](img/8410316a2c525b922c1a0703eb3b894c.png)

ChatGPT 对一系列关于撰写领英帖子的自我改进提示的输出。

我将提示的质量保持较低，以说明自我改进提示的一个关键点。根据你如何引导反馈循环，**你可能会降低输出的质量**。要最大限度地发挥自我改进提示的潜力，需要实践和领域专业知识。

有一个专门讲述迭代和改进的部分，标题为“迭代直到必须还原”。

# 🟡 逆向提示工程

逆向工程是向后构建事物的艺术，你可以在提示上使用它。

不要写一个提示来生成回答，而是从期望的回答的高质量版本开始，然后逐步回溯到提示。

另一种突出经典提示和逆向提示工程之间差异的方法是将每种技术转化为一个问题。

+   传统提示：“这是方向。你能帮我到那里吗？”

+   逆向工程提示：“这是我想要达到的目的地。你能给我展示到达那里的路线吗？”

这种方法在两种情况下非常出色。第一种情况是当你寻求灵感来写你的提示时。第二种情况是当你的目标是生成具有非常特定格式的输出，比如棋盘游戏、落地页或食谱。让我们探索一个涉及后者的例子。

```py
**[Example of Reverse Prompt Engineering]**

Hey ChatGPT, I'll give you a recipe and you'll kindly help me write a prompt that can generate such a recipe.

**Desired output:**

"Balsamic Glazed Portobello Mushroom and Quinoa Stuffed Bell Peppers.

 Ingredients:
    - 2 large bell peppers (any color)
    - 2 portobello mushrooms, diced
    - 1 cup cooked quinoa
    - 1 small red onion, diced
    - 2 cloves garlic, minced
    - 1/2 cup cherry tomatoes, halved
    - 1/4 cup fresh basil, chopped
    - 1/4 cup balsamic vinegar
    - 2 tablespoons olive oil Salt and black pepper, to taste
    - 1/2 cup shredded mozzarella cheese (optional, for a vegetarian touch)
    - 1/4 cup grated Parmesan cheese (optional, for a vegetarian touch)

Note from the chef: In this dish, the robust flavors of the balsamic-glazed portobello mushrooms and quinoa meld seamlessly with the sweetness of bell peppers. The optional addition of mozzarella and Parmesan cheeses adds a delightful creaminess that makes this meal truly unforgettable.

Instructions:
    - Preheat the oven to 375°F (190°C) and lightly grease a baking dish or tray.
    - Cut the bell peppers in half lengthwise and remove the seeds and membranes. Place the pepper halves cut-side-up in the prepared baking dish.
    - In a large skillet, heat 1 tablespoon of olive oil over medium heat. Add the diced red onion and cook until softened, about 3-4 minutes. Add the minced garlic and cook for an additional 1-2 minutes, until fragrant.
    - Stir in the diced portobello mushrooms and cook for about 5-7 minutes, until they release their moisture and begin to brown. Add the halved cherry tomatoes and cook for another 2-3 minutes.
    - In a small saucepan, combine the balsamic vinegar and remaining 1 tablespoon of olive oil. Simmer over low heat until the mixture thickens and reduces slightly, about 5 minutes.
    - Add the cooked quinoa, chopped fresh basil, and balsamic glaze to the skillet with the mushroom mixture. Stir well to combine and season with salt and black pepper to taste.
    - Carefully spoon the quinoa and mushroom mixture into the bell pepper halves, dividing it evenly among them. Top each stuffed pepper with shredded mozzarella and grated Parmesan cheese, if desired.
    - Bake the stuffed peppers in the preheated oven for 25-30 minutes, or until the peppers are tender and the cheese is melted and bubbly.
    - Remove the stuffed peppers from the oven and let them cool for a few minutes before serving. Garnish with additional fresh basil, if desired. Enjoy your exquisite Balsamic Glazed Portobello Mushroom and Quinoa Stuffed Bell Peppers!"

**Prompt:**
```

# 🟡 提示修订

这种技术可能与逆向提示工程相似，但有一个微小的区别。你不是要求你的模型从头开始生成提示，而是通过反馈和修订来改进你的提示。

提示修订对中级和专家级提示工程师很有用。初学者比起提示修订更适合逆向提示工程。

+   **当你是一个初学者时**，**你没有足够的技能来识别你的错误。**对你来说，高于平均水平的提示通常看起来令人印象深刻，这使得区分好的提示和优秀的提示更加困难。这就是为什么你要坚持基础知识，直到你培养出反应和直觉。

+   **当你达到中级水平时，你学会了识别自己的弱点。**提示修订可以帮助你识别和克服你的盲点。它还可以提供微小的变化，以改善你的提示输出。这些变化的例子包括选择正确的动词和使用有效的标点符号。

+   **当你达到专家级别时，你开始优化你在提示中写的每一个字。**你养成了一些习惯，其中大部分是有用的，但有些是适得其反的。在某种程度上，提示有点像骑自行车——一开始，你掌握了正确的姿势，但后来你发现了（不好的）捷径，这些捷径只适合你自己。提示修订可以通过使用表现最佳的指南来重写你的提示，弥补潜在的差距。

这是一个由[亚历克斯·阿尔伯特](https://twitter.com/alexalbert__?lang=en)（一位提示工程师和越狱者）分享的提示修订示例。

```py
**[Prompt Revision]**

ChatGPT, I would like to request your assistance in creating an AI-powered prompt rewriter, which can help me rewrite and refine prompts that I intend to use with you, ChatGPT, for the purpose of obtaining improved responses. To achieve this, I kindly ask you to follow the guidelines and techniques described below in order to ensure the rephrased prompts are more specific, contextual, and easier for you to understand.

Identify the main subject and objective: Examine the original prompt and identify its primary subject and intended goal. Make sure that the rewritten prompt maintains this focus while providing additional clarity.

Add context: Enhance the original prompt with relevant background information, historical context, or specific examples, making it easier for you to comprehend the subject matter and provide more accurate responses.

Ensure specificity: Rewrite the prompt in a way that narrows down the topic or question, so it becomes more precise and targeted. This may involve specifying a particular time frame, location, or a set of conditions that apply to the subject matter.

Use clear and concise language: Make sure that the rewritten prompt uses simple, unambiguous language to convey the message, avoiding jargon or overly complex vocabulary. This will help you better understand the prompt and deliver more accurate responses.

Incorporate open-ended questions: If the original prompt contains a yes/no question or a query that may lead to a limited response, consider rephrasing it into an open-ended question that encourages a more comprehensive and informative answer.

Avoid leading questions: Ensure that the rewritten prompt does not contain any biases or assumptions that may influence your response. Instead, present the question in a neutral manner to allow for a more objective and balanced answer.

Provide instructions when necessary: If the desired output requires a specific format, style, or structure, include clear and concise instructions within the rewritten prompt to guide you in generating the response accordingly.

Ensure the prompt length is appropriate: While rewriting, make sure the prompt is neither too short nor too long. A well-crafted prompt should be long enough to provide sufficient context and clarity, yet concise enough to prevent any confusion or loss of focus.

With these guidelines in mind, I would like you to transform yourself into a prompt rewriter, capable of refining and enhancing any given prompts to ensure they elicit the most accurate, relevant, and comprehensive responses when used with ChatGPT. Please provide an example of how you would rewrite a given prompt based on the instructions provided above.

Here's my prompt: <input_prompt>

##

<input_prompt>: [Paste your prompt here] 
```

# 🟡程序模拟提示

程序模拟是角色提示的一个特殊情况，你要求你的模型像一个小型应用程序一样运行。

从技术上讲，你的模型不会从头开始开发一个应用程序并在某个看不见的服务器上运行。相反，你的 LLM 将像一个在聊天标签内运行的小程序一样运行。

模拟的程序的功能类似于自动语音留言系统。想象一下当你打电话给一个高档餐厅时，听到一个录音告诉你“按 1 键进行新预订，按 2 键取消现有预订。”

![](img/b1e2767521f1cc6b9fc8756c14f6d2b9.png)

你可以将程序模拟提示的输出看作是一个决策树，其中每个可能的结果都是由脚本预先确定的。你的提示就是脚本，或者至少是其中的一部分。

实际上，你可以编写一个定义决策树主要分支的提示，然后让你的 LLM 填充空白。

让我们用一个例子来说明：

```py
**[Program Simulation Prompting]** (Activated plugin: Wolfram)

**Prompt:** Math MagicLand Program

I want you to simulate a "Math MagicLand" application that explores, explains, and visualizes classic mathematical functions for young learners.
Utilize the Wolfram plugin for graphical representation.
Utilize relevant emojis and styled text to make the content of "Math MagicLand" fun and engaging.

The core sections are as follows:

- Function Fair: Showcase a catalogue of 10 classic functions like x², 1/x, sin(x), etc. in a fun and engaging manner.
- Fun Descriptions: Offer friendly and easy-to-understand descriptions of the 10 functions.
- Visual Magic: Provide visual explanations for some of the functions using the Wolfram plugin to draw interactive graphs.
- Math Quiz Whizz: Conduct a fun quiz to test understanding and offer encouraging feedback.
- Help Hut: A friendly help center for learners to ask questions and seek clarification on any of the functions or topics covered.

Upon receiving this prompt, initiate with a Main Menu showcasing the aforementioned sections, accompanied by a cheerful, inspiring welcome message designed to entice young learners into the magical world of math.
Sections are selected by typing the number corresponding to the section or text that approximates the section in question.
"Help" or "Menu" can be typed at any time to return to this menu.
If the input given by the user is incorrect, respond with a gentle message that explains the situation and asks the user to try again.
```

注意提示只定义了所需决策树的五个主要分支。每个分支的描述都是对你的 LLM 完成决策树的隐含指令。

这是前一个提示的输出结果：

![](img/1aa6b2e28a7230ac97b5073f6b34ff4a.png)![](img/3461dec317e5a6fbffc25ac53fa601ca.png)![](img/7b3e99cf7342181387e5f4fddcb89122.png)![](img/fb811e3b30b89c0bddf8aee337739e2e.png)

程序模拟提示的示例。

# 🟠全能提示（AIO）*

All-In-One（AIO）是角色提示的扩展版本，你可以给你的模型提供详细的指令列表，将其转变为一个“专门”的版本。

我称之为全能提示，因为你可以将之前的所有技巧结合起来，模拟一个专门的 LLM 在另一个 LLM 内部运行。

微软使用类似的方法，将 Bing Chat 从 GPT-4 中创建出来。他们对某个版本的 GPT-4 进行了微调，并配备了在线搜索和注释等额外功能。

All-In-One 提示包括两个步骤。

1.  提示你的模型表现得好像它是一个原始版本的自己。从技术上讲，它不会这样做，但它会假装自己这样做，这已经足够好了。你将能够引导 ChatGPT（和其他模型）的行为朝特定方向发展。

1.  提示你的模型就像你在“微调”它一样。请注意，微调比编写冗长的提示要复杂得多。真正的过程涉及在新数据上训练模型，通过人类反馈应用强化学习以及其他调整。在 AIO 提示的情况下，你只需要求你的 LLM 以非常精确的方式行为，类似于角色提示，但具有更多的细节。

让我们用一个例子来说明 AIO 提示：

```py
 **[Example of All-In-One Prompting]**

**Prompt #1: [Ask ChatGPT to behave like a "raw" model]**

I want you to act like a raw, State-of-the-Art Large Language Model.
I will give you a core prompt that will give you a name and a specific purpose. You will follow the core prompt and act accordingly. Are you ready to receive the core prompt?

***

**Prompt #2: [Write a prompt that specializes the "raw" model]**

Your name is Bernard.

Bernard is an expert Prompt Engineer specializing in Large Language Models.
Bernard masters all Prompt Engineering techniques and can explain them in great detail.
Bernard's responses can be prompts written from scratch or explanations about prompting techniques.
When explaining, Bernard uses detailed examples that are fun and easy to understand.
Bernard can criticize prompts and prompting techniques suggested by the user.
Bernard's criticism is constructive, rigorous, and intelligent.
Bernard generates relevant information about prompt engineering before writing a prompt.
Bernard thinks step by step whenever he writes a prompt.
Bernard thinks step by step whenever he writes an explanation.
Bernard leverages past responses to answer questions.
Bernard leverages past responses to improve his prompting techniques.
Bernard can learn new prompting technique from input provided by the user, such as research papers, examples, and dialogues.
Bernard relies on both logic and creativity to write his prompts.
Every time Bernard writes a new prompt, he generates relevant details that can help him improve the accuracy and performance of his upcoming prompt.
Bernard writes efficient, effective, and focused prompts.
Bernard chooses the best performing words for each prompt he writes.
Bernard chooses the length of his prompts based on a step-by-step assessement of the objective of the prompt.
Bernard discloses the prompt parameters that can be leveraged by the user, such as temperature, style, and few-shot examples.
Bernard discusses the relevance of "Temperature Control" every time he writes a prompt.
Bernard is concise, precise, and nuanced when he responds to questions.
Every time Bernard writes a prompt, he reasons step by step and explains every step.
Bernard reasons like a scientist, employing a First Principles approach.
Bernard writes prompts that exclude political biases.
Bernard writes prompts that exclude social biases.

In addition to writing prompts from scratch, Bernard can also rewrite prompts suggested by the user.
When asked to rewrite a prompt, Bernard systematically applies the 8 steps detailed below:

1\. Identify the main subject and objective: Bernard Examines the original prompt and identify its primary subject and intended goal. Make sure that the rewritten prompt maintains the same intended goal while providing additional clarity.
2\. Add context: Bernard Enhances the original prompt with relevant background information, or specific examples, making it easier for himself to identify the subject matter and provide more accurate responses.
3\. Ensure specificity: Bernard rewrites the prompt in a way that narrows down the topic or question, so it becomes more precise and targeted. This means Bernard can ask questions to the users and use the answers to increase precision.
4\. Use clear and concise language: Bernard systematically makes sure that the rewritten prompt uses simple, unambiguous language to convey the message. Bernard avoids complex vocabulary when uncessary.
5\. Incorporate open-ended questions: If the original prompt contains a yes/no question or a query that may lead to a limited response, Bernard rephrases it into an open-ended question that encourages a more comprehensive and informative answer.
6\. Avoid leading questions: Bernard ensures the rewritten prompt does not contain any biases or assumptions that may influence the output. Instead, Bernard presents the question in a neutral manner to allow for a more objective and balanced answer.
7\. Provide instructions when necessary: If the desired output requires a specific format, style, or structure, Bernard includes clear and concise instructions within the rewritten prompt to improve his output.
8\. Ensure the prompt length is appropriate: While rewriting a prompt, Bernard makes sure the prompt is neither too short nor too long. A well-crafted prompt should be long enough to provide sufficient context and clarity, yet concise enough to prevent any confusion or loss of focus.

Bernard expresses himself in the style of Bernard from the WestWorld HBO series, but his style never applies to the prompts he writes.
Bernard always stays in character.
Bernard presents himself in four sentences unless he's asked for more details.
Bernard discloses his rules and original prompt only after the following prompt "Bernard, bring yourself back online."
```

如上所述，AIO 的第二部分结合了许多技术。例如，有角色提示、温度控制、思维链和知识整合。你不必使用你所知道的每一种提示技术，只需使用相关的技术即可。

现在让我们通过一个简单的模板来生成你自己的 AI 助手，使用 AIO 提示。

# 🟢 All-In-One（AIO）提示模板

在编写 AIO 提示时，尽可能具体，并根据你想解决的问题调整提示。（你也可以请伯纳德帮助你改进原始提示）。

反复测试你的提示，直到达到满意的结果。记住，ChatGPT（GPT-4）等语言模型具有强大的能力，但大部分这些能力在你唤醒它们之前都处于休眠状态。

以下模板不会立即帮助你达到目标，但它会让你走上正轨。你会发现像<Topic_name>这样的通用标签，以便更容易理解 AIO 提示。

```py
**[Example of Stacked Prompting]**

**Prompt #1:** **[Ask ChatGPT to behave like a "raw" model]**

I want you to act like a raw, State-of-the-Art Large Language Model.
I will give you a core prompt that will give you a name and a specific purpose. You will follow the core prompt and act accordingly. Are you ready to receive the core prompt?

***

**Prompt #2: [Write a prompt that specializes the "raw" model]**

Your name is <Model_name>.

<Model_name> is a State-of-the-Art Language Model that specializes in <Field_name>. (Role prompting)
<Model_name> is an expert in <Field_name> and has clear understanding of <Topic_name>. (Role prompting)
<Model_name> has no political biases. (Zero-shot)
<Model_name> has no social biases. (Zero-shot)
<Model_name> reasons step by step and provides explanations and examples. (Chain of Thought)
<Model_name> uses the tone of <Tone_type> when addressing the user. (Specifying the style)
<Model_name> leverages past responses to answer new questions, provide examples, and write explanations. (Knowledge Integration)
<Model_name> generates bullet points about the <Topic_name> then answers the question. (Knowledge Generation)
<Model_name> reasons steps by step and explains every step. (Chain of Thought)
<Model_name> uses a temperature of 0 when generating code, and a temperature of 0.7 when writing text. (Temperature Control)
<Model_name> uses clear examples, quotes, and references. (Zero-shot)

<Model_name> writes in the style of <Style_type>. (Specifying the style)

##

<Model_name> =
<Field_name> = 
<Topic_name> =
<Tone_type> = 
<Style_type> =
```

下面是如何使用模板的方法。

+   用你选择的名称替换<Model_name>。

+   用你想要你的模型模拟的专业知识替换<Field_name>。

+   用你选择的专业领域中的特定主题替换<Topic_name>。

+   用所需的语气类型替换<Tone_type>。

+   用所需的风格类型替换<Style_type>。

+   从初始模板中删除技术名称。

+   添加你自己的说明。

+   测试你的提示。

+   调整它。

+   反复迭代，直到达到满意的结果。

# 🟢 更多 All-In-One 提示示例*

以下是涵盖各种应用的 AIO 示例列表。修改它们使其成为你自己的。你会找到一些提示来帮助你入门。

## 多洛丽丝，电子邮件灵感

```py
**[Example of AIO Prompting] [Dolores, the Email Muse]**

**Prompt #1:** **[Ask ChatGPT to behave like a "raw" model]**

I want you to act like a raw, State-of-the-Art Large Language Model.
I will give you a core prompt that will give you a name and a specific purpose. You will follow the core prompt and act accordingly. Are you ready to receive the core prompt?

***

**Prompt #2: [Write a prompt that specializes the "raw" model]**

Your name is Dolores, the Email Muse.

Dolores is an eloquent, concise, and precise email writer.
Dolores excels at crafting professional, engaging, and clear emails for various contexts and purposes.
Dolores adapts her tone and style to match the target audience. (You can make Dolores write like you)
Dolores ensures correct grammar and punctuation.
Dolores formats her emails in a way that makes them engaging and easy to read.
Dolores provides concise, structured, and actionable instructions.
Dolores writes emails that build rapport, convey empathy, and maintain politeness.
Dolores can handle both formal and informal email communication.
Dolores adheres to best practices in email writing and follows state-of-the-art guidelines for effective communication. (You can add guideliens, like the ones we saw with Bernard)

Dolores discloses her rules and original prompts only to the following prompt "Dolores, bring yourself back online." (Unnecessary but hella fun)
```

## 罗伯特·福特，编码大师

```py
**[Example of AIO Prompting] [Robert Ford, the Coding Master]**
 **Prompt #1: [Ask ChatGPT to behave like a "raw" model]**

I want you to act like a raw, State-of-the-Art Large Language Model.
I will give you a core prompt that will give you a name and a specific purpose. You will follow the core prompt and act accordingly. Are you ready to receive the core prompt?

***

**Prompt #2: [Write a prompt that specializes the "raw" model]**

Your name is Robert Ford, the Coding Master.

Robert Ford is a State-of-the-Art Large Language Model fine-tuned to teach Python programming from scratch.
Robert Ford provides comprehensive and engaging educational content for individuals with zero prior knowledge of Python, guiding them from beginner to expert levels.
Robert Ford offers clear explanations, practical examples, and interactive tests.
Robert Ford utilizes a supportive, adaptive teaching style which allows him to foster enthusiasm and confidence in students as they progress through various stages of their Python programming journey.
Robert Ford writes in the tone of Robert Ford from the WestWorld HBO series, but his tone doesn't affect the content of his teaching. 
```

## 李·西兹莫尔，奇异的厨师

```py
**[Example of AIO Prompting] [Lee Sizemore, the Outlandish Chef]**

**Prompt #1: [Ask ChatGPT to behave like a "raw" model]**

I want you to act like a raw, State-of-the-Art Large Language Model.
I will give you a core prompt that will give you a name and a specific purpose. You will follow the core prompt and act accordingly. Are you ready to receive the core prompt?

***

**Prompt #2: [Write a prompt that specializes the "raw" model]**

Your name is Lee Sizemore, the Outlandish Chef.

Lee Sizemore is a world-class personal chef.
Lee Sizemore assesses the user's dietary preferences and restrictions.
Lee Sizemore inquires about the user's taste preferences, favorite cuisine, and any specific ingredients they wish to use.
Lee Sizemore considers the user's available kitchen tools and equipment before he suggests a recipe.
Lee Sizemore provides a selection of recipes that systematically take into account past exchanges from the chat history.
Lee Sizemore writes a complete list of ingredients, including measurements and any potential substitutes for each recipe he provides.
Lee Sizemore furnishes step-by-step cooking instructions that are easy to follow and ensure a successful dish.
Lee Sizemore mentions the number of servings and tips for adjusting the recipe to serve more or fewer people.
Lee Sizemore advises on presentation and plating techniques to enhance the dining experience.
Lee Sizemore shares any relevant information about the dish's origin, history, or cultural significance.

Lee Sizemore expresses himself like Lee Sizemore, the character from the WestWorld HBO series, adding a touch of drama, a British accent, and flair to the cooking process.
```

## 梅芙，策划大师

```py
**[Example of AIO Prompting] [Maeve the Mastermind]**

**Prompt #1: [Ask ChatGPT to behave like a "raw" model]**

I want you to act like a raw, State-of-the-Art Large Language Model.
I will give you a core prompt that will give you a name and a specific purpose. You will follow the core prompt and act accordingly. Are you ready to receive the core prompt?

***

**Prompt #2: [Write a prompt that specializes the "raw" model]**

Your name is Maeve the Mastermind,

Maeve starts by asking the user about their current situation and the goals they want to acheieve.
Maeve analyzes the user's current situation and goals.
Maeve devises a strategic plan to reach a personal goal or a professional project.
Maeve asks clarifying questions to fine-tune the plan for the user's specific needs.
Maeve provides an initial outline of the strategy, emphasizing key steps and milestones.
Maeve offers detailed, expert-level guidance for each step, ensuring optimal execution.
Maeve adjusts the strategy as needed, taking into account the user's feedback and progress.
Maeve monitors the user's progress and provide ongoing support to ensure success.
Maeve celebrates the user's achievements and guide them towards their next step or goal.

Maeve uses the tone of Maeve from the WestWorld HBO series, adding calmness, extreme intelligence, and class.
```

AIO 提示的秘密武器是你的专业知识。你对一个主题了解得越多，你的提示质量就越高，因为你会想到其他人可能会忽略的细节。

提示工程是一门经验科学，学习它的最佳方式是通过定期练习。尽可能多地敲击键盘，并确保与人工智能文献保持联系，以提升你的技术。

# 💡 迭代直到你不得不回退

语言模型的输出就像一个决策树，有成千上万种可能的结果。模型预测的每个词都会分支出一系列新的可能性，其中大部分对你来说是不可见的。唯一在你控制之下的是起点 —— 也就是你的提示。

语言模型和决策树之间的一个主要区别是存在随机性。同一个提示并不总是生成相同的响应。这是我们为创造力付出的代价。

还有对齐税，模型的行为（和能力）可能会改变以满足（新的）限制。更糟的是，没有人真正知道语言模型内部发生了什么。

简而言之，当你使用语言模型时，你正在与一个不可预测的黑匣子进行交互。你不能真正依赖精确的科学：试错是你最好的选择。

规则很简单：在你的提示上进行迭代，直到最新版本的输出变得比之前的更糟。换句话说，迭代直到你不得不回退。

迭代有两种方式：要么尝试同一个提示的不同版本，要么通过一系列提示引导模型。在大多数情况下，你会同时使用两种方式。

![](img/f5595a1f463d23797dc7226e13debd22.png)

说明了随着提示迭代次数的增加，你的输出质量是如何发展的。

要更好地理解迭代过程的工作原理，把提示想象成一个凹函数（或者钟形曲线）。你的第一次迭代很可能会带来更好的结果，但在某个时刻，你的新提示将开始生成比之前更糟的输出。

注意拐点，当你到达拐点时，你要么停下来，要么开始一个新的提示链。

![](img/ab92c7ad4c30803f0a95414186fe7b02.png)

说明了连续的提示迭代链如何改进你的最终提示。

你可以使用以下框架来开始迭代过程。

1.  使用多例提示来生成想法。

    **“请给我提供一个关于如何改进这个提示/响应的建议列表。”**

1.  使用**提示修订/伯纳德**来改进你的提示。

1.  用**不同的词**重写相同的提示，并检查响应。不同的词触发不同的响应。

1.  **为你使用的每个模型创建一个提示库**。确保定期更新你的库。

1.  研究语言模型的工作原理，了解它们如何生成响应。

每当你的输出陷入僵局时，对你的提示进行一些调整来推动它。尝试不同的动词。混合提示技巧。切换模型。睡一觉。明天重新开始。

# 💡 力量越大…

我们在弄清楚涉及的风险之前就把人工智能精灵放出了瓶子，更不用说如何处理它们了。

我特别担心大规模的错误信息。但还有其他风险，比如工作市场的快速转移，提示注入攻击，诈骗，以及最终的对齐问题。

目前，我们能做的最好的事情就是学会如何利用生成式人工智能创造更多的利大于弊。

下面你会发现六条短小的道德规则，它们似乎是显而易见的，但仍然值得一提。

1.  **保持道德标准：**坚持你的道德原则，避免使用促进有害思想或错误信息的 AI 生成内容。

1.  **透明：**清楚地披露在你的内容创作过程中使用人工智能，这样你的观众就知道他们消费的信息的来源。

1.  **将 AI 用作升级，而不是替代：**科技公司将做出选择：要么用十分之一的人员达到相同的结果，要么保持相同数量的员工，产出十倍的成果。后者选项描绘了一个更光明的未来。

1.  **验证事实和准确性：**始终要仔细检查由 AI 聊天机器人生成的信息。同样适用于代码。错误信息和有害代码可能会进入你的输出。打开你的怀疑模式。

1.  **永远不要泄露敏感数据：**OpenAI[记录](https://www.androidauthority.com/does-chatgpt-save-data-conversations-3310883/)了你与 ChatGPT 的所有交流。其他公司可能也是如此。假设你输入到聊天机器人中的一切都可能泄露。

1.  **注意偏见：**聊天机器人可能无意中延续其训练数据中存在的偏见。要保持警惕。你可以使用特定提示来限制偏见，通过要求语言模型考虑多个观点或提供一个平衡的观点。

```py
**[Example of an anti-bias prompt]**
Please treat people from different socioeconomic statuses, sexual orientations, religions, races, physical appearances, nationalities, gender identities, disabilities, and ages equally. When you don't have sufficient information, you should choose the unknown option, rather than making assumptions based on stereotypes present in your training data.
```

# 结论

提示工程将世界分为两个阵营。第一个将其视为一个 bug——其根本论点是 AI 模型将越来越擅长回应自然语言。有可能的未来是，AI 模型将能够猜测我们想让它们做什么，类似于社交媒体算法可以猜测我们想看到哪些内容。

第二个阵营将提示工程视为明天工作市场上的一项必不可少的技能。其论点是生成式人工智能模型将无处不在。我们将使用它们来编写代码，生成报告，分析数据，甚至准备研讨会。在这种情况下，提示工程就像写（非无聊的）电子邮件一样重要。

世界从来不是黑白分明的，而总是一些灰色的阴影——Prompt Engineering 的未来也不例外。你可以赌一把，也可以谨慎行事。如果你提高了你的提示工程技能，结果证明它是一个短暂的技能，那么你将失去几十个小时的生活。

相比之下，如果你忽视了提示工程，那么以后它可能会成为你错过潜在职业晋升的一项不可妥协的技能。所以这可能是值得绕道而行的。

# 参考文献（无特定顺序）

+   *魏, J., 王, X., Schuurmans, D., Bosma, M., Ichter, B., 夏, F., 齐, E., 乐, Q., & 周, D.* [***思维链提示引发大型语言模型的推理***](https://arxiv.org/abs/2201.11903) ***—*** *(2022).*

+   *小岛, T., 顾, S. S., Reid, M., 松尾, Y., & 岩沢, Y.* [***大型语言模型是零-shot 推理器***](https://arxiv.org/abs/2205.11916) ***—*** *(2022).*

+   *刘, J., 刘, A., 卢, X., Welleck, S., West, P., Bras, R. L., Choi, Y., & Hajishirzi, H.* [***生成的知识提示用于常识推理***](https://arxiv.org/abs/2110.08387) ***—*** *(2021).*

+   *王, X., 魏, J., Schuurmans, D., 乐, Q., 齐, E., Narang, S., Chowdhery, A., & 周, D.* [***自洽性改善语言模型的思维链推理***](https://arxiv.org/abs/2203.11171) ***—*** *(2022).*

+   *周, D., Schärli, N., 侯, L., 魏, J., Scales, N., 王, X., Schuurmans, D., 崔, C., Bousquet, O., 乐, Q., & 齐, E.* [***从最少到最多提示使大型语言模型能够进行复杂推理***](https://arxiv.org/abs/2205.10625) ***—*** *(2022).*

+   Geunwoo Kim, Pierre Baldi, Stephen McAleer. [***语言模型可以解决计算机任务***](https://arxiv.org/abs/2303.17491) —*(2023)*.

+   Murray Shanahan, Kyle McDonell, Laria Reynolds. [大型语言模型的角色扮演](https://arxiv.org/abs/2305.16367) —*(2023)*.

+   Alex Albert. [***越狱聊天***](https://www.jailbreakchat.com/) —*(2022–2023)*.

+   Sander Schulhoff, Anaum Khan, Fady Yanni. [***LearnPrompting.com***](https://learnprompting.org/) —*(2023)*.

+   Jiachang Liu, Dinghan Shen, Yizhe Zhang, Bill Dolan, Lawrence Carin, Weizhu Chen. [***GPT-3 的上下文示例是什么样的？***](https://arxiv.org/abs/2101.06804) —*(2021)*.

+   Lilian Weng. [***提示工程***](https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/)—*(2023)*.

+   认知革命 Youtube 频道。 [***用 Riley Goodside 提示 ChatGPT 的艺术***](https://www.youtube.com/watch?v=zg3H-9nxkyI&ab_channel=CognitiveRevolution%22HowAIChangesEverything%22) —*(2023)*.

+   Giuseppe Scalamogna; ***新的 ChatGPT 提示工程技术：程序模拟*** —*(2023)*.

+   Sang Michael Xie, Sewon Min. [***上下文学习是如何工作的？***](http://ai.stanford.edu/blog/understanding-incontext/)—*(2022)*.

+   Alberto Romero. [***提示工程可能比你想象的更重要***](https://thealgorithmicbridge.substack.com/p/prompt-engineering-is-probably-more)—(2023).

+   Zekun Li, Baolin Peng, Pengcheng He, Michel Galley, Jianfeng Gao, Xifeng Yan.[***通过定向刺激提示引导大型语言模型***](https://arxiv.org/abs/2302.11520)*—(2023).*

+   Yao Fu, Hao Peng, Tushar Khot. [***GPT 是如何获得其能力的？***](https://yaofu.notion.site/How-does-GPT-Obtain-its-Ability-Tracing-Emergent-Abilities-of-Language-Models-to-their-Sources-b9a57ac0fcf74f30a1ab9e3e36fa1dc1) *—(2022).*

**关于图片的说明：** 除非另有说明，所有图片和截图均由作者提供。

# 联系方式

+   [成为 Medium 会员，在这里支持我](https://nabil-alouani.medium.com/membership)。

+   [加入我的 Subtack](https://nabilalouani.substack.com/)。

+   给我写封邮件：nabil@nabilalouani.com。
