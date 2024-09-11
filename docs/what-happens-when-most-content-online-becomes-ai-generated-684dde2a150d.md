# 当大多数在线内容变成AI生成时会发生什么？

> 原文：[https://towardsdatascience.com/what-happens-when-most-content-online-becomes-ai-generated-684dde2a150d?source=collection_archive---------9-----------------------#2023-10-17](https://towardsdatascience.com/what-happens-when-most-content-online-becomes-ai-generated-684dde2a150d?source=collection_archive---------9-----------------------#2023-10-17)

## 了解生成模型在训练数据时如何退化，以及如何应对这一问题

[](https://medium.com/@aichabokbot?source=post_page-----684dde2a150d--------------------------------)[![Aicha Bokbot](../Images/1aa9ba6ae6296d8be3350b14dba97dd2.png)](https://medium.com/@aichabokbot?source=post_page-----684dde2a150d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----684dde2a150d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----684dde2a150d--------------------------------) [Aicha Bokbot](https://medium.com/@aichabokbot?source=post_page-----684dde2a150d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F50566ce7e21&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-happens-when-most-content-online-becomes-ai-generated-684dde2a150d&user=Aicha+Bokbot&userId=50566ce7e21&source=post_page-50566ce7e21----684dde2a150d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----684dde2a150d--------------------------------) ·6 min read·2023年10月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F684dde2a150d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-happens-when-most-content-online-becomes-ai-generated-684dde2a150d&user=Aicha+Bokbot&userId=50566ce7e21&source=-----684dde2a150d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F684dde2a150d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-happens-when-most-content-online-becomes-ai-generated-684dde2a150d&source=-----684dde2a150d---------------------bookmark_footer-----------)![](../Images/0f749c7a7ddaf8046a779a545072100f.png)

Spiral — 图片来源于 [Ludde Lorentz](https://unsplash.com/@luddelorentz?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 引言

最近，生成性AI的突破带来了可以生成高度逼真和复杂的文本、图像和声音的公开AI模型，这些模型正在彻底改变内容创作。

这些模型是在从互联网抓取的大型数据集上训练的。例如，在文本数据的情况下，像ChatGPT这样的高级语言模型（LLMs）主要是在在线找到的人类生成文本上进行训练的。

生成模型已经在社会上获得了广泛的声誉和快速的采纳，以至于越来越多的AI生成内容出现在互联网上，而这正是它们训练数据的主要来源。

我们在这里看到一个循环：生成模型将不可避免地在由生成AI而非人类生成的合成数据上进行训练。这就引出了一个问题：如果发生这种情况，模型会有什么表现？

# 生成模型与退化

> 一旦大型语言模型（LLMs）贡献了大量在线语言内容，GPT-{n}会发生什么？

这是一个由一组研究人员提出的问题，他们发表了论文 [*递归的诅咒：在生成的数据上训练使模型遗忘*](https://browse.arxiv.org/pdf/2305.17493v2.pdf?)（I. Shumailov等，2023）。

作者们通过让模型在生成的数据上进行多次迭代学习进行实验。他们将此应用于高斯混合模型（GMMs）、变分自编码器（VAE）和大型语言模型（LLMs）。对于这三种模型，这导致了他们称之为“*模型崩溃*”的现象：

> 一种退化过程，其中，随着时间的推移，即使在分布未发生变化的情况下，模型也会遗忘真实的基础数据分布。

*模型崩溃* 描述了模型遗忘真实分布的尾部（即不太可能、较少见但重要的事件）并过度代表分布中心的行为。随着这一过程的重复，模型会收敛到一个与原始分布相似度很低的分布。

在论文 [*自我消耗的生成模型会变得MAD*](https://arxiv.org/abs/2307.01850)（S. Alemohammad等，2023）中，其他研究人员类似地将这种逐渐远离原始分布的现象描述为“*模型自噬紊乱*”（MAD）：

> […] 如果每次生成中没有足够的新鲜真实数据，未来的生成模型注定会陷入模型自噬紊乱（MAD），即它们的质量（以精确度衡量）或多样性（以召回率衡量）将逐渐退化。

结论很明确：当生成模型主要在它们生成的内容上进行训练时，它们往往会退化。考虑到它们的训练数据主要来自互联网，这种退化过程代表了多大的风险？

# 当人类生成的内容变得稀缺时

如果人类生成的内容与AI生成的内容的比例保持在目前的水平，并且在线内容中人类生成的内容占据明显多数，那么情况不应令人担忧，生成模型的表现也不应出现前述的退化问题。

然而，关于由 AI 生成的内容主导互联网的前景，确实存在合理的担忧。如果我们关注文本生成，支持这种预测的主要论点是 LLMs，如 GPT 提供的便利。这些工具在写作中解决了真正的痛点：找到合适的词汇、改善风格、获得灵感。那些尝试过这些工具的人可以证明，依赖这些工具是多么具有诱惑力和成瘾性：既然我们可以轻松地提示 LLM 生成在风格和时间效率上都更好的内容，为什么要挣扎呢？

如果人类生成的内容变得不那么普遍，我们可以采取什么措施来减轻将生成模型暴露于 AI 生成内容的风险？一种可能的方法是通过鼓励没有 AI 工具的内容创作来对抗这种趋势。另一种方法涉及开发检测 AI 生成数据的方法，并在模型训练过程中将其筛选出去。

第一个方法的想法是找到激励创作者减少使用生成模型的方式。然而，这种方法的可行性可能有限。即使我们能找到这样的激励措施（例如奖励或惩罚），挑战在于验证一段内容是否真正是人类生成的。这将我们引向第二种方法，这种方法也依赖于区分人类生成和 AI 生成数据的能力。

# 如何检测 AI 生成的数据

在 2023 年，公司 AI21 Labs 领导了迄今为止最大的图灵测试：超过 150 万用户与人类或 AI 聊天机器人进行在线聊天，并被要求猜测他们与谁对话。这一任务被证明并非易事，因为 68% 的人猜对了，显示了生成型 AI 能多么好地模仿人类。

有鉴于此，让我们回顾一些识别 AI 生成数据的可用技术。

## 水印

文献中讨论的主要方法来区分人类生成的数据和 AI 生成的数据是*水印*。水印是将隐藏的信号添加到数据中，这些信号对人类不可见但算法能够检测到。

水印将使检测 AI 生成的数据变得容易，但前提是生成型 AI 提供者将其添加到他们的产品中。目前情况并非如此，并且可能过于乐观地期望近期能实现。

在他的公开信[《水印不可行》](https://www.deeplearning.ai/the-batch/watermarking-is-a-no-go/?ref=dl-staging-website.ghost.io)中，Andrew Ng 对水印的采用持悲观态度。他解释了在当前的生成 AI 竞赛中，水印如何看起来像是 AI 公司的一种竞争劣势。

## 使用 ML 分类器

一种替代水印的方法是训练一个机器学习分类器，让其学习标记内容是 AI 生成还是人类生成。

执行此任务的系统已经存在，例如 [GPTzero](https://gptzero.me/?ref=the-batch-deeplearning-ai)。然而，这些系统显示出较高的错误率，并且未能提供可靠的解决方案。即使是机器学习模型，检测AI生成的数据也证明是一项困难的任务。

## 一种零样本方法

论文 [DetectGPT: Zero-Shot Machine-Generated Text Detection using Probability Curvature](https://arxiv.org/abs/2301.11305)（E. Mitchell等，2023）中讨论的另一种方法不需要任何训练，基于观察LLMs对数概率函数的曲率。

作者发现的标准是，AI生成的文本往往倾向于占据具有负对数概率曲率的区域，而不是人类撰写的文本。

这似乎是一种有前景的技术，尽管我们可能对其可扩展性存有疑虑：它不是模型无关的（它检测内容是否由特定的LLM生成，而不是任何给定的LLM），并且需要访问模型的对数概率。

# 结论

我们了解到，训练生成模型时使用人类生成的数据非常重要，以避免性能下降。这带来了一个重大挑战，特别是因为互联网是它们主要的训练数据来源，并且越来越多的在线内容是AI生成的。

有两种方法可以解决这个问题：通过推动人类内容创作来扭转趋势，以及防止模型在合成数据上进行训练。然而，这两种解决方案都引发了复杂的开放性问题。我们如何激励在线内容创作者减少对AI的依赖？我们如何建立可靠、可扩展的AI生成内容检测方法？

令人鼓舞的是，白宫在2023年7月做出了一个重要的 [声明](https://www.whitehouse.gov/briefing-room/statements-releases/2023/07/21/fact-sheet-biden-harris-administration-secures-voluntary-commitments-from-leading-artificial-intelligence-companies-to-manage-the-risks-posed-by-ai/?utm_campaign=The+Batch&utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz-_KWnjJMok4p7lxxBJgIa1f-qA07jSWCt3GXAPWGm9rOoVgsUvfcPNMlM5GrOOOH3knocHJ)。七家主要的AI公司——亚马逊、Anthropic、谷歌、Inflection、Meta、微软和OpenAI——同意尊重一系列负责任的AI承诺，其中包括水印。未来将揭示这些承诺是否得到遵守。

## 进一步阅读

+   [AI21 Labs的发布](https://www.ai21.com/blog/human-or-not-results)他们分析了“Human or Not？”实验的结果并解释了他们的方法。

+   [人工通用智能的火花：对GPT-4的早期实验](https://arxiv.org/abs/2303.12712)，这是微软研究团队对GPT-4的深入调查和对AGI的有趣讨论。

+   [水印技术无望](https://www.deeplearning.ai/the-batch/watermarking-is-a-no-go/?ref=dl-staging-website.ghost.io)，这是Andrew Ng的一封信，讨论了金融“市场竞争中的激励措施如何使得[水印技术]的采用变得具有挑战性”。
