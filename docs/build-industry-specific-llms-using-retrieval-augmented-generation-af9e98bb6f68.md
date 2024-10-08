# 使用检索增强生成技术构建行业特定的语言模型

> 原文：[`towardsdatascience.com/build-industry-specific-llms-using-retrieval-augmented-generation-af9e98bb6f68`](https://towardsdatascience.com/build-industry-specific-llms-using-retrieval-augmented-generation-af9e98bb6f68)

## 组织们正在竞相采用大型语言模型。让我们深入了解如何通过 RAG 构建行业特定的语言模型。

[](https://skanda-vivek.medium.com/?source=post_page-----af9e98bb6f68--------------------------------)![Skanda Vivek](https://skanda-vivek.medium.com/?source=post_page-----af9e98bb6f68--------------------------------)[](https://towardsdatascience.com/?source=post_page-----af9e98bb6f68--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----af9e98bb6f68--------------------------------) [Skanda Vivek](https://skanda-vivek.medium.com/?source=post_page-----af9e98bb6f68--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----af9e98bb6f68--------------------------------) ·阅读时间 10 分钟·2023 年 5 月 31 日

--

公司可以通过像 ChatGPT 这样的语言模型获得大量生产力提升。但试着问 ChatGPT “美国当前的通货膨胀率是多少”，它给出的答案是：

> 对于混淆表示歉意，但作为一个人工智能语言模型，我没有实时数据或浏览能力。我的回答基于截至 2021 年 9 月的信息。因此，我无法提供美国当前的通货膨胀率。

这就是一个问题。ChatGPT 明显缺少相关的及时上下文，而这些上下文在做出明智决策时可能至关重要。

# 微软是如何解决这个问题的

在微软的 Build 会议中 [Vector Search Isn’t Enough](https://build.microsoft.com/en-US/sessions/038984b3-7c5d-4cc6-b24e-5d9f62bc2f0e?wt.mc_ID=Build2023_esc_corp_em_oo_mto_Marketo_FPnews_Elastic)，他们展示了将较少上下文感知的语言模型与向量搜索结合的产品，以创造更具吸引力的体验。

讲座从与本文相反的方向开始——从 Elastic Search（或向量搜索）的角度——以及搜索本身的局限性，并且添加语言模型层可以大幅提升搜索体验。

基本思路是，向语言模型中添加相关上下文可以大幅提升用户体验，尤其是在大多数商业场景中，在这些场景下，语言模型未曾见过这些数据。向量搜索帮助选择相关上下文，当你拥有大量数据，包括数百份（或更多）文档时。

## 向量搜索 101

![](img/df4126ecf386cf34f81c491832c86c37.png)

向量搜索 101 | Skanda Vivek

向量化是将数据转换为维度。在上述情况下，你可以看到 2 个维度：大小和类型。大小有 2 个值（小或大），类型有 2 个值（树或动物）。这只是一个概念性示例，可以扩展到数百个（或更多值）。

![](img/98f0411346236ea58fff19a27e99849e.png)

向量搜索 101 与词汇 | Skanda Vivek

你可以用单词或句子做同样的事情，而不是图片。注意在上述示例中，向量化能够捕捉到语义表示，即它知道谈论一只鸟俯冲到小松鼠上的句子应该位于（小，动物）象限，而谈论昨天风暴中一棵大树倒在路上的句子应该位于（大，树）象限。实际上，维度不止 2 个——句子转换嵌入有 384 个维度（句子或文本片段通过 BERT 转换器，输出 384 个数字，对应 384 个维度）。

## 向量搜索 + LLM（RAG）

向量搜索现在在所有搜索引擎中都很常见（Google、Bing，选择你喜欢的搜索引擎）。想象一下，如果你的整个搜索数据库仅包含这 4 份文档。你可以提出问题，“告诉我关于一棵大树的事情。”

输出将是：

> 在昨天的风暴中，一棵大树倒在了路上。

仅仅是数据库中最相关的文档。但如果你向 LLM 提出相同的问题（并在最后附上这 4 份“文档”），你会得到这样的结果（以 ChatGPT 为例）：

> 一棵大树通常是一个大型的、耸立的植物，具有坚固的树干、枝条和广泛的根系。这些树木可以达到令人印象深刻的高度，通常高出周围的植被。它们是许多生态系统的重要组成部分，为各种生物提供遮荫、庇护和食物。
> 
> 你提到的关于一棵大树在风暴中倒在路上的事件表明了恶劣天气条件的力量和影响。强风的风暴可以连根拔起或折断树木，导致道路阻塞，对车辆和行人构成潜在的危险。
> 
> 在你提到的另一个场景中，当你在班迪布尔的道路上行驶时，你遇到了一只大象穿过道路。班迪布尔是印度的一座国家公园和野生动物保护区，以其丰富的生物多样性和各种动物群体而闻名。大象是班迪布尔的主要物种之一，通常会穿越该地区，有时会穿过道路。这些遭遇突显了野生动物保护的重要性以及人类与动物在其自然栖息地共存的必要性。
> 
> 转到盆景，它们是生长在小花盆中的迷你树木，通过精心修剪来保持其小巧的尺寸。盆景栽培是一种起源于东亚的古老艺术形式，现已在全球范围内获得普及。由于其根部空间有限，盆景树需要定期浇水以防止干枯。小花盆可能导致土壤迅速干燥，因此必须监控湿度水平并相应地浇水。
> 
> 最后，你提到通过窗户观察到一只鸟俯冲向一只小花栗鼠。这一观察反映了自然行动的一个瞬间，其中一只鸟可能在寻找食物。猛禽和某些鸟类，如鹰或猫头鹰，通常会俯冲捕捉像花栗鼠这样的微小哺乳动物，作为其自然猎食行为的一部分。目睹这种互动非常吸引人，因为它们提供了关于动物王国动态的见解。
> 
> 总的来说，这些自然片段——无论是涉及大树、大象、盆景还是鸟类捕食——都突显了围绕我们自然世界的多样性和奇观。

那个回答似乎更加全面。ChatGPT 利用了其从数万亿文档中训练得到的内部知识，并结合了所提供的上下文。

但假设你有的不仅仅是 4 个文档——可能是成千上万甚至百万个，无法全部放入 ChatGPT 的提示中。在这种情况下，你可以使用向量搜索来缩小最可能包含答案的上下文，将其附加到提示中，并按照以下方式提问：

这是它现在给出的（截断的）答案：

![](img/916cbd5dcf05aabd6693ebb0a7bb4d78.png)

ChatGPT 回答 | Skanda Vivek

然后你可以有一个数据库，存储文档和嵌入。你可以有另一个数据库，存储查询，并根据查询找到最相关的文档：

![](img/9574fca57d2d8141c4113402aa2b5ee7.png)

文档数据库（左）和 Quey 数据库（右） | Skanda Vivek

一旦你通过查询获得最相似的文档，你可以将其输入到任何像 ChatGPT 这样的语言模型中。通过这个简单的技巧，你已经增强了你的语言模型，使用了文档检索！这也被称为检索增强生成（RAG）。

# 使用 RAG 构建行业特定的问答模型

![](img/a9add121e8626cc6254e3c2d362647db.png)

RAG 原型 | Skanda Vivek

上面的图表概述了如何构建一个基本的 RAG，该 RAG 利用 LLM 对自定义文档进行问答。第一部分是将多个文档拆分成可管理的片段，相关的参数是 ***最大片段长度***。这些片段应为包含回答典型问题所需的文本的典型（最小）大小。这是因为有时你提出的问题可能在文档的多个位置都有答案。例如，你可能会问“X 公司在 2015 年到 2020 年的表现如何？”而你可能有一份大文档（或多份文档）包含公司多年表现的具体信息，这些信息分布在文档的不同部分。你理想的做法是捕获包含这些信息的文档的所有不同部分，将它们链接在一起，然后传递给 LLM 进行基于这些过滤和拼接后的文档片段的回答。

***最大上下文长度*** 基本上是将各种片段拼接在一起的最大长度——为问题本身和输出答案留出一些空间（记住，像 ChatGPT 这样的 LLM 有严格的长度限制，包括所有内容：问题、上下文和答案）。

***相似度阈值*** 是将问题与文档片段进行比较的方式，以找到最有可能包含答案的顶部片段。余弦相似度是通常使用的度量，但你可能希望对不同的度量进行加权。例如，可以包括一个关键词度量，以便对包含特定关键词的上下文给予更多权重。例如，当你要求 LLM 总结一份文档时，你可能希望对包含“摘要”或“总结”这两个词的上下文给予更多权重。

如果你想测试生成性问答在自定义文档上的简单方法，可以查看我的 [API](https://rapidapi.com/skandavivek/api/chatgpt-powered-question-answering-over-documents) 和 [代码](https://github.com/skandavivek/web-qa)，它们在后台使用了 ChatGPT。

## 原型 ChatGPT 通过 RAG 增强

让我们通过一个例子来说明 RAG 的实用性。[EMAlpha](https://www.emalpha.com/) 是一家提供新兴市场洞察的公司——基本上是像印度、中国、巴西等新兴国家的经济（完全披露——我在 EMAlpha 担任顾问）。该公司正在开发一个 ChatGPT 驱动的应用程序，根据用户输入生成对新兴经济体的洞察。仪表盘大致如下——你可以比较 ChatGPT 的输出与能够在后台从 IMF 查询财务文档的 RAG 版本 ChatGPT（EM-GPT）的结果：

![](img/15afaf108e3ad7cb76296e37a68b7029.png)

[EMAlpha](https://www.emalpha.com/) 的 EM-GPT | Skanda Vivek

以下是 ChatGPT 对问题“尼泊尔按年份的 GDP 是多少？”的回答：

![](img/0ac4fe7cefd5459cfa8b5988647b2988.png)

ChatGPT 响应 | Skanda Vivek

ChatGPT 只返回了 2019 年之前的 GDP 数据，并表示如果需要更多信息，可以查看 IMF。但是，如果你想找出这些数据在 IMF 网站上的具体位置，这很困难，你需要对网站上的文档存储位置有个大致了解。经过一些搜索，你[可以在这里找到文档](https://www.imf.org/en/Publications/CR/Issues/2023/05/04/Nepal-Staff-Report-for-the-2023-Article-IV-Consultation-First-and-Second-Reviews-Under-the-533075)。即便如此，确定 GDP 信息的具体位置仍需要大量的滚动。

![](img/12c8e35f80594a211a2905f72a983f9c.png)

IMF 关于尼泊尔经济的文档 | Skanda Vivek

如上所示，找到这些数据确实很困难。但当你问 EM-GPT 同样的问题时，它会追踪到相关上下文，并找到如下答案：

![](img/166f3df5dbabead53a551c71c3bad470.png)

EM-GPT 答案 | Skanda Vivek

以下是发送给 ChatGPT 以回答这个问题的确切提示。它能够理解这个格式化的文本，提取正确的信息——并将其格式化成一个易读的格式，这一点相当令人印象深刻！

![](img/4cb6feca4c004dd7d864f864002c6633.png)

使用基于查询检索的上下文的 ChatGPT 提示 | Skanda Vivek

我花了半小时在 IMF 网站上查找这些信息，而 RAG 修改版的 ChatGPT 只用了几秒钟。仅使用向量搜索是不够的，因为它最多只能找到“名义 GDP”这段文本，而不会将数字与年份关联起来。ChatGPT 在过去已经训练过多个此类文档，因此一旦添加了相关上下文，它就知道文本中的哪些部分包含答案以及如何以良好的可读格式格式化这个答案。

# 结论

RAG 提供了一种很好的方式来使用基于自定义文档的 LLMs。像[微软](https://www.elastic.co/blog/may-2023-launch-announcement?utm_source=organic-social&utm_medium=twitter&utm_campaign=cee-gc&ultron=t1_launch&blade=twitter&hulk=social&utm_content=10092954746&linkId=215905348)、谷歌和亚马逊这样的公司正在竞相开发组织可以以即插即用的方式使用的应用程序。然而，这一领域仍处于初期阶段，利用向量搜索驱动的 LLMs 对自定义文档进行行业特定应用的公司可以成为首批先行者并超越竞争对手。

虽然有人问我应该使用哪个 LLM，以及是否需要对自定义文档进行微调或完全训练模型，**LLM 与向量搜索之间的同步工程作用被低估了。** 以下是一些可以显著提高或降低响应质量的考虑因素：

1.  **文档块的长度。** 如果正确答案更可能分布在文本的不同部分并需要拼接在一起，则文档应分割成较小的块，以便可以将多个上下文附加到查询中。

1.  **相似性和检索度量。** 有时候，单纯的余弦相似度是不够的。例如，如果许多文档包含关于同一主题的冲突信息，你可能需要根据这些文档中的元数据限制搜索范围。为此，除了相似度，还可以使用其他过滤度量。

1.  **模型架构：** 我展示的架构是一个原型。为了提高效率和可扩展性，需要考虑多个方面，包括向量嵌入模型、文档数据库、提示、LLM 模型选择等。

1.  **避免幻觉：** 你可能注意到我上面展示的例子是***几乎***正确的。增强版 ChatGPT 对尼泊尔 GDP 的金额是正确的——但年份是错误的。在这种情况下，需要在选择提示、以 ChatGPT 友好的格式提取数据和评估在多少情况下会出现幻觉以及哪些解决方案有效之间进行大量反馈。

现在你已经知道如何将 LLM 应用到你的自定义数据上，快去构建令人惊叹的 LLM 基础产品吧！

*如果你喜欢这篇文章，请关注我——我写的内容涉及在实际应用中应用最先进的 NLP 技术，以及数据与社会之间的交集。*

*随时通过* [*LinkedIn*](https://www.linkedin.com/in/skanda-vivek-01619311b/)*与我联系！*

*如果你还不是 Medium 会员并希望支持像我这样的作者，请通过我的推荐链接注册：* [*https://skanda-vivek.medium.com/membership*](https://skanda-vivek.medium.com/membership)

**以下是一些相关的文章：**

[你应该在什么时候微调 LLM？](https://medium.com/towards-data-science/when-should-you-fine-tune-llms-2dddc09a404a)

[LLM 经济学：ChatGPT 与开源](https://medium.com/towards-data-science/llm-economics-chatgpt-vs-open-source-dfc29f69fec1)

[如何构建一个 ChatGPT 驱动的应用程序？](https://medium.com/geekculture/how-do-you-build-a-chatgpt-powered-app-89c83f3e2143)

[提取式问答与生成式问答 — 哪种更适合你的业务？](https://medium.com/towards-data-science/extractive-vs-generative-q-a-which-is-better-for-your-business-5a8a1faab59a)

[在自定义数据上微调 Transformer 模型以进行问答](https://medium.com/towards-data-science/fine-tune-transformer-models-for-question-answering-on-custom-data-513eaac37a80)

[释放生成 AI 的力量，为你的客户带来价值](https://medium.com/geekculture/unleashing-the-power-of-generative-ai-for-your-customers-70297f1c9698)
