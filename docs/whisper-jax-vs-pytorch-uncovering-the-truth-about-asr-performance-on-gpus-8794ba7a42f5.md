# Whisper JAX 与 PyTorch：揭示 GPU 上 ASR 性能的真相

> 原文：[`towardsdatascience.com/whisper-jax-vs-pytorch-uncovering-the-truth-about-asr-performance-on-gpus-8794ba7a42f5?source=collection_archive---------0-----------------------#2023-04-29`](https://towardsdatascience.com/whisper-jax-vs-pytorch-uncovering-the-truth-about-asr-performance-on-gpus-8794ba7a42f5?source=collection_archive---------0-----------------------#2023-04-29)

## 深入探讨自动语音识别：在多个平台上对比 Whisper JAX 和 PyTorch 实现

[](https://medium.com/@luisroque?source=post_page-----8794ba7a42f5--------------------------------)![Luís Roque](https://medium.com/@luisroque?source=post_page-----8794ba7a42f5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8794ba7a42f5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8794ba7a42f5--------------------------------) [Luís Roque](https://medium.com/@luisroque?source=post_page-----8794ba7a42f5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2195f049db86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhisper-jax-vs-pytorch-uncovering-the-truth-about-asr-performance-on-gpus-8794ba7a42f5&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=post_page-2195f049db86----8794ba7a42f5---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8794ba7a42f5--------------------------------) · 8 分钟阅读 · 2023 年 4 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8794ba7a42f5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhisper-jax-vs-pytorch-uncovering-the-truth-about-asr-performance-on-gpus-8794ba7a42f5&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=-----8794ba7a42f5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8794ba7a42f5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhisper-jax-vs-pytorch-uncovering-the-truth-about-asr-performance-on-gpus-8794ba7a42f5&source=-----8794ba7a42f5---------------------bookmark_footer-----------)

# 介绍

在自动语音识别（ASR）的世界里，速度和准确性非常重要。数据和模型的规模最近大幅增长，这使得效率变得困难。然而，竞争才刚刚开始，我们每周都会看到新的进展。本文重点介绍了 Whisper JAX，这是一个使用不同后端框架实现的 Whisper 版本，其运行速度似乎比 OpenAI 的 PyTorch 实现快 70 倍。我们测试了 CPU 和 GPU 实现，并测量了准确性和执行时间。此外，我们还为小型和大型模型定义了实验，同时调整了批量大小和数据类型，以查看是否能够进一步改进。

正如我们在上一篇文章中所看到的，Whisper 是一个多用途的语音识别模型，在多个语音处理任务中表现出色。它可以进行多语言语音识别、翻译，甚至是语音活动检测。它使用 Transformer 序列到序列架构来联合预测单词和任务。Whisper 作为语音处理任务的元模型之一。Whisper 的一个缺点是效率；与其他最先进的模型相比，它通常被发现速度相对较慢。
