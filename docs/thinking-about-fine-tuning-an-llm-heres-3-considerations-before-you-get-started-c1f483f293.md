# 在考虑微调 LLM 吗？这里有三个开始之前的注意事项

> 原文：[`towardsdatascience.com/thinking-about-fine-tuning-an-llm-heres-3-considerations-before-you-get-started-c1f483f293?source=collection_archive---------0-----------------------#2023-06-15`](https://towardsdatascience.com/thinking-about-fine-tuning-an-llm-heres-3-considerations-before-you-get-started-c1f483f293?source=collection_archive---------0-----------------------#2023-06-15)

[](https://medium.com/@smsmith714?source=post_page-----c1f483f293--------------------------------)![Sean Smith](https://medium.com/@smsmith714?source=post_page-----c1f483f293--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c1f483f293--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c1f483f293--------------------------------) [Sean Smith](https://medium.com/@smsmith714?source=post_page-----c1f483f293--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6957f6523097&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthinking-about-fine-tuning-an-llm-heres-3-considerations-before-you-get-started-c1f483f293&user=Sean+Smith&userId=6957f6523097&source=post_page-6957f6523097----c1f483f293---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c1f483f293--------------------------------) · 11 分钟阅读 · 2023 年 6 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc1f483f293&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthinking-about-fine-tuning-an-llm-heres-3-considerations-before-you-get-started-c1f483f293&user=Sean+Smith&userId=6957f6523097&source=-----c1f483f293---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc1f483f293&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthinking-about-fine-tuning-an-llm-heres-3-considerations-before-you-get-started-c1f483f293&source=-----c1f483f293---------------------bookmark_footer-----------)![](img/fabd8f334f6e560c8750dadc546fdd8e.png)

照片由[Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上

目前，大型语言模型（LLMs）和生成型人工智能（generative AI）正受到极大关注。[IBM](https://www.ibm.com/thought-leadership/institute-business-value/report/generative-ai-data-story)的一个惊人统计数据显示，近三分之二的 C 级高管感受到来自投资者的压力，要求加快生成型人工智能的应用。这种压力自然也传递到了数据科学和机器学习团队，他们负责应对这种炒作并创建成功的实施方案。

随着形势的发展，LLMs 的生态系统在开源和行业模型之间出现了分化，并且存在一个[迅速填补的护城河](https://www.semianalysis.com/p/google-we-have-no-moat-and-neither)。这一新兴局面促使许多团队考虑以下问题：我们如何使 LLM 更符合我们的使用案例？

在本文中，我们探讨了在考虑投入时间和工程周期来构建特定领域 LLM 时应该重点关注的一些关键因素。在这个过程中，了解一些关于潜在限制和最佳实践的最新研究至关重要，以便构建精细调整的语言模型。阅读本文后，你将获得一些更多的思路，以帮助你的组织做出是否训练以及如何训练的正确决定。

# 你可能无法用开源模型模拟 GPT
