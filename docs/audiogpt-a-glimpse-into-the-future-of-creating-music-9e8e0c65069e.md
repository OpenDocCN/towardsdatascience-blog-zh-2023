# AudioGPT — 未来音乐创作的展望

> 原文：[https://towardsdatascience.com/audiogpt-a-glimpse-into-the-future-of-creating-music-9e8e0c65069e?source=collection_archive---------3-----------------------#2023-05-02](https://towardsdatascience.com/audiogpt-a-glimpse-into-the-future-of-creating-music-9e8e0c65069e?source=collection_archive---------3-----------------------#2023-05-02)

## 研究论文的分析与解读

[](https://medium.com/@maxhilsdorf?source=post_page-----9e8e0c65069e--------------------------------)[![Max Hilsdorf](../Images/01da76c553e43d5ed6b6849bdbfd00da.png)](https://medium.com/@maxhilsdorf?source=post_page-----9e8e0c65069e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9e8e0c65069e--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9e8e0c65069e--------------------------------) [Max Hilsdorf](https://medium.com/@maxhilsdorf?source=post_page-----9e8e0c65069e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd0c085a74ae8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faudiogpt-a-glimpse-into-the-future-of-creating-music-9e8e0c65069e&user=Max+Hilsdorf&userId=d0c085a74ae8&source=post_page-d0c085a74ae8----9e8e0c65069e---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----9e8e0c65069e--------------------------------) ·10分钟阅读·2023年5月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9e8e0c65069e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faudiogpt-a-glimpse-into-the-future-of-creating-music-9e8e0c65069e&user=Max+Hilsdorf&userId=d0c085a74ae8&source=-----9e8e0c65069e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9e8e0c65069e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faudiogpt-a-glimpse-into-the-future-of-creating-music-9e8e0c65069e&source=-----9e8e0c65069e---------------------bookmark_footer-----------)![](../Images/7c61267fd6c17c0ecfb42939253873df.png)

图片来源 [Pixabay](https://www.pexels.com/de-de/foto/low-angle-view-von-beleuchtungsgeraten-im-regal-257904/)。

随着 [MusicLM](https://google-research.github.io/seanet/musiclm/examples/) 在2023年1月发布，音乐家和数据科学家们都明白了：人工智能将改变我们制作音乐的方式。就在几天前，下一代大型音频AI模型也已发布。在这篇文章中，我们将探讨为什么我认为**这个模型可能会成为音乐制作技术重大变革的基础**。

本文解答了以下问题：

+   什么是AudioGPT？

+   它是如何工作的？

+   它的能力与局限是什么？

+   这对未来的音乐制作意味着什么？

# 什么是 AudioGPT？

AudioGPT 是一个由中美研究人员于2023年4月发布的研究项目**[1]**。但它到底做什么，如何与GPT模型相关呢？好吧，让我们来问它吧！

![](../Images/e7bbdf3f190c060c8b60a5e39a454a8a.png)

AudioGPT 回答了这个问题：“什么是 AudioGPT？”。图像由作者提供。

## AudioGPT 是一个对话助手

从截图中可以看出，AudioGPT 可以在类似 ChatGPT 的聊天界面中使用。事实上……
