# 针对 LLM 的思维链提示

> 原文：[https://towardsdatascience.com/chain-of-thought-prompting-for-llms-33c963eead38?source=collection_archive---------4-----------------------#2023-07-24](https://towardsdatascience.com/chain-of-thought-prompting-for-llms-33c963eead38?source=collection_archive---------4-----------------------#2023-07-24)

## 一种“推理”LLM 的实用简单方法

[](https://wolfecameron.medium.com/?source=post_page-----33c963eead38--------------------------------)[![Cameron R. Wolfe, Ph.D.](../Images/52bb88d7cf1105501be2fae5ccbe7a03.png)](https://wolfecameron.medium.com/?source=post_page-----33c963eead38--------------------------------)[](https://towardsdatascience.com/?source=post_page-----33c963eead38--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----33c963eead38--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----33c963eead38--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchain-of-thought-prompting-for-llms-33c963eead38&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----33c963eead38---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----33c963eead38--------------------------------) ·16分钟阅读·2023年7月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F33c963eead38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchain-of-thought-prompting-for-llms-33c963eead38&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----33c963eead38---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F33c963eead38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchain-of-thought-prompting-for-llms-33c963eead38&source=-----33c963eead38---------------------bookmark_footer-----------)![](../Images/29d7dcc5bce6b373e4e07c854001e445.png)

（照片由 [Matthew Lancaster](https://unsplash.com/@matthewelancaster?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/chain?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)）

大型语言模型（LLMs）的成功源于我们能够在大规模文本语料库上进行预训练（使用 [语言建模目标](https://cameronrwolfe.substack.com/i/85568430/language-modeling)）的 [仅解码器变换器](https://twitter.com/cwolferesearch/status/1640446111348555776?s=20) 模型。鉴于我们预训练了足够大的模型，LLMs 在 [少量学习](https://cameronrwolfe.substack.com/i/88082618/language-models-are-few-shot-learners) 中表现得非常出色。换句话说，这意味着我们可以通过制定一个文本提示（可能包含一些正确输出的示例）并让 LLM 生成正确答案，来解决各种不同的问题（例如，翻译、句子分类、总结等）。

尽管大语言模型（LLMs）具有强大的能力，但这些模型在解决某些问题时仍然存在困难。特别是，推理问题（例如，算术或常识推理）非常难以解决。最初尝试解决这一问题的方法是对 LLMs 进行微调，并利用任务特定的验证模块，在一个包含各种推理问题解决方案和解释的监督数据集上进行训练 [3, 4]。然而，最近的研究发现，少量学习可以作为更简单的解决方案。

> “本文的目标是赋予语言模型生成思维链的能力——一个连贯的中间推理步骤系列，最终得出答案……”
