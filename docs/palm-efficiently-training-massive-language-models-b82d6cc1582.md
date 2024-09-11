# PaLM：高效训练大规模语言模型

> 原文：[https://towardsdatascience.com/palm-efficiently-training-massive-language-models-b82d6cc1582?source=collection_archive---------8-----------------------#2023-06-19](https://towardsdatascience.com/palm-efficiently-training-massive-language-models-b82d6cc1582?source=collection_archive---------8-----------------------#2023-06-19)

## LLM的前所未有的规模、效率和性能

[](https://wolfecameron.medium.com/?source=post_page-----b82d6cc1582--------------------------------)[![Cameron R. Wolfe, Ph.D.](../Images/52bb88d7cf1105501be2fae5ccbe7a03.png)](https://wolfecameron.medium.com/?source=post_page-----b82d6cc1582--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b82d6cc1582--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b82d6cc1582--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----b82d6cc1582--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpalm-efficiently-training-massive-language-models-b82d6cc1582&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----b82d6cc1582---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----b82d6cc1582--------------------------------) ·17分钟阅读·2023年6月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb82d6cc1582&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpalm-efficiently-training-massive-language-models-b82d6cc1582&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----b82d6cc1582---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb82d6cc1582&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpalm-efficiently-training-massive-language-models-b82d6cc1582&source=-----b82d6cc1582---------------------bookmark_footer-----------)![](../Images/451e0862c498cfad7a6e861d29efbd84.png)

(照片由[Corey Agopian](https://unsplash.com/@corey_lyfe?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/images/nature/palm-tree?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText))

近年来，大型深度神经网络已经成为解决大多数语言理解和生成任务的首选架构。最初，提出了如 [BERT](https://cameronrwolfe.substack.com/p/language-understanding-with-bert) [2] 和 T5 [3] 等模型，它们使用了 [两阶段训练方法](https://cameronrwolfe.substack.com/i/76273144/training-bert)，即在大规模文本语料库上进行预训练（使用 [自监督“填空”目标](https://cameronrwolfe.substack.com/i/76273144/self-supervised-learning)），然后在目标数据集上进行微调；见下文。尽管这些技术非常有用，但近期关于大型语言模型（LLMs）的研究表明，大型自回归（仅解码器）变换器模型在少样本学习方面表现极其出色，在对下游任务进行最小调整的情况下取得了令人印象深刻的性能。

![](../Images/dc4d51598c9de0703ecde8cfa28d78cd.png)

(来自 [4])

LLM 的少样本学习能力最初由 [GPT-3](https://cameronrwolfe.substack.com/p/language-model-scaling-laws-and-gpt) [4] 展示，它是一个包含 1750 亿参数的 LLM。为了进行少样本预测，该模型在一个庞大的文本语料库上进行预训练（使用基本的 [语言建模目标](https://cameronrwolfe.substack.com/i/85568430/language-modeling)），然后提供任务描述和少量任务解决示例；见上文。进一步分析 LLM 表明模型性能有所提升…
