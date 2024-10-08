# 提示工程可能是 2024 年最热门的编程语言——原因如下

> 原文：[`towardsdatascience.com/prompt-engineering-could-be-the-hottest-programming-language-of-2024-heres-why-a9ccf4ba8d49`](https://towardsdatascience.com/prompt-engineering-could-be-the-hottest-programming-language-of-2024-heres-why-a9ccf4ba8d49)

## 大型语言模型是下一代操作系统

[](https://nabil-alouani.medium.com/?source=post_page-----a9ccf4ba8d49--------------------------------)![Nabil Alouani](https://nabil-alouani.medium.com/?source=post_page-----a9ccf4ba8d49--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a9ccf4ba8d49--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a9ccf4ba8d49--------------------------------) [Nabil Alouani](https://nabil-alouani.medium.com/?source=post_page-----a9ccf4ba8d49--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a9ccf4ba8d49--------------------------------) ·阅读时间 16 分钟·2023 年 12 月 21 日

--

![](img/8914fb851cbf6bd3ef466345eacf0eb1.png)

除非另有说明，所有图片均由作者使用 Midjourney、DALL-E 和 Canva 生成。

“我不认为把大型语言模型看作聊天机器人或某种文字生成器是准确的，” OpenAI 创始成员之一的[Andrej Karpathy](https://karpathy.ai/)说。“更正确的想法是把它们看作**新兴操作系统的内核进程**。”

等等，但那到底是什么意思？

大型语言模型（LLMs）将逐渐成为你与计算机系统之间的界面。

现在，你手上拿着一个拥有一些计算能力的设备，但你不能直接访问这些能力。你的交互由操作系统（如 Windows、Mac OS 和 Android）介导，它将一组芯片和电路转换成一个用户友好的界面。

你的操作系统（OS）允许你通过一系列运行在其上的应用程序执行各种活动（如阅读某个秃头家的文章）。每个应用程序都有自己的用户界面（UI）和可以完成的任务。你根据需要在不同应用程序和用户界面之间跳转。

![](img/702445b34dcdd1c17bfd8c0e23d264c3.png)

明天，你将拥有一个单一的用户界面来完成从编写年度业务报告到从头构建新应用的所有任务。这个用户界面将是一个聊天框或一个“上下文窗口”，你可以在其中用自然语言提交指令——这就是提示工程发挥作用的地方。

Prompt Engineering 是一种花哨的说法，用来表示“为你的 AI 模型编写越来越好的指令，直到它完全按照你的要求执行。”不过，这不仅仅是文字游戏；它是未来编程的蓝图。

# 编程作为（便宜的）商品

“编程指的是一种技术过程，用于告诉计算机执行哪些任务以解决问题，”Coursera [在他们的网站上写道](https://www.coursera.org/articles/what-is-programming)。“你可以将编程看作是人类与计算机之间的协作，其中人类创建计算机可以理解的指令（代码）供计算机遵循。”

换句话说，编程将计算能力转变为一种商品：你可以用来实现目标的资源。Prompt Engineering 是一种将编程本身转变为商品的工具。你提交一个指令给 LLM，它会为你编写代码。

比如说，你想为一个工作项目分析一个小数据集。通常，你会先从公司云端收集数百个 CSV 文件。然后，你双击 Jupyter Notebook，输入几行 Python 代码，将输入数据编译成一个数据框。

从这里开始，你施加一些数据科学的魔法，运行几轮迭代，恭喜你：你获得了一系列优雅的表格、花哨的图表和数据驱动的预测。你最后一步是将六周的工作压缩成 42 张漂亮的幻灯片，展示在另一个叫做 Microsoft PowerPoint 的应用程序上。

![](img/234a6b82ed7ee30a81061e3d3691a201.png)

你典型的数据科学家在一个随意的星期二。

你刚刚将现成的应用程序与自己编写的代码结合起来，构建了一个运行特定数据分析的程序。但如果你只需用简单的英语写几条指令呢？

“嘿，AI 伙伴，”你会说，“这是一个关于我们公司在过去五年中在巴黎进行的交付的混乱数据集。请清理这些数据，运行一个聚类算法。展示热力图，并放大高密度区域。加上两年的预测，并利用这些结果优化我们配送车队的每日行程。当你完成数学部分时，生成一个包含清晰图表和简洁评论的报告。慢慢来！我会离开至少六小时。”

每次你编写这样的提示时，你实际上是在编程一个解决特定问题的应用程序。

这几乎就像 Coursera 描述的那样——你与计算机协作以实现目标。唯一的区别是，你将使用简单的英语，而不是代码。好吧，提示可能离终点线还有几步之遥，但原则上，这就是你与未来 LLM 互动的方式。

以更具体的例子来看，考虑下面的著名演示，其中 GPT-4 将手绘草图转换为功能性 HTML 代码。

# 提示就是你所需要的一切

在我们能够用一堆聪明的提示替代数据科学家、网页开发者和软件工程师之前，还需要一段时间。在此期间，我们将通过 AI 助手来增强他们的工作效率——这些 AI 助手将用普通英语编程。

你将不再使用如 Google Workspace、Jupyter Notebook 和 Microsoft PowerPoint 等一系列互补应用，而是创建一个名为“StatSniffer: 你的个人数据科学专家”的助手。

![](img/34aa9d60a397b4baeaa48b9d8c8ab62e.png)

类似于当前的 ChatGPT PLUS，你的个性化 StatSniffer 将是一个连接到一系列工具的 LLM，这些工具赋予它额外的能力，比如浏览文件、运行代码和生成图表。你还可以通过让它访问研究论文、案例研究和学术教科书来注入顶级的方法论。

OpenAI 已经通过 GPT 商店实验个性化 AI 助手，你可以构建名为 GPT 的助手。然而，目前的 GPTs 确实显得笨拙。例如，它们容易受到简单的破解，使其暴露“核心指令”。GPTs 也往往在与用户交流几次后恢复到默认模式（GPT-4）。

这并不令人惊讶，因为技术仍处于初期阶段。随着 AI 研究的进展和开源模型的改善，AI 助手的生态系统将演变为覆盖更多功能，并提高可靠性。说到这，还有很长的路要走。

像规划和多步骤推理等问题仍未解决，部分原因是 [LLMs 在理解物理现实方面仍落后于人类](https://nabilalouani.substack.com/p/chatgpt-hype-is-proof-nobody-really)（甚至是猫）。

这并不意味着人们会期待完全自主的助手掌握量子引力来利用现有的 AI 模型。即便是所谓的‘愚蠢’ LLMs，也能将你的效率提高一倍。以下是 [麦肯锡关于](https://www.mckinsey.com/capabilities/mckinsey-digital/our-insights/unleashing-developer-productivity-with-generative-ai#/) 开发人员如何使用 LLMs 加速工作的研究摘录。

“我们最新的实证研究发现，基于生成 AI 的工具在许多常见开发任务中提供了令人印象深刻的**速度提升**。**记录代码**功能以便于维护（考虑到代码如何容易改进）可以在**一半的时间**内**完成**，**编写新代码几乎在一半的时间**内完成，**优化现有代码**（称为代码重构）则**在近三分之二的时间**内完成。” [作者强调]。

“为了让开发人员有效地利用这些技术来增强日常工作，他们可能需要培训和指导的结合，”麦肯锡研究团队解释说。“初始培训应包括最佳实践和将自然语言提示输入工具的动手练习，通常称为提示工程。”

这不仅对软件相关任务是正确的。这种模式也扩展到更广泛的公司活动中。例如，[哈佛商学院](https://www.hbs.edu/faculty/Pages/item.aspx?num=64700)的研究人员进行了一项[研究](https://www.hbs.edu/faculty/Pages/item.aspx?num=64700)，评估了为波士顿咨询集团（BCG）的员工配备生成 AI 工具的影响。

“在 AI 能力前沿的一组 18 个现实咨询任务中，使用 AI 的顾问生产力显著提高（他们平均完成的任务多出 12.2%，任务完成速度提高了 25.1%），并且产生了显著更高质量的结果（与对照组相比质量高出 40%以上），”HBS 研究人员写道。

这是[Rajiv Shah](https://www.linkedin.com/in/rajistics/overlay/about-this-profile/)的研究总结，Rajiv Shah 是一位机器学习工程师和 YouTuber。

这些研究为新的陈词滥调提供了支持：“AI 不会取代你，但使用 AI 的人会。”也许更花哨的表述是：“AI 不会取代你，但提示工程师会。”

“‘AI 前沿’越大（意味着 AI 模型可以高准确度执行的任务越多），我们通过提示可以解决的问题就越多。”这带我们到一个普遍的谬论，认为更强大的 AI 模型需要更少的提示工程技能。

# “提示工程死了吗？”不，它是最前沿的技术。

看到大型语言模型和提示工程之间的关系的一种方式是把前者想象成一个多元宇宙，后者则像一个指示器——没错，就像一个激光指示器。

当你向 LLM 提问时，它会考虑一个相关文档的多元宇宙。在每个文档内，有一组可能的答案，每个可能的答案都是一系列概率。

你的提示指向最有可能包含所需答案的宇宙——然后，你的模型尝试一步步地导航到那个所需的答案。

每当模型预测一个标记时，它会排除数百条替代路径，并继续缩小选项，直到剩下的只有一系列构成“目的地”的词。

![](img/262ca56dd5ecd01082e543569bd55375.png)

你的 LLM 在多元宇宙中导航可能的答案（这里更像是“在模糊的星球上导航”，但你明白我的意思）。

然而，这个目的地从未相同。即使你使用完全相同的提示，你几乎不会达到完全相同的地址。相反，你会在“邻域”中找到最相关的答案。

这里有一篇[更技术性的描述](https://fchollet.substack.com/p/how-i-think-about-llm-prompt-engineering)，作者是**弗朗索瓦·肖利**，他是一位软件工程师和 AI 研究员，目前在谷歌工作：

> 如果一个 LLM 就像是一个包含数百万个向量程序的数据库，那么提示就像是在那个数据库中的搜索查询[...] 这个“程序数据库”是连续的和插值的——它不是一个离散的程序集合。
> 
> 这意味着，像“以 x 风格重新措辞此文本”这样略有不同的提示仍然会指向程序空间中非常相似的位置，导致生成的程序行为非常接近但不完全相同。[...]
> 
> 提示工程是搜索程序空间以找到在你的目标任务上表现最佳的程序的过程。

正如**肖利**指出的，你的提示目标是调用适合你要完成的任务的正确程序。许多人陷入的推理陷阱是相信未来的 LLMs 应该能够预测我们希望它们运行哪个程序，即使我们给出的是模糊的任务。

除非，就像人类一样，即使你雇佣了技术能力最强的工程师，她也无法读懂你的心思。你必须准确说明你想要的，否则你是在浪费时间和精力。

假设你指示你那位杰出的工程师去开发一个产品，但你不满意结果。你可以选择更换工程师或更改你的指示。既然你知道你的工程师非常熟练，常识告诉你选择第二种选项。

同样地，如果你高度能力的语言模型没有产生你想要的答案，你不会把它扔掉。你也不会坐等下一个模型能读懂你的心思。最合理且具成本效益的方法是改进你的提示。

这就是微软研究团队在 GPT-4 上所做的。与其为特定用例微调模型，[他们使用了提示工程技术来提高其性能](https://arxiv.org/abs/2311.16452)。

GPT-4 在九个不同的医学基准测试中的分数提高了多达 9%。因此，该模型的准确性超过了 90%，优于专门为医学应用微调的模型。

**赖利·古德赛德**可以说是首位提示工程师。

请注意，微调需要额外的资源，比如雇佣专家来制作高质量的训练数据以及一些计算资源来重新训练模型。虽然微调所需的计算能力比预训练少，但这仍然是额外的成本。

此外，每次你想为新的专业领域微调模型时，都需要投入相同的资源。相比之下，微软开发了一种提高不同领域（如电气工程、机器学习、哲学、会计、法律、护理和临床心理学）表现的提示技术。

另一个[突显提示工程强大之处的例子来自 Anthropic](https://www.anthropic.com/index/claude-2-1-prompting)。他们的团队通过在提示中添加一句话，将 Claude 2.1 模型在信息检索评估中的表现提高了 98%。仅仅一句话。

操控 LLM 就像玩一个外星工具。弄清楚它能做什么的唯一办法是以不同的方式按它的按钮。当这个外星工具的新版本出来时，你会期望它有更多的功能，但也有更多的按钮。

天真的方法是认为“更强大的模型需要更少的提示”。实际上，你的模型越强大，你可以通过正确的提示解锁的功能就越多。

# 展现风采的时候到了，人类代理！

从长远来看，我们将使用 LLM 作为操作系统来运用计算能力并解决各种问题。在中期，我们将编程 AI 代理以执行以前需要编写代码的任务。在这两个阶段中，我们都将使用英语作为主要编程语言。

好的，但现在发生了什么？

在 AI 代理赶上之前，正是你大展身手的时候。把自己想象成一个技术工匠，将 AI 模型、代码和传统应用结合起来，以应对复杂的挑战。可以说，你将玩乐高，而且特别感谢开源社区，你将拥有无尽的积木，可以组合起来创造创新项目。

LLM 是这个假设的乐高积木中的特殊部分，因为你常常会发现它们在你的创作中心。这将我们引向 Prompt Engineering 的两个互补风格。

见，[Prompt Engineering 有两个含义](https://simonwillison.net/2023/Feb/21/in-defense-of-prompt-engineering/)：(1) 为大型语言模型（LLM）编写高质量的自然语言指令，以及 (2) 在 LLM 之上编写代码，通过条件提示和其他技术来改善其输出。

第二个定义包含了第一个，因为即使你在 LLM 周围包裹了代码，你仍然使用英语与其互动。这两种定义如何与 LLM 的使用交织在一起。

+   **LLM 作为独立程序：** 在这里，你编写高质量的自然语言提示以解锁最佳输出。示例包括创意生成、文档总结和编写代码。

+   **LLM 作为你设计的程序的一部分：** 在这里，你编写软件（用 Python、Java、C++或其他编程语言）来包裹 LLM 以完成特定任务。示例包括社交媒体评论的情感分析、专用聊天机器人和自主代理。

现在让我们深入探讨一下 Prompt Engineering 在每种用例中的表现。

## 1️⃣ “LLM 作为独立程序”的 Prompt Engineering

LLM 最常见的用例是通过像 ChatGPT 和 Bard 这样的网页界面与它们互动。

根据你的具体需求，你可以建立个人提示库。你希望你的提示是模板化且易于更新的。这样，你就不必每次运行提示时从头编写或在聊天记录中搜索它们。

以下是三种不同的灵活提示示例，可能会激发你的灵感：

```py
**[PROMPT TEMPLATE #1 DOCUMENT SUMMARY]**

Act like a research assistant in the field of <field_name>.
I will give you a report titled <title_of_the_report> as input.
Please access the report through the following link <URL_of_the_report> using the online browsing feature.
Summarize the report in less than <summary_wordcount> and add <number_of_quotes> from the authors.
Make sure to pick precise quotes and list them as bullet points.

##

Desired format:

Title: <title_of_the_report> 
Link: <URL_of_the_report>

Summary of the report based on the previous instructions.

- Quote #1
- Quote #2
- Quote #3
- etc.

##

Inputs:

<field_name> = Placeholder for the field of expertise of your document.
<title_of_the_report> = Placeholder for the title of the report you want summarize.
<URL_of_the_report> = Placeholder for web address where the report can be found.
<summary_wordcount> = Placeholder for the maximum word count for the summary. 
<number_of_quotes> =  Placeholder for the number of quotes to be extracted from the report. 
```

```py
**[PROMPT TEMPLATE #2 PRODUCT DESCRIPTION]**

Act like an expert copywriter.

##
Role:
Write a product description for an e-commerce shop.
Use the following structure and fill in the details based on the placeholders provided:

Product Name: <product_name>

Introduction: Start with a captivating opening sentence about <product_name>, suitable for <target_audience>.

Key Features: List the main features of <product_name>. Include <product_features>.

Benefits: Explain how <product_name> benefits the user, addressing <target_audience> needs.

Call to Action: Encourage the reader to make a purchase decision with a compelling call-to-action.

SEO Keywords: Integrate <seo_keywords> naturally within the text for SEO purposes.

Tone: Maintain a <brand_tone> throughout the description to align with the brand's voice.

##

Inputs:

<product_name> = Placeholder for the name of the product.
<product_features> = Placeholder for listing the specific features of the product.
<target_audience> = Placeholder to specify the target audience or demographic for the product.
<seo_keywords> = Placeholder for SEO-optimized keywords relevant to the product.
<brand_tone> = Placeholder to define the brand's tone of voice to be reflected in the product description. 
```

```py
**[PROMPT TEMPLATE #3 PROGRAMMING ASSISTANT]**

Act like a software engineer.

##
Role:
Your role is to write a program in <programming_language>.
Your program must follow these instructions: <user_instructions>.
Reason step by step to make sure you understand the user's instructions before you generate the code.
Ensure the code is clear, well-commented, and adheres to best practices in <programming_language>.

##
Format:
Give a clear title to each code snippet you generate.
For example you can title the first snippet "Snippet #1 version 1," the second snippet "Snippet #2 version 1," and an updated version of the second snippet can be "Snippet #2 version 2."

##
Inputs:
<programming_language> = Placeholder for the programming language in which you want the code to be written (e.g., Python, JavaScript).
<user_instructions> = Placeholder for the specific instructions or description of the task you want to be turned into code.
```

## 2️⃣ **“将 LLMs 作为你设计的程序的一部分的提示工程”**

在这种情况下，你将使用你的 LLMs 作为可以调用的函数来处理、分析和生成自然语言。

例如，你的代码可以调用 LLM 来分析一系列与某个产品相关的评论的情感。在处理这些评论后，你可以使用另一个依赖 LLM 的函数来生成基于先前结果的回应。

现在，让我们看看如何在代码中嵌入 LLM。有三种主要方式：

+   通过另一个公司提供的 API 进行连接；

+   使用公司网络中的本地服务器；

+   直接在你的计算机上安装开源 LLM。

这是一个如何在程序中使用 LLM 的基本示例：

```py
 # Objective: Carry out a sentiment analysis on a series of comments stored in an Excel file
# LLM choice: We'll use OpenAI API to call the gpt-4 model

import pandas as pd         # version used 2.1.3
from openai import OpenAI   # version used 1.2.0

# Define the path to your API key file
API_key_path = "C:/Users/.....API_Key.txt"  # Replace with your actual API key file path

# Read the API key from the file
with open(API_key_path, 'r') as file:
    API_key = file.read().strip()  # .strip() removes any leading/trailing whitespace

# Create the OpenAI client object with the API key
client = OpenAI(api_key=API_key)

# Define a function that analyzes sentiment in a series of comments stored in an Excel file

def analyze_sentiments(input_file):
    # Read the input Excel file
    inputs_df = pd.read_excel(input_file)

    # Select the column by its name 'comments'
    if 'comments' in inputs_df.columns:
        comments = inputs_df['comments']
    else:
        raise ValueError("Column 'comments' not found in the input file")

    sentiments = []

    for comment in comments:
        # Prepare API call
        messages = [
            {
                "role": "system",
                "content": "Analyze the sentiment of the following comment. Use a single word to describe the sentiment. Either `Positive` or `Negative`. Refrain from writing any extra text. Thanks!",            },
            {
                "role": "user",
                "content": comment,
            }
        ]
        model = "gpt-4"
        temperature = 1.0
        max_tokens= 100 # Adjust based on expected response length

        # Make the API call
        chat_completion = client.chat.completions.create(
            model=model,
            temperature=temperature,
            max_tokens= max_tokens,  
            messages=messages
        )

        # Extract response
        if chat_completion.choices:
            sentiment_result = chat_completion.choices[0].message.content.strip()
        else:
            sentiment_result = "No response"

        sentiments.append(sentiment_result)

    # Combine comments with their sentiments
    result_df = pd.DataFrame({
        'Comment': comments,
        'Sentiment': sentiments
    })

    # Write to a new Excel file
    output_file = "path_to_your_output_file.xlsx"  # Replace with your desired output file path
    result_df.to_excel(output_file, index=False)
    print("Sentiment analysis complete. Results saved to", output_file)

# Example usage
input_file = "path_to_your_input_file.xlsx"  # Replace with your input file path
analyze_sentiments(input_file)

# Homework exercice: Write a function that uses a dynamic prompt to write a different response based on the result of sentiment analysis.
```

# 如何提高你的提示工程技能

简短的回答是我从 Stephen King 那里偷来的优雅名言。“你必须做两件事，” [他说](https://www.goodreads.com/quotes/312517-if-you-want-to-be-a-writer-you-must-do)。“多读书，多写作。”

就像写作一样，提示工程看似简单，直到你坐下来动手键盘。由于我们使用自然语言编写提示，我们对它有一种虚假的简单感。

请原谅这里的重复，但 AI 模型还无法读懂你的思维（尚未）。

如果你想要高质量的回应，你必须学会尽可能清晰地表达你的意图。跟进文献，学习新技术，并尽可能多地练习将它们融入。

你可能会厌倦在闪烁的上下文窗口中输入随机指令。解药是找到困难的问题来解决。你如何使输出的某些部分随机化？如何动态改变提示的内容？你能编程一个抵抗越狱尝试的助手吗？

找到难以解决的问题，提示工程将从“你必须学习的技能”转变为每日的“愉快（但有时令人沮丧）的智力刺激”。

此外，还有两个其他话题值得你关注：机器学习总体以及深度学习特别。你需要探讨它们的优缺点，因为一旦你理解了生成式 AI 背后的技术，你将对你的模型为何以特定方式表现出直觉。

以下是一些可以帮助你入门的资源：

+   [**机器学习的朋友**](https://www.youtube.com/watch?v=lKXv19eRLZg&list=PLRKtJ4IpxJpDxl0NTvNYQWKCYzHNuy2xG&ab_channel=CassieKozyrkov)由 Cassie Kozyrkov 制作。（YouTube 视频系列）

+   [**大语言模型简介**](https://www.youtube.com/watch?v=zjkBMFhNj_g&ab_channel=AndrejKarpathy)由 Andrej Karpathy 提供。（YouTube 视频）

+   [**开发者的提示工程**](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/)由 Isa Fulford 和 Andrew Ng 提供（免费在线课程）

+   **如何为 LLMs 编写专家级提示**由这位秃顶兄弟提供（完整的 16,000 字指南，包括 25+种提示技术、示例和评论）。

[](/how-to-write-expert-prompts-for-chatgpt-gpt-4-and-other-language-models-23133dc85550?source=post_page-----a9ccf4ba8d49--------------------------------) ## 如何为 ChatGPT（GPT-4）及其他语言模型编写专家级提示

### 适合初学者的 LLM 提示工程指南

[towardsdatascience.com

# TL;DR 版

LLMs 正逐渐成为我们与计算能力的接口。提示工程是编写指令的艺术，以从你的 LLMs（及其他 AI 模型）中获得最佳结果。

+   我们将使用自然语言与未来的操作系统互动。

+   在那之前，我们将使用自然语言编程 AI 助手。

+   提示工程大多用自然语言进行，但这并不意味着 AI 模型能读懂你的心思。你仍需清晰地表达指令。

+   如果你能编写高质量的提示，你也能编写代码。

+   那些能编写更好指令的人将构建更好的程序并获得更好的结果。

+   提示工程是解锁语言模型（以及 AI 模型整体）潜在能力的关键。

+   提示工程是一门经验科学。你可以从他人的经验中学习，但从自己的经验中学到的最多。

# 保持联系？

你可以[**订阅以获取电子邮件通知**](https://nabil-alouani.medium.com/subscribe)当我发布新文章时。

我也活跃于[**Linkedin**](https://www.linkedin.com/in/nabil-alouani/)和[**X**](https://twitter.com/Nabil_Alouani_)，并回复每一条消息。

如需咨询提示工程，请联系我：**nabil@nabilalouani.com**。

如果你在想：这篇文章是 100%人工生成的。
