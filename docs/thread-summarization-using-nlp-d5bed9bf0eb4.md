# 使用 NLP 进行线程总结

> 原文：[`towardsdatascience.com/thread-summarization-using-nlp-d5bed9bf0eb4`](https://towardsdatascience.com/thread-summarization-using-nlp-d5bed9bf0eb4)

## 使用 POS 标记、NER 和情感分析进行抽取式总结

[](https://jagota-arun.medium.com/?source=post_page-----d5bed9bf0eb4--------------------------------)![Arun Jagota](https://jagota-arun.medium.com/?source=post_page-----d5bed9bf0eb4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d5bed9bf0eb4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d5bed9bf0eb4--------------------------------) [Arun Jagota](https://jagota-arun.medium.com/?source=post_page-----d5bed9bf0eb4--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d5bed9bf0eb4--------------------------------) ·13 分钟阅读·2023 年 1 月 7 日

--

![](img/2df0e0ffea8820634c8b6f6285b32460.png)

图片由 [Holger](https://pixabay.com/users/hodihu-84128/) 提供，来自 [Pixabay](https://pixabay.com/)

设想一个包含多个参与者的短信（或电子邮件）线程。下面是一个涉及两个人共同规划旅行的示例。每条消息之间由空行分隔。

```py
Here is some lodging near Pismo beach: Cliffs Hotel, SeaCrest, 
Madonna Inn, Shell beach inn

Shell beach & Madonna - a bit pricey.

Check out Inn at the Pier.

Looks good.

What about food?

Coya Peruvian

Any seafood restaurants?

Ada's Fish House. Oyster Loft.

+1 on Oyster loft.

What about bars?

11 22, Boardroom

breweries?

Shell beach brewhouse

Check this lodge out: Wayfarer

Not close enough to where we are going.
```

反复的对话可能会产生大量的信息，尽管这些信息分散在不同的短信中。当这个线程在智能手机的短信应用上展开时，随着更多消息的持续流入，滚动浏览以保持跟进对话可能会是一个挑战。

如果有一个 NLP 机器人能够阅读线程中的所有消息，并将它们总结成一条新的消息，那岂不是很好？然后这条新消息可以作为新消息发布在同一个线程中。

本文解决了这个问题。

首先让我们观察这个线程中的重复模式，其中一些可能会推广到其他线程。

A 先生建议了一些特定的住宿地点。B 先生给出了一些意见，并建议了另一个地方。其中一人询问关于食物的问题。另一人回应了具体的餐馆。对话接着转到酒吧。提出了一些具体建议。随后是意见。接着是后续问题。然后是回应和意见。

**NLP 方法**

是否有现有的 NLP 方法适用于这个用例？从总体上看，有的。这被称为*抽取式总结*。

让我们先从基本层面理解它。

```py
The summary is represented as a suitable subset of the content in the text.
```

这种方法并不一定试图使总结具有可读性，而是旨在捕捉文本中的关键信息。

在深入了解抽取式总结之前，还有一种 NLP 总结方法叫做*抽象式总结*，值得考虑。

这种方法产生的总结是对原始文本的总结性改述。

提取式摘要似乎更适合我们的用例，因为我们的主要目标是创建一个线程文本的简化版本，并保留所有关键内容。不是改写。

此外，请注意，抽象摘要是一个更具挑战性的问题，因为它似乎涉及更深入的自然语言理解和生成。

**在我们的设置中进行提取式摘要**

让我们考虑以下基本方法，如[1]所述。

1.  将文本标记化为句子。

1.  将每个句子标记化为词语。

1.  删除停用词。像*is*、*a*、*an*、*the*、*to*、*for*、*of*这样的词，

1.  以某种方式根据词频对每个句子的剩余文本进行评分。

在我们的线程设置中，我们将用“消息”替换“句子”。也就是说，将文本标记化为句子意味着将其标记化为消息。

这种方法在我们的线程中可能效果如何？

词语标记化确实有帮助。它有助于识别和删除停用词。

也就是说，稍后我们将看到一种不同的提取方法，完全避免删除停用词。

**我们的线程已标记化**

首先，我们将把线程标记化为消息序列，然后将每条消息标记化为词语序列。在 python 术语中，这只是一个列表的列表。

最重要的是，在这种表示中，我们没有保留发送特定消息的人的身份。

**去除停用词的效果是否足够？**

以下是去除停用词后的文本版本。在这样做时，我们必须决定什么是停用词，什么不是。我们遵循了常见的做法，这通常偏向保守。

要删除的停用词在/…/中标记。

```py
Here /is/ some lodging near Pismo beach: Cliffs Hotel, SeaCrest, 
Madonna Inn, Shell beach inn. Shell beach & Madonna - /a/ bit pricey.
Check out Inn /at the/ Pier. Looks good. What about food? Coya Peruvian. 
Any seafood restaurants? Ada's Fish House. Oyster Loft. +1 on Oyster loft.
What about bars? 11 22, Boardroom breweries? Shell beach brewhouse 
Check this lodge out: Wayfarer Not close enough /to/ where we /are/ going.
```

斜体和粗体中的词是我们删除的停用词。显然，这帮助不大。此外，请注意，旅馆名称*Inn at the Pier*中的*at the*也被删除了。

**朝着更好的解决方案**

正如我们刚刚看到的，单独去除停用词是无效的。首先，它没有删除足够的词。其次，它有时会删除出现在实体名称中的停用词。

让我们更一般地考虑一下。如果有某种方法来确定哪些词应该删除，哪些词应该保留，那就太好了。有人可能会说，这不就是在重新描述问题吗？

是的。不过，以这种方式来看，打开了新的思维方式。

在我们的情况下，接下来我们介绍的自然语言处理方法——词性标注，将比单独去除停用词带来显著改善。事实上，它将消除对去除停用词的需求。

**词性分析**

一种广泛使用的自然语言处理方法是为文本中的每个词标记其词性类别。例如*nouns*、*verbs*、*adjectives*等。

那么我们为什么认为这会对我们的线程摘要需求有所帮助呢？这是因为我们认为一个词的词性将作为是否应该在摘要中保留该词的良好预测因素。我们期望删除动词、冠词和代词。我们期望保留名词。

让我们在示例中测试这个想法。

我们在[`parts-of-speech.info/`](https://parts-of-speech.info/)上应用了词性标注服务于我们的线程文本。（所有消息被串联，消息边界以段落分隔符表示。）

下面是我们得到的结果。

![](img/15f2fbdd1c195cc230d404ae3358c9de.png)

颜色到词性标签的映射如下。

![](img/d6544fa4cfa8074a6ed2b8ad28c44751.png)

各种词的词性标签确实对总结很有用。我们希望保留所有名词，也许还包括形容词，因为四个中有两个携带明确的情感（价格，好）。

也就是说，虽然相对于仅仅去除停用词已有所改进，但仅仅基于词性标签的简单过滤可能还不够。原因如下。

首先，即使我们仅保留名词，仍然有太多的词语。

其次，就像我们在去除停用词时发生的情况一样，我们最终删除了旅馆名称*Inn at the Pier*中的*at the*。这不好。我们遮蔽了一个可能值得进一步研究的旅馆名称。

第三，形容词词性标签虽然提供了一些情感信号，但与情感的相关性并不足够强。

**另一种方法**

我们首先观察到，线程中的几乎所有显著信息都属于三种 NLP 类别之一：*命名实体*、*情感*和*显著短语*。

如我们将很快看到的，能够区分这些将有助于改善我们的线程摘要器。

我们将从命名实体开始，因为这是该线程中的主要类别。

**命名实体**

在 NLP 中，任何短小的文本，通常是一到三个词，（i）是一个“事物”并且（ii）可以命名的，都称为*命名实体*。例子包括人名、国家名、餐厅名、酒店名等等。有关 NLP 中命名实体识别的详细帖子，请参见[2]。

拥有能够识别城市名称、餐厅名称和酒店名称的命名实体识别器，我们将能够更好地进行摘要。只要我们能够组装出足够丰富的城市名称、餐厅名称和酒店名称集合，就可以通过训练现成的命名实体识别器实现这一点。

下面是识别了上述命名实体并删除了其余文本后的摘要文本样式。我们还删除了段落分隔符，并在需要的地方添加了句号。（段落分隔符对摘要文本没有价值。）

```py
Pismo beach: Cliffs Hotel, SeaCrest, Madonna Inn, Shell beach inn. Shell 
beach & Madonna - . Inn at the Pier. Coya Peruvian. Ada's Fish House. 
Oyster Loft. Oyster loft. Shell beach brewhouse. Wayfarer.
```

由于我们的文本摘要器进行命名实体识别，如果需要的话，我们可以利用这一点，通过标注命名实体来呈现摘要。像这样：

```py
CITY: Pismo beach: 
HOTELS: Cliffs Hotel, SeaCrest, Madonna Inn, Shell beach inn. 
HOTELS: Shell beach & Madonna — . Inn at the Pier. 
RESTAURANTS: Coya Peruvian. Ada’s Fish House. Oyster Loft. Oyster loft. 
Shell beach brewhouse. 
HOTEL: Wayfarer.
```

还不错。不过，我不确定添加实体名称是否增加了价值，或者在这个线程中是否造成了杂乱。这确实告诉我们这是一种可能性。在较长的线程中，这可能效果更好。

不过，仍然有改进的空间。我们将改进分为以下几种类型，顺序不分先后。

1.  *牡蛎阁*被提到两次。幸运的是，去重很容易（在这个具体的情况下），因为它已经被识别为一个命名实体。

1.  情感没有被提取出来。例如*看起来不错*、*有点贵*、+1 和*离我们要去的地方不够近*。

1.  显著短语，即标签，也没有被提取出来。具体而言，*海鲜餐厅、酒吧、啤酒厂、旅馆（住宿）*。

关于 1)没有更多要说的。

**情感**

关于 2)，这属于 NLP 情感分析的范畴。更具体地说，是基于方面的情感分析，即检测哪些情感适用于文本的哪些部分。与情感分类不同，情感分类是确定文本中的整体情感是积极、消极还是中性的任务。

详细的情感分析帖子请参见[3]。它详细介绍了基于方面的情感分析和情感分类，并提供了说明性例子。

话虽如此，在我们具体的讨论中，让我们尝试更好地理解我们真正需要什么，能够做出哪些假设，并考虑我们可能将它们推进多远。简而言之，让我们(i) 检查我们的具体用例，并(ii) 逐步推进。

首先，让我们将焦点缩小到我们想要提取的情感的消息和紧接在其后的消息。我们选择后者，因为消息中的情感可能在前一条消息中。我们已经在/…/中标注了携带情感的区域。

```py
Here is some lodging near Pismo beach: Cliffs Hotel, SeaCrest, 
Madonna Inn, Shell beach inn. Shell beach & Madonna - /a bit pricey/.
Check out Inn at the Pier. /Looks good/.
…
Ada's Fish House. Oyster Loft.
/+1/ on Oyster loft.
…
Check this lodge out: Wayfarer. /Not close enough to where we are going/.
```

首先，让我们假设我们检测到了携带情感的术语。即使是基于词典的方法在这里也是一个很好的起点，尽管它的扩展性仍然是一个未解的问题。

我们的第一个想法是保持检测到的术语在总结中的位置不变。让我们看看最终结果是什么样的。与之前一样，携带情感的术语用/…/标记。

```py
Pismo beach: Cliffs Hotel, SeaCrest, Madonna Inn, Shell beach inn. Shell 
beach & Madonna — /a bit pricey/. Inn at the Pier. /Looks good/. Coya 
Peruvian. Ada’s Fish House. Oyster Loft. /+1/ on Oyster loft. Shell beach 
brewhouse. Wayfarer. /Not close enough/
```

好的。

第一点，一个小点是，由于我们知道哪些文本块是携带情感的术语，我们可以将它们与实体视觉区分开来。这在/…/中有所说明。

第二点是我们可以更明确地将携带情感的术语与紧接其前的实体关联起来。当然，这样做有一定的风险。让我们看看结果会是什么样的。

```py
Pismo beach: Cliffs Hotel, SeaCrest, Madonna Inn, Shell beach inn. 
Shell beach & Madonna — a bit pricey. Inn at the Pier (Looks good). 
Coya Peruvian. Ada’s Fish House. Oyster Loft (+1) on Oyster loft. 
Shell beach brewhouse. Wayfarer (Not close enough)
```

在这种情况下很好。

**显著短语**

现在让我们关注我们希望被检测并添加到总结中的显著短语。在我们的讨论中，这些是*海鲜餐厅、酒吧、啤酒厂、旅馆（住宿）*。再次，我们假设这些短语已被检测到，并查看包含它们的结果。这次我们用/…/标记检测到的显著短语。

```py
lodging Pismo beach: Cliffs Hotel, SeaCrest, Madonna Inn, Shell beach inn. 
Shell beach & Madonna — a bit pricey. Inn at the Pier (Looks good). 
Coya Peruvian. /seafood restaurants/. Ada’s Fish House. Oyster Loft (+1) 
on Oyster loft. /bars/. /breweries/. Shell beach brewhouse. Wayfarer 
(Not close enough)
```

嗯。注入的显著短语没有像注入的情感那样增加太多价值。

差别在哪里？让我们分析一下。首先，添加情感丰富了总结，因为情感不能从周围的文本中预测。

在我们的线程中，检测到的突出的短语并没有提供太多信息。例如，读者可以看到接近*住宿*的实体是酒店，因此添加该词并不会丰富总结。同样的观点也适用于*海鲜餐馆*、*酒吧*和*酿酒厂*。

用数据科学的语言，我们可以说，将情感添加到总结中获得的信息比添加突出的短语获得的信息要高。

第二点是，在文本消息的线程设置中，情感通常会集中在刚刚提到的实体上，无论是在同一消息中还是在之前的消息中。相比之下，突出的短语更为开放。在我们的线程中，突出的短语出现在问题中，这些问题的答案稍后会出现，可能会分散在不同的位置。

第三点是从读者的角度来看这些总结。这些读者通常是线程中的一些参与者。他们对线程中特定实体的其他人的情感将对他们很重要。

话虽如此，问题并非非黑即白，因为可以认为添加突出的短语确实增加了一些信息。考虑*海鲜餐馆*。我们现在知道对话中的某人对海鲜餐馆感兴趣。如果其他人对此建议有情感反馈，那么这一短语所传递的信息只会增加。

话虽如此，我们仍然可以说，相对而言，命名实体和情感比突出的短语更值得保留在总结中。我们可以这样做：最初保留突出的短语，但如果总结变得过长，则将其删除。

**回顾**

现在让我们回顾并反思 POS 标记与命名实体识别、情感分析和突出的短语提取的组合。

单独去除停用词非常粗略。根据词性决定去除哪些词和保留哪些词则更细致。用命名实体识别、情感分析和突出的短语提取的组合替代 POS 分析则更为精细。

因此，随着我们从最简单的方法进展到最复杂的方法，总结的质量会提高。当然，工作量也会增加。对于第三种方法，我们需要使用或训练合适的命名实体识别器。此外，还需要使用情感分析器，并可能进一步改进它。识别突出的短语是另一个问题。我们在本文中没有深入探讨这一点，因为在某个阶段，保留这些短语在总结中的理由减弱了。

**出版后的更新**

在我们示例中的线程中，一个之前遗漏的反复出现的主题是讨论各个相关人员的工作时间。

在我们的线程示例中，让我们将下面的内容作为前几个消息添加进去。

```py
What days work for you all? For me, April 10–11, 17–18, or May 21–22 work.

None of these work for me. How about April 2–3?

...

Yup, Apr 2–3 works.
```

上述的“…”是一个指示，表示在两个包夹消息之间还有其他消息。

现在让我们致力于将上述消息中的关键信息整合到我们的总结中。

首先，我们希望扩展我们的命名实体识别器以识别日期。下面是相同的消息，我们希望识别为日期的部分已用粗体标出。

大家都能工作哪些天？对我来说，**4 月 10–11 日**、**17–18 日**或**5 月 21–22 日**都可以。这些日期对我来说都不行。**4 月 2–3 日**怎么样？是的，**4 月 2–3 日**可以。

识别*日期*实体在上述消息中并不难。字典基础、序列基础和模式基础机制的混合将是一个有效的开始，并可以随着时间的推移进行优化。

例如，模式<*month*> <*num*>-<*num*>将捕捉到上述一些日期。<*month*>来自字典，包括模糊条目如*Apr*，<*num*>可以通过正则表达式匹配一位或两位数字。

这将无法检测到*17–18*在*4 月 10–11 日、17–18 日或 5 月 21–22 日*中是一个日期。

将日期作为命名实体检测也提出了以下想法。我们可以将那些检测到日期实体的消息归入总结的一个单独部分，我们可以恰当地称之为*日期*。

下面是一个增强了这些信息的总结版本。

```py
Pismo beach: Cliffs Hotel, SeaCrest, Madonna Inn, Shell beach inn. Shell 
beach & Madonna - a bit pricey. Inn at the Pier (Looks good). Coya 
Peruvian. Ada's Fish House. Oyster Loft (+1) on Oyster loft. Shell beach 
brewhouse. Wayfarer (Not close enough)

Dates: For me, April 10–11, 17–18, or May 21–22 work. None of these work 
for me. How about April 2–3? Yup, Apr 2–3 works.
```

如果我们能够推断出*4 月 2 日*–*3 日*适用于每个人，那就更好了。我们在这里不会追求这一点。

**总结**

在这篇文章中，我们讨论了构建一个总结文本消息的问题，该消息概述了特定对话线程中各个消息的内容。

我们取了一个现实的例子，并讨论了一些可能在这里有用的 NLP 方法。

我们观察到，去除停用词并基于词频评分是不够的。

从这里，我们进展到了使用文本中各种词汇的词性信息。这效果更好，但仍不够理想。

从这里我们进展到了使用命名实体识别和情感分析。在我们的线程中，这效果相当好，因为该线程充满了命名实体。我们能够识别这些命名实体并将其纳入总结中，并附上相关的情感。

最后，虽然我们看到命名实体及其附带的情感对于检测和添加到总结中是至关重要的，至少在我们的线程中，将此扩展到检测额外的显著短语并将其添加到总结中的价值较小。我们也解释了原因。

**参考文献**

1.  [使用 NLTK 在 Python 中进行提取式文本摘要](https://www.mygreatlearning.com/blog/text-summarization-in-python/)

1.  NLP 中的命名实体识别。实际用例、模型、方法……

1.  NLP 中的文本情感分析。问题、用例和方法
