# 提示工程：如何让 AI 解决你的问题

> 原文：[`towardsdatascience.com/prompt-engineering-how-to-trick-ai-into-solving-your-problems-7ce1ed3b553f`](https://towardsdatascience.com/prompt-engineering-how-to-trick-ai-into-solving-your-problems-7ce1ed3b553f)

## 7 个提示技巧、LangChain 和 Python 示例代码

[](https://shawhin.medium.com/?source=post_page-----7ce1ed3b553f--------------------------------)![Shaw Talebi](https://shawhin.medium.com/?source=post_page-----7ce1ed3b553f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7ce1ed3b553f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7ce1ed3b553f--------------------------------) [Shaw Talebi](https://shawhin.medium.com/?source=post_page-----7ce1ed3b553f--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7ce1ed3b553f--------------------------------) ·14 分钟阅读·2023 年 8 月 25 日

--

这是大型语言模型实践系列中的第四篇文章。在这里，我将讨论提示工程（PE）以及如何使用它来构建支持 LLM 的应用程序。我首先回顾关键的 PE 技术，然后通过 Python 示例代码演示如何使用 LangChain 构建基于 LLM 的应用程序。

![](img/51cee05215f7e6b279687f028ed20dcc.png)

图片由 [Jason Leung](https://unsplash.com/@ninjason?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

当首次听说提示工程时，许多技术人员（包括我自己）往往对这个想法嗤之以鼻。我们可能会想，“*提示工程？噗，这太无聊了。告诉我怎么从头开始构建一个 LLM 吧。*”

然而，深入探讨之后，我要提醒开发者不要自动忽视提示工程。我甚至可以说，**提示工程可以实现大多数 LLM 使用案例的 80%价值**，且（相对）花费的精力非常少。

我写这篇文章的目标是通过对提示工程的实际回顾和示例来传达这一观点。虽然提示工程的功能确实有一些不足，但它为发现简单而聪明的解决方案打开了大门。

附加视频。

# **什么是提示工程？**

在本系列的第一篇文章中，我将**提示工程**定义为**任何开箱即用的 LLM 的使用**（即不训练任何内部模型参数）。然而，还有更多可以说的。

1.  提示工程是“*通过提示编程 LLM 的方法。*” [[1](https://arxiv.org/abs/2302.11382)]

1.  提示工程是“*一种将提示进行构建和格式化的经验性艺术，以最大化模型在特定任务上的表现*。” [[2](https://arxiv.org/abs/2106.09685)]

1.  *“语言模型…希望完成文档，因此你可以通过安排虚假的文档来欺骗它们执行任务*。” [[3](https://www.youtube.com/watch?v=bZQun8Y4L2A)]

第一个定义传达了来自 LLM 的关键创新，即**计算机现在可以使用简单的英语进行编程**。第二点将提示工程框架化为一种主要经验性的工作，其中从业者、修补者和构建者是这一新编程方式的主要探索者。

第三点（来自 [Andrej Karpathy](https://medium.com/u/ac9d9a35533e?source=post_page-----7ce1ed3b553f--------------------------------)）提醒我们**LLM 并未明确训练来做几乎我们要求的任何事情**。因此，从某种意义上说，我们是在“欺骗”这些语言模型以解决问题。我觉得这捕捉到了提示工程的本质，它依赖于你的创造力而非技术技能。

# **提示工程的两个层次**

可以通过两种不同的方式进行提示工程，我在本系列的 第一篇文章 中将其称为“**简单方法**”和“**较难的方法**”。

## 简单的方法

这是大多数世界上人们进行提示工程的方式，即通过 ChatGPT（或类似的工具）。这是一种直观的、无需编码且无需费用的与 LLM 互动的方式。

虽然这种方法适合快速和简单的任务，例如总结一页文本、重写一封邮件、帮助你头脑风暴生日派对计划等，但它也有其缺点。一个主要问题是**将这种方法整合到更大的自动化流程或软件系统中并不容易**。为了做到这一点，我们需要更进一步。

## 较难的方法

这通过以编程方式与 LLM 互动来解决了“简单方法”的许多缺点，即使用 Python。我们在本系列的前两篇文章中了解了如何做到这一点，其中探索了 OpenAI 的 Python API 和 Hugging Face Transformers 库。

虽然这需要更多的技术知识，**但这正是提示工程的真正力量所在**，因为它允许开发人员将基于 LLM 的模块集成到更大的软件系统中。

一个好的（也许是具有讽刺意味的）例子是 ChatGPT。这个产品的核心是提示一个预训练的模型（即 GPT-3.5-turbo）充当聊天机器人，然后将其封装在一个易于使用的网页界面中。

当然，开发 GPT-3.5-turbo 是困难的，**但这不是我们需要担心的事情**。借助我们手头的所有预训练 LLM，几乎任何具备基本编程技能的人都可以创建一个像 ChatGPT 这样的强大 AI 应用程序，而不必是 AI 研究员或机器学习博士。

# **利用提示工程构建 AI 应用**

更困难的方法解锁了**编程和软件开发的新范式**。开发者不再需要在软件系统中定义每一寸逻辑。他们现在可以选择将非琐碎的部分转移给 LLM。让我们来看一个具体的例子，这可能是什么样的。

假设你想为高中历史课创建一个**自动评分系统**。问题在于所有问题都有书面回答，因此通常会有多个正确答案的版本。例如，以下对“*谁是美国的第 35 任总统？*”的回答可能都是正确的。

+   约翰·F·肯尼迪

+   JFK

+   杰克·肯尼迪（一个常见的昵称）

+   约翰·F·肯尼迪（可能试图获得额外的学分）

+   约翰·F·肯尼迪（拼写错误的姓氏）

在**传统编程范式**中，开发者需要找出如何处理所有这些变体。为此，他们可能会列出所有可能的正确答案，并使用精确的字符串匹配算法，甚至可能使用模糊匹配来帮助处理拼写错误的单词。

然而，借助这种新的**LLM 启用的范式**，**问题可以通过简单的提示工程来解决**。例如，我们可以使用以下提示来评估学生的答案。

```py
You are a high school history teacher grading homework assignments. \
Based on the homework question indicated by “Q:” and the correct answer \
indicated by “A:”, your task is to determine whether the student's answer is \
correct.
Grading is binary; therefore, student answers can be correct or wrong.
Simple misspellings are okay.

Q: {question}
A: {correct_answer}

Student Answer: {student_answer}
```

我们可以将这个提示视为一个函数，给定一个***问题***、***正确答案***和***学生答案***，它生成学生的评分。然后，这可以集成到一个更大的实现自动评分系统的软件中。

从节省时间的角度来看，这个提示我花了大约 2 分钟来编写，而如果我尝试开发一个算法来做同样的事情，它可能需要几个小时（甚至几天），而且性能可能更差。**因此，这类任务的时间节省是 100–1000 倍**。

当然，有许多任务中 LLM 并没有提供实质性的好处，其他现有方法更适合（例如预测明天的天气）。LLM 绝不是解决所有问题的方案，但它们确实创造了一套新的解决方案，用于处理需要有效处理自然语言的任务——这是计算机历史上一直困难的任务。

# **提示工程的 7 个技巧**

虽然之前的提示示例看起来像是一种自然且明显的自动评分任务框架，但它刻意使用了特定的提示工程启发式方法（或者说“技巧”，如我所称）。这些（以及其他）技巧已成为提高 LLM 响应质量的可靠方法。

尽管有许多撰写良好提示的技巧和窍门，但在这里我将讨论那些基于少数参考资料（IMO）看起来最基本的技巧。对于更深入的了解，我建议读者探索此处引用的来源。

## **技巧 1：描述性强（多多益善）**

LLMs 的一个决定性特征是它们在大量文本语料库上进行训练。这使它们具备了广泛的世界知识和执行各种任务的能力。然而，这种令人印象深刻的普遍性可能会在没有提供适当上下文的情况下，影响特定任务的表现。

例如，让我们比较两个生成我爸爸生日祝福的提示。

***不使用技巧***

```py
Write me a birthday message for my dad.
```

***使用技巧***

```py
Write me a birthday message for my dad no longer than 200 \
characters. This is a big birthday because he is turning 50\. To celebrate, \
I booked us a boys' trip to Cancun. Be sure to include some cheeky humor, he \
loves that.
```

## **技巧 2：提供示例**

下一个技巧是给 LLM 示例响应，以提高其在特定任务上的表现。这个技术术语是**少量学习**，已被证明能显著提高 LLM 的表现 [6]。

让我们看一个具体的例子。假设我们想为 Towards Data Science 文章写一个副标题。我们可以使用现有的示例来指导 LLM 完成。

***不使用技巧***

```py
Given the title of a Towards Data Science blog article, write a subtitle for it.

Title: Prompt Engineering—How to trick AI into solving your problems
Subtitle:
```

***使用技巧***

```py
Given the title of a Towards Data Science blog article, write a subtitle for it.

Title: A Practical Introduction to LLMs
Subtitle: 3 levels of using LLMs in practice

Title: Cracking Open the OpenAI (Python) API
Subtitle: A complete beginner-friendly introduction with example code

Title: Prompt Engineering-How to trick AI into solving your problems
Subtitle:
```

## **技巧 3：使用结构化文本**

确保提示遵循有组织的结构，不仅使其更易读和编写，还往往有助于模型生成良好的完成。我们在**技巧 2**的示例中应用了这一技术，其中我们明确标记了每个示例的*标题*和*副标题*。

然而，我们可以以无数种方式为提示提供结构。这里有一些例子：使用全大写来强调，使用分隔符如 ```py to highlight a body of text, use markup languages like Markdown or HTML to format text, use JSON to organize information, etc.

Now, let’s see this in action.

***Without Trick***

```

给我一个巧克力曲奇饼干的食谱。

```py

***With Trick***

```

创建一个组织良好的巧克力曲奇饼干食谱。使用以下\

格式元素：

**标题**：经典巧克力曲奇饼干

**材料**：列出配料及其准确的测量和格式。

**步骤**：以编号格式提供逐步说明，详细说明烘焙过程。

**提示**：包括一个单独的部分，提供有用的烘焙提示和可能的变化。

```py

## **Trick 4: Chain of Thought**

This trick was proposed by Wei et al. [7]. The basic idea is to guide an LLM to think “step by step”. This helps break down complex problems into manageable sub-problems, which gives the LLM “time to think” [3,5]. Zhang et al. showed that this could be as simple as including the text “*Let’s think step by step*” in the prompt [8].

This notion can be extended to any recipe-like process. For example, if I want to create a LinkedIn post based on my latest Medium blog, I can guide the LLM to mirror the step-by-step process I follow.

***Without Trick***

```

根据以下 Medium 博客写一篇 LinkedIn 帖子。

Medium 博客：{Medium 博客文本}

```py

***With Trick***

```

根据逐步过程和 Medium 博客写一篇 LinkedIn 帖子\

如下所示。

第 1 步：想出一个与博客相关的一句话引子。

第 2 步：从文章中提取 3 个关键点

第 3 步：将每个要点压缩到 50 个字符以内。

第 4 步：将引子、第 3 步中的压缩要点和行动号召结合起来\

以生成最终输出。

Medium 博客：{Medium 博客文本}

```py

## **Trick 5: Chatbot Personas**

A somewhat surprising technique that tends to improve LLM performance is to prompt it to take on a particular persona e.g. “*you are an expert*”. This is helpful because you may not know the best way to describe your problem to the LLM, but you may know who would help you solve that problem [[1](https://arxiv.org/abs/2302.11382)]. Here’s what this might look like in practice.

***Without Trick***

```

给我制定一个在纽约市度过周末的旅行计划。

```py

***With Trick***

```

充当一位了解纽约市的一切的纽约本地人和出租车司机。\

请根据\

你的经历。不要忘记在你的\

响应。

```py

## **Trick 6: Flipped Approach**

It can be difficult to optimally prompt an LLM when **we do not know what it knows or how it thinks**. That is where the “flipped approach” can be helpful. This is where you prompt the LLM to ask you questions until it has a sufficient understanding (i.e. context) of the problem you are trying to solve.

***Without Trick***

```

一个基于 LLM 的应用程序的想法是什么？

```py

***With Trick***

```

我希望你问我问题，以帮助我提出基于 LLM 的

应用程序想法。一次问我一个问题，以保持对话性。

```py

## **Trick 7: Reflect, Review, and Refine**

This final trick prompts the model to reflect on its past responses to improve them. Common use cases are having the model critically evaluate its own work by asking it if it “*completed the assignment*” or having it “*explain the reasoning and assumptions*” behind a response [1, 3].

Additionally, you can ask the LLM to refine not only its responses but **your prompts**. This is a simple way to automatically rewrite prompts so that they are easier for the model to “understand”.

***With Trick***

```

复查你之前的响应，找出改进的地方，并提供

改进版。然后解释你如何改进响应的理由。

```py

# **Example Code: Automatic Grader with LangChain**

Now that we’ve reviewed several prompting heuristics let’s see how we can apply them to a specific use case. To do this, we will return to the automatic grader example from before.

```

你是一名高中历史老师，负责评分作业。

基于由 "Q:" 指示的作业问题和正确答案

由 "A:" 指示，你的任务是确定学生的答案是否

正确。

评分是二元的，因此，学生的回答可以是正确的或错误的。

简单的拼写错误是可以的。

Q: {question}

A: {correct_answer}

学生答案: {student_answer}

```py

On second look, a few of the previously mentioned tricks should be apparent i.e. **Trick 6**: chatbot persona, **Trick 3**: use structured text, and **Trick 1**: be descriptive. This is what good prompting typically looks like in practice, namely combining multiple techniques in a single prompt.

While we could copy-paste this prompt template into ChatGPT and replace the *question*, *correct_answer*, and *student_answer* fields, **this is not a scalable way to implement the automatic grader**. Rather, what we want is to integrate this prompt into a larger software system so that we can build a user-friendly application that a human can use.

## LangChain

One way we can do this is via **LangChain**, which is **a Python library that helps simplify building applications on top of large language models**. It does this by providing a variety of handy abstractions for using LLMs programmatically.

The central class that does this is called **chain** (hence the library name). This abstracts the process of generating a prompt, sending it to an LLM, and parsing the output so that it can be easily called and integrated into a larger script.

Let’s see how to use LangChain for our automatic grader use case. The example code is available on the [GitHub Repo](https://github.com/ShawhinT/YouTube-Blog/tree/main/LLMs/langchain-example) for this article.

## Imports

We first start by importing the necessary library modules.

```

从 langchain.chat_models 导入 ChatOpenAI

从 langchain.prompts 导入 PromptTemplate

从 langchain.chains 导入 LLMChain

从 langchain.schema 导入 BaseOutputParser

```py

Here we will use gpt-3.5-turbo which requires a secret key for OpenAI’s API. If you don’t have one, I gave a step-by-step guide on how to get one in a past article of this series. I like to store the secret key in a separate Python file (*sk.py*) and import it with the following line of code.

```

从 sk 导入 my_sk #从另一个 Python 文件导入密钥

```py

## Our 1st chain

To define our chain, we need two core elements: the **LLM** and the **prompt**. We start by creating an object for the LLM.

```

# 定义 LLM 对象

chat_model = ChatOpenAI(openai_api_key=my_sk, temperature=0)

```py

LangChain has a class specifically for OpenAI (and many other) chat models. I pass in my secret API key and set the temperature to 0\. The default model here is *gpt-3.5-turbo*, but you can alternatively use *gpt-4* using the “model_name” input argument. You can further customize the chat model by setting other [input arguments](https://api.python.langchain.com/en/latest/chat_models/langchain.chat_models.openai.ChatOpenAI.html#langchain.chat_models.openai.ChatOpenAI).

Next, we define our **prompt template**. This object allows us to generate prompts dynamically via input strings that automatically update a base template. Here’s what that looks like.

```

# 定义提示模板

prompt_template_text = """你是一名高中历史老师，负责评分

作业。基于由 “**Q:**” 指示的作业问题

以及由 “**A:**” 指示的正确答案，你的任务是确定

确定学生的答案是否正确。评分是二元的，因此，

学生的回答可以是正确的或错误的。简单的拼写错误是可以的。

**Q:** {question}

**A:** {correct_answer}

**学生的答案:** {student_answer}

"""

prompt = PromptTemplate(

            input_variables=["question", "correct_answer", "student_answer"], 

            template = prompt_template_text)

```py

With our LLM and prompt, we can now define our chain.

```

# 定义链

chain = LLMChain(llm=chat_model, prompt=prompt)

```py

Next, we can pass inputs to the chain and obtain a grade in one line of code.

```

# 定义输入

问题 = "谁是美国第 35 任总统？"

正确答案 = "John F. Kennedy"

student_answer = "FDR"

# 运行链

chain.run({'question':question, 'correct_answer':correct_answer,

    'student_answer':student_answer})

# 输出: 学生的答案是错误的。

```py

While this chain can perform the grading task effectively, its outputs may not be suitable for an automated process. For instance, in the above code block, the LLM correctly said the student’s answer of “FDR” was wrong, but it would be better if the LLM gave us an output in a standard format that could be used in downstream processing.

## Output parser

This is where **output parsers** come in handy. These are functions we can integrate into a chain to convert LLM outputs to a standard format. Let’s see how we can make an output parser that converts the LLM response to a boolean (i.e. *True* or *False*) output.

```

# 定义输出解析器

class GradeOutputParser(BaseOutputParser):

    """确定评分是否正确或错误"""

    def parse(self, text: str):

        """解析 LLM 调用的输出。"""

        返回 "wrong" 不在 text.lower() 中

```py

Here, we create a simple output parser that checks if the word “wrong” is in the LLM’s output. If not, we return *True*, indicating the student's correct answer. Otherwise, we return *False*, indicating the student's answer was incorrect.

We can then incorporate this output parser into our chain to seamlessly parse text when we run the chain.

```

# 更新链

chain = LLMChain(

    llm=chat_model,

    prompt=prompt,

    output_parser=GradeOutputParser()

)

```py

Finally, we can run the chain for a whole list of student answers and print the outputs.

```

# 在 for 循环中运行链

student_answer_list = ["John F. Kennedy", "JFK", "FDR", "John F. Kenedy",

                "John Kennedy", "Jack Kennedy", "Jacquelin Kennedy",

                "Robert F. Kenedy"]

对于 student_answer_list 中的 student_answer:

    print(student_answer + " - " +

    str(chain.run({'question':question, 'correct_answer':correct_answer,

                    'student_answer':student_answer})))

    print('\n')

# 输出:

# John F. Kennedy - 正确

# JFK - 正确

# FDR - 错误

# John F. Kenedy - 正确

# John Kennedy - 正确

# Jack Kennedy - 正确

# Jacqueline Kennedy - 错误

# Robert F. Kenedy - 错误

```

[](https://github.com/ShawhinT/YouTube-Blog/tree/main/LLMs/langchain-example?source=post_page-----7ce1ed3b553f--------------------------------) [## YouTube-Blog/LLMs/langchain-example at main · ShawhinT/YouTube-Blog

### 代码用于补充 YouTube 视频和 Medium 博客文章。- YouTube-Blog/LLMs/langchain-example at main ·…

github.com](https://github.com/ShawhinT/YouTube-Blog/tree/main/LLMs/langchain-example?source=post_page-----7ce1ed3b553f--------------------------------)

# 限制

Prompt Engineering 不仅仅是向 ChatGPT 求助写电子邮件或了解量子计算。它是一个***改变开发者构建应用程序方式的新编程范式***。

尽管这是一个强大的创新，但它也有其局限性。例如，最佳的提示策略依赖于 LLM。例如，提示 GPT-3 “逐步思考” 在简单的数学推理任务中带来了显著的性能提升 [8]。然而，对于最新版本的 ChatGPT，相同的策略似乎并没有帮助（它已经逐步思考）。

Prompt Engineering 的另一个限制是它需要大规模的通用语言模型，例如 ChatGPT，这需要显著的计算和经济成本。这对于许多更狭义的用例，例如字符串匹配、情感分析或文本摘要，可能过于复杂。

我们可以通过**微调**预训练语言模型来克服这两种限制。这是我们**对现有语言模型进行调整以适应特定用例的过程**。在[下一篇文章](https://medium.com/towards-data-science/fine-tuning-large-language-models-llms-23473d763b91)中，我们将探讨流行的微调技术，并附有示例 Python 代码。

👉 **关于 LLMs 的更多信息**: 介绍 | [OpenAI API](https://medium.com/towards-data-science/cracking-open-the-openai-python-api-230e4cae7971) | [Hugging Face Transformers](https://medium.com/towards-data-science/cracking-open-the-hugging-face-transformers-library-350aa0ef0161) | [微调](https://medium.com/towards-data-science/fine-tuning-large-language-models-llms-23473d763b91) | 构建 LLM | QLoRA | RAG | 文本嵌入

![Shaw Talebi](img/02eefb458c6eeff7cd29d40c212e3b22.png)

[Shaw Talebi](https://shawhin.medium.com/?source=post_page-----7ce1ed3b553f--------------------------------)

## 大型语言模型（LLMs）

[查看列表](https://shawhin.medium.com/list/large-language-models-llms-8e009ae3054c?source=post_page-----7ce1ed3b553f--------------------------------)13 个故事![](img/82e865594c68f5307e75665842d197bb.png)![](img/b9436354721f807e0390b5e301be2119.png)![](img/59c8db581de77a908457dec8981f3c37.png)

# 资源

**联系**: [我的网站](https://shawhintalebi.com/) | [预约电话](https://calendly.com/shawhintalebi) | [问我任何问题](https://shawhintalebi.com/contact/)

**社交媒体**: [YouTube 🎥](https://www.youtube.com/channel/UCa9gErQ9AE5jT2DZLjXBIdA) | [LinkedIn](https://www.linkedin.com/in/shawhintalebi/) | [Twitter](https://twitter.com/ShawhinT)

**支持**: [请我喝咖啡](https://www.buymeacoffee.com/shawhint) ☕️

[](https://shawhin.medium.com/subscribe?source=post_page-----7ce1ed3b553f--------------------------------) [## 免费获取我写的每一个新故事

### 免费获取我写的每一个新故事 P.S. 我不会将你的电子邮件分享给任何人 通过注册，你将创建一个…

shawhin.medium.com](https://shawhin.medium.com/subscribe?source=post_page-----7ce1ed3b553f--------------------------------)

[1] [arXiv:2302.11382](https://arxiv.org/abs/2302.11382) **[cs.SE]**

[2] [arXiv:2106.09685](https://arxiv.org/abs/2106.09685) **[cs.CL]**

[3] [GPT 状态](https://www.youtube.com/watch?v=bZQun8Y4L2A) 由 [Andrej Karpathy](https://medium.com/u/ac9d9a35533e?source=post_page-----7ce1ed3b553f--------------------------------) 在 Microsoft Build 2023 上演讲

[4] [arXiv:2206.07682](https://arxiv.org/abs/2206.07682) **[cs.CL]**

[5] [为开发者设计的 ChatGPT 提示工程](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/) 由 deeplearning.ai 提供

[6] [arXiv:2005.14165](https://arxiv.org/abs/2005.14165) **[cs.CL]**

[7] [arXiv:2201.11903](https://arxiv.org/abs/2201.11903) **[cs.CL]**

[8] [arXiv:2210.03493](https://arxiv.org/abs/2210.03493) **[cs.CL]**
