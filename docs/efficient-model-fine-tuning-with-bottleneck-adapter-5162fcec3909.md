# 使用瓶颈适配器进行高效模型微调

> 原文：[`towardsdatascience.com/efficient-model-fine-tuning-with-bottleneck-adapter-5162fcec3909?source=collection_archive---------9-----------------------#2023-11-22`](https://towardsdatascience.com/efficient-model-fine-tuning-with-bottleneck-adapter-5162fcec3909?source=collection_archive---------9-----------------------#2023-11-22)

## 如何使用瓶颈适配器微调基于 Transformer 的模型

[](https://medium.com/@marcellusruben?source=post_page-----5162fcec3909--------------------------------)![Ruben Winastwan](https://medium.com/@marcellusruben?source=post_page-----5162fcec3909--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5162fcec3909--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5162fcec3909--------------------------------) [Ruben Winastwan](https://medium.com/@marcellusruben?source=post_page-----5162fcec3909--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5dae9da73c9b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-model-fine-tuning-with-bottleneck-adapter-5162fcec3909&user=Ruben+Winastwan&userId=5dae9da73c9b&source=post_page-5dae9da73c9b----5162fcec3909---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5162fcec3909--------------------------------) ·14 分钟阅读·2023 年 11 月 22 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5162fcec3909&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-model-fine-tuning-with-bottleneck-adapter-5162fcec3909&user=Ruben+Winastwan&userId=5dae9da73c9b&source=-----5162fcec3909---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5162fcec3909&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-model-fine-tuning-with-bottleneck-adapter-5162fcec3909&source=-----5162fcec3909---------------------bookmark_footer-----------)![](img/b79802cb5528fbfe5bb1a2f50166d41b.png)

Karolina Grabowska 的照片：[`www.pexels.com/photo/set-of-modern-port-adapters-on-black-surface-4219861/`](https://www.pexels.com/photo/set-of-modern-port-adapters-on-black-surface-4219861/)

微调是我们可以做的最常见的事情之一，以在特定任务上从深度学习模型中获得更好的性能。我们需要用来微调模型的时间通常与其大小相对应：模型越大，微调所需时间越长。

我们可以达成一致的是，如今深度学习模型，如基于 Transformer 的模型，变得越来越复杂。总体而言，这是一件好事，但也有一个警告：它们往往具有大量参数。因此，微调大型模型变得更加具有挑战性，我们需要一种更高效的方法来实现这一目标。

在本文中，我们将讨论一种高效的微调方法——瓶颈适配器。尽管你可以将这种方法应用于任何深度学习模型，但我们将仅关注其在基于 Transformer 的模型上的应用。

本文的结构如下：首先，我们将对一个特定数据集上的 BERT 模型进行正常的微调。接着，我们将借助`adapter-transformers`库在 BERT 模型中插入一些瓶颈适配器，以观察它们如何帮助我们进行微调……
