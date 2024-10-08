# LLM 的魔力——提示工程

> 原文：[`towardsdatascience.com/the-magic-of-llms-prompt-engineering-9c3e46130131`](https://towardsdatascience.com/the-magic-of-llms-prompt-engineering-9c3e46130131)

## 垃圾进垃圾出，从未如此真实。

[](https://franklyai.medium.com/?source=post_page-----9c3e46130131--------------------------------)![Frank Neugebauer](https://franklyai.medium.com/?source=post_page-----9c3e46130131--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9c3e46130131--------------------------------)![数据科学进展](https://towardsdatascience.com/?source=post_page-----9c3e46130131--------------------------------) [Frank Neugebauer](https://franklyai.medium.com/?source=post_page-----9c3e46130131--------------------------------)

·发布于 [数据科学进展](https://towardsdatascience.com/?source=post_page-----9c3e46130131--------------------------------) ·6 分钟阅读·2023 年 4 月 8 日

--

![](img/8c28b5ab704518e11aef83823349fb09.png)

图片由 [Gary Chan](https://unsplash.com/es/@gary_at_unsplash?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 机器的崛起

大型语言模型（LLMs）能够在*提出正确的问题*时提供前所未有的机器学习生成的信息。例如，LLMs 可以生成文章（不，LLM 并没有生成这段文字）、诗歌、歌词、与人讨论某个话题、解释某些类型的文档/表单等等。然而，LLMs 一个很少被讨论的问题是，知道**正确的问题**并不总是容易的，而没有这个关键部分，LLMs 可能会过于准确（即，信息过多）、不准确，或对许多商业应用过于多变。

提问的艺术，即**提示工程**，是我们克服这一限制（至少在短期内）的方式，也是 LLM 的*神奇背后的魔法*。

# 转变问题

在没有提供关于 LLM（由 LLM 撰写）的概述的情况下，我想直接进入 LLM 的一个重要特性——它们是非确定性的；你不一定每次都能得到相同的答案。例如，如果我连续多次向 Google Bard 询问一个简单的问题“1 + 1 是多少”，以下是未经编辑的结果：

```py
What's 1 + 1?
>> 1+1=2
What's 1 + 1?
>> 1 + 1 = 2.
What's 1 + 1?
>> 1 + 1 = 2.
What's 1 + 1?
>> I understand that you are asking me what is 1 + 1\. The answer is 2.
```

只有第 2 和第 3 个提示产生了相同的答案，但还要注意答案不仅仅是“2”，而是提示和答案的变化以及一些相当不可预测的答案格式。（注意：上面的输出并不是 Bard 的样子，仅仅是文本。）

> 从无结构到无结构并不能真正解决问题，它只是转移了问题。

在 1+1 的例子中，转变在于如何解析/转换答案；处理可变答案就像在旋转的房间里投掷飞镖——答案的变化太大，我们几乎回到了原来的问题。

相反，我们专注于提示工程，或询问问题以获得特定回应的艺术形式。这里是 Bard 对提示工程的定义：

> 提示工程是为大型语言模型设计和优化提示的过程，用于各种应用和研究主题。提示是用于引导语言模型输出的短文本片段。它们可以用来指定语言模型需要完成的任务，提供语言模型完成任务所需的额外信息，或仅仅使任务更易于语言模型理解。

谢谢你，Bard，这个描述相当不错，尽管在这个上下文中“短”是相对的。

# 问题工程 — 你控制这一点

在 1+1 的例子中，我控制的唯一是问题或提示。因此，重点是设计问题（或提示），通过添加具体性和上下文来进行。

看看当我为 1+1 的提示添加一些具体内容时会发生什么。

```py
What's 1+1 stated as an integer?

>> 1+1 stated as an integer is 2.
  An integer is a whole number that can be positive, negative, or zero. It is not a fraction or a decimal.
```

等等！那**糟糕得多**。我不想要叙述性答案，我只要一个数字！这就是为什么提示工程是一门艺术。让我再试一次…

```py
What's 1+1? Only return the numeric answer.
>> 2
What's 1+1? Only return the numeric answer.
>> 2
What's 1+1? Only return the numeric answer.
>> 2
```

看看这个！再来一个例子…

在这个例子中，我将做两件事——首先，展示如何将数据包含在提示中，其次，展示如何设计提示以获得具体的内容。

我首先使用 OCR 将以下表格提取为文本。

![](img/c8f6523f875aeadd5cb01d5eb256e459.png)

图片由作者提供

我的目标是向数据询问一些关于它自身的问题。这是一个相当有趣（技术上说，甚至是很棒的）用例，适用于许多（但不是全部）情况。

我想知道 2022 Q2 的文件大小（忽略表格中的高亮行，我想要下面的那一行）。答案是**48.79 MB**。

```py
Given the following text, what is the file size for Q2 2022? File
Format
Size
2023 Q1
ZIP
53.11 MB
2022 Q4
ZIP
45.91 MB
2022 Q3
ZIP
45.16 MB
2022 Q2
ZIP
48.79 MB
2022 Q1
ZIP
52.61 MB

>> The file size for Q2 2022 is 48.79 MB.
   The table you provided shows the file size for each quarter of 2022\. The file size for Q2 is 48.79 MB.
```

所以这里有趣的部分是它得出了正确的答案（48.79 MB）。不好的部分是，如同我们看到的 1+1，答案是无结构的。我需要通过具体性来修改提示。

```py
Given the following text, what is the Q2 file size? Only return the answer.
>> Sure, here is the file size for Q2: 48.79 MB

Given the following text, what is the Q2 file size in MB, don't include any text in the reply?
>> 48.79

Given the following text, what is the Q2 file size in MB, don't include any text in the reply?
>> 48.79
```

这更好（即使不是完美的），确实使其作为一种零样本方式来读取某些类型的数据（甚至可能是表单数据）非常有用。在现实世界中，我会继续迭代提示，直到得到一个完全一致且确定的答案。

# 当心炒作的时机

阅读一个简单的表格并不意味着大型语言模型（LLMs）能够读取所有表格或表单，它们真的不能。一个好的例子是保险行业，其中非常复杂（甚至不合逻辑）的表单是信息交换的方式。LLMs 可以理解某些表单，但不能理解复杂的表单，因为：

1.  LLMs 不理解保险（它们可以，但它们不理解）。一个背书可以包含一个背书或一个表单，而背书的第二种用法是名词，而“表单”的使用与实际表单无关，这真的是超出了普通 LLM 的细微差别。

1.  复杂的表单包含了上下文，这对语言模型来说非常难以理解（也许是不可能的？），因为表单被用作布局结构，而不是内容结构。（表格有时也会出现类似的问题，原因相同，有时表格就是用于布局结构的机制。）

为了避免陷入 LLM 的炒作旋涡，重要的是要善于使用这些模型，了解它们能做什么，不能做什么，以及你如何能影响它们的行为。同时也要理解，模型本身在不断发展，因此不要因为它现在不起作用就否定它。

# 真实（现实）

宣传周期可能会导致不切实际的期望。我们在历史上见过这样的重复——例如，面向服务的架构（SOA）从未导致动态应用程序，但我们确实看到了一种相当大规模的 API 扩展。LLMs 也可能是这样，因为我们似乎期望 LLMs 解决所有世界上的问题（我的预测是：它不会）。如果你对这些模型能做什么保持现实和好奇，期望和现实可以对齐，而我相信这种对齐就是历史被创造的地方。

# 走出去，改变世界

大型语言模型（LLMs）强大且适用于多种任务，但它们也有局限性，其中一些可以通过提示工程克服。那种工程是一种艺术形式，需要大量实验，这真的是无法通过书本获得的——去做吧。擅长提示工程，你将能够最大化 LLM 的价值，甚至可能让自己远离炒作旋涡。

本文中表达的观点是我个人的，并不代表我的雇主。
