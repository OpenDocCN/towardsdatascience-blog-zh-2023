# 高效企业级语音识别的 Vosk：评估与实施指南

> 原文：[`towardsdatascience.com/vosk-for-efficient-enterprise-grade-speech-recognition-an-evaluation-and-implementation-guide-87a599217a6c?source=collection_archive---------8-----------------------#2023-05-15`](https://towardsdatascience.com/vosk-for-efficient-enterprise-grade-speech-recognition-an-evaluation-and-implementation-guide-87a599217a6c?source=collection_archive---------8-----------------------#2023-05-15)

## 对基于 Vosk 的语音识别系统进行实施和评估的全面指南

[](https://medium.com/@luisroque?source=post_page-----87a599217a6c--------------------------------)![路易斯·罗克](https://medium.com/@luisroque?source=post_page-----87a599217a6c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----87a599217a6c--------------------------------)![数据科学之路](https://towardsdatascience.com/?source=post_page-----87a599217a6c--------------------------------) [路易斯·罗克](https://medium.com/@luisroque?source=post_page-----87a599217a6c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2195f049db86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvosk-for-efficient-enterprise-grade-speech-recognition-an-evaluation-and-implementation-guide-87a599217a6c&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=post_page-2195f049db86----87a599217a6c---------------------post_header-----------) 发表在 [数据科学之路](https://towardsdatascience.com/?source=post_page-----87a599217a6c--------------------------------) · 9 分钟阅读 · 2023 年 5 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F87a599217a6c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvosk-for-efficient-enterprise-grade-speech-recognition-an-evaluation-and-implementation-guide-87a599217a6c&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=-----87a599217a6c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F87a599217a6c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvosk-for-efficient-enterprise-grade-speech-recognition-an-evaluation-and-implementation-guide-87a599217a6c&source=-----87a599217a6c---------------------bookmark_footer-----------)

# 介绍

在我们最新的文章中，我们广泛使用不同的模型和方法进行语音识别。开源社区在过去几年中一直在开发这一领域的解决方案，为我们提供了许多选择。我们尝试了 Whisper、WhisperX 和 Whisper-JAX。它们都基于最近由 OpenAI 开源的 Whisper 模型训练。这是一个在多语言和多任务上训练的自动语音识别（ASR）系统，使其对语音甚至语言任务都具有通用性。

Whisper 的一个缺点是运行它所需的资源，特别是处理较长音频文件的执行时间。在本文中，我们将指导您如何利用 Vosk 开发企业级离线语音识别模型工具包。这些模型速度显著更快，在大量使用案例中，这是一个决定性因素。

我们深入研究了四种 Vosk 语音识别模型的性能，突出它们在准确性、执行时间和模型大小方面的优势和劣势。我们的研究发现了一些有趣的见解，例如在过渡时准确性提高了 20%……
