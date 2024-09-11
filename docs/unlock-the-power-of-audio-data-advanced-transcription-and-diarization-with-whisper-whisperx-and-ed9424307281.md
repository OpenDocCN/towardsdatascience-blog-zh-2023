# 解锁音频数据的力量：使用 Whisper、WhisperX 和 PyAnnotate 进行高级转录和分话

> 原文：[`towardsdatascience.com/unlock-the-power-of-audio-data-advanced-transcription-and-diarization-with-whisper-whisperx-and-ed9424307281?source=collection_archive---------2-----------------------#2023-04-17`](https://towardsdatascience.com/unlock-the-power-of-audio-data-advanced-transcription-and-diarization-with-whisper-whisperx-and-ed9424307281?source=collection_archive---------2-----------------------#2023-04-17)

## 利用最先进的语音识别和说话人归属技术简化音频分析

[](https://medium.com/@luisroque?source=post_page-----ed9424307281--------------------------------)![Luís Roque](https://medium.com/@luisroque?source=post_page-----ed9424307281--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ed9424307281--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ed9424307281--------------------------------) [Luís Roque](https://medium.com/@luisroque?source=post_page-----ed9424307281--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2195f049db86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlock-the-power-of-audio-data-advanced-transcription-and-diarization-with-whisper-whisperx-and-ed9424307281&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=post_page-2195f049db86----ed9424307281---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ed9424307281--------------------------------) · 9 分钟阅读 · 2023 年 4 月 17 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fed9424307281&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlock-the-power-of-audio-data-advanced-transcription-and-diarization-with-whisper-whisperx-and-ed9424307281&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=-----ed9424307281---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fed9424307281&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlock-the-power-of-audio-data-advanced-transcription-and-diarization-with-whisper-whisperx-and-ed9424307281&source=-----ed9424307281---------------------bookmark_footer-----------)

# 介绍

在我们快速变化的世界中，我们生成了大量的音频数据。想想你喜欢的播客或工作中的会议电话。这些数据在原始形式下已经很丰富了；我们作为人类可以理解它。即便如此，我们仍然可以进一步处理，例如，将其转换为书面格式，以便以后进行搜索。

为了更好地理解当前任务，我们介绍了两个概念。第一个是**转录**，即将口语转换为文本。本文中我们探讨的第二个概念是**说话人分离**。说话人分离帮助我们为无结构内容提供额外的结构。在这种情况下，我们有兴趣将特定的讲话片段归属到不同的说话人。

在上述背景下，我们使用不同的工具来处理这两个任务。我们使用了由 OpenAI 开发的通用语音识别模型**Whisper**。它在多样化的音频样本数据集上进行了训练，研究人员开发它以执行多种任务。其次，我们使用了用于说话人分离的库**PyAnnotate**。最后，我们使用了研究项目**WhisperX**，它帮助将两者结合，同时解决了 Whisper 的一些限制。
