# Zephyr 7B Beta：*一个好老师是你所需的一切*

> 原文：[`towardsdatascience.com/zephyr-7b-beta-a-good-teacher-is-all-you-need-c931fcd0bfe7?source=collection_archive---------4-----------------------#2023-11-11`](https://towardsdatascience.com/zephyr-7b-beta-a-good-teacher-is-all-you-need-c931fcd0bfe7?source=collection_archive---------4-----------------------#2023-11-11)

## Mistral 7B 的知识蒸馏

[](https://medium.com/@bnjmn_marie?source=post_page-----c931fcd0bfe7--------------------------------)![Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----c931fcd0bfe7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c931fcd0bfe7--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----c931fcd0bfe7--------------------------------) [Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----c931fcd0bfe7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fad2a414578b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fzephyr-7b-beta-a-good-teacher-is-all-you-need-c931fcd0bfe7&user=Benjamin+Marie&userId=ad2a414578b3&source=post_page-ad2a414578b3----c931fcd0bfe7---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----c931fcd0bfe7--------------------------------) · 8 分钟阅读 · 2023 年 11 月 11 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc931fcd0bfe7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fzephyr-7b-beta-a-good-teacher-is-all-you-need-c931fcd0bfe7&user=Benjamin+Marie&userId=ad2a414578b3&source=-----c931fcd0bfe7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc931fcd0bfe7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fzephyr-7b-beta-a-good-teacher-is-all-you-need-c931fcd0bfe7&source=-----c931fcd0bfe7---------------------bookmark_footer-----------)![](img/c9f381fbf16adcf625a9df3101552363.png)

图片来自 [Pixabay](https://pixabay.com/illustrations/man-drinking-booze-drinker-5334659/)

Mistral 7B 是其中之一 [最佳预训练大语言模型（LLM）](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard)。通过发布 [Zephyr 7B Alpha](https://huggingface.co/HuggingFaceH4/zephyr-7b-alpha)，Hugging Face 展示了 Mistral 7B 通过 DPO 微调后能够超越体积大十倍的聊天模型，甚至在某些任务上匹敌 GPT-4 的表现。

由于模型名称中包含“Alpha”，Hugging Face 显然计划发布更好的 Zephyr 7B 版本。确实，他们在仅仅 2 周后发布了 [Zephyr 7B Beta](https://huggingface.co/HuggingFaceH4/zephyr-7b-beta)。有关该模型及其评估的技术报告已发布在 arXiv 上：

[Zephyr: 语言模型对齐的直接蒸馏](https://arxiv.org/abs/2310.16944)（Tunstall 等，2023）

在这篇文章中，我们将了解是什么让 Zephyr 7B Beta 比更大的 LLM 更出色。更具体地说，我们将看到 Hugging Face 如何利用更大的 LLM，如 GPT-4，来教 Mistral 7B 回答指令并将答案与人类偏好对齐。

# 蒸馏：当较小的 LLM 从较大的 LLM 学习

由于 Hugging Face 依赖知识蒸馏（KD）来训练 Zephyr，让我们简要回顾一下 KD 在 LLM（大规模语言模型）中的含义。

大多数 LLM 是基于人类撰写的文本进行训练的。人类文本展示了高...
