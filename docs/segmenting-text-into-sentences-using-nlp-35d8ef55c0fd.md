# 使用 NLP 将文本分割成句子

> 原文：[`towardsdatascience.com/segmenting-text-into-sentences-using-nlp-35d8ef55c0fd`](https://towardsdatascience.com/segmenting-text-into-sentences-using-nlp-35d8ef55c0fd)

## **特征工程、统计模型以及从反馈中学习**

[](https://jagota-arun.medium.com/?source=post_page-----35d8ef55c0fd--------------------------------)![Arun Jagota](https://jagota-arun.medium.com/?source=post_page-----35d8ef55c0fd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----35d8ef55c0fd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----35d8ef55c0fd--------------------------------) [Arun Jagota](https://jagota-arun.medium.com/?source=post_page-----35d8ef55c0fd--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----35d8ef55c0fd--------------------------------) ·10 分钟阅读·2023 年 1 月 30 日

--

![](img/b2d3d765f44d8d3e011a1660ef32353e.png)

图片由 [Nile](https://pixabay.com/users/nile-598962/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=623167) 提供，来源于 [Pixabay](https://pixabay.com/)

在自然语言处理（NLP）中，将文本文档分割成句子是一个有用的基本操作。这是许多更复杂的 NLP 任务的第一步。例如，在写作过程中检测和纠正文本中的错误[1]，或者检测命名实体[2]。

在前者中，想法是常见错误不会跨越句子边界。这对于后者也是成立的。命名实体也通常不会跨越句子边界。

在任何情况下，这都大大简化了问题。例如，训练和评估可以依赖于一个句子语料库。即使是从更长的文档中提取的句子也可以独立处理。

在这篇文章中，我们将讨论将文本分割成句子的问题。我们将采用一种“苏格拉底式方法”的风格。通过这种方法，我们将从头开始迭代地构建一个启发式预测器，采用一种“迭代假设精炼”的风格。适当的猜测和问题推动精炼。在这个过程中，ChatGPT 将非常有帮助。

我们最终采用的特定方法在精神上类似于流行的 Punkt 算法 [5]。

让我们开始吧。

首先，让我们明确一点：我们将从原始文本开始。没有诸如分词、转小写、去除停用词等预处理。这些操作可能会丢失一些对预测任务有用的信号。

我们都知道，大多数句子以句号结束。因此，我们最简单的预测器是

```py
If the current character is a period
  Predict that we are at a sentence boundary
```

显而易见的问题是，“这在哪些方面会失败”？例如，句子可能以问号结束。或者以感叹号结束。好吧，让我们稍微抽象一下这个规则。

```py
If the current character is boundary_marker
  Predict that we are at a sentence boundary
```

我们将 boundary_marker 设置为{*句号* (.), *问号* (?), *感叹号* (!) }。

好吧，那么现在同样的问题是，“**这**在哪儿失败”？考虑“Ph.D.”。我们不希望第一个句号被视为句子边界。

直观上，如果句号在一个单词中，我们就不是在句子边界。

所以下面的规则应该会更有效。

```py
If the current character is boundary_marker and followed by a white space
  Predict that we are at a sentence boundary
```

好吧，这样可以过滤掉涉及单词内句号的误报，并且似乎没有丢失任何召回率。

在查看本文的句子时，我们还注意到紧接着的句子中的第一个词是大写的。呃，这是一个基本的语法规则。好吧，现在它也是句子边界的预测指标。

鉴于上述情况，让我们进一步优化规则。

```py
If the current character is boundary_marker and followed by right_context
  Predict that we are at a sentence boundary
```

其中 right_context 是“空格后跟大写字母”。

通过添加“大写字母”，我们收紧了条件。这如何影响精确度和召回率？

精确度不可能下降。召回率可能下降。考虑以下情况。文本短且非正式，也许是匆忙写成的——例如在短信中。我们可能会错过实际的句子边界，因为新句子的第一个词可能没有大写。

好吧，这实际上让我们思考一下我们是否需要一个用于干净文本的句子边界预测器，还是用于不一定干净的文本。看来问题的复杂性发生了变化。

如果我们想在不一定干净的文本中预测句子边界，因为召回率可能会下降，我们需要问一下，当我们收紧条件时，精确度是否提高。

考虑这个例子，我在询问 ChatGPT 时设法诱导出来的。

```py
give me examples of sentence boundary false positives involving periods
```

我不确定它是否理解了问题，但我确实在其回答中找到了一些我可以使用的答案。

```py
The U.S. is a big country.
```

“句号后跟空格”的条件会在*U.S*之后立即标记为句子边界，这显然是错误的。

好吧，我们继续前进。接下来思考一下。如果我们在条件中添加左侧上下文呢？新的条件将是

```py
If the current character is boundary_marker and is preceded by 
left_context and is followed by right_context
  Predict that we are at a sentence boundary
```

在不一定干净的文本中，可能会这样。我们可以从右侧上下文中去掉“大写字母”。这将有助于召回，但可能会降低精确度。我们可以通过添加左侧上下文来尝试提高精确度。

我问了 ChatGPT 这个问题。

```py
give me examples of words that serve as the last word in a sentence.
```

其回答的前几行如下。

```py
Run.
Stop.
Wait.
```

好吧，看来它没有完全理解我的问题，可能是因为表达得不够清楚。尽管如此，这些答案是有用的，即使非常有限。确实，在单词句子中，这些词是很好的左侧上下文。

这也帮助我完善了对 ChatGPT 的提问。首先，我将问题重新表述为

```py
give me examples of words that serve as the last word in a multi-word 
sentence.
```

那没有帮助。它的回答仍然由单词句子组成，尽管实际的单词是很好的新候选词，适合在左侧上下文中使用。

以下问题

```py
give me examples of left context to predict sentence boundaries.
```

生成了一个有趣且可能有用的回答，尽管不是我所寻找的。所以我不会详细阐述。

以下问题

```py
Which words tend to be the last word in a sentence.
```

的确导致了更有用的回答。

```py
Words that tend to be the last word in a sentence in written English 
include:

questions (e.g. What?, How?)
exclamations (e.g. Wow!, Oh!)
commands (e.g. Stop!, Go!)
short, one-word sentences that express a complete thought (e.g. Yes, No, Okay)
proper nouns (e.g. London, Bob)
some adverbs (e.g. Today, Yesterday, Finally)

Note: These are general tendencies, and the last word in a sentence can 
vary depending on the context and the type of sentence.
```

很好的响应！从中我们可以推测，添加左上下文可能有帮助。这个响应还提供了以下实际的见解，推动我们前进。

替换

```py
If the current character is boundary_marker and is preceded by 
left_context and is followed by right_context
```

通过较不严格的条件。例如*left_context*特征、*right_context*特征、*boundary_marker*。关键是 left_context 和 right_context 在条件中表示可能会导致组合爆炸。特征可以更柔和地结合以用于预测目的。

这可能很强大，特别是如果我们有办法利用监督学习，即反馈。支配特征集合如何用于预测句边界的参数就变得可学习，并可能减轻在遇到新场景时引入新特征的需要。

在这种设置中利用监督学习并不像人们想象的那么困难。我们不需要构建大量的句边界正例和句边界负例训练集。相反，我们可以按如下方式进行。

从初始条件开始，例如

```py
if the current character is a boundary_marker and it is followed by 
white space
```

具有高召回率。额外增加 left_context 和 right_context 特征。

运行最初配置的句边界预测器，在一个或多个足够大的文档上，并以易于可视化和标记的格式输出相邻预测句对。即使是 CSV 文件也足够了。检查 CSV 文件并将发现的任何假阳性标记出来。即使时间限定这个工作也可能会提升句边界预测器的性能。

当然，投入更多时间进行标记数据可能会进一步提高预测准确性。一个丰富的标记数据集可能会产生更好的结果。不过，这项投资不需要提前完成。可以逐步进行。

接下来，我们讨论要包含的具体合理特征。这些特征受到我们迄今为止的调查的启发。首先，我们的示例中添加一些我们从[5]找到的特征，因为这些特征将有助于特征工程。

```py
... Mr. Smith ...
... Johann S. Bach ...
```

> 1\. 当前句子中在 boundary_marker 之前的单词数。
> 
> 2\. 如果 boundary_marker 是句号，则当前标记中在其之前的额外句号数量。
> 
> 3\. 上一个词的词性。
> 
> 4\. 边界标记符前面单词的身份。
> 
> 5\. 边界标记符前面单词的长度。
> 
> 6\. 如果 boundary_marker 是句号，那么它是否嵌入在一个命名实体中。

特征 1) 与特征 4) 结合显然对单词句子有用。

特征 2) 受到示例的启发。

