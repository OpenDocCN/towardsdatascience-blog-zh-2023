# 将多个LoRA适配器结合为Llama 2

> 原文：[https://towardsdatascience.com/combine-multiple-lora-adapters-for-llama-2-ea0bef9025cf?source=collection_archive---------4-----------------------#2023-11-30](https://towardsdatascience.com/combine-multiple-lora-adapters-for-llama-2-ea0bef9025cf?source=collection_archive---------4-----------------------#2023-11-30)

## 向您的 LLM 添加技能，无需调整新的适配器

[](https://medium.com/@bnjmn_marie?source=post_page-----ea0bef9025cf--------------------------------)[![Benjamin Marie](../Images/3ea1ad230cb1e67610418a8e36a5e5dd.png)](https://medium.com/@bnjmn_marie?source=post_page-----ea0bef9025cf--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ea0bef9025cf--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ea0bef9025cf--------------------------------) [Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----ea0bef9025cf--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fad2a414578b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcombine-multiple-lora-adapters-for-llama-2-ea0bef9025cf&user=Benjamin+Marie&userId=ad2a414578b3&source=post_page-ad2a414578b3----ea0bef9025cf---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ea0bef9025cf--------------------------------) ·12 分钟阅读·2023 年 11 月 30 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fea0bef9025cf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcombine-multiple-lora-adapters-for-llama-2-ea0bef9025cf&user=Benjamin+Marie&userId=ad2a414578b3&source=-----ea0bef9025cf---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fea0bef9025cf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcombine-multiple-lora-adapters-for-llama-2-ea0bef9025cf&source=-----ea0bef9025cf---------------------bookmark_footer-----------)![](../Images/6c4a40091827bce0f2a546522f563d4a.png)

作者提供的图片 — 利用 [Pixabay](https://pixabay.com/vectors/llama-alpaca-animal-mammal-zoo-297668/) 的图片制作

完全调整预训练的大型语言模型（LLM）以适应不同任务非常昂贵。相反，我们可以冻结LLM的参数，只需微调通过LoRA适配器添加的几百万可训练参数。

换句话说，我们只需微调一个适配器即可使模型执行目标任务。例如，如果我们想将预训练的LLM转化为翻译模型，我们会为翻译微调一个适配器。我们可以为希望LLM执行的每个任务微调一个适配器。

*但是我们能否将几个适配器组合成一个单一的多任务适配器？*

比如说，如果我们有一个用于翻译的适配器和一个用于摘要的适配器，我们能否将它们组合起来，以便 LLM 能够进行翻译和摘要？

在这篇文章中，我展示了如何将多个 LoRA 适配器组合成一个多任务适配器。我们将看到这非常简单，并且最终的适配器可以和用于组合的适配器一样好。

使用 Llama 2 7B，我们将看到如何将一个针对翻译的微调适配器与另一个针对聊天的微调适配器结合起来。通过最终的适配器，我们将能够进行……
