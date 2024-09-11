# 算术推理问题的提示工程

> 原文：[`towardsdatascience.com/prompt-engineering-for-arithmetic-reasoning-problems-28c8bcd5bf0e?source=collection_archive---------1-----------------------#2023-11-18`](https://towardsdatascience.com/prompt-engineering-for-arithmetic-reasoning-problems-28c8bcd5bf0e?source=collection_archive---------1-----------------------#2023-11-18)

## 探索针对算术推理问题的各种提示工程技术、最佳实践以及通过 Vellum.ai 进行生产级提示的快速实验。

[](https://medium.com/@kaustubhbhavsar?source=post_page-----28c8bcd5bf0e--------------------------------)![Kaustubh Bhavsar](https://medium.com/@kaustubhbhavsar?source=post_page-----28c8bcd5bf0e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----28c8bcd5bf0e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----28c8bcd5bf0e--------------------------------) [Kaustubh Bhavsar](https://medium.com/@kaustubhbhavsar?source=post_page-----28c8bcd5bf0e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3c727e10b97f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprompt-engineering-for-arithmetic-reasoning-problems-28c8bcd5bf0e&user=Kaustubh+Bhavsar&userId=3c727e10b97f&source=post_page-3c727e10b97f----28c8bcd5bf0e---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----28c8bcd5bf0e--------------------------------) · 14 分钟阅读 · 2023 年 11 月 18 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F28c8bcd5bf0e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprompt-engineering-for-arithmetic-reasoning-problems-28c8bcd5bf0e&user=Kaustubh+Bhavsar&userId=3c727e10b97f&source=-----28c8bcd5bf0e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F28c8bcd5bf0e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprompt-engineering-for-arithmetic-reasoning-problems-28c8bcd5bf0e&source=-----28c8bcd5bf0e---------------------bookmark_footer-----------)![](img/ad64d3dd5f3b645f4ecda8a7905df74b.png)

四种不同提示技术的架构：输入-输出、链式思维（CoT）、链式思维下的自我一致性（CoT）和思维树（ToT）（图源：[Yao et al. (2023)](https://arxiv.org/abs/2305.10601)）

# **简介**

大型语言模型（LLMs）由于其理解和生成语言的能力，越来越受到学术研究人员和行业专家的关注。这些文本理解能力的原因在于它们的训练过程，涉及大量数据，以预测后续单词为主要目标。为了使这些模型在特定任务上表现更佳，精细调整是必需的。这可以通过两种方法实现：‘预训练和精细调整’或‘提示精细调整’。

在传统的‘预训练和精细调整’方法中，LLM 在与其后续任务相关的数据集上进行精细调整，从而在精细调整阶段更新参数。相反，‘提示精细调整’则通过文本片段引导模型执行任务。

提示是用户提供的输入，模型旨在对其作出响应。提示可以包含指令、上下文、问题或输出指示符。提示工程是一个新兴领域，致力于开发和完善提示，以有效利用语言模型。

然而，一个重大挑战在于确保模型能够处理需要算术和常识推理的问题。在本文中，我们专注于算术问题的提示工程。

# **先决条件**

不需要任何先前知识。下面提供的所有示例可以在 [OpenAI Playground](https://platform.openai.com/playground) 上执行，或通过 [OpenAI API](https://openai.com/blog/openai-api) 运行。虽然本文主要使用 OpenAI 模型，但需要注意的是，这些仅仅是我们将要探讨的提示技术，你可以自由使用市场上任何可用的 LLM。

# **算术问题的提示工程**

下面提到的所有问题均取自 [GSM8K](https://huggingface.co/datasets/gsm8k) 数据集，并使用 OpenAI 的 **GPT-3.5 Turbo Instruct** 模型，配置为默认设置进行测试。

我们将对以下算术问题测试各种技术：

```py
Jennifer purchased 40 cans of milk at the store before meeting her 
classmate Mark, who was also buying milk. Jennifer bought 6 additional 
cans for every 5 cans Mark bought. If Mark purchased 50 cans, how many 
cans of milk did Jennifer bring home from the store?
```

上述问题的正确答案是 **100 升**。

# **零-shot 提示**

**shot** 本质上指的是 **一个示例**。零-shot 是一种基本的提示技术，在这种技术中，问题被直接提问给模型，而不提供任何示例。一般来说，这种技术在经过大量数据训练的大型模型中会产生良好的效果。

## ***提示：***

```py
Q: {{ question }}
A:
```

## ***输出：***

```py
 Jennifer brought home 76 cans of milk from the store.
```

在前面的提示中，我们没有在问题前加上任何示例，导致模型未能提供正确的算术问题答案。推理和常识问题在零-shot 提示下很少产生令人满意的结果。因此，处理此类问题时必须包含示例。

# Few-Shot 提示

通过少量示例提示，我们向模型提供少量演示，也称为示例。这些示例用于指导模型，以便后续问题可以类似处理。所需的演示数量根据问题的复杂性和所使用的特定模型而异。

## 提示：

```py
Q: Ken created a care package to send to his brother, who was away at boarding school. Ken placed a box on a scale, and then he poured into the box enough jelly beans to bring the weight to 2 pounds. Then, he added enough brownies to cause the weight to triple. Next, he added another 2 pounds of jelly beans. And finally, he added enough gummy worms to double the weight once again. What was the final weight of the box of goodies, in pounds?
A: 16

Q: Mark has a garden with flowers. He planted plants of three different colors in it. Ten of them are yellow, and there are 80% more of those in purple. There are only 25% as many green flowers as there are yellow and purple flowers. How many flowers does Mark have in his garden?
A: 35 

Q: James writes a 3-page letter to 2 different friends twice a week. How many pages does he write a year? 
A: 624 

Q: Albert is wondering how much pizza he can eat in one day. He buys 2 large pizzas and 2 small pizzas. A large pizza has 16 slices and a small pizza has 8 slices. If he eats it all, how many pieces does he eat that day?
A: 48

Q: Betty is saving money for a new wallet which costs $100\. Betty has only half of the money she needs. Her parents decided to give her $15 for that purpose, and her grandparents twice as much as her parents. How much more money does Betty need to buy the wallet?
A: 5

Q: {{ question }}
A:
```

## 输出：

```py
76
```

尽管这种技术在解决困难问题时效果良好，但模型在算术推理问题上仍可能无法找到令人满意的结果。这是因为模型未能推理出即时步骤。

# **少量示例链式思维（CoT）提示**

[少量示例链式思维（CoT）](https://arxiv.org/abs/2201.11903)提示为模型提供了少量示例，以指导其完成解决方案的过程。这种指导，也称为中间推理步骤，对于帮助模型逐步工作并逻辑地产生所需输出至关重要。

## ***提示：***

```py
Q: James writes a 3-page letter to 2 different friends twice a week. How many pages does he write a year? 
A: He writes each friend 3*2=6 pages a week So he writes 6*2=12 pages every week That means he writes 12*52=624 pages a year. The answer is 624.

Q: {{ question }}
A:
```

## ***输出：***

```py
 Jennifer bought 40+6/5*50=40+12*10=40+120=160 cans of milk. The answer is 160.
```

在上述输出中，模型尝试推理步骤但得出了不正确的最终答案。我们观察到这种差异可能是由于提供了无关示例。解决此问题有两种方法：要么为模型提供更多示例，要么确保提供的示例是相关的。下面，我们尝试用相关示例来纠正这一点。

## ***提示：***

```py
Q: Mark has a garden with flowers. He planted plants of three different colors in it. Ten of them are yellow, and there are 80% more of those in purple. There are only 25% as many green flowers as there are yellow and purple flowers. How many flowers does Mark have in his garden?
A: There are 80/100*10=8 more purple flowers than yellow flowers. So in Mark's garden, there are 10+8=18 purple flowers. Purple and yellow flowers sum up to 10+18=28 flowers. That means in Mark's garden there are 25/100*28=7 green flowers. So in total Mark has 28+7=35 plants in his garden. The answer is 35.

Q: {{ question }}
A:
```

## ***输出：***

```py
 For every 5 cans Mark bought, Jennifer bought 6 additional cans. So for 50 cans, Jennifer bought 6/5*50=60 additional cans. In total, Jennifer bought 40+60=100 cans of milk. Therefore, Jennifer brought home 100 cans of milk from the store.
```

最终得到的答案现在是正确的。考虑任务、模型的选择以及示例的相关性，这些都对得出正确解决方案有贡献。

# 程序辅助语言模型（PAL）

[程序辅助语言模型（PAL）](https://arxiv.org/abs/2211.10435)不是一种提示技术；相反，它是提示工程的应用。与 CoT 类似，PAL 旨在推理中间步骤。然而，PAL 使用程序运行时的中间编程步骤，如 Python 解释器，而不是使用常规文本作为推理步骤。

## ***提示：***

```py
Q: James writes a 3-page letter to 2 different friends twice a week. How many pages does he write a year? 
A: #James has
number_of_friends=2

#He writes each friend 3-page letter each time.
page_letter_size=3

#He writes twice to each friend in a week.
number_of_times_written_to_friend=2

#Total pages written in a week to each friend
pages_written_to_each_friend_per_week=page_letter_size*number_of_times_written_to_friend=6

#So, total pages written by him per week,
total_pages_each_week=pages_written_to_each_friend_per_week*number_of_friends=12

#Number of weeks in a year
total_weeks_in_a_year=52

#That means he writes,
total_pages_written_in_year=total_pages_each_week*total_weeks_in_a_year=624
#pages per year.

The answer is 624.

Q: {{ question }}
A:
```

## ***输出：***

```py
#Jennifer initially bought 40 cans of milk.
initial_cans_bought=40

#For every 5 cans Mark bought, Jennifer bought 6 additional cans.
additional_cans_bought=6
mark_cans_bought=50

#So, for every 5 cans Mark bought, Jennifer bought 6 additional cans.
additional_cans_bought=6
mark_cans_bought=50

#Number of times Mark bought 5 cans
number_of_times_mark_bought=mark_cans_bought/5=10

#Total additional cans bought by Jennifer
total_additional_cans_bought=additional_cans_bought*number_of_times_mark_bought=60

#Total cans of milk brought home by Jennifer
total_cans_brought_home=initial_cans_bought+total_additional_cans_bought=40+60=100

The answer is 100 cans of milk.
```

即使使用少量示例 CoT 技术的无关示例未能得出正确答案，PAL 仍成功找到了正确的最终解决方案。

# 零-Shot 链式思维（CoT）提示

在没有少量示例链式思维（CoT）技术演示的情况下，一个直接且有效的方法是让模型自行推理中间步骤。这被称为零-shot 链式思维（CoT）。你可以通过在提示问题中添加类似于“我们一步一步来想…”的文本来实现这一点。这种简单的技术已经显示出卓越的效果。

## ***提示：***

```py
Q: {{ question }}
A: Let's think step by step...
```

## ***输出：***

```py
 1\. Jennifer initially bought 40 cans of milk.
2\. For every 5 cans Mark bought, Jennifer bought 6 additional cans.
3\. Since Mark bought 50 cans, Jennifer bought 6 additional cans for every 5 cans, which means she bought (6/5) x 50 = 60 cans.
4\. Therefore, Jennifer brought home a total of 40 + 60 = 100 cans of milk from the store.
```

从上述示例中，我们观察到模型成功地推理了步骤，并得出了正确的解决方案，而无需任何示例或指导。

# 思维树（ToT）提示

[思维树 (ToT)](https://arxiv.org/abs/2305.08291) 框架涉及以类似于导航思维树结构的方式探索潜在解决方案，这类似于人类的问题解决过程。这种方法使得在需要时能够重新追溯步骤，反映了人类在问题解决过程中可能重新评估和调整思维的方式。本质上，ToT 旨在通过试错法复制人类推理的适应性和迭代性。

基于 ToT 框架，[Dave Hulbert](https://github.com/dave1010/tree-of-thought-prompting) 提出了思维树提示法。这种技术采用了简单的单一提示方法，要求模型评估中间步骤。

## 提示：

```py
Identify and behave as three different experts that are appropriate to answering this question.
All experts will write down the step and their thinking about the step, then share it with the group.
Then, all experts will go on to the next step, etc.
At each step all experts will score their peers response between 1 and 5, 1 meaning it is highly unlikely, and 5 meaning it is highly likely.
If any expert is judged to be wrong at any point then they leave.
After all experts have provided their analysis, you then analyze all 3 analyses and provide either the consensus solution or your best guess solution.

Following is the question:
Q: {{ question }}
A:
```

## 输出：

```py
 Expert 1: Math Expert
Step 1: Calculate the total number of cans of milk purchased by Mark.
Mark bought 50 cans of milk.
Score: 5 (highly likely)

Step 2: Calculate the number of cans of milk purchased by Jennifer before meeting Mark.
Jennifer purchased 40 cans of milk.
Score: 5 (highly likely)

Step 3: Calculate the number of additional cans of milk purchased by Jennifer for every 5 cans Mark bought.
For every 5 cans Mark bought, Jennifer bought 6 additional cans.
Score: 5 (highly likely)

Step 4: Calculate the total number of cans of milk purchased by Jennifer.
Jennifer bought 40 + (6/5)*50 = 40 + 60 = 100 cans of milk.
Score: 5 (highly likely)

Step 5: Conclusion
Jennifer brought home 100 cans of milk from the store.
Score: 5 (highly likely)

Expert 2: Retail Expert
Step 1: Determine the average number of cans of milk purchased by customers at the store.
Based on previous sales data, the average number of cans of milk purchased by customers is 10.
Score: 4 (likely)

Step 2: Calculate the total number of cans of milk purchased by Mark.
Mark bought 50 cans of milk.
Score: 5 (highly likely)

Step 3: Calculate the number of cans of milk purchased by Jennifer before meeting Mark.
Jennifer purchased 40 cans of milk.
Score: 5 (highly likely)

Step 4: Calculate the total number of cans of milk purchased by Jennifer.
Jennifer bought 40 + (6/5)*50 = 40 + 60 = 100 cans of milk.
Score: 5 (highly likely)

Step 5: Conclusion
Jennifer brought home 100 cans of milk from the store.
Score: 5 (highly likely)

Expert 3: Psychology Expert
Step 1: Analyze Jennifer's behavior and decision-making process.
Jennifer may have purchased 40 cans of milk based on her own needs and preferences.
Score: 4 (likely)

Step 2: Analyze Mark's behavior and decision-making process.
Mark may have purchased 50 cans of milk based on his own needs and preferences.
Score: 4 (likely)

Step 3: Consider the influence of social norms and peer pressure.
Jennifer may have felt pressured to buy more cans of milk after seeing Mark's purchase.
Score: 3 (somewhat likely)

Step 4: Analyze the impact of Jennifer's decision to buy additional cans for every 5 cans Mark bought.
Jennifer may have made this decision based on a desire to keep up with Mark or to appear generous.
Score: 3 (somewhat likely)

Step 5: Conclusion
Jennifer brought home 100 cans of milk from the store, possibly influenced by social norms and peer pressure.
Score: 3 (somewhat likely)

Consensus Solution:
Based on the analysis of all three experts, it is highly likely that Jennifer brought home 100 cans of milk from the store. The math expert's analysis provides a solid calculation, while the retail expert's analysis takes into account the average number of cans purchased by customers. The psychology expert's analysis adds a different perspective by considering the influence of social norms and peer pressure. Overall, the consensus solution is that Jennifer brought home 100 cans of milk from the store.
```

上述示例中使用的 ToT 技术能够产生准确的解决方案。值得一提的是，所使用的 ToT 提示来源于 [Dave Hulbert 的 ToT 提示](https://github.com/dave1010/tree-of-thought-prompting/blob/main/tree-of-thought-prompts.txt)。不过，这种技术的一个缺点是提示的尺寸显著增加。

除了上述工程技术之外，还可以采用一些额外的方法来解决算术推理问题。然而，值得注意的是，这些技术 — [**自动链式思维 (Auto-CoT)**](https://arxiv.org/abs/2210.03493)、[**自我一致性**](https://arxiv.org/abs/2203.11171)、[**自动推理和工具使用 (ART)**](https://arxiv.org/abs/2303.09014) 和 [**主动提示**](https://arxiv.org/pdf/2302.12246.pdf) — 超出了本文的范围。

# 最佳实践

模型在使用设计良好的提示进行微调时表现效果良好。[Elvis Saravia](https://www.promptingguide.ai/introduction/elements) 指出，一个提示可能由四个元素组成，尽管每次并不都需要所有元素：

1.  **指令：** 这指定了你希望模型执行的特定任务或行动。

1.  **背景：** 外部信息或额外细节，用于指导模型生成更准确的响应。

1.  **输入数据：** 我们寻求响应的特定输入或问题。

1.  **输出指标：** 期望的输出类型或格式。

建议将指令放在开头，与其他提示内容用指定的分隔符（如‘#’）分开。

[Santu 和 Feng (2023)](https://arxiv.org/pdf/2305.11430.pdf) 介绍了一种全面的分类法 TELeR（Turn、Expression、Level of Details、Role），旨在使多个 LLM 在各种任务中的表现之间进行更有意义的比较。该提示分类法将提示指令根据提供的细节水平分为六个级别。第六级可以有效地用于优化模型在给定提示下的表现。

1.  提供高层目标的简要描述。

1.  提供详细的子任务的项目列表。

1.  指定评估语言模型（LLM）输出的标准或包含几个示例。

1.  包括通过检索技术获得的额外相关信息。

1.  请求 LLM 提供明确的声明，解释其输出结果。

> 通常建议具体明确，提供清晰的细节，通过迭代不断完善提示，适当格式化提示并进行清晰分隔，并提及应做和不应做的事项。

让我们重新审视这个提示，考虑上述提到的做法，并应用零-shot 提示技术。

**提示：**

```py
### INSTRUCTION ###
You are required to answer Arithmetic Reasoning questions. Please follow the steps below as you go about solving the problem. 

1\. All problems should be solved in as many steps as required and each step should be numbered.
2\. All calculations should be clear and simple to understand.
3\. Provide an accurate final answer at the end of your reasoning in the following format: 'Answer is '.

You'll be evaluated on:
- how easier are the calculations done to understand?
- how accurate is the answer?

### INPUT DATA ###
Q: {{ question }}
A:
```

**输出：**

```py
1\. Jennifer bought 40 cans of milk at the store.
2\. Mark bought 50 cans of milk.
3\. Jennifer bought 6 additional cans for every 5 cans Mark bought.
4\. For every 5 cans Mark bought, Jennifer bought 6 additional cans.
5\. Therefore, for 50 cans Mark bought, Jennifer bought (6/5)*50 = 60 additional cans.
6\. Total cans of milk Jennifer bought = 40 + 60 = 100 cans.
Answer is 100 cans.
```

最初，模型未能使用零-shot 提示技术提供正确的解决方案。然而，通过应用适当的指令和格式化，相同的模型成功地使用零-shot 提示技术生成了正确的响应。

**免责声明：** 以下部分提供关于 Vellum 的信息，这是一个对快速生产化提示非常有用的开发平台。提及 Vellum 仅供参考，并不构成推荐或推广。我与 Vellum 没有任何关联，也没有因其包含而获得任何报酬。

# Vellum

确定合适的提示可能是一项挑战。该过程从基本提示开始，然后通过多次迭代逐步完善——这是一项难以监督的任务。电子表格可能适用于少量实验，但面对需要大量迭代的复杂问题时，它们就显得不足。因此，问题是——我们如何有效地监控我们的提示？如果我们的目标是与团队无缝分享我们的提示实验呢？引入 Vellum —— 这些问题的答案。

[Vellum](https://www.vellum.ai/) 提供了一套工具，旨在支持提示工程、语义搜索、版本控制、定量测试和性能监控，以帮助开发生产级 LLM 应用。在提示工程的背景下，它支持在市场上所有主要 LLM 中测试和评估给定的提示。它还促进了对提示的协作。

![](img/e6bcc602b5a9c2e18a7ecc48a18f35d1.png)

Vellum 沙箱的截图（作者截图）

上面的图像是 [Vellum 沙箱](https://app.vellum.ai/prompt-sandboxes) 的代表性截图。Vellum 允许在文本和聊天模型之间轻松切换，并可以轻松调整模型参数。它还提供延迟跟踪和四种评估指标的定量评估的优势：精确匹配、正则匹配、语义相似性和 Webhook。

[](https://www.vellum.ai/?source=post_page-----28c8bcd5bf0e--------------------------------) [## Vellum - 适用于 LLM 应用的开发平台

### Vellum 是一个用于构建 LLM 应用的开发平台，提供提示工程、语义搜索、版本…

[www.vellum.ai](https://www.vellum.ai/?source=post_page-----28c8bcd5bf0e--------------------------------)

# 摘要

本文首先介绍了提示，然后深入探讨了算术推理问题的提示工程。我们探讨了各种提示工程技术，包括零样本、少样本、少样本链式思维、零样本链式思维、程序辅助语言模型和思维树。

随后，我们学习了一些编写更好提示的最佳实践。一般来说，建议具体明确，提供清晰的细节，通过迭代不断完善提示，适当地格式化提示以保持清晰的分隔，并提及应做的事项和不应做的事项。

最后，为了应对跟踪和评估提示以及在团队成员之间共享提示的挑战，我们研究了 Vellum 工具。

如果你喜欢这篇文章，确保在[这里](https://medium.com/@kaustubhbhavsar)关注我。你也可以通过[LinkedIn](http://www.linkedin.com/in/kaustubhbhavsar)和[X](https://twitter.com/bhavsarkaustubh)（正式名称为 Twitter）与我联系。