```py
The U.S. is a big country.
```

这可以帮助预测*U.S.*中的第二个句号不是句边界。

特征 3) 受到 ChatGPT 对专有名词的响应的启发。

特征 5) 与特征 2) 结合时可能对预测边界标记实际上是前一个单词的一部分有用，也就是说这是一个假阳性。

特征 6) 有助于这个示例。

```py
... Johann S. Bach ...
```

**监督统计算法**

现在我们将描述我们认为是一种有效的统计性质的监督算法。针对这个问题进行了调优。相对容易实现。

算法将假设我们正处于一个`boundary_marker`字符上。它将进行一个概率推断，我们将其称为等式 (1)。

*P*(该边界真实|`boundary_marker`，左上下文特征，右上下文特征)

这里的`boundary_marker`指的是标记的实际值：*句点*，*问号*，或*感叹号*。

尽管我们不会详细开发这个算法，但我们希望指出一些我们心中所想的特征：

1.  如果我们认为合适，可以预先指定某些特征的贡献，并且不允许其发生变化。例如，如果紧随`boundary_marker`之后的字符不是空格，而我们希望确保在这种情况下`boundary_marker`被预测为假阳性，我们可以强制使等式 (1) 始终评估为零。

1.  我们不一定要假设等式 (1) 中的特征在结果—边界是否真实—给定的情况下是条件独立的。例如，这些特征可能通过一个合适的概率图模型连接起来，该模型的结构可能由模型设计者为特定用例设定。或者，它可以是一个专家组合模型，其中“专家”是特定的特征值或特征组合，它们强烈预测边界标记是假阳性。有关专家组合模型，请参见 [4]。

