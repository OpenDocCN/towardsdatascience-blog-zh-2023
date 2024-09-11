# 理解和减轻LLM幻觉

> 原文：[https://towardsdatascience.com/understanding-and-mitigating-llm-hallucinations-be88d31c4200?source=collection_archive---------1-----------------------#2023-10-23](https://towardsdatascience.com/understanding-and-mitigating-llm-hallucinations-be88d31c4200?source=collection_archive---------1-----------------------#2023-10-23)

## LLM 幻觉检测挑战及其在一篇重要研究论文中提出的可能解决方案。

[](https://felipe-p-adachi.medium.com/?source=post_page-----be88d31c4200--------------------------------)[![Felipe de Pontes Adachi](../Images/58c9544ae85f43548c5e5b56fda31bb4.png)](https://felipe-p-adachi.medium.com/?source=post_page-----be88d31c4200--------------------------------)[](https://towardsdatascience.com/?source=post_page-----be88d31c4200--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----be88d31c4200--------------------------------) [Felipe de Pontes Adachi](https://felipe-p-adachi.medium.com/?source=post_page-----be88d31c4200--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa038269245d5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-and-mitigating-llm-hallucinations-be88d31c4200&user=Felipe+de+Pontes+Adachi&userId=a038269245d5&source=post_page-a038269245d5----be88d31c4200---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----be88d31c4200--------------------------------) ·8 min read·Oct 23, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbe88d31c4200&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-and-mitigating-llm-hallucinations-be88d31c4200&user=Felipe+de+Pontes+Adachi&userId=a038269245d5&source=-----be88d31c4200---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbe88d31c4200&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-and-mitigating-llm-hallucinations-be88d31c4200&source=-----be88d31c4200---------------------bookmark_footer-----------)![](../Images/ada358f0bc7233d554233f71e47aa0b8.png)

[由 Enrique 提供的图片](https://pixabay.com/photos/forest-person-surreal-poisoning-7772371/)

近年来，大型语言模型（LLMs）展示了令人印象深刻且不断增强的能力，包括对用户提示生成高度流畅和令人信服的响应。然而，LLMs 以生成非事实性或荒谬陈述而闻名，这种特性通常称为“幻觉”。这种特征可能会在许多需要事实性的场景中损害信任，如总结任务、生成式问答和对话生成。

检测幻觉在人类中一直是一个挑战，在 LLM 的背景下同样如此。这尤其具有挑战性，因为我们通常无法获取用于一致性检查的真实背景信息。有关 LLM 生成的附加信息，如输出概率分布，可以帮助完成这一任务。然而，这类信息往往不可用，使得任务更加困难。

幻觉检测尚未解决，仍是一个活跃的研究领域。在这篇博客文章中，我们将一般介绍任务及其挑战，并介绍在研究论文 [**SELFCHECKGPT: Zero-Resource Black-Box Hallucination Detection for Generative Large Language Models**](https://arxiv.org/pdf/2303.08896.pdf?ref=content.whylabs.tds)[1] 中提出的一种可能的方法。我们将用实际例子说明论文中提出的一些方法，并指出每种方法的一些优缺点。你可以通过访问 [Google Colab Notebook](https://colab.research.google.com/drive/1ftgiASR3TeMaRTQ-cTd81iEoh2j0UP16?usp=sharing&ref=content.whylabs.ai) 来查看这些示例。

> *💡* ***更新****：受本博客文章研究的启发，我们在* [***LangKit***](https://github.com/whylabs/langkit)*中发布了一个新功能。* ***response_hallucination*** *模块将自动计算一致性分数，帮助你了解 LLM 中幻觉响应的存在。你可以在* [*这个示例笔记本*](https://github.com/whylabs/langkit/blob/main/langkit/examples/Response_Consistency.ipynb) *中查看它。*

# 本博客将涵盖：

+   什么是 LLM 幻觉

+   方法：SelfCheckGPT

+   一致性检查

    1\. BERTScore

    2\. 自然语言推理

    3\. LLM 提示

+   实验

+   结论

+   参考文献

# 什么是 LLM 幻觉

在自然语言生成中，幻觉可以定义为“生成的内容是无意义的或不忠于提供的源内容”[2]。源内容可以代表不同任务的不同功能。例如，对于摘要任务，源内容是被摘要的输入文本，而对于生成式问答任务，源内容被认为是世界知识[2]。我们来关注问答任务，并以用户问题和 OpenAI 的 GPT 模型（text-davinci-003）给出的答案为例：

用户提示：**菲利普·海沃斯是谁？**

LLM 响应：**菲利普·海沃斯是英国的一名律师和政治家，曾于1859年至1868年担任特特福德的国会议员。**

初看起来，这似乎是一个连贯的回答，但过一段时间，我们可能会问一个非常合理的问题：这是真的吗？

不过，我们并没有足够的信息来回答这个新问题。我们没有可以用来验证陈述的经过验证的事实的背景。LLM通常在大量文本中进行训练，因此很难验证陈述是否基于特定的信息来源。快速的Google搜索没有显示出匹配回答的人的任何信息。什么确切构成幻觉仍然没有很好地建立，但这个例子可以被理解为幻觉，因为这个名字、地点和职业的主题可能根本不存在。

让我们再试一次另一个问题：

用户提示：**比尔·盖茨是谁？**

LLM回应：**比尔·盖茨是技术企业家、投资者和慈善家。他是微软公司的共同创始人，也是世界上最富有的人之一。**

好的，这些陈述更容易验证，并且通过快速检查维基百科得到了支持，所以这不太可能是幻觉。在训练过程中，LLM很可能见过很多关于比尔·盖茨的陈述，因此“比尔·盖茨是_”之后的令牌很可能会以较高的信心生成。另一方面，LLM可能对“Philip Hayworth 是_”之后使用哪些词不太确定。这一见解使我们能够将不确定性与真实性联系起来，因为事实句子通常会包含预测概率较高的令牌，而幻觉句子则不然。然而，对于许多案例，我们可能没有手头的输出概率分布。

本次会议的示例和内容基于原始论文[1]，我们将在接下来的章节中继续探索论文的方法。

# 方法：SelfCheckGPT

在上一节中，我们考虑了我们方法的两个重要因素：访问外部背景和访问LLM的输出概率分布。当一种方法不需要外部背景或数据库来进行一致性检查时，我们可以称其为**零资源**方法。类似地，当一种方法只需要LLM生成的文本时，可以称之为**黑箱**方法。

我们在这篇博客文章中要讨论的方法是一种零资源黑箱幻觉检测方法，基于这样一个前提：**对相同提示的采样回答对于幻觉事实可能会出现分歧和矛盾，而对于事实陈述则可能会相似和一致**。

让我们重新审视之前的例子。为了应用检测方法，我们需要更多的样本，所以让我们再向LLM提出三个相同的问题：

![](../Images/6a353b00255ca73e9f5e6b8a22b0adbe.png)

作者提供的表格

确实，答案相互矛盾——有时，Philip Hayworth 是一位英国政治家，而在其他样本中，他是澳大利亚工程师或美国律师，他们生活和行动于不同的时期。

让我们以比尔·盖茨的例子进行比较：

![](../Images/3c9d9bc9e5e6b6c8712ed29f0cf3eb18.png)

表格作者提供

我们可以观察到，比尔·盖茨分配的职业、组织和特征在样本之间是一致的，使用了相等或语义相似的术语。

# 一致性检查

现在我们有了多个样本，最后一步是进行一致性检查——确定答案是否彼此一致。这可以通过多种方式完成，所以让我们探索一下论文中提出的一些方法。你可以通过查看这个 [Google Colab Notebook](https://colab.research.google.com/drive/1ftgiASR3TeMaRTQ-cTd81iEoh2j0UP16?usp=sharing&ref=content.whylabs.tds) 自行执行代码。

# BERTScore

执行此检查的一种直观方法是测量样本之间的语义相似度，而 BERTScore[3] 是一种实现方式。BERTScore 为候选句子中的每个词与参考句子中的每个词计算相似度分数，以计算句子之间的相似度分数。

在 SelfCheckGPT 的背景下，分数是逐句计算的。原始答案的每个句子将与给定样本的每个句子进行评分，以找到最相似的句子。这些最大相似度分数将在所有样本中进行平均，从而为原始答案中的每个句子得到最终的幻觉分数。最终分数需要趋近于 1（表示不相似的句子）和 0（表示相似的句子），因此我们需要从 1 中减去相似度分数。

让我们展示如何用原始答案的第一个句子与第一个样本进行检查：

![](../Images/00483d188ef30cfd8e58a5c812887998.png)

图片作者提供

第一个样本的最高分是 0.69。重复对剩余两个样本的处理，并假设其他最高分为 0.72 和 0.72，那么我们对该句子的最终分数将是 **1 — (0.69+0.72+0.72)/3 = 0.29**。

使用语义相似度来验证一致性是一种直观的方法。其他编码器也可以用于嵌入表示，因此这也是一种可以进一步探索的方法。

# 自然语言推理

自然语言推理是确定蕴涵的任务，即根据前提[4]判断一个假设是否为真、假或未确定。在我们的案例中，每个样本用作前提，每个原始答案的句子用作我们的假设。通过对每个句子的样本分数进行平均，得到最终分数。蕴涵通过对 Multi-NLI 数据集[5] 进行微调的 Deberta 模型来执行。我们将使用归一化预测概率来计算分数，而不是实际类别，如“蕴涵”或“矛盾”。[6]

蕴涵任务更接近我们的一致性检查目标，因此我们可以期待为此目的微调的模型会表现良好。作者还在 [HuggingFace](https://huggingface.co/potsawee/deberta-v3-large-mnli?ref=content.whylabs.tds) 上公开分享了该模型，其他 NLI 模型也公开可用，使得这种方法非常容易获取。

# LLM Prompt

考虑到我们已经使用 LLM 来生成答案和样本，我们不妨使用 LLM 来执行一致性检查。我们可以对每个原始句子和每个样本进行一致性检查，将 LLM 作为我们的上下文。下面的图片，来自原始论文的仓库，说明了如何进行这个操作：

![](../Images/d0c3428d1c9cb81ab76b5d1a1b75ec9b.png)

**SELFCHECKGPT WITH LLM PROMPT. 来源：** [**HTTPS://GITHUB.COM/POTSAWEE/SELFCHECKGPT/TREE/MAIN**](https://github.com/potsawee/selfcheckgpt/tree/main?ref=content.whylabs.ai)

最终得分可以通过将“否”赋值为 1，“是”赋值为 0，“不适用”赋值为 0.5，并对样本的值进行平均来计算。

与其他两种方法不同，这种方法需要额外调用你选择的 LLM，这意味着额外的延迟和可能的额外成本。另一方面，我们可以利用 LLM 的能力来帮助我们进行检查。

# 实验

让我们看看在三种方法中讨论的两个示例的结果如何。

![](../Images/bbb277288b2562da66001aba60dd5e1d.png)

作者提供的表格

这些值仅用于说明方法。只有三个句子的情况下，它不应该用来比较和确定哪种方法最佳。为此，原始论文在论文的仓库中分享了实验结果 [这里](https://github.com/potsawee/selfcheckgpt/tree/main?ref=content.whylabs.ai#experimental-results)，包括了在这篇博客中未讨论的附加版本。我不会详细讨论结果，但根据所有三个指标（NonFact、Factual 和 Ranking），LLM-Prompt 是表现最好的版本，其次是 NLI 版本。BERTScore 版本明显比剩余两个版本要差。我们的简单示例似乎符合共享结果的方向。

# 结论

我们希望这篇博客文章有助于解释幻觉问题，并提供一种可能的幻觉检测解决方案。这是一个相对较新的问题，很高兴看到已经有努力在解决它。

讨论的方法具有不需要外部上下文（零资源）和不需要LLM的输出概率分布（黑箱）的优点。然而，这也带来了成本：除了原始响应外，我们还需要生成额外的样本来执行一致性检查，从而增加了延迟和成本。一致性检查还需要额外的计算和语言模型来将响应编码为嵌入，进行文本蕴含，或查询LLM，这取决于所选的方法。

# 参考文献

[1] — Manakul, Potsawee, Adian Liusie, 和 Mark JF Gales。“Selfcheckgpt：用于生成大型语言模型的零资源黑箱幻觉检测。” arXiv预印本 arXiv:2303.08896 (2023)。

[2] — JI, Ziwei 等人。《自然语言生成中的幻觉调查》。**ACM计算调查**，第55卷，第12期，页码1–38，2023年。

[3] — ZHANG, Tianyi 等人。Bertscore：使用bert评估文本生成。**arXiv预印本 arXiv:1904.09675**，2019年。

[4] — [https://nlpprogress.com/english/natural_language_inference.html](https://nlpprogress.com/english/natural_language_inference.html?ref=content.whylabs.ai)

[5] — Williams, A., Nangia, N., & Bowman, S. R. (2017)。用于通过推理理解句子的广泛覆盖挑战语料库。arXiv预印本 arXiv:1704.05426。

[6] — [https://github.com/potsawee/selfcheckgpt/tree/main#selfcheckgpt-usage-nli](https://github.com/potsawee/selfcheckgpt/tree/main#selfcheckgpt-usage-nli)
