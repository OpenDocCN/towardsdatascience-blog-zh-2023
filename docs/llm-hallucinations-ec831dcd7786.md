# 理解 LLM 幻觉

> 原文：[`towardsdatascience.com/llm-hallucinations-ec831dcd7786`](https://towardsdatascience.com/llm-hallucinations-ec831dcd7786)

## 观点

## LLMs 如何编造内容以及该如何应对

[](https://franklyai.medium.com/?source=post_page-----ec831dcd7786--------------------------------)![Frank Neugebauer](https://franklyai.medium.com/?source=post_page-----ec831dcd7786--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ec831dcd7786--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ec831dcd7786--------------------------------) [Frank Neugebauer](https://franklyai.medium.com/?source=post_page-----ec831dcd7786--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----ec831dcd7786--------------------------------) ·阅读时间 6 分钟·2023 年 5 月 8 日

--

![](img/95f952aadd9b08c63fbd537fcf27100e.png)

图片来源：[Ahmad Dirini](https://unsplash.com/@ahmadirini?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 主要目标

使用大型语言模型并非没有风险，其中包括基于所谓的 LLM“幻觉”的响应。幻觉可能对 LLMs 构成严重问题，因为它们可能导致虚假信息的传播、暴露机密信息，并对 LLMs 的能力产生不切实际的期望。理解幻觉并对它们生成的信息保持批判性，有助于解释和减轻这些幻觉可能造成的问题。

# 什么是 LLM 幻觉？

LLMs 是一种人工智能（AI），通过大量的文本和代码数据集进行训练。它们可以生成文本、翻译语言、创作不同类型的内容，并以信息性方式回答问题。然而，LLMs 也容易“幻觉”，这意味着它们可能生成事实不准确或毫无意义的文本。正如经常讨论的那样，“LLMs 可能充满了胡说八道。”这种幻觉发生是因为 LLMs 在经常不完整或矛盾的数据上进行训练。因此，它们可能会学会将某些词语或短语与某些概念联系起来，即使这些联系并不准确或无意中“过于准确”（我的意思是它们可能会捏造一些真实但不应被分享的内容）。这可能导致 LLMs 生成事实不准确、无意中过于纵容或纯粹毫无意义的文本。

# 幻觉的类型

## 谎言！谎言！谎言！

LLMs 有时会生成事实不准确的文本。以下是一个例子，其中有些内容是正确的，但突出显示的部分则完全不真实：

```py
Prompt: Write a brief history of the United States

Response: The 20th century was a time of great change for the United States. 
The country fought in two world wars, and it emerged as a global superpower 
after World War II. The Civil Rights Movement of the 1950s and 1960s led to 
the end of segregation and discrimination against African Americans.
```

说隔离和歧视已经不存在是不符合事实的。将其称为“谎言”对我来说也是技术上不准确的，因为模型不理解真理或谎言，只是如何组装单词。无论原因如何，LLMs 仍然可能生成事实不准确的内容。这并不少见，因此必须对所有内容进行事实检查。

## 胡说八道

从一个很高的层面来看，LLMs 使用概率来安排单词。尽管单词和其概率的范围可能会生成有意义的内容，但这并不总是如此；LLMs 也可以生成无意义的文本。例如，如果你让 LLM 写一首诗，它可能会生成语法正确但毫无意义的内容。

```py
Prompt: write a funny poem about swimming in green water

Partial (summarized) Response:
Swimming in Green Water

I went swimming in the green water,
The water was so green,
I couldn't see my feet,
I couldn't see my hand,
I couldn't see my head.

I looked around and saw a fish,
A big green fish,
With big green eyes,
And a big green smile,
And a big green fin.

I said, "Hello, fish,"
And the fish said, "Hello,"
And we swam around together,
In the green water,
For a long time.

Then we got tired,
And we lay down on the beach,
And we took a nap,
In the sun,
And the green water.
```

我能理解创作者可能会说，在诗中说鱼可以躺在沙子里是完全可以接受的（诗意许可及其所有），但我也可以争论模型是在编造一些胡说八道，这也是我在这种情况下的看法。思想从逻辑偏离到了非逻辑。然而，注意如果你与 LLMs 合作一段时间，你会看到更严重的例子。再次检查模型输出，并在必要时进行更正。在诗歌示例中，我可能会简单地将“…我们躺在海滩上…”改为“…我们躺在珊瑚礁旁…”或干脆删除那一行，因为人类实际上无法在水下小憩。

## 来源混淆

LLMs 有时会混淆不同来源的信息，这可能导致它们生成不准确或误导性的文本。例如，如果你让 LLM 写一篇关于当前事件的新闻文章，它可能会将来自不同新闻来源的信息结合在一起，即使这些来源相互矛盾。还要注意，将历史信息的推断与（例如）LangChain 等内容结合在一起，可能会真正混淆信息（和格式）。

这是一个示例，说明混淆如何导致事实不准确（或至少非常误导性）的信息。

```py
Prompt: write a one paragraph summary of the miami grand prix

Response:
The 2023 Miami Grand Prix was the fifth round of the 2023 Formula One 
World Championship. It was held on May 7, 2023 at the Miami International 
Autodrome in Miami Gardens, Florida, United States.

Sergio Pérez of Red Bull Racing started the race on pole position, but 
was overtaken by Charles Leclerc of Ferrari on the opening lap. Leclerc 
led the race for the first 20 laps, but was then passed by Max Verstappen 
of Red Bull Racing. Verstappen went on to win the race, with Leclerc 
finishing second and Pérez finishing third.

The Miami Grand Prix was a popular event, with over 270,000 spectators in 
attendance. The race was also well-received by the media, with many 
praising the new circuit and the atmosphere at the event.
```

回应在第一段准确地开头，描述了 2023 年 5 月 7 日举行的比赛。然后，模型似乎在之后混淆了 2022 年的结果。勒克莱尔在 2023 年排位第七，并没有在前 20 圈领跑，但他确实在 2022 年获得了第二名，并且可能在那场比赛的前 20 圈中领跑。（2023 年塞尔吉奥·佩雷斯获得第二名。）在这种情况下，混淆可能发生在用于结合当前事件和 LLM 文本的任何工具（例如 LangChain）中，但混淆引起的幻觉的想法仍然适用。

## 过度纵容

鉴于大型语言模型（LLMs）能够利用概率生成一些相当重要的文本，并且可能会混淆信息，因此统计上来说，LLM 有可能编造出“意外”泄露机密信息的内容。

为了保护机密信息，我不能提供关于我最近遇到的 LLM 出现这种情况的详细信息。然而，我曾询问过一个 LLM 一个我知道不应该披露的特定话题，模型给出了一个逻辑上正确但过于放纵的回答。虽然我遇到的情况不是国家安全问题，但在适当的情况下可能会非常严重。

# 如何管理幻觉？

这里有一些管理幻觉的技巧：

+   测试不同的设置，比如温度和 TopK（模型如何管理概率）。这是管理模型输出的最重要方法之一。

+   不要完全相信输出——进行事实核查（不用担心，你仍然节省了大量时间！）。

+   通常将 LLM 输出视为草稿机制——例如，LLM 创建了这篇文章的基本布局和一些内容。但我进行了大量编辑。

+   调整你使用的模型。根据你如何使用模型输出，你可能需要调整模型——这有许多方法，包括提示工程、参数高效调整（PET）和完全模型调整。这个简单列表中有相当多的细微差别和复杂性，但如果你知道如何做，这种调整可以减少幻觉。

+   接受模型出现幻觉的现实。与人类（在大多数情况下）不同，LLM 的幻觉通常是一个无意的结果，我认为通常正面的影响远远超过负面的影响。接受这一点并承认/沟通幻觉发生的可能性。

+   探索！虽然这篇文章提供了 LLM 幻觉的概述，但这对你和你的应用意味着什么可能会有显著差异。此外，你对这些词的理解可能并不完全符合现实。唯一真正理解和欣赏 LLM 幻觉如何影响你所做的事的方法是广泛探索 LLM。

# 更多内容待揭示

LLM 的广泛使用实际上还处于初期阶段，优缺点尚未准确列举。在我看来，开放的心态是理解 LLM 的所有维度，包括幻觉的最佳技巧。享受这个过程，尽可能多地探索，因为这种快速演变很少发生（根据我的经验），那些拥抱这一过程的人将从中获得最多。
