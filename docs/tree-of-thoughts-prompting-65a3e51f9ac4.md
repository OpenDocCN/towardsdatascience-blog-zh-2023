# 思维树提示

> 原文：[https://towardsdatascience.com/tree-of-thoughts-prompting-65a3e51f9ac4?source=collection_archive---------7-----------------------#2023-12-22](https://towardsdatascience.com/tree-of-thoughts-prompting-65a3e51f9ac4?source=collection_archive---------7-----------------------#2023-12-22)

## 通过深思熟虑的规划和探索来解决多步骤问题…

[](https://wolfecameron.medium.com/?source=post_page-----65a3e51f9ac4--------------------------------)[![Cameron R. Wolfe, Ph.D.](../Images/52bb88d7cf1105501be2fae5ccbe7a03.png)](https://wolfecameron.medium.com/?source=post_page-----65a3e51f9ac4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----65a3e51f9ac4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----65a3e51f9ac4--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----65a3e51f9ac4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftree-of-thoughts-prompting-65a3e51f9ac4&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----65a3e51f9ac4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----65a3e51f9ac4--------------------------------) · 20分钟阅读 · 2023年12月22日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F65a3e51f9ac4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftree-of-thoughts-prompting-65a3e51f9ac4&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----65a3e51f9ac4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F65a3e51f9ac4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftree-of-thoughts-prompting-65a3e51f9ac4&source=-----65a3e51f9ac4---------------------bookmark_footer-----------)![](../Images/3a1d8bce28681223f010bacd921267e1.png)

（图片由 [约翰·西门斯](https://unsplash.com/@emben?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，来源于 [Unsplash](https://unsplash.com/photos/green-tree-on-grassland-during-daytime-EPy0gBJzzZU?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)）

当大型语言模型（LLMs）首次开始受到关注时，它们因在解决复杂的推理问题上的不足而受到批评。虽然扩大这些模型（即，增加更多参数和数据）在任务上几乎提供了统一的性能提升，但我们几乎没有看到现代LLMs在推理任务上的性能提升。这一情况随着先进提示技术的提出而发生了变化，例如思维链提示 [2] 和自一致性 [3]。这些方法向我们展示了LLMs完全有能力进行推理和解决复杂的多步骤问题。它们只需适当的提示即可充分发挥这些能力。

> “或许令人惊讶的是，这些进展背后仍然是最初的自回归机制，它逐一进行标记级决策，并且是从左到右的。” *— 引自 [1]*

即使适当的提示能够使LLMs解决复杂问题，这些技术仍然存在不足。即，我们通常 *i)* 向LLM提供提示，并 *ii)* 期望模型利用下一个标记预测来生成完整的解决方案。某些方法可能会以逐步方式生成解决方案（例如，从最少到最多...
