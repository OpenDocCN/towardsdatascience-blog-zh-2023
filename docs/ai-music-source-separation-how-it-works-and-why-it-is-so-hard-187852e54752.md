# AI 音乐源分离：它是如何工作的以及为什么如此困难

> 原文：[https://towardsdatascience.com/ai-music-source-separation-how-it-works-and-why-it-is-so-hard-187852e54752?source=collection_archive---------4-----------------------#2023-09-21](https://towardsdatascience.com/ai-music-source-separation-how-it-works-and-why-it-is-so-hard-187852e54752?source=collection_archive---------4-----------------------#2023-09-21)

## 源分离 AI 解释

[](https://medium.com/@maxhilsdorf?source=post_page-----187852e54752--------------------------------)[![Max Hilsdorf](../Images/01da76c553e43d5ed6b6849bdbfd00da.png)](https://medium.com/@maxhilsdorf?source=post_page-----187852e54752--------------------------------)[](https://towardsdatascience.com/?source=post_page-----187852e54752--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----187852e54752--------------------------------) [Max Hilsdorf](https://medium.com/@maxhilsdorf?source=post_page-----187852e54752--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd0c085a74ae8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fai-music-source-separation-how-it-works-and-why-it-is-so-hard-187852e54752&user=Max+Hilsdorf&userId=d0c085a74ae8&source=post_page-d0c085a74ae8----187852e54752---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----187852e54752--------------------------------) ·9 分钟阅读·2023 年 9 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F187852e54752&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fai-music-source-separation-how-it-works-and-why-it-is-so-hard-187852e54752&user=Max+Hilsdorf&userId=d0c085a74ae8&source=-----187852e54752---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F187852e54752&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fai-music-source-separation-how-it-works-and-why-it-is-so-hard-187852e54752&source=-----187852e54752---------------------bookmark_footer-----------)![](../Images/1592eebc641690038dd7c665c19845a6.png)

作者提供的图片。

# 源分离

## 什么是源分离？

在信号处理领域，源分离描述了将音频信号分解为多个源音频信号的任务。这个概念不仅与音乐相关，还涉及到语音或机器声音。例如，你可能想要分离播客中两个说话者的声音，以便可以单独编辑这些声音。

## 为什么源分离如此困难？

并不是每个人都是音乐家。更少有人是对数据和人工智能充满兴趣的音乐家。通常，当我与非音乐家交谈时，我会得到这样的印象：他们认为你可以简单地“从音频中去除声音”。这很有道理，因为如果不是这样，为什么专辑的B面会有纯音乐版本，或者每个酒吧都能找到成千上万的流行歌曲卡拉OK版本呢？实际上，从伴奏中分离人声真的很简单——当你可以访问混音的各个音轨时……

然而，在现实世界中，我们只有波形。波形是我们最接近真实、物理音频事件的计算机表示方式。波形也是……
