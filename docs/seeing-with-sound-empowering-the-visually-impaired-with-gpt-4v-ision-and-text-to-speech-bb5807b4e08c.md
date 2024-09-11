# 用声音看见：用 GPT-4V(ision) 和文本转语音技术赋能视障人士

> 原文：[`towardsdatascience.com/seeing-with-sound-empowering-the-visually-impaired-with-gpt-4v-ision-and-text-to-speech-bb5807b4e08c?source=collection_archive---------6-----------------------#2023-11-16`](https://towardsdatascience.com/seeing-with-sound-empowering-the-visually-impaired-with-gpt-4v-ision-and-text-to-speech-bb5807b4e08c?source=collection_archive---------6-----------------------#2023-11-16)

## 增强视障导航：集成 GPT-4V(ision) 和 TTS 实现高级感知辅助

[](https://medium.com/@luisroque?source=post_page-----bb5807b4e08c--------------------------------)![Luís Roque](https://medium.com/@luisroque?source=post_page-----bb5807b4e08c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bb5807b4e08c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb5807b4e08c--------------------------------) [Luís Roque](https://medium.com/@luisroque?source=post_page-----bb5807b4e08c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2195f049db86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fseeing-with-sound-empowering-the-visually-impaired-with-gpt-4v-ision-and-text-to-speech-bb5807b4e08c&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=post_page-2195f049db86----bb5807b4e08c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb5807b4e08c--------------------------------) ·12 分钟阅读·2023 年 11 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbb5807b4e08c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fseeing-with-sound-empowering-the-visually-impaired-with-gpt-4v-ision-and-text-to-speech-bb5807b4e08c&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=-----bb5807b4e08c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbb5807b4e08c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fseeing-with-sound-empowering-the-visually-impaired-with-gpt-4v-ision-and-text-to-speech-bb5807b4e08c&source=-----bb5807b4e08c---------------------bookmark_footer-----------)

*本文与 Rafael Guedes 共同撰写。*

# Introduction

OpenAI 最新的发展通过发布 GPT-4V(ision) 和 Text-to-Speech (TTS) API，将人工智能的可用性提升到一个全新的水平。为什么这么说呢？让我们通过一个使用案例来解释它们的有用性。对于大多数人来说，走在街上是一件简单的事情，但对于视力受损的人来说，每一步都可能是一个挑战。传统的辅助工具如导盲犬和拐杖非常有用，但整合人工智能技术为盲人社区的独立性和移动性带来了新的希望。一副简单的配备离散摄像头的眼镜足以彻底改变视力受损者体验周围环境的方式。我们将使用 OpenAI 最新发布的技术来详细解释如何实现这一目标。

另一个有趣的应用案例是改变我们在博物馆和类似场所的体验。想象一下，常见于博物馆的音频导览系统被固定在你衣服上的离散摄像头取代。假设你正在参观一个艺术博物馆。当你在博物馆内行走时，这项技术可以为你提供每幅画作的相关信息，并且可以根据你选择的特定风格进行展示...
