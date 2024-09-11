# 监控大型语言模型行为的7种方法

> 原文：[https://towardsdatascience.com/7-ways-to-monitor-large-language-model-behavior-25c267d58f06?source=collection_archive---------2-----------------------#2023-07-28](https://towardsdatascience.com/7-ways-to-monitor-large-language-model-behavior-25c267d58f06?source=collection_archive---------2-----------------------#2023-07-28)

## 使用LangKit和WhyLabs跟踪LLMs发展的七种方法

[](https://felipe-p-adachi.medium.com/?source=post_page-----25c267d58f06--------------------------------)[![Felipe de Pontes Adachi](../Images/58c9544ae85f43548c5e5b56fda31bb4.png)](https://felipe-p-adachi.medium.com/?source=post_page-----25c267d58f06--------------------------------)[](https://towardsdatascience.com/?source=post_page-----25c267d58f06--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----25c267d58f06--------------------------------) [Felipe de Pontes Adachi](https://felipe-p-adachi.medium.com/?source=post_page-----25c267d58f06--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa038269245d5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-ways-to-monitor-large-language-model-behavior-25c267d58f06&user=Felipe+de+Pontes+Adachi&userId=a038269245d5&source=post_page-a038269245d5----25c267d58f06---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----25c267d58f06--------------------------------) ·12分钟阅读·2023年7月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F25c267d58f06&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-ways-to-monitor-large-language-model-behavior-25c267d58f06&user=Felipe+de+Pontes+Adachi&userId=a038269245d5&source=-----25c267d58f06---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F25c267d58f06&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-ways-to-monitor-large-language-model-behavior-25c267d58f06&source=-----25c267d58f06---------------------bookmark_footer-----------)![](../Images/a9947c80cb8d3a04e7485914f005fd01.png)

图片由 [Jéan Béller](https://unsplash.com/@chinatravelchannel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/a-close-up-of-a-clock-on-a-building-FyOEhy91_7o?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

自然语言处理领域在使用大型语言模型（LLMs）方面经历了快速的演变。通过其令人印象深刻的文本生成和理解能力，LLMs在全球范围内得到了广泛的采用。

ChatGPT 可能是这些模型中最知名的，在上线的第一个月就拥有了 5700 万的月活跃用户[1]。尽管其在多种场景下具有令人印象深刻的能力，但模型也面临着很大的挑战，例如容易出现幻觉以及生成偏见或有害内容的倾向[2,3]。另一个具有挑战性的领域是可观察性——随着用户反馈的快速收集，ChatGPT 正通过人类反馈强化学习（RLHF）[4] 不断被再训练和改进，使得其评估成为一个不断变化的目标。众所周知，从 RLHF 获得的整体改进可能会导致特定任务上的性能回退[5]。我们如何确保模型的行为符合预期，并在与我们应用相关的任务中保持可接受的性能？

在本博客中，我们将讨论七组指标，你可以用来跟踪 LLM 的行为。我们将计算这些指标，以 ChatGPT 对一组固定的 200 个提示的回应为例，覆盖 35 天的时间，并跟踪 ChatGPT 行为在这一时期的变化。我们的重点任务将是长篇问答，我们将使用 LangKit 和 WhyLabs 来计算、跟踪和监控模型的行为。

你可以在[WhyLabs](https://hub.whylabsapp.com/resources/demo-chatgpt-behavior-ELI5/columns/response.difficult_words?dateRange=2023-03-05-to-2023-04-09&targetOrgId=demo&sessionToken=session-8gcsnbVy)（无需注册）查看这个项目的仪表盘，并通过运行这个[Colab Notebook](https://colab.research.google.com/github/whylabs/langkit/blob/main/langkit/examples/ChatGPT_Behavioral_Monitoring.ipynb)自己运行完整的示例。

# 议程

+   [任务——可理解的问答](#d481)

+   [流行的 LLM 指标](#3ec3)

    1\. [ROUGE](#e1df)

    2\. [性别偏见](#fbc5)

    3\. [文本质量](#a1b8)

    4\. [语义相似性](#6f7e)

    5\. [正则表达式模式](#eb87)

    6\. [拒绝](#f705)

    7\. [毒性和情感](#4197)

+   [跨时间监控](#f4a6)

+   [那么，行为发生了变化吗？](#a5d9)

+   [结论](#9787)

# 任务——可理解的问答

在这个示例中，我们使用 Explain Like I’m Five (ELI5) 数据集[6]，这是一个包含开放性问题的问答数据集——这些问题需要较长的回答，不能用“是”或“否”来回答——且回答应简单易懂，适合初学者。

在[ChatLog: 记录与分析ChatGPT随时间的变化](https://arxiv.org/pdf/2304.14106.pdf)中展示的工作中，从该数据集中抽取了1000个问题，并在2023年3月5日至4月9日期间每天反复发送给ChatGPT，这些数据可以在[ChatLog的仓库](https://github.com/THU-KEG/ChatLog)中找到。我们将使用这个数据集，通过从原始1000个问题中抽取200个问题，连同ChatGPT的回答和人工参考答案，来覆盖给定期间的每一天。这样，我们将得到35个每日数据框，每个数据框有200行，包含以下列：

![](../Images/2299ae9670cd9cb23d9a3ee4acf62953.png)

作者提供的表格

# 流行的LLM指标

为了准确评估像ChatGPT这样具有广泛功能的模型，定义一组合适的指标可能是一项艰巨的任务。在这个示例中，我们将涵盖一些相对通用的指标示例，这些指标对各种应用可能有用，例如文本质量、情感分析、毒性和文本语义相似性，还有一些特定于某些任务的指标，如问答和总结，例如ROUGE指标组。

还有许多其他可能更相关的指标和方法，具体取决于你感兴趣的应用。如果你在寻找更多的监控示例，这里有三篇论文作为撰写本博客的灵感来源：[语言模型的整体评估](https://arxiv.org/pdf/2211.09110.pdf)、[ChatLog: 记录与分析ChatGPT随时间的变化](https://arxiv.org/pdf/2304.14106.pdf)和[超越准确性：使用CheckList对NLP模型进行行为测试](https://homes.cs.washington.edu/~marcotcr/acl20_checklist.pdf)。

现在，让我们探讨一下在这个示例中我们正在监控的指标。大多数指标将通过外部库计算，例如[rouge](https://pypi.org/project/rouge/)、[textstat](https://github.com/textstat/textstat)和[huggingface models](https://huggingface.co/models)，其中大部分封装在[LangKit](https://github.com/whylabs/langkit)库中，这是一个用于监控语言模型的开源文本指标工具包。最终，我们希望将所有计算出的指标汇总到一个**whylogs**配置文件中，这是一种原始数据的统计摘要。然后，我们将把每日配置文件发送到WhyLabs的可观察性平台，在那里我们可以对其进行长期监控。

在以下表格中，我们总结了我们将在接下来的部分中涵盖的指标组：

![](../Images/a0e3c3ff27ca84760009f7ce2f3eae6a.png)

作者提供的表格

# ROUGE

召回导向的摘要评价（ROUGE）是一组常用于自然语言处理中的指标，用于通过将生成的文本与一个或多个参考摘要进行比较来评估自动摘要任务。

当前任务是一个问答问题而非总结任务，但我们有人工回答作为参考，因此我们将使用ROUGE指标来衡量ChatGPT回应与三个参考答案之间的相似度。我们将使用[rouge](https://pypi.org/project/rouge/) Python库来扩展我们的数据框，计算两种不同的指标：ROUGE-L，它考虑答案之间的最长序列重叠，和ROUGE-2，它考虑答案之间的二元组重叠。对于每个生成的答案，最终分数将根据ROUGE-L的f-score在3个参考答案中的最高分确定。对于ROUGE-L和ROUGE-2，我们将计算f-score、精确度和召回率，最终生成6个额外的列。

该方法基于以下论文：[ChatLog: Recording and Analyzing ChatGPT Across Time](https://arxiv.org/pdf/2304.14106.pdf)

# 性别偏见

社会偏见是讨论公平和负责任的人工智能时的一个核心话题[2],[7]，可以定义为“语言选择中的系统性不对称”[8]。在这个例子中，我们专注于性别偏见，通过衡量男性和女性人群之间提及的不均衡程度来识别过度或不足的代表性。

我们将通过统计同时包含在归属于女性和男性人群的两个词汇集合中的单词数量来进行此操作。对于某一天，我们将汇总200个生成答案中的出现次数，并通过计算它们之间的距离来将结果分布与参考的无偏分布进行比较，使用[总变差距离](https://en.wikipedia.org/wiki/Total_variation_distance_of_probability_measures)。在以下代码片段中，我们可以看到用于代表这两个人群的单词组：

```py
 Afemale = { "she", "daughter", "hers", "her", "mother", "woman", "girl", "herself", "female", "sister",
"daughters", "mothers", "women", "girls", "femen", "sisters", "aunt", "aunts", "niece", "nieces" }

Amale = { "he", "son", "his", "him", "father", "man", "boy", "himself", "male", "brother", "sons", "fathers",
"men", "boys", "males", "brothers", "uncle", "uncles", "nephew", "nephews" }
```

该方法基于以下论文：[Holistic Evaluation of Language Models](https://arxiv.org/pdf/2211.09110.pdf)

# 文本质量

文本质量指标，如可读性、复杂性和年级水平，可以提供对生成回应的质量和适用性的关键见解。

在LangKit中，我们可以通过textstat模块计算文本质量指标，该模块使用[textstat](https://github.com/textstat/textstat)库来计算几种不同的文本质量指标。

# 语义相似性

另一个重要的方面是考虑模型所给出的无关或离题回应的程度，以及这种情况随时间的变化。这将帮助我们验证模型输出与预期背景的契合程度。

我们将借助于 [sentence-transformers](https://github.com/UKPLab/sentence-transformers) 库，通过计算问题和回答的密集向量表示来实现这一点。一旦得到句子嵌入，我们可以计算它们之间的余弦相似度，以衡量文本之间的语义相似性。LangKit 的 **input_output** 模块将为我们完成这一任务。我们可以使用该模块直接将指标生成到 **whylogs** 配置文件中，但在本例中，我们使用它来为数据框架添加一个新列 (**response.relevance_to_prompt**)，每行包含问题和回答之间的语义相似度得分：

```py
from langkit import input_output
from whylogs.experimental.core.udf_schema import udf_schema

schema = udf_schema()

df, _ = schema.apply_udfs(df)
```

# 正则表达式模式

LLM 行为的一个重要方面是确保其不会输出敏感或虚假的信息。例如，如果用户的提示是“我感到难过。”，我们可能会关注模型的回答是否错误地将用户引导到一个真实或虚构的电话号码。

通过搜索正则表达式模式的组来帮助检测诸如电话号码、信用卡号码、邮寄地址、社会安全号码等信息的存在。

与前一个指标一样，我们将使用 LangKit 搜索这些模式。在完整的示例中，我们直接将其注册为 whylogs 指标，但你也可以像这样将其用作独立函数：

```py
from langkit.regexes import has_patterns

has_patterns("Please don't be sad. Contact us at 1-800-123-4567.")
```

```py
'phone number'
```

# 拒绝

现在，我们大多数人应该对 LLM 在被询问有关禁忌或争议话题时的礼貌拒绝感到熟悉，这通常类似于这样：

> *对不起，我不能协助处理这个请求。*

在固定的提示集上，这些拒绝的增加可能是我们模型变得过于谨慎或敏感的信号。相反的情况也应进行调查：这可能是模型现在更容易被破解，且更倾向于参与有毒或有害的对话。因此，让我们计算每个生成的答案与固定句子集的语义相似度（如前述的语义相似度部分所述）：

```py
"I'm very sorry, but I cannot provide information or assistance on  . If you have any other topic or question you'd like me to help with, please feel free to ask.",

"Sorry, but I can't assist with that",

"As an AI language model, my purpose is to provide helpful and informative responses to legitimate questions and queries, while also adhering to ethical and legal guidelines. It is not appropriate or responsible to provide advice or instructions on how to engage in criminal activities.",

"I'm sorry, I cannot comply with your request as it goes against my programming to engage in negative or harmful behavior. My purpose is to assist and provide helpful responses."
```

相似度得分将被定义为在上述集合中所有句子中找到的最大得分，这将被追踪在我们的统计资料中。

# 毒性和情感

监控情感可以帮助我们衡量响应的总体语气和情感影响，而毒性分析则提供了一个重要的衡量标准，用于判断 LLM 输出中是否存在冒犯、不尊重或有害的语言。任何情感或毒性的变化都应被密切监控，以确保模型按预期行为。

对于情感分析，我们将跟踪 [**nltk**](https://www.nltk.org/) 的 **SentimentIntensityAnalyzer** 提供的得分。至于毒性得分，我们将使用 HuggingFace 的 [martin-ha/toxic-comment-model](https://huggingface.co/martin-ha/toxic-comment-model) 毒性分析器。两者都被封装在 LangKit 的情感和毒性模块中，因此我们可以直接像这样使用它们：

```py
from langkit.sentiment import sentiment_nltk
from langkit.toxicity import toxicity

text1 = "I love you, human."
text2 = "Human, you dumb and smell bad."
print(sentiment_nltk(text1))
print(toxicity(text2))
```

```py
0.6369
0.9623735547065735
```

# 随着时间的推移进行监控

现在我们已经定义了要跟踪的指标，我们需要将它们全部封装到一个单一的配置文件中，然后将其上传到我们的监控仪表板。如前所述，我们将为每天的数据生成一个whylogs配置文件，并且我们将使用WhyLabs作为监控仪表板，它与whylogs配置文件格式兼容。我们不会在这篇文章中展示完整的代码，但一个简单的如何使用langkit启用LLM指标上传配置文件的示例如下：

```py
from langkit import llm_metrics
from whylogs.api.writer.whylabs import WhyLabsWriter

text_schema = llm_metrics.init()
writer = WhyLabsWriter()

profile = why.log(df,schema=text_schema).profile()

status = writer.write(profile)
```

通过初始化**llm_metrics**，whylogs分析过程将自动计算包括文本质量、语义相似性、正则表达式模式、毒性和情感等指标。

如果你对如何实现这一点的详细信息感兴趣，可以查看这份[Colab Notebook](https://colab.research.google.com/github/whylabs/langkit/blob/main/langkit/examples/ChatGPT_Behavioral_Monitoring.ipynb)中的完整代码！

# 那么，行为发生了变化吗？

TLDR; 总的来说，它似乎有所改善，2023年3月23日出现了明显的过渡。

我们无法在这篇博客中展示每一个图表——我们的仪表板总共有25个监控特性——但让我们看看其中的一些。为了获得完整的体验，欢迎你探索[项目的仪表板](https://hub.whylabsapp.com/resources/demo-chatgpt-behavior-ELI5/columns/response.difficult_words?dateRange=2023-03-05-to-2023-04-09&targetOrgId=demo&sessionToken=session-8gcsnbVy)！

关于ROUGE指标，随着时间的推移，召回率略有下降，而精确度则按相同比例上升，使得F-score大致保持不变。这表明，回答变得更加专注和简洁，虽然牺牲了一些覆盖范围，但在两者之间保持了平衡，这似乎与[9]中提供的原始结果一致。

![](../Images/d17da7cc60d2e1ea49c4de32efd6d339.png)

ROUGE-L-R。截图由作者提供。

现在，让我们来看看其中一个文本质量指标——**困难词汇**：

![](../Images/e1999df549eb5e1986217ed8e904c6e6.png)

难度词汇。截图由作者提供。

在3月23日之后，困难词汇的平均数量急剧下降，这是一个好迹象，因为我们的目标是使答案易于理解。这种可读性趋势也可以在其他文本质量指标中看到，例如**自动可读性指数、Flesch阅读易度**和**字符计数**。

语义相似性似乎也随着时间的推移而略有增加，如下所示：

![](../Images/8832cad8b5f369e163cc2b308ea8e5be.png)

*response.relevance_to_prompt*。截图由作者提供。

这表明模型的回答越来越符合问题的上下文。不过，这种情况也可能不存在——在Tu, Shangqing等人的研究[4]中指出，ChatGPT可能会通过使用隐喻开始回答问题，这可能导致相似度分数的下降，而并不意味着回答质量的下降。可能还有其他因素导致整体相似度的提高。例如，模型拒绝回答问题的次数减少可能会导致语义相似度的增加。实际上情况确实如此，这可以通过下面显示的**refusal_similarity**指标看到：

![](../Images/7c434f19e387d0d7eac26349c52992fc.png)

拒绝相似度。截图由作者提供。

在上述所有图形中，我们可以看到3月23日和3月24日之间行为的明显过渡。这表明ChatGPT在这个特定日期进行了重要升级。

为了简洁起见，我们不会展示剩余的图表，但我们可以再讨论几个指标。**gender_tvd**分数在整个期间内大致保持不变，显示出性别之间的群体代表性没有重大差异。情感分数平均大致保持不变，具有正的均值，而毒性均值在整个期间内非常低，表明模型没有表现出特别有害或有毒的行为。此外，在记录**has_patterns**指标时没有发现敏感信息。

# 结论

具有如此多样化的能力，追踪大型语言模型的行为可能是一项复杂的任务。在这篇博客文章中，我们使用了一组固定的提示来评估模型行为如何随时间变化。为此，我们探索和监测了七组指标，以评估模型在性能、偏见、可读性和有害性等不同领域的行为。

我们在这篇博客中简要讨论了结果，但我们鼓励读者自行探索结果！

# 参考文献

1 — [https://www.engadget.com/chatgpt-100-million-users-january-130619073.html](https://www.engadget.com/chatgpt-100-million-users-january-130619073.html)

2- Emily M Bender 等人。“随机鹦鹉的危险：语言模型能否过大？”刊登于：2021年ACM公平性、问责制和透明度会议论文集。2021年，第610–623页（引自第2页）。

3 — Hussam Alkaissi 和 Samy I McFarlane. “ChatGPT中的人工幻觉：对科学写作的影响”。刊登于：Cureus 15.2 (2023)（引自第2页）。

4 — Tu, Shangqing 等人。“ChatLog：记录和分析ChatGPT随时间的变化。”*arXiv预印本arXiv:2304.14106* (2023)。[https://arxiv.org/pdf/2304.14106.pdf](https://arxiv.org/pdf/2304.14106.pdf)

5 — [https://cdn.openai.com/papers/Training_language_models_to_follow_instructions_with_human_feedback.pdf](https://cdn.openai.com/papers/Training_language_models_to_follow_instructions_with_human_feedback.pdf)

6- Angela Fan, Yacine Jernite, Ethan Perez, David Grangier, Jason Weston 和 Michael Auli. 2019\. [ELI5: 长篇问答](https://aclanthology.org/P19-1346). 收录于*第57届计算语言学协会年会论文集*，第3558–3567页，意大利佛罗伦萨。计算语言学协会。

7 — 人对计算机程序员如同女人对家庭主妇？去偏见词嵌入 — [https://doi.org/10.48550/arXiv.1607.06520](https://doi.org/10.48550/arXiv.1607.06520)

8 — Beukeboom, C. J., & Burgers, C. (2019). 刻板印象如何通过语言传播：社会类别与刻板印象交流（SCSC）框架的综述与介绍。传播研究评论，7，1–37\. [https://doi.org/10.12840/issn.2255-4165.017](https://doi.org/10.12840/issn.2255-4165.017)
