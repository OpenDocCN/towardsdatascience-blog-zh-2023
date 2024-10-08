# 提示工程指南

> 原文：[`towardsdatascience.com/prompt-engineering-guide-for-data-analysts-54f480ba4d98`](https://towardsdatascience.com/prompt-engineering-guide-for-data-analysts-54f480ba4d98)

![](img/7eb4d92c8f54e60146f7dd4ba68cb20b.png)

图片由[Emiliano Vittoriosi](https://unsplash.com/@emilianovittoriosi?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 原则、技术和应用，以作为数据分析师利用 LLM 中的提示力量

[](https://tanuwidjajaolivia.medium.com/?source=post_page-----54f480ba4d98--------------------------------)![Olivia Tanuwidjaja](https://tanuwidjajaolivia.medium.com/?source=post_page-----54f480ba4d98--------------------------------)[](https://towardsdatascience.com/?source=post_page-----54f480ba4d98--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----54f480ba4d98--------------------------------) [Olivia Tanuwidjaja](https://tanuwidjajaolivia.medium.com/?source=post_page-----54f480ba4d98--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----54f480ba4d98--------------------------------) ·阅读时间 7 分钟·2023 年 5 月 7 日

--

[大型语言模型 (LLM)](https://www.youtube.com/watch?v=iR2O2GPbB0E) 的崛起得益于[OpenAI 的 ChatGPT](https://openai.com/blog/chatgpt)的流行，这一技术引发了互联网的轰动。作为数据领域的从业者，我寻找最佳方法来在工作中利用这一技术，特别是在数据分析师的富有洞察力却实用的工作中。

LLM 可以通过“提示”技术解决任务，而无需额外的模型训练，其中**问题以文本提示的形式呈现给模型**。获取“**正确的提示**”对于确保模型为分配的任务提供高质量和准确的结果非常重要。

在这篇文章中，我将分享提示的原则、构建提示的技巧，以及数据分析师在这个“提示时代”中可以发挥的角色。

# 什么是提示工程？

引用[Ben Lorica from Gradient Flow](https://gradientflow.com/the-future-of-prompt-engineering-getting-the-most-out-of-llms/)， “**提示工程是设计有效输入提示以引出基础模型期望输出的艺术**。” 这是一个迭代过程，通过开发能够有效利用现有生成型 AI 模型的提示来实现特定目标。

提示工程技能可以帮助我们理解大型语言模型的能力和局限性。**提示本身作为模型的输入，这影响了模型的输出。** 一个好的提示将使模型产生期望的输出，而从一个不好的提示开始逐步工作将帮助我们了解模型的局限性及如何与之合作。

Isa Fulford 和 Andrew Ng 在 [ChatGPT 提示工程开发者课程](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/) 中提到了提示的两个主要原则：

+   原则 1：**编写清晰而具体的指令**

+   原则 2：**给模型时间“思考”**

> 我认为提示*就像是给一个天真的“机器小孩”下指令*。

孩子非常聪明，但你需要**清楚你需要什么**（通过提供解释、示例、指定的输出格式等）并且**给予他一些时间来消化和处理**（指定解决问题的步骤，要求他慢慢处理）。孩子在得到相应的暴露之后，也可以在提供答案时表现出*非常有创意和想象力*——这被称为[LLM 的幻觉](https://thestack.technology/the-big-hallucination-large-language-models-consciousness/)。理解上下文并提供正确的提示可能有助于避免这个问题。

# 提示工程技术

提示工程是一个不断发展的领域，自 2022 年以来，这方面的研究迅速增加。一些常用的最先进的提示技术包括 n-shot 提示、思维链（CoT）提示和生成知识提示。

*一个演示这些技术的 Python 笔记本* *可以在* [*这个 GitHub 项目中找到。*](https://github.com/oliviatan29/prompt-engineering)

## **1\. N-shot 提示（Zero-shot 提示、Few-shot 提示）**

N-shot 提示以其变体如 Zero-shot 提示和 Few-shot 提示而闻名，其中 N 代表给模型的“训练”或提示的数量，以便进行预测。

**Zero-shot 提示** 是模型在**没有任何额外训练的情况下**进行预测。这适用于分类（即情感分析、垃圾邮件分类）、文本转换（即翻译、总结、扩展）以及 LLM 已经广泛训练的简单文本生成。

![](img/506a7bd75f299d281a79c711e7f63a67.png)

零-shot 提示：直接询问模型关于情感的问题（图像作者提供）

**Few-shot 提示** 使用**少量数据**（通常在两个到五个之间）**来调整其输出**，基于这些小示例。这些示例旨在引导模型在更具上下文特定的问题上表现更好。

![](img/b9d3dfc9576322375ef78cec97c31c4f.png)

Few-shot 提示：提供我们期望模型输出的示例

## 2\. 思维链（CoT）提示

[思维链提示](https://ai.googleblog.com/2022/05/language-models-perform-reasoning-via.html)是谷歌研究人员在 2022 年提出的。在*思维链提示*中，模型被提示**在给出最终答案之前生成中间推理步骤**，以应对多步骤问题。这个理念是，模型生成的思维链会模拟处理多步骤推理问题时的直观思维过程。

![](img/03c679acfed8e015cadabbfde18c7164.png)

思维链提示有助于驱动模型相应地分解问题。

这种方法使模型能够将多步骤问题分解为中间步骤，从而解决那些无法通过标准提示方法解决的复杂推理问题。

思维链提示的进一步变体包括：

+   [自一致性提示](https://arxiv.org/pdf/2203.11171.pdf)：采样多个不同的推理路径，并**选择最一致的答案**。通过利用多数投票系统，模型可以得到更准确和可靠的答案。

+   [从少到多提示 (LtM)](https://arxiv.org/abs/2205.10625)：指定思维链以首先将问题分解为一系列更简单的子问题，然后**按顺序解决它们**。解决每个子问题的过程得益于之前解决的子问题的答案。这一技术受到儿童教育策略的启发。

+   [主动提示](https://arxiv.org/abs/2302.12246)：**扩展 CoT 方法**，通过确定哪些问题对人工注释最重要和最有帮助。它首先计算 LLM 预测中的不确定性，然后选择最不确定的问题，这些问题会被选择进行人工注释，然后再放入 CoT 提示中。

## 3\. 生成知识提示

[生成知识提示](https://arxiv.org/pdf/2110.08387.pdf)的理念是让 LLM**生成潜在有用的信息**，然后**利用这些提供的知识**作为生成最终回应的额外输入。

比如说，你想写一篇关于网络安全的文章，特别是关于 cookie 窃取的内容。在要求 LLM 编写文章之前，你可以先让它生成一些关于 cookie 窃取的危险性和保护措施。这将帮助 LLM 写出更具信息性的博客文章。

![](img/63b735ddbcebc6a4f71b74db8974c0a1.png)

生成知识提示： (1) 让模型生成一些内容

![](img/ffc1680c472203be05e038dde1a2beb5.png)

生成知识提示： (2) 将生成的内容作为模型的输入

## 其他策略

除了上述指定的技术外，你还可以使用以下策略来提高提示的效果：

+   **使用分隔符** 如三重反引号（```）、尖括号（<>）或标签（<tag> </tag>）来指示输入的不同部分，使其在调试时更清晰，并避免提示注入。

+   **要求结构化输出**（即 HTML/JSON 格式），这对于将模型输出用于其他机器处理非常有用。

+   指定 **预期的文本语气** 以获得所需的模型输出的语气、格式和长度。例如，你可以指示模型将语言正式化，生成不超过 50 个字等。

+   修改模型的 **温度参数** 以调整模型的随机性。温度越高，模型输出会越随机而不准确，甚至可能产生幻觉。

*一个展示这些技术的 Python 示例笔记本* *在* [*这个 GitHub 项目中共享。*](https://github.com/oliviatan29/prompt-engineering)

![](img/e6afbdbdd508428b09ef83b6f52eb01d.png)

照片由 [Camylla Battani](https://unsplash.com/@camylla93?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 分析师在提示工程中的角色

正如你从上述示例中可能推断出的那样，提示工程需要非常具体的技术沟通技巧。虽然你仍然需要业务背景和问题解决技能，但这仍然是一种新的工艺，并不完全包含在 [传统数据分析技能集](https://medium.com/towards-data-science/how-does-a-remarkable-data-analyst-look-like-dd0e4326c670)中。

数据分析师可以利用他们的上下文知识、问题解决技能以及统计/技术能力，再加上有效的沟通来进行提示工程。这些是与提示工程（和 LLM）相关的关键任务，可能由分析师完成：

+   **指定要解决的 LLM 问题**。了解 LLM 概念后，我们可以定义模型要执行的操作（即是文本分类、生成还是转换问题）以及要作为提示的正确问题和参考点。

+   **迭代提示**。在开发数据模型时，我们通常会经历一个迭代过程。在构建初始模型后，我们评估结果，进行优化，并在过程中重新尝试。对于提示也是如此，我们分析结果与期望不符的地方，并通过更清晰的指令、额外的示例或特定的步骤进行优化。这需要批判性思维，而大多数数据分析师已经擅长这一点。

+   **提示版本控制和管理**。通过迭代提示，你将得到大量的提示尝试和识别出的模型能力和/或限制。重要的是记录这些发现以供团队学习和持续改进，就像其他现有的数据分析一样。

+   **设计安全提示**。尽管表现出令人印象深刻的能力，LLM 仍处于非常早期的阶段，容易出现漏洞和局限性。存在[幻觉问题](https://thestack.technology/the-big-hallucination-large-language-models-consciousness/)，模型提供高度误导性的信息，以及[提示注入](https://simonwillison.net/2023/Apr/14/worst-that-can-happen/)的风险，其中不可信的文本作为提示的一部分使用。根据模型和提示的使用情况，分析师可以建议编程保护措施，以限制提示的使用和问题提示的检测。

除了利用现有技能外，分析师还需要**磨练沟通技巧和分解问题的能力**，以提供更好的提示。

# 结论

大型语言模型在执行各种语言任务方面显示出了令人鼓舞的结果，而**提示工程是解锁这些能力的关键**。提示工程是与 AI 有效沟通以实现预期结果的过程。

可以使用几种技术进行提示工程，但基本原则是一致的。这涉及向模型提供明确的指令，并帮助其理解和处理这些指令。**数据分析师可以利用他们的背景知识和解决问题的技能来框定正确的提示，并利用他们的技术能力来设计提示保护措施**。

欲获取更多有关提示工程的资源，请查看：

+   [ChatGPT 提示工程课程](https://learn.deeplearning.ai/chatgpt-prompt-eng)（DeepLearning.AI 课程）

+   [LearnPrompting.org](https://learnprompting.org/docs/intro)

+   [PromptingGuide.ai](https://www.promptingguide.ai/)

+   [OpenAI 应用文档](https://platform.openai.com/examples)

+   [提示工程的未来：最大限度地发挥 LLM 的潜力](https://gradientflow.com/the-future-of-prompt-engineering-getting-the-most-out-of-llms/)（Gradient Flow 文章）

我相信这个领域在接下来的几年中会进一步发展，我很兴奋能够见证并参与这场演变。
