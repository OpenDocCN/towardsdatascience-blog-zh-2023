# Whisper JAX 与 PyTorch：揭示 GPU 上 ASR 性能的真相

> 原文：[https://towardsdatascience.com/whisper-jax-vs-pytorch-uncovering-the-truth-about-asr-performance-on-gpus-8794ba7a42f5?source=collection_archive---------0-----------------------#2023-04-29](https://towardsdatascience.com/whisper-jax-vs-pytorch-uncovering-the-truth-about-asr-performance-on-gpus-8794ba7a42f5?source=collection_archive---------0-----------------------#2023-04-29)

## 深入探讨自动语音识别：在多个平台上对比 Whisper JAX 和 PyTorch 实现

[](https://medium.com/@luisroque?source=post_page-----8794ba7a42f5--------------------------------)[![Luís Roque](../Images/e281d470b403375ba3c6f521b1ccf915.png)](https://medium.com/@luisroque?source=post_page-----8794ba7a42f5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8794ba7a42f5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8794ba7a42f5--------------------------------) [Luís Roque](https://medium.com/@luisroque?source=post_page-----8794ba7a42f5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2195f049db86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhisper-jax-vs-pytorch-uncovering-the-truth-about-asr-performance-on-gpus-8794ba7a42f5&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=post_page-2195f049db86----8794ba7a42f5---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8794ba7a42f5--------------------------------) · 8分钟阅读 · 2023年4月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8794ba7a42f5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhisper-jax-vs-pytorch-uncovering-the-truth-about-asr-performance-on-gpus-8794ba7a42f5&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=-----8794ba7a42f5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8794ba7a42f5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhisper-jax-vs-pytorch-uncovering-the-truth-about-asr-performance-on-gpus-8794ba7a42f5&source=-----8794ba7a42f5---------------------bookmark_footer-----------)

# 介绍

在自动语音识别（ASR）的世界里，速度和准确性非常重要。数据和模型的规模最近大幅增长，这使得效率变得困难。然而，竞争才刚刚开始，我们每周都会看到新的进展。本文重点介绍了Whisper JAX，这是一个使用不同后端框架实现的Whisper版本，其运行速度似乎比OpenAI的PyTorch实现快70倍。我们测试了CPU和GPU实现，并测量了准确性和执行时间。此外，我们还为小型和大型模型定义了实验，同时调整了批量大小和数据类型，以查看是否能够进一步改进。

正如我们在[上一篇文章](/unlock-the-power-of-audio-data-advanced-transcription-and-diarization-with-whisper-whisperx-and-ed9424307281)中所看到的，Whisper是一个多用途的语音识别模型，在多个语音处理任务中表现出色。它可以进行多语言语音识别、翻译，甚至是语音活动检测。它使用Transformer序列到序列架构来联合预测单词和任务。Whisper作为语音处理任务的元模型之一。Whisper的一个缺点是效率；与其他最先进的模型相比，它通常被发现速度相对较慢。
