# 面向无偏见的大型语言模型评估

> 原文：[`towardsdatascience.com/towards-unbiased-evaluation-of-large-language-models-9a7315144389?source=collection_archive---------4-----------------------#2023-12-09`](https://towardsdatascience.com/towards-unbiased-evaluation-of-large-language-models-9a7315144389?source=collection_archive---------4-----------------------#2023-12-09)

## 如何基准测试泄漏和数据污染削弱 LLM 评估

[](https://donatoriccio.medium.com/?source=post_page-----9a7315144389--------------------------------)![Donato Riccio](https://donatoriccio.medium.com/?source=post_page-----9a7315144389--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9a7315144389--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9a7315144389--------------------------------) [Donato Riccio](https://donatoriccio.medium.com/?source=post_page-----9a7315144389--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe384fc71d292&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-unbiased-evaluation-of-large-language-models-9a7315144389&user=Donato+Riccio&userId=e384fc71d292&source=post_page-e384fc71d292----9a7315144389---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9a7315144389--------------------------------) ·7 分钟阅读·2023 年 12 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9a7315144389&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-unbiased-evaluation-of-large-language-models-9a7315144389&user=Donato+Riccio&userId=e384fc71d292&source=-----9a7315144389---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9a7315144389&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-unbiased-evaluation-of-large-language-models-9a7315144389&source=-----9a7315144389---------------------bookmark_footer-----------)![](img/dc6cb8fa100c8187fb0ad581ac1e56a3.png)

作者提供的图像。（AI 协助制作）

> “我们的新 LLM 在每个基准测试中都超越了 GPT！”

听到这样的**大胆**声明越来越常见，因为围绕 LLM 的炒作非常巨大。每周都有新模型出现，目前每个人都在试图与 GPT-4 竞争，而 GPT-4 仍然是最强大的 LLM。

基准测试是评估大型语言模型进展的重要部分。

基准测试如**MMLU**和**HellaSwag**是评估语言模型在推理和理解等技能上的标准。这些分数提供了进展的快照，新出现的最先进结果被誉为突破性进展。LLMs 通常在零样本设置下进行评估，没有在测试集上进行明确训练，以衡量其通用能力。

这篇文章展示了操控基准测试结果是多么容易，并提供了保持评估完整性的建议。

## 基准测试的问题

通常，基准测试并不能反映在现实生活场景中的实用性。谷歌最新的模型，Gemini Ultra，在 MMLU 上得分为**90.04%**。虽然这是一个令人印象深刻的分数，但仔细查看评估方法时，它是***CoT@32***…
