# 语音增强简介：第1部分 — 概念与任务定义

> 原文：[https://towardsdatascience.com/introduction-to-speech-enhancement-part-1-df6098b47b91?source=collection_archive---------19-----------------------#2023-01-17](https://towardsdatascience.com/introduction-to-speech-enhancement-part-1-df6098b47b91?source=collection_archive---------19-----------------------#2023-01-17)

## 介绍了改善退化语音质量或抑制噪声的概念、方法和算法

[](https://medium.com/@mattiadigangi?source=post_page-----df6098b47b91--------------------------------)[![Mattia Di Gangi](../Images/ccd89021df6724797d45cc3c655a38a5.png)](https://medium.com/@mattiadigangi?source=post_page-----df6098b47b91--------------------------------)[](https://towardsdatascience.com/?source=post_page-----df6098b47b91--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----df6098b47b91--------------------------------) [Mattia Di Gangi](https://medium.com/@mattiadigangi?source=post_page-----df6098b47b91--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8a5b9f193a3c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-speech-enhancement-part-1-df6098b47b91&user=Mattia+Di+Gangi&userId=8a5b9f193a3c&source=post_page-8a5b9f193a3c----df6098b47b91---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----df6098b47b91--------------------------------) · 6分钟阅读 · 2023年1月17日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdf6098b47b91&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-speech-enhancement-part-1-df6098b47b91&user=Mattia+Di+Gangi&userId=8a5b9f193a3c&source=-----df6098b47b91---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdf6098b47b91&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-speech-enhancement-part-1-df6098b47b91&source=-----df6098b47b91---------------------bookmark_footer-----------)![](../Images/fb50f60823ea39daa9134c875473e489.png)

图片由 [Wan San Yip](https://unsplash.com/ja/@wansan_99?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本文是系列文章的一部分：

+   **语音增强简介：第1部分 — 概念与任务定义**

+   [语音增强简介：第2部分 — 信号表示](https://medium.com/towards-data-science/introduction-to-speech-enhancement-part-2-signal-representation-ab1deca2fa74)

语音增强是一组旨在通过语音音频信号处理技术提高语音质量（无论是清晰度还是感知质量）的方法和技术[[维基百科](https://en.wikipedia.org/wiki/Speech_enhancement)]。它有许多实际应用，包括在助听器中清除语音信号中的噪声，或从嘈杂的信道/环境中恢复难以理解的语音信号。

这与**语音分离**[1]类似但有所不同，即将一个音频信号算法性地分离成两个不同的通道，一个用于语音，另一个用于背景。可以先应用语音增强来获取语音信号，然后应用额外的算法来计算**残余信号**，即原始信号与增强语音之间的*差异*。