1.  我们可以想象一个等式 (1) 的变体，它不是完全概率性的，同时保持相同的精神。具体来说，我们可以用两个评分函数替换等式 (1)：*score_true*(`boundary_marker`，*左上下文*特征，*右上下文*特征) 和 *score_false*(`boundary_marker`，*左上下文*特征，*右上下文*特征)。这样做，我们可能将模型中可学习的参数数量翻倍，从而使模型更为丰富。

**学习示例**

现在让我们在具体的设置中说明学习过程。我们将采用类似专家组合的形式，因为它在我们的设置中直观有效，并且学习过程也易于解释。

假设我们想要学习如果边界标记是句点而前一个标记有额外的句点，则我们应该预测这个边界标记不真实。出于防御目的，我们还将前一个标记的长度作为额外的预测因子。

我们希望第二个‘.’在*美国*中被标记为假阳性`boundary_marker`。

让我们设想将这个专家表述为

*P*(`boundary_marker` 真实| `boundary_marker` 是 ‘.’，*前一个标记的长度*，*前一个标记中的句点数量*)。

这模拟了一个特定的专家，当`boundary_marker`是句点时会参与其中。在这种情况下，它使用前一个标记的长度和其中句点的数量来决定是否预测`boundary_marker`为真实。

假设我们最初设定先验为

*P*(*boundary_marker* 为真实|*boundary_marker* 为‘.’, *l*, *np*) 为 1

针对各种合理的 *l* 和 *np* 组合。假设初始算法将句号（例如 *U.S.* 中的第二个）预测为句子边界。假设我们提供反馈，指出这些是误报。如果我们看到足够多的这种例子，我们应该能够学习到

*P*(*boundary_marker* 为真实|*boundary_marker* 为‘.’, 3, 1) 接近零

我们选择 *l*=3 和 *np*=1 作为示例，因为它适用于 *U.S.*

接下来，设想一个不同的专家，建模为

*P*(*boundary_marker* 为真实|紧随 *boundary_marker* 之后的字符不是空格)

同样，我们可以将先验设定为接近 1，这样最初，例如，*U.S.* 中的第一个句号就会被预测为真实的句子边界。用户随后提供反馈，将其标记为假阳性。这些反馈可以用于推动

*P*(*boundary_marker* 为真实|紧随 *boundary_marker* 之后的字符不是空格)

逼近零。

**总结**

在这篇文章中，我们讨论了将文本分割成句子的问题。我们以如下方式处理这个问题。我们通过从最基本的规则开始，迭代地构建句子边界预测器，并通过问答来完善它。我们询问某个规则可能导致的假阳性或假阴性。答案帮助我们完善规则。在此过程中，我们按照之前描述的方式请求 ChatGPT 的帮助。

在某个阶段，我们意识到我们应该从硬规则转向基于特征的方法。我们在一个特定的概率模型下开发了这种方法，引入了我们早期调查中建议的特定特征，将其框定在专家产品的设置中，并讨论了在这种设置中从反馈中学习。

**参考文献**

1.  使用 NLP 进行文本修正。检测和修正常见错误… | 作者 Arun Jagota | 2023 年 1 月 | Towards Data Science

1.  NLP 中的命名实体识别。真实世界的应用案例、模型、方法…

1.  ChatGPT

1.  [Geoff Hinton — 专家产品](https://www.cs.toronto.edu/~hinton/absps/icann-99.html)

1.  [nltk.tokenize.punkt](https://www.nltk.org/_modules/nltk/tokenize/punkt.html)
