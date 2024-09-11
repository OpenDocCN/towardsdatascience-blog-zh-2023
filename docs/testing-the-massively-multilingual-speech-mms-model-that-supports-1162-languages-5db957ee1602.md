# 测试支持 1162 种语言的大规模多语言语音 (MMS) 模型

> 原文：[https://towardsdatascience.com/testing-the-massively-multilingual-speech-mms-model-that-supports-1162-languages-5db957ee1602?source=collection_archive---------3-----------------------#2023-05-26](https://towardsdatascience.com/testing-the-massively-multilingual-speech-mms-model-that-supports-1162-languages-5db957ee1602?source=collection_archive---------3-----------------------#2023-05-26)

## 探索 Meta 最新的自动语音识别 (ASR) 模型的尖端多语言功能

[](https://medium.com/@luisroque?source=post_page-----5db957ee1602--------------------------------)[![Luís Roque](../Images/e281d470b403375ba3c6f521b1ccf915.png)](https://medium.com/@luisroque?source=post_page-----5db957ee1602--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5db957ee1602--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5db957ee1602--------------------------------) [Luís Roque](https://medium.com/@luisroque?source=post_page-----5db957ee1602--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2195f049db86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftesting-the-massively-multilingual-speech-mms-model-that-supports-1162-languages-5db957ee1602&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=post_page-2195f049db86----5db957ee1602---------------------post_header-----------) 在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5db957ee1602--------------------------------) ·8 min read·2023 年 5 月 26 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5db957ee1602&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftesting-the-massively-multilingual-speech-mms-model-that-supports-1162-languages-5db957ee1602&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=-----5db957ee1602---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5db957ee1602&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftesting-the-massively-multilingual-speech-mms-model-that-supports-1162-languages-5db957ee1602&source=-----5db957ee1602---------------------bookmark_footer-----------)

# 介绍

大规模多语言语音 (MMS)¹ 是 Meta AI 最新发布的产品 (仅几天前)。它通过构建一个单一的多语言语音识别模型，将语音技术的边界从大约 100 种语言扩展到 1000 多种。该模型还能识别超过 4000 种语言，较之前的能力扩大了 40 倍。

MMS 项目的目标是使人们更容易使用自己偏好的语言获取信息和使用设备。它扩展了文本转语音和语音转文本技术到服务不足的语言，继续减少我们全球世界的语言障碍。现有的应用程序现在可以包括更广泛的语言，例如虚拟助手或语音激活设备。同时，跨文化交流中也出现了新的应用场景，例如在消息服务或虚拟现实与增强现实中。

在本文中，我们将深入探讨如何使用 MMS 进行英语和葡萄牙语的自动语音识别，并提供逐步指南以设置运行模型的环境。

![](../Images/a2b17ee13d06f5681126543d65c4f142.png)

图 1：Massively Multilingual Speech (MMS) 能够识别超过 4,000 种语言，并支持 1162 种语言（[来源](https://unsplash.com/photos/ZzWsHbu2y80)）。
