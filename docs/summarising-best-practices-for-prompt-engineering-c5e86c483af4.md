# 总结最佳实践以进行提示工程

> 原文：[https://towardsdatascience.com/summarising-best-practices-for-prompt-engineering-c5e86c483af4?source=collection_archive---------0-----------------------#2023-05-29](https://towardsdatascience.com/summarising-best-practices-for-prompt-engineering-c5e86c483af4?source=collection_archive---------0-----------------------#2023-05-29)

## 如何使用 OpenAI API 构建自己的基于 LLM 的应用

[](https://medium.com/@andimid?source=post_page-----c5e86c483af4--------------------------------)[![Dmytro Nikolaiev (Dimid)](../Images/4121156b9c08ed20e7aa620712a391d9.png)](https://medium.com/@andimid?source=post_page-----c5e86c483af4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c5e86c483af4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c5e86c483af4--------------------------------) [Dmytro Nikolaiev (Dimid)](https://medium.com/@andimid?source=post_page-----c5e86c483af4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F97b5279dad26&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsummarising-best-practices-for-prompt-engineering-c5e86c483af4&user=Dmytro+Nikolaiev+%28Dimid%29&userId=97b5279dad26&source=post_page-97b5279dad26----c5e86c483af4---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c5e86c483af4--------------------------------) · 13分钟阅读 · 2023年5月29日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc5e86c483af4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsummarising-best-practices-for-prompt-engineering-c5e86c483af4&user=Dmytro+Nikolaiev+%28Dimid%29&userId=97b5279dad26&source=-----c5e86c483af4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc5e86c483af4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsummarising-best-practices-for-prompt-engineering-c5e86c483af4&source=-----c5e86c483af4---------------------bookmark_footer-----------)![](../Images/29f7df4f533997ab03fde5e13ffcb7b5.png)

图片由[Glenn Carstens-Peters](https://unsplash.com/@glenncarstenspeters?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，来源于[Unsplash](https://unsplash.com/photos/RLw-UC03Gwc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。

提示工程指的是为大型语言模型（LLMs），如OpenAI的ChatGPT，创建称为*提示*的指令的过程。利用LLMs解决各种任务的巨大潜力，借助提示工程可以节省大量时间，并促进令人印象深刻的应用程序的开发。它是**释放这些大型模型全部能力**的关键，改变我们与这些模型的互动方式及其带来的好处。

在这篇文章中，我尝试总结提示工程的最佳实践，以帮助你更快地构建基于LLM的应用程序。虽然这个领域发展迅速，但以下这些“经过时间考验”的:) 技术往往效果很好，并能让你取得出色的成果。特别是，我们将涵盖：

+   **迭代提示开发**的概念，使用分隔符和结构化输出；

+   **思维链**推理；

+   **少样本学习**。

结合直观的解释，我将分享实际的示例和未来调查的资源。

然后我们将探索如何使用[OpenAI API](https://platform.openai.com/docs/introduction)免费构建一个简单的基于LLM的本地应用程序。我们将使用Python来描述逻辑，使用[Streamlit库](https://streamlit.io/)来构建网页界面。

让我们开始吧！

# 提示工程的最佳实践

在这篇文章中，我将通过网页界面和API与ChatGPT互动。我将使用的`gpt-3.5-turbo`模型是ChatGPT背后的模型，因此你可以直接在浏览器中实验你的提示。

> 这里一个重要的点是，ChatGPT不仅仅是大型语言模型（LLM）；如你所知，它还是一个SFT（监督微调）模型，经过了[来自人类反馈的强化学习（RLHF）](https://huyenchip.com/2023/05/02/rlhf.html)的进一步微调。虽然许多开发者目前利用OpenAI的模型进行实验项目和个人探索，但由于隐私和其他原因，其他模型可能更适合在大型企业的生产环境中部署。
> 
> 如果你想知道为什么基础模型（如[GPT-3](https://en.wikipedia.org/wiki/GPT-3)，[Chinchilla](https://arxiv.org/abs/2203.15556)，[LLaMA](https://ai.facebook.com/blog/large-language-model-llama-meta-ai/)）的功能与微调和RLHF训练的助手（如[ChatGPT](https://openai.com/blog/chatgpt)，[Koala](https://bair.berkeley.edu/blog/2023/04/03/koala/)，[Alpaca](https://crfm.stanford.edu/2023/03/13/alpaca.html)）不同，可以参考[Andrej Karpathy关于训练和使用类似GPT模型的讲座](https://build.microsoft.com/en-US/sessions/db3f4859-cd30-4445-a0cd-553c3304f8e2)。我强烈推荐查看这个讲座以获得更深入的理解，时间只有40分钟。总结可以参考[这个Twitter线程](https://threadreaderapp.com/thread/1661236778458832896.html)。

现在让我们深入探讨针对指令调优LLMs的最佳实践！

## 迭代提示开发

就像任何机器学习模型都是通过*迭代过程*构建的，有效的提示也是通过类似的迭代方法构建的。即使是最有才华的开发者也可能在第一次尝试时没有创建完美的提示，因此要准备好接受现实，可能需要*多次*尝试才能实现预期目标。

![](../Images/4083d7c372cb4ac15b64bd8af2107112.png)

基于数据的应用程序构建始终是一个迭代过程。[公共领域](https://commons.wikimedia.org/wiki/File:CRISP-DM_Process_Diagram.png)

通过示例理解事物总是更好。让我们开始构建一个**从职位描述中提取信息的系统**。我们将在示例中使用的职位描述是来自 LinkedIn 的机器学习工程师招聘广告。

![](../Images/2d4285bb3456098a7f92e0604ba754c5.png)

示例职位描述。来自[LinkedIn 职位页面](https://www.linkedin.com/jobs/)的截图

初始提示可以简单地请求模型提取特定信息。此外，我会使用分隔符（你可以在稍后提到的[ChatGPT 提示工程师课程](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/)中了解更多）。虽然本地应用程序不太可能受到[提示注入攻击](https://learnprompting.org/docs/prompt_hacking/injection)的影响，但这仍然是一个好习惯。

![](../Images/3f70008e054894ce2493fe290b9c3ad0.png)

提示 v1 的输出。由作者使用[ChatGPT](https://chat.openai.com/chat)创建的图像

嗯，这样并没有太大帮助。我们需要更具体地说明我们希望从模型中获得什么。让我们要求它提取职位名称、公司名称、所需关键技能以及总结的职位描述。

> 记住，这只是一个示例，你可以设计你的提示来提取你想要的任何信息：学位、所需经验年限、地点等等。

![](../Images/f9638fb4a5327f81e5a5cf32ff957ffb.png)

提示 v2 的输出。由作者使用[ChatGPT](https://chat.openai.com/chat)创建的图像

看起来更好！为了使输出更简洁明了，让我们要求模型将技能输出为列表，并提供更简短的职位描述总结。

![](../Images/c77323df87ea345febfaa32f5f4bf160.png)

提示 v3 的输出。由作者使用[ChatGPT](https://chat.openai.com/chat)创建的图像

这是我们初次尝试的显著改进，不是吗？我们仅仅用两次迭代就达到了这一点！所以，当事情最初进展不顺利时，不要失去希望。继续指导模型，进行实验，你一定会取得成功。

## 请求结构化输出

我想讨论的第二点是要求模型以某种预期的结构化格式输出结果。虽然这对通过网页界面与 LLM 交互（例如我们与 ChatGPT 的方式）可能不是那么关键，但对于基于 LLM 的应用程序来说是**极其有用的**，因为解析结果的过程要容易得多。

一种常见的做法是使用 JSON 或 XML 等格式，并定义特定的键来组织输出数据。让我们修改提示以展示模型预期的 JSON 结构。

![](../Images/7ec32d518783fe76a5e156fe8e630909.png)

对于提示 v4 的输出，要求 JSON 输出。由 [ChatGPT](https://chat.openai.com/chat) 创作的作者提供的图像

这样的输出更容易在应用程序的后续逻辑中解析和处理。

值得提及的是这个方向的发展。一些工具旨在严格将模型的输出适配到给定的格式，这对某些任务非常有用。只需看看下面的示例！

其中一个可能的应用是生成特定格式的大量内容（例如，使用 [guidance](https://github.com/microsoft/guidance) 的游戏角色信息）。

![](../Images/947eb4216ade29b99b26d82cb16f0ca2.png)

生成 JSON 格式的游戏角色信息。来自 [guidance GitHub 仓库](https://github.com/microsoft/guidance) 的 gif

像 [LMQL](https://lmql.ai/) 这样的语言为提示语言模型引入了类似编程的方法。随着这些工具的不断发展和改进，它们有可能彻底改变我们与 LLM 的互动方式，从而提供更准确和结构化的响应。

![](../Images/5fe7d232aafae7011bd6de1799076779.png)

LMQL 查询示例。请参阅 [LMQL 网页，更多示例请见此处](https://lmql.ai/) 的截图

## Chain-of-Thought 推理

Chain-of-Thought (CoT) 推理被发现对于需要……好吧，推理的任务非常有帮助。因此，如果你有机会通过将任务分解成多个更简单的步骤来解决问题，这对于 LLM 来说可能是一个很好的方法。

看看原始论文中的示例。通过将问题拆分成更小的步骤并提供明确的指示，我们可以帮助模型生成正确的输出。

![](../Images/8ed7855cdb779bf6d4e88425bf2cbcfe.png)

介绍 CoT 提示。来自 [Chain-of-Thought Prompting Elicits Reasoning in LLMs 论文](https://arxiv.org/pdf/2201.11903.pdf) 的图 1

有趣的是，后来发现，在提示的末尾附加一个简单的且*神奇的* **“让我们一步一步思考”** 可以改善结果——这种技巧被称为 *零-shot CoT*。因此，构建允许模型“思考过程”的提示是很有帮助的，因为模型没有其他表达思想的能力，除了生成标记。

目前最佳的零-shot CoT 提示是“**让我们一步一步地解决这个问题，以确保我们得到正确的答案**”。

![](../Images/d9ad914d665075aee6752b6276e50124.png)

最佳零样本提示。来自[LLMs 是人类水平的提示工程师论文](https://arxiv.org/pdf/2211.01910.pdf)的表7

更复杂的解决方案正在积极开发中。虽然它们在某些场景中显著超越其他方法，但其实际应用仍然有限。我将提到两种这样的技术：*自一致性*和*思维树*。

[自一致性论文](https://arxiv.org/pdf/2203.11171.pdf)的作者提出了以下方法。他们建议不要仅仅依赖于初始模型输出，而是通过多次采样并通过多数投票来聚合结果。通过依赖直觉和[集成方法](https://en.wikipedia.org/wiki/Ensemble_learning)在经典机器学习中的成功，这种技术增强了模型的鲁棒性。

![](../Images/c7c6be883fb384c6a33edc479af020de.png)

自一致性。来自[自一致性改进语言模型中的链式推理论文](https://arxiv.org/pdf/2203.11171.pdf)的图1

你也可以在不实施聚合步骤的情况下应用自一致性。对于短输出的任务，要求模型**建议几个选项**并选择最佳选项。

思维树（ToT）将这一概念进一步扩展。它提出了为模型的“推理思维”应用树搜索算法的想法，当模型遇到不良假设时，实际上是回溯。

![](../Images/a0fb05671b6dee06c9e97bfb2c687e18.png)

思维树。来自[思维树：使用LLMs进行深思熟虑问题解决论文](https://arxiv.org/pdf/2305.10601.pdf)的图1

> 如果你感兴趣，可以查看[Yannic Kilcher的ToT论文评论视频](https://www.youtube.com/watch?v=ut5kp56wW_4)。

对于我们的具体场景，利用链式思维推理并非必要，但我们可以将模型的总结任务分为两个阶段。最初，它可以概括整个职位描述，然后再对得出的总结进行集中于职位职责的总结。

![](../Images/8c95872561a59e8ab6fde59c6ae006c2.png)

带有逐步指令的提示v5的输出。由[ChatGPT](https://chat.openai.com/chat)创建的作者图像

在这个特定的例子中，结果没有显示出显著的变化，但这种方法对大多数任务非常有效。

## 少样本学习

我们将介绍的最后一种技术叫做*少样本学习*，也称为*上下文学习*。它的简单之处在于将几个示例纳入提示中，以便为模型提供更清晰的任务描述。

这些示例不仅要**与任务相关**，还要**多样化**，以涵盖数据的多样性。对于少量样本学习，使用 CoT 可能会有些挑战，特别是当你的流程有很多步骤或输入较长时。然而，通常来说，结果是值得这些努力的。另外，请记住，标记少量示例的成本远低于在传统机器学习模型开发中标记整个训练/测试集的成本。

如果我们在提示中添加一个示例，它将更好地理解要求。例如，如果我们表明我们希望最终总结以要点形式呈现，模型将会按照我们的模板进行回应。

这个提示可能会让人感到有些压倒，但不要害怕：它只是一个以前的提示（v5）和一个标记的示例，采用 `For example: 'input description' -> 'output JSON'` 格式。

![](../Images/2be54202a408a5d5282c61effc8f4fe8.png)

包含一个示例的提示 v6 输出。图像由作者使用[ChatGPT](https://chat.openai.com/chat)创建

## 总结最佳实践

总结提示工程的最佳实践，请考虑以下几点：

+   **不要害怕实验**。尝试不同的方法，逐步迭代，纠正模型并一次进行小的步骤；

+   在输入中**使用分隔符**（例如 <>），并要求**结构化输出**（例如 JSON）；

+   **提供完成任务的行动列表**。尽可能地，向模型提供一组行动，并让它输出“内部想法”；

+   如果输出较短，**请求多个建议**；

+   **提供示例**。如果可能，向模型展示几个代表数据的多样化示例，并显示期望的输出。

我认为这个框架为自动化各种日常任务提供了足够的基础，例如信息提取、总结、文本生成（如电子邮件）等。然而，在生产环境中，仍然可以通过在特定数据集上[微调模型](https://magazine.sebastianraschka.com/p/finetuning-large-language-models)来进一步优化性能。此外，[插件](https://openai.com/blog/chatgpt-plugins)和[代理](https://www.pinecone.io/learn/langchain-agents/)的快速发展也值得关注，但那是完全不同的话题。

## DeepLearning.AI 和 OpenAI 的提示工程课程

除了之前提到的[Andrej Karpathy 的讲座](https://build.microsoft.com/en-US/sessions/db3f4859-cd30-4445-a0cd-553c3304f8e2)，这篇博客文章还从[DeepLearning.AI 和 OpenAI 的 ChatGPT 提示工程课程](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/)中汲取了灵感。该课程完全免费，完成仅需几小时，并且我个人非常喜欢的一点是，它允许你在无需注册的情况下实验 OpenAI API！

这是一个很好的实验场地，所以一定要去看看。

# 使用 OpenAI API 和 Streamlit 构建基于 LLM 的应用程序

哇，我们涵盖了很多信息！现在，让我们继续前进，开始使用我们获得的知识构建应用程序吧。

## 生成 OpenAI 密钥

要开始使用，你需要注册一个 OpenAI 账户并创建你的 API 密钥。[OpenAI 目前为每个人提供**3 个月的 $5 免费信用额**](https://openai.com/pricing#:~:text=Simple%20and%20flexible-,Start%20for%20free,-Start%20experimenting%20with)。请参考[OpenAI API 入门介绍](https://platform.openai.com/docs/api-reference/introduction)页面来注册你的账户和[生成你的 API 密钥](https://platform.openai.com/docs/api-reference/introduction)。

一旦你有了密钥，创建一个`OPENAI_API_KEY` [环境变量](https://www.twilio.com/blog/how-to-set-environment-variables-html)以便通过`os.getenv('OPENAI_API_KEY')`在代码中访问它。

## 使用标记器实验室估算成本

在这个阶段，你可能会好奇仅凭免费试用可以做多少事情，以及初始三个月后有哪些选项。这是个很好的问题，特别是当你考虑到[LLMs 需要花费数百万美元](https://medium.com/towards-data-science/behind-the-millions-estimating-the-scale-of-large-language-models-97bd7287fb6b)！

当然，这些数百万是关于训练的。事实证明，推理请求非常实惠。虽然 GPT-4 可能被认为比较贵（尽管价格可能会下降），但`gpt-3.5-turbo`（默认 ChatGPT 背后的模型）仍然足以应对大多数任务。实际上，考虑到这些模型的原始参数量以亿计，OpenAI 做了一个令人难以置信的工程工作，因为这些模型现在既便宜又快速。

`gpt-3.5-turbo` 模型的费用是每 1,000 个标记 $0.002。

那么，具体多少钱呢？让我们看看。首先，我们需要了解什么是标记。简单来说，标记指的是单词的一部分。在英语中，你可以预期**每 10 个单词大约有 14 个标记**。

为了更准确地估算你特定任务和提示的标记数量，最好的方法是亲自尝试！幸运的是，OpenAI 提供了一个[标记器实验室](https://platform.openai.com/tokenizer)来帮助你。

## 附注：不同语言的标记化

由于英语在互联网的广泛使用，这种语言的标记化效果最好。正如在[“所有语言的标记化并不相等”](https://blog.yenniejun.com/p/all-languages-are-not-created-tokenized)博客文章中强调的那样，标记化在不同语言中并不统一，某些语言可能需要更多的标记来表示。如果你想构建一个涉及多语言提示的应用程序，比如翻译，请记住这一点。

为了说明这一点，我们来看看不同语言中 [全句](https://clagnut.com/blog/2380) 的分词情况。在这个玩具示例中，英语需要 9 个标记，法语 — 12，保加利亚语 — 59，日语 — 72，俄语 — 73。

![](../Images/ff25f16bfa9d4e539d0b727755bced5c.png)

不同语言的分词。截图来自 [OpenAI 分词器游乐场](https://platform.openai.com/tokenizer)

## 成本与性能

正如你可能注意到的那样，提示可能会变得相当长，尤其是在包含示例时。通过增加提示的长度，我们可能会提高质量，但同时随着使用更多标记，成本也会增加。

我们最新的提示（v6）大约由 1.5k 个标记组成。

![](../Images/0d86ff60b17ea35cb57608a7b0e76e0a.png)

提示的分词 v6。截图来自 [OpenAI 分词器游乐场](https://platform.openai.com/tokenizer)

考虑到输出长度通常与输入长度相同，我们可以估计每次请求的平均标记数约为 3k 个（*输入标记 + 输出标记*）。通过将这个数字乘以初始费用，我们发现**每次请求约为 0.006 美元或 0.6 美分**，这相当实惠。

即使我们考虑到每次请求稍高的费用为1美分（相当于大约5k个标记），你仍然可以以**仅需1美元进行100次请求**。此外，OpenAI 提供了[设置软限制和硬限制](https://openai.com/pricing#:~:text=How%20can%20I%20manage%20my%20spending%3F)的灵活性。软限制会在你接近定义的限制时发出通知，而硬限制则会限制你超出指定阈值。

> 对于本地使用的 LLM 应用程序，你可以舒适地配置每月 1 美元的硬限制，确保在预算范围内享受模型的好处。

## Streamlit 应用模板

现在，让我们构建一个网页界面，以编程方式与模型互动，消除每次手动复制提示的需要。我们将使用 [Streamlit](https://streamlit.io/) 来完成这一任务。

Streamlit 是一个 Python 库，它允许你创建简单的网页界面，无需使用 HTML、CSS 和 JavaScript。它对初学者友好，并允许使用最少的 Python 知识创建基于浏览器的应用程序。现在我们来创建一个简单的模板，用于基于 LLM 的应用程序。

首先，我们需要处理与 OpenAI API 通信的逻辑。在下面的示例中，我假设 `generate_prompt()` 函数已定义并返回给定输入文本的提示（例如，类似于你之前看到的）。

就这样！了解更多关于不同参数的信息，请参考 [OpenAI 的文档](https://platform.openai.com/docs/api-reference/chat/create#:~:text=given%20chat%20conversation.-,Request%20body,-model)，但默认设置已经很好。

有了这些代码，我们可以设计一个简单的网页应用程序。我们需要一个输入文本的字段，一个处理文本的按钮，以及几个输出小部件。我更倾向于访问完整的模型提示和输出，以便进行调试和探索原因。

整个应用程序的代码大致如下，可以在[这个 GitHub 仓库](https://github.com/Winston-503/streamlit_app_template)中找到。由于共享 OpenAI 密钥不是一个好主意，我添加了一个名为`toy_ask_chatgpt()`的占位符函数。目前，这个应用程序只是将提示复制到输出中。

如果不定义函数和占位符，这只有大约50行代码！

幸好[Streamlit 最近的更新现在允许嵌入它](https://docs.streamlit.io/streamlit-community-cloud/get-started/embed-your-app)到这篇文章中！所以你应该能够在下方看到它。

现在你可以看到这有多么简单。如果你愿意，你可以[使用 Streamlit Cloud 部署你的应用程序](https://docs.streamlit.io/streamlit-community-cloud/get-started/deploy-an-app)。但要小心，因为如果你在其中放置你的 API 密钥，每个请求都会花费你金钱！

# 结论

在这篇博客文章中，我列出了几种提示工程的最佳实践。我们讨论了迭代提示开发、使用分隔符、请求结构化输出、思维链推理以及少量示例学习。我还提供了一个模板，用于在不到100行代码的情况下使用 Streamlit 构建一个简单的网页应用程序。现在，轮到你来提出一个令人兴奋的项目创意并将其变为现实了！

现代工具允许我们在仅仅几小时内创建复杂的应用程序，这真是令人惊叹。即使没有丰富的编程知识、Python 熟练度或对机器学习的深刻理解，你也可以快速构建一些有用的东西并自动化一些任务。

如果你是初学者并且想创建类似的项目，请随时向我提问。我将非常乐意协助你，并尽快回复。祝你的项目好运！

# 资源

这里是我关于 LLM 的其他文章，希望对你有所帮助。我已经涵盖了：

+   [估算大语言模型的规模](/behind-the-millions-estimating-the-scale-of-large-language-models-97bd7287fb6b)：LLM 是什么，如何训练，它们需要多少数据和计算资源；

+   [使用 ChatGPT 进行调试](/using-chatgpt-for-efficient-debugging-fc9e065b7856#da94-27cac6b3f550)：如何使用 LLM 进行调试和代码生成。

你可能还会对以下内容感兴趣：

+   免费的[Learn Prompting 课程](https://learnprompting.org/)，以深入了解提示和相关技术；

+   最近发布的[DeepLearning.AI 短期课程](https://www.deeplearning.ai/short-courses/)，用于构建 OpenAI API 应用程序

# 感谢阅读！

+   希望这些材料对你有所帮助。[在 Medium 上关注我](https://medium.com/@andimid)，以获取更多类似的文章。

+   如果你有任何问题或评论，我会很高兴收到任何反馈。可以在评论中问我，或通过[LinkedIn](https://www.linkedin.com/in/andimid/)或[Twitter](https://twitter.com/dimid_ml)联系我。

+   支持我作为作者并访问其他成千上万篇Medium文章，请通过[我的推荐链接](https://medium.com/@andimid/membership)获取Medium会员（对你没有额外费用）。
