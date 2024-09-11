# 思维链提示法促进 LLM 的推理能力

> 原文：[https://towardsdatascience.com/chain-of-thought-prompting-facilitate-llms-reasoning-abilities-313cd7714938?source=collection_archive---------5-----------------------#2023-06-12](https://towardsdatascience.com/chain-of-thought-prompting-facilitate-llms-reasoning-abilities-313cd7714938?source=collection_archive---------5-----------------------#2023-06-12)

## 通过示例展示

[](https://sonery.medium.com/?source=post_page-----313cd7714938--------------------------------)[![Soner Yıldırım](../Images/c589572e9d1ee176cd4f5a0008173f1b.png)](https://sonery.medium.com/?source=post_page-----313cd7714938--------------------------------)[](https://towardsdatascience.com/?source=post_page-----313cd7714938--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----313cd7714938--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----313cd7714938--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchain-of-thought-prompting-facilitate-llms-reasoning-abilities-313cd7714938&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----313cd7714938---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----313cd7714938--------------------------------) ·6 min read·2023年6月12日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F313cd7714938&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchain-of-thought-prompting-facilitate-llms-reasoning-abilities-313cd7714938&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=-----313cd7714938---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F313cd7714938&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchain-of-thought-prompting-facilitate-llms-reasoning-abilities-313cd7714938&source=-----313cd7714938---------------------bookmark_footer-----------)![](../Images/002f67384a5ad5ec102fb526b86c9eea.png)

图片来源：[Juan Rumimpunu](https://unsplash.com/@earbiscuits?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 由 [Unsplash](https://unsplash.com/photos/nLXOatvTaLo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供

大型语言模型（LLMs）已被证明在解决各种任务方面非常高效，从总结文档到用不同编程语言编写代码。

此外，随着新发布的模型如 ChatGPT 和 GPT-4，它们的表现只会变得更好，为基于 LLM 的应用打开了无限的可能性。

尽管具有非凡的技能，LLM有时仍未能展现非常简单的推理能力，无法解决那些四年级学生也能轻松应对的问题。

许多研究已经在这一领域开展，旨在理解为何LLM在执行此类任务时会失败，并使其表现更好。

针对这一特定问题的研究之一是[连锁思维提示](https://arxiv.org/abs/2201.11903)，由Google研究团队引入。

> 连锁思维提示
> 
> 一种提示技术，其结构为{输入，连锁思维，输出}，其中连锁思维是一系列中间自然语言推理步骤。

模型会被提供几个带有输入和输出的示例（少量学习），然后提出一个涉及多步骤或算术推理任务的问题。
