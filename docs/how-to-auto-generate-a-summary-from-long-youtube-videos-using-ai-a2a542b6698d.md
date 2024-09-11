# 如何利用 AI 自动生成长 YouTube 视频的摘要

> 原文：[`towardsdatascience.com/how-to-auto-generate-a-summary-from-long-youtube-videos-using-ai-a2a542b6698d?source=collection_archive---------8-----------------------#2023-04-14`](https://towardsdatascience.com/how-to-auto-generate-a-summary-from-long-youtube-videos-using-ai-a2a542b6698d?source=collection_archive---------8-----------------------#2023-04-14)

## 一步一步指南，教你如何在本地 PC 上使用 Whisper 和 BART 模型来总结 Stephen Wolfram 的演讲

[](https://medium.com/@anna.bildea?source=post_page-----a2a542b6698d--------------------------------)![Ana Bildea, PhD](https://medium.com/@anna.bildea?source=post_page-----a2a542b6698d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a2a542b6698d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a2a542b6698d--------------------------------) [Ana Bildea, PhD](https://medium.com/@anna.bildea?source=post_page-----a2a542b6698d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc57d3db39a47&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-auto-generate-a-summary-from-long-youtube-videos-using-ai-a2a542b6698d&user=Ana+Bildea%2C+PhD&userId=c57d3db39a47&source=post_page-c57d3db39a47----a2a542b6698d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a2a542b6698d--------------------------------) ·7 min read·2023 年 4 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa2a542b6698d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-auto-generate-a-summary-from-long-youtube-videos-using-ai-a2a542b6698d&user=Ana+Bildea%2C+PhD&userId=c57d3db39a47&source=-----a2a542b6698d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa2a542b6698d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-auto-generate-a-summary-from-long-youtube-videos-using-ai-a2a542b6698d&source=-----a2a542b6698d---------------------bookmark_footer-----------)![](img/73936710bcb5eeef18ac5b7cbe3fa600.png)

作者生成的图像

# 动机

在当今快速变化的世界中，保持信息灵通和灵感来源可能具有挑战性，尤其是当时间有限时。就个人而言，我非常喜欢 YouTube 上的播客和讲座。这些播客和讲座是知识的宝库，充满了来自各个领域最聪明的头脑的见解。然而，由于时间限制，我无法观看每一个有趣的视频，因为它们通常超过一个小时。这让我产生了一个想法：如果我能创建一个从头到尾自动提取主要亮点的解决方案呢？因此，我开始探索 AI 生成的解决方案，帮助我获取一些我错过的播客/讲座的自动摘要。

在这篇文章中，我讨论了在本地 PC 上实现的端到端解决方案。首先，我将介绍如何使用开源的[Whisper Model](https://huggingface.co/openai/whisper-medium)，对一个[斯蒂芬·沃尔夫拉姆关于 ChatGPT、AI 和 AGI 的讲座](https://www.youtube.com/watch?v=szxiPMyuMGY)进行转录，该讲座在 YouTube 上可用。然后，我将演示如何使用开源的[BART](https://arxiv.org/abs/1910.13461)模型来总结长文本。
