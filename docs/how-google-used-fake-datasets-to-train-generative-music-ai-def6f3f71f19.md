# 如何使用虚假数据集训练生成音乐 AI

> 原文：[`towardsdatascience.com/how-google-used-fake-datasets-to-train-generative-music-ai-def6f3f71f19?source=collection_archive---------3-----------------------#2023-05-28`](https://towardsdatascience.com/how-google-used-fake-datasets-to-train-generative-music-ai-def6f3f71f19?source=collection_archive---------3-----------------------#2023-05-28)

[](https://medium.com/@maxhilsdorf?source=post_page-----def6f3f71f19--------------------------------)![Max Hilsdorf](https://medium.com/@maxhilsdorf?source=post_page-----def6f3f71f19--------------------------------)[](https://towardsdatascience.com/?source=post_page-----def6f3f71f19--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----def6f3f71f19--------------------------------) [Max Hilsdorf](https://medium.com/@maxhilsdorf?source=post_page-----def6f3f71f19--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd0c085a74ae8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-google-used-fake-datasets-to-train-generative-music-ai-def6f3f71f19&user=Max+Hilsdorf&userId=d0c085a74ae8&source=post_page-d0c085a74ae8----def6f3f71f19---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----def6f3f71f19--------------------------------) ·6 分钟阅读·2023 年 5 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdef6f3f71f19&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-google-used-fake-datasets-to-train-generative-music-ai-def6f3f71f19&user=Max+Hilsdorf&userId=d0c085a74ae8&source=-----def6f3f71f19---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdef6f3f71f19&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-google-used-fake-datasets-to-train-generative-music-ai-def6f3f71f19&source=-----def6f3f71f19---------------------bookmark_footer-----------)![](img/ed2c5ce433310ba9f1ace3de4d277849.png)

图片由 [James Stamler](https://unsplash.com/@jamesstamler?utm_source=medium&utm_medium=referral) 提供，刊登在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇文章中，我们探讨了谷歌在训练其卓越的文本到音乐模型（如 MusicLM 和 Noise2Music）时的创新方法。我们将深入探讨“虚假”数据集的概念及其在这些突破性模型中的应用。如果你对这些技术的内部工作原理及其在推动音乐 AI 方面的影响感兴趣，你来对地方了。

# **缺乏标记音乐数据**

像 ChatGPT 或 Bard 这样的大型语言模型（LLMs）是在大量非结构化文本数据上进行训练的。虽然收集数百万个网站的内容可能会很昂贵，但公共网络上有大量的训练数据。相比之下，像 DALL-E 2 这样的文本到图像模型则需要一种完全不同的数据集，由图像和对应描述的配对组成。

同样，文本到音乐的模型依赖于带有音乐内容描述的歌曲。然而，与图像不同的是，标注过的音乐在互联网上真的很难找到。有时，像乐器编排、风格或情绪这样的元数据是可以获取的，但完整的深入描述却异常难以获得。这给试图收集数据以训练生成音乐模型的研究人员和公司带来了严重问题。
