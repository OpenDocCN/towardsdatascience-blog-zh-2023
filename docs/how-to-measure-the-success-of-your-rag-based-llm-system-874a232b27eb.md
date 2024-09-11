# 如何衡量您的基于 RAG 的 LLM 系统的成功

> 原文：[https://towardsdatascience.com/how-to-measure-the-success-of-your-rag-based-llm-system-874a232b27eb?source=collection_archive---------0-----------------------#2023-10-23](https://towardsdatascience.com/how-to-measure-the-success-of-your-rag-based-llm-system-874a232b27eb?source=collection_archive---------0-----------------------#2023-10-23)

## 使用机器来评估机器

## 包括一种新的定性评分方法和详细解释。

[](https://medium.com/@ccrngd1?source=post_page-----874a232b27eb--------------------------------)[![Nicholaus Lawson](../Images/5c19c9bea340514d6839fdd9f5a485aa.png)](https://medium.com/@ccrngd1?source=post_page-----874a232b27eb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----874a232b27eb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----874a232b27eb--------------------------------) [Nicholaus Lawson](https://medium.com/@ccrngd1?source=post_page-----874a232b27eb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F87247d6fe280&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-measure-the-success-of-your-rag-based-llm-system-874a232b27eb&user=Nicholaus+Lawson&userId=87247d6fe280&source=post_page-87247d6fe280----874a232b27eb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----874a232b27eb--------------------------------) ·11分钟阅读·2023年10月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F874a232b27eb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-measure-the-success-of-your-rag-based-llm-system-874a232b27eb&user=Nicholaus+Lawson&userId=87247d6fe280&source=-----874a232b27eb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F874a232b27eb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-measure-the-success-of-your-rag-based-llm-system-874a232b27eb&source=-----874a232b27eb---------------------bookmark_footer-----------)![](../Images/3bb2a6412f41e639ce0cc4167f345371.png)

由 Stable Diffusion XL 生成的图像

增强生成研究，或RAG，是今年出现的大型语言模型（LLMs）最常见的使用案例。尽管文本摘要和生成通常是个别用户的重点，但企业已经意识到他们需要利用自己的数据来利用这项技术。回顾我仍然使用LLM的情况，文本生成位居高位。我想问Bard问题并让它搜索网络；我希望Claude重写电子邮件或博客文章，以增强我的内容。但我遇到的最令人兴奋的使用是将我自己的数据输入LLM。我想搜索我的笔记、电子邮件、日历和Slack消息，并让Llama充当另一个我（但这个我能记住今天之前发生的细节）。

这不是关于如何构建RAG的帖子（已经有很多这样的帖子了……我还在为其他时间写这篇帖子）。今天我们将探讨的是如何评估RAG系统。

# 我们如何从RAG中获取数据？

在深入细节之前，我们先设定一个基础。当我们谈论RAG时，我们指的是系统的两个部分。

## 知识源

知识源可以是向量数据库、搜索引擎、加载到内存中的几个文本文件、SQL 数据，或任何存储我们数据的地方。

## LLM

一旦我们有了数据，我们将其输入到LLM中。这是通过上下文窗口完成的。因此，*最终*，我们进行搜索，得到一些文本，将找到的文本放入提示中，然后将问题传递给LLM。模型随后从该上下文窗口中获取所有内容并提供答案。

## 为什么这很重要？

当我们谈论评估RAG系统时，我们必须知道在定义如何评估之前我们要评估什么。我们现在可以看到需要检查两个部分。初始数据检索是最关键的部分。LLM通常在总结/回答问题方面表现出色，前提是提供了上下文数据。可能缺乏的部分是搜索功能本身。

这些知识源有一些内建的限制。例如，当使用向量数据库存储大型文本文件时，你必须对数据进行‘切块’处理。这是什么意思？假设你有一个100页的文档，但数据库一次只能处理1页。加载文档并进行搜索时，数据库只能逐页检查（好吧，这有点简化，但请耐心一点；这对于实际应用已经足够接近了）。当我们找到匹配的搜索结果时，确实有可能整个问题的答案不在这一页上。真遗憾！我们只能得到一页！这很好地说明了在担心LLM的输出之前需要检查系统这一部分的原因。

# 我们需要评估什么？

## 评估初始搜索

这可能不是大多数技术专家想听到的答案。评估结果需要一定程度的人为评估。

为什么？如果一个企业使用的是私有数据，自动化测试来验证搜索结果是否完全准确将很困难。不要担心，这不必完全手动；我们可以自动化其中的一部分。让我们深入了解一下。

我看到有两种实现方式用于初步验证和评估。

第一个选项是为数据集设置一组常见的预期问题，并让人工QA团队验证搜索结果。例如，如果你的团队负责为银行构建一个客服问答机器人，一些常见问题可能是：“我必须在账户中保持的最低金额是多少？”，“我怎么还款？”，“我的分行几点开门？”。如果你的QA能够提供问题和预期答案，并以类似CSV文件的形式提供，这样可以程序化读取，那就最好不过了；然后，我们可以使用一些自动化测试，稍后会在本文中进一步介绍。

如果没有时间或资源，第二种方法是QA团队实时搜索和审查。这是早期POC和原型的一个选项，但请注意，这不能扩展到实际生产工作负载中。

## 评估LLM响应

一旦我们对知识来源的数据的可靠性感到放心，我们必须确保最终答案的准确性。RAG系统在减少幻觉的可能性方面表现出色，这可以通过调整底层提示来进一步延伸。然而，它可能会遗漏信息、误解输入的数据，或试图引入训练中的先验知识。

评估这一步骤类似于评估之前的搜索。如果QA团队可以提供问题和预期答案，我们可以尝试程序化地评估答案。

现在让我们看看其中的一些选项。

# 评估框架

需要记住的是，LLM和RAG在其成熟周期中还处于早期阶段。自ChatGPT首次亮相以来仅过去了一年，每天都有更多的进展、模型、框架和研究。尽管如此，一些指标正成为衡量这些系统性能的标准方式。

我们不会涵盖评估基础LLM的方法。像ARC、MMLU、HellaSwag等都针对底层语言模型。你不需要自己运行这些指标；可以查看像这样的站点

[https://llm-leaderboard.streamlit.app/](https://llm-leaderboard.streamlit.app/) 和 [https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard)

查看不同模型的表现。我们只对从RAG系统中得到的结果感兴趣。

这引导我们关注像 ROUGE、BLEU、BLEURT 和 METEOR 这样的算法。让我们逐一详细了解每个指标。我还会附上一小段代码，展示如何调用每个指标以及输出分数的样子。我引入评估框架以便开始，并包括了我想要评分的参考和答案。

```py
!pip install evaluate --quiet
!pip install rouge_score --quiet
!pip install importlib-metadata --quiet
!pip install datasets==2.10.1 --quiet 
!pip install git+https://github.com/google-research/bleurt.git --quiet
!pip install sacrebleu --quiet
!pip --no-cache-dir install bert_score==0.3.9 --quiet
!pip install sacremoses --quiet
!pip install jiwer==2.5.1 --quiet
!pip install Cython 

import evaluate

# If you have a translation and reference corpus:
predictions = ["In Dungeons & Dragons, the metallic dragons come in brass, bronze, copper, gold, and silver varieties. Each has scales in hues matching their name - brass dragons have brassy scales, bronze have bronze scales, etc. The metallic dragons are generally more benign than the chromatic dragons in D&D lore."]

references =["""The five basic chromatic dragons (red, blue, green, black, and white) and metallic dragons (copper, brass, silver, gold, and bronze) appeared in the fifth edition Monster Manual (2014) in wyrmling, young, adult, and ancient. Gem dragons and other new-to-fifth-edition dragons appeared in Fizban's Treasury of Dragons (2021)"""]
```

## [ROUGE](https://huggingface.co/spaces/evaluate-metric/rouge)（召回导向的摘要评估指标）

ROUGE 是一组用于评估自动摘要和机器翻译输出的指标。它基于系统输出与参考摘要之间的重叠 n-gram 的计数。

```py
#predictions (list): list of predictions to score. Each prediction should be a string with tokens separated by spaces.
#references (list or list[list]): list of reference for each prediction or a list of several references per prediction. Each reference should be a string with tokens separated by spaces.
#rouge_types (list): A list of rouge types to calculate. Defaults to ['rouge1', 'rouge2', 'rougeL', 'rougeLsum'].
#Valid rouge types:
##"rouge1": unigram (1-gram) based scoring
##"rouge2": bigram (2-gram) based scoring
##"rougeL": Longest common subsequence based scoring.
##"rougeLSum": splits text using "\n"
#use_aggregator (boolean): If True, returns aggregates. Defaults to True.
#use_stemmer (boolean): If True, uses Porter stemmer to strip word suffixes. Defaults to False.

rouge = evaluate.load('rouge')

results = rouge.compute(predictions=predictions, references=references, use_aggregator=False)
print(results)
```

```py
{
'rouge1': [0.3636363636363636], 
'rouge2': [0.06185567010309278], 
'rougeL': [0.22222222222222224], 
'rougeLsum': [0.22222222222222224]
}
```

## [BLEU](https://huggingface.co/spaces/evaluate-metric/bleu)（双语评估指标）

BLEU 是一种用于自动评估机器翻译输出的指标。它基于候选翻译与一组参考翻译的 n-gram 精度。

```py
#predictions (list of strs): Translations to score.
#references (list of lists of strs): references for each translation.
#max_order (int): Maximum n-gram order to use when computing BLEU score. Defaults to 4.
#smooth (boolean): Whether or not to apply Lin et al. 2004 smoothing. Defaults to False.

#bleu (float): bleu score
#precisions (list of floats): geometric mean of n-gram precisions,
#brevity_penalty (float): brevity penalty,
#length_ratio (float): ratio of lengths,
#translation_length (int): translation_length,
#reference_length (int): reference_length

bleu = evaluate.load(“bleu”)

results = bleu.compute(predictions=predictions, references=references,max_order=4)
print(results)
```

```py
{
'bleu': 0.07342349837092484, 
'precisions': [0.4262295081967213, 0.11666666666666667, 0.03389830508474576, 0.017241379310344827], 
'brevity_penalty': 1.0, 
'length_ratio': 20.333333333333332, 
'translation_length': 61, 
'reference_length': 3
}
```

## [BLEURT](https://huggingface.co/spaces/evaluate-metric/bleurt)（基于变换器的 BLEU 回归）

BLEURT 是一种自然语言生成（NLG）的评估指标。它基于 BERT，使得 BLEURT 能够学习词汇和短语之间的统计关系，并识别 NLG 输出中的模式。

BLEURT 已被证明在各种任务上优于其他自然语言生成（NLG）评估指标，如 BLEU 和 ROUGE，包括机器翻译、文本摘要和问答系统。

```py
#output is always a number between 0 and (approximately 1). 
#This value indicates how similar the generated text is to the reference texts, with values closer to 1 representing more similar texts.

bleurt = evaluate.load("bleurt", module_type="metric")
results = bleurt.compute(predictions=predictions, references=references)

print(results)
```

```py
{
'scores': [0.6028875708580017]
}
```

## [METEOR](https://huggingface.co/spaces/evaluate-metric/meteor)（显式排序翻译评估指标）

METEOR 是一种用于机器翻译输出的自动评估指标。它还具有其他指标所没有的特性，如词干提取、同义词匹配和标准的精确词汇匹配。该指标旨在解决更流行的 BLEU 指标遇到的一些问题，并在句子或片段级别上与人工判断产生良好的相关性。

```py
#predictions: a list of predictions to score. Each prediction should be a string with tokens separated by spaces.
#references: a list of references (in the case of one reference per prediction), or a list of lists of references (in the case of multiple references per prediction. Each reference should be a string with tokens separated by spaces.
#alpha: Parameter for controlling relative weights of precision and recall. The default value is 0.9.
#beta: Parameter for controlling shape of penalty as a function of fragmentation. The default value is 3.
#gamma: The relative weight assigned to fragmentation penalty. The default is 0.5.

#outputs 0-1 - .317 is acceptable score
meteor = evaluate.load('meteor')
results = meteor.compute(predictions=predictions, references=references)

print(results)
```

```py
{
'meteor': 0.19316493313521543
}
```

# 我被承诺会有新东西！

虽然我已经引起了你的注意，但我想介绍一个新的想法。虽然这四种算法会给出一个量化的分数，让你的 QA 团队能够快速确定答案/摘要是否相似，但仍存在一些不足之处。

首先，参考句子和结果可能足够相似以回答用户的问题，但它仍然可能获得较低的分数。运行一组已知的问题和答案以建立良好的基准并将未来的答案与该基准进行比较是至关重要的。

其次，它无法告诉你分数为何受损。是因为重复词汇会受到惩罚吗？还是因为缺少了一些词汇？摘要是否完全遗漏了答案中的重要部分？没有办法得知。

最后，仅仅因为一个回答获得低分并不一定意味着人类会认为这个回答不足或错误。基准在这里可能有帮助，用于确定什么是可接受的评分，但在使用这些评分来判断RAG答案时，保持一些怀疑态度是重要的。

## LLMs评分LLMs

BLEURT向我们介绍了我们可以以某种方式使用LLMs来评估RAG系统的答案。如果我们直接利用这一点呢？我们指示LLM给我们的答案一个定性评分，并提供列点的理由和评分的叙述解释。这让我们兼得两全其美的效果。我们可以提取一个数值评分来报告给用户和QA；我们也可以提供更多细节来解释为什么答案得分低。

这里是一个可以用于ClaudeV2的示例提示模板。我们传递关于如何希望模型评分我们答案的指令，传递参考数据，并传递我们从RAG系统收到的答案。

```py
prompt_data = """
Human: 
You are to rate a summarization on the scale of 0-10, with 0 being completely incorrect and 10 being a perfect summzarization of the given text.
Explain why you give your score.
Give a bullet point list of major differences between the reference and the summary.
I will supply a reference text by denoting REF: and the summarization to compare against with SUMMARY:.

REF:
The five basic chromatic dragons (red, blue, green, black, and white) 
and metallic dragons (copper, brass, silver, gold, and bronze) 
appeared in the fifth edition Monster Manual (2014) in wyrmling, young, adult, and ancient. 
Gem dragons and other new-to-fifth-edition dragons appeared in Fizban's Treasury of Dragons (2021)

SUMMARY:
In Dungeons & Dragons, the metallic dragons come in brass, bronze, copper, gold, and silver varieties. 
Each has scales in hues matching their name - brass dragons have brassy scales, bronze have bronze scales, etc. 
The metallic dragons are generally more benign than the chromatic dragons in D&D lore.

Assistant:"""
```

```py
 Score: 5/10

Explanation:

The summary captures some accurate high-level information about metallic dragons in D&D, such as listing the main types and noting their more benign nature compared to chromatic dragons. However, it leaves out a lot of key details from the reference text:

- Does not mention the chromatic dragons (red, blue, green, black, white) that were described.
- Does not mention the different age categories (wyrmling, young, adult, ancient).
- Does not mention the Monster Manual or Fizban's Treasury of Dragons sourcebooks.
- Only focuses on metallic dragons, while the reference text had information about both metallic and chromatic dragons.

Major differences:

- No mention of chromatic dragons
- No mention of dragon age categories
- No mention of Monster Manual or Fizban's Treasury of Dragons
- Only discusses metallic dragons, not full scope of reference text
- Does not convey the introduction timeline of dragons across 5th edition books
```

就这样。如果团队能提供预期的答案，我们可以将RAG答案反馈到LLM中进行评分。好处是我们不必依赖LLM的先验知识，因为我们仍在传输相关数据。我们可以使用与RAG系统不同的LLM，这意味着我们甚至可以让多个模型来评分我们的输出，以确保评估的平衡性。

这种方法还给我们提供了出错的优秀解释。在这个例子中，我问了关于DND宇宙中存在哪些龙的问题。评判LLM正确识别出它没有提到色彩龙。然而，它还因为没有包括龙的年代、DND怪物手册或扩展冒险而扣分。这些遗漏对我提出的问题并不重要，但这允许QA团队自行决定一次。

# 接下来我们怎么做？

基于RAG的系统及其创建框架每天都在进步。评分它们的新方法和机制也会不断进步。甚至有来自像LangChain这样的巨头的工具可以帮助完成这个任务，例如[LangSmith](https://www.langchain.com/langsmith)。

在我们等待更多进展的同时，结合一些手动验证数据和HuggingFace度量库或LLMs本身，给我们提供了一个很好的开始信任这些系统的方法。

记住，一旦你对你的新RAG充满信心并准备投入生产，答案的评估不会停止！作为常规监控和审计工作的一部分，你必须继续存储问题和答案，并计划进行人工评估工作，以评分和标记提供给最终用户的答案。然而，这个话题另说。
