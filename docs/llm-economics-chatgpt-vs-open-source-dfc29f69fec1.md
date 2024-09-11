# LLM经济学：ChatGPT与开源模型的对比

> 原文：[https://towardsdatascience.com/llm-economics-chatgpt-vs-open-source-dfc29f69fec1?source=collection_archive---------2-----------------------#2023-04-26](https://towardsdatascience.com/llm-economics-chatgpt-vs-open-source-dfc29f69fec1?source=collection_archive---------2-----------------------#2023-04-26)

## 部署像ChatGPT这样的LLM需要多少成本？开源LLM的部署成本是否更低？有什么权衡？

[](https://skanda-vivek.medium.com/?source=post_page-----dfc29f69fec1--------------------------------)[![Skanda Vivek](../Images/9d25bee2fb75176ca7f7ea6eff7d7ab5.png)](https://skanda-vivek.medium.com/?source=post_page-----dfc29f69fec1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dfc29f69fec1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----dfc29f69fec1--------------------------------) [Skanda Vivek](https://skanda-vivek.medium.com/?source=post_page-----dfc29f69fec1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F220d9bbb8014&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-economics-chatgpt-vs-open-source-dfc29f69fec1&user=Skanda+Vivek&userId=220d9bbb8014&source=post_page-220d9bbb8014----dfc29f69fec1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dfc29f69fec1--------------------------------) ·6分钟阅读·2023年4月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdfc29f69fec1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-economics-chatgpt-vs-open-source-dfc29f69fec1&user=Skanda+Vivek&userId=220d9bbb8014&source=-----dfc29f69fec1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdfc29f69fec1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-economics-chatgpt-vs-open-source-dfc29f69fec1&source=-----dfc29f69fec1---------------------bookmark_footer-----------)![](../Images/fa975d94faf37f22c807696c80da0324.png)

对比LLM成本的卡通示意图 | Skanda Vivek

总结：在每天使用量为1000次请求范围内，ChatGPT的成本低于在AWS上部署的开源LLM。对于每天请求数达百万的情况，AWS上的开源模型更便宜。（截至本文撰写日期2023年4月24日）

大型语言模型正在席卷全球。变压器模型于2017年问世，随后出现了突破性的模型如BERT、GPT和BART——这些模型拥有数亿个参数，能够执行多种语言任务，如情感分析、问答、分类等。

几年前——来自 OpenAI 和 Google 的研究人员记录了多篇论文，表明拥有超过 10 亿参数的大型语言模型开始展现出涌现的能力，这些模型似乎能够理解语言的复杂方面，并且在回应上几乎像人类一样。

![](../Images/f39567219981d5f1f495ab8bd9f9174f.png)

[GPT-3 论文](https://arxiv.org/abs/2005.14165)展示了大语言模型令人印象深刻的学习能力。

GPT-3 论文显示，参数大于 10-100 亿的模型展现了令人印象深刻的学习能力……
