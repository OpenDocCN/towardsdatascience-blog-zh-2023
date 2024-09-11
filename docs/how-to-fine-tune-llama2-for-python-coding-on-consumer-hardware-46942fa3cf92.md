# 如何在消费级硬件上微调 Llama2 以进行 Python 编程

> 原文：[`towardsdatascience.com/how-to-fine-tune-llama2-for-python-coding-on-consumer-hardware-46942fa3cf92?source=collection_archive---------0-----------------------#2023-08-17`](https://towardsdatascience.com/how-to-fine-tune-llama2-for-python-coding-on-consumer-hardware-46942fa3cf92?source=collection_archive---------0-----------------------#2023-08-17)

## *通过监督微调和低秩适应技术提升 Llama2 在 Python 中的表现*

[](https://medium.com/@luisroque?source=post_page-----46942fa3cf92--------------------------------)![路易斯·罗克](https://medium.com/@luisroque?source=post_page-----46942fa3cf92--------------------------------)[](https://towardsdatascience.com/?source=post_page-----46942fa3cf92--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----46942fa3cf92--------------------------------) [路易斯·罗克](https://medium.com/@luisroque?source=post_page-----46942fa3cf92--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2195f049db86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-fine-tune-llama2-for-python-coding-on-consumer-hardware-46942fa3cf92&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=post_page-2195f049db86----46942fa3cf92---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----46942fa3cf92--------------------------------) · 18 分钟阅读 · 2023 年 8 月 17 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F46942fa3cf92&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-fine-tune-llama2-for-python-coding-on-consumer-hardware-46942fa3cf92&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=-----46942fa3cf92---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F46942fa3cf92&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-fine-tune-llama2-for-python-coding-on-consumer-hardware-46942fa3cf92&source=-----46942fa3cf92---------------------bookmark_footer-----------)

# 关于我

连续创业者和 AI 领域的领导者。我为企业开发 AI 产品，并投资于 AI 相关的初创公司。

[创始人 @ ZAAI](http://zaai.ai) | [LinkedIn](https://www.linkedin.com/in/luisbrasroque/) | [X/Twitter](https://x.com/luisbrasroque)

# 介绍

我们之前的文章详细介绍了 Llama 2，展示了 Meta 最近推出并提供给社区进行研究和商业使用的大家族大型语言模型（LLMs）。已经有为特定任务设计的变体，例如 Llama2-Chat 用于聊天应用。尽管如此，我们可能还希望获得一个更贴合我们应用的 LLM。

基于这个思路，我们提到的技术是迁移学习。这种方法涉及利用像 Llama2 这样模型中已经存在的大量知识，并将这种理解转移到新领域。微调是迁移学习的一种子集或特定形式。在微调中，整个模型的权重，包括预训练层，通常会根据新数据进行调整。这意味着在预训练过程中获得的知识会根据新任务的具体情况进行精细化。

在本文中，我们概述了一种系统的方法来增强 Llama2 在 Python 编程中的能力……
