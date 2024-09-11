# MusicLM — 谷歌是否解决了AI音乐生成的问题？

> 原文：[https://towardsdatascience.com/musiclm-has-google-solved-ai-music-generation-c6859e76bc3c?source=collection_archive---------2-----------------------#2023-02-02](https://towardsdatascience.com/musiclm-has-google-solved-ai-music-generation-c6859e76bc3c?source=collection_archive---------2-----------------------#2023-02-02)

## 论文及其意义解释

[](https://medium.com/@maxhilsdorf?source=post_page-----c6859e76bc3c--------------------------------)[![Max Hilsdorf](../Images/01da76c553e43d5ed6b6849bdbfd00da.png)](https://medium.com/@maxhilsdorf?source=post_page-----c6859e76bc3c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c6859e76bc3c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c6859e76bc3c--------------------------------) [Max Hilsdorf](https://medium.com/@maxhilsdorf?source=post_page-----c6859e76bc3c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd0c085a74ae8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmusiclm-has-google-solved-ai-music-generation-c6859e76bc3c&user=Max+Hilsdorf&userId=d0c085a74ae8&source=post_page-d0c085a74ae8----c6859e76bc3c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c6859e76bc3c--------------------------------) ·11分钟阅读·2023年2月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc6859e76bc3c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmusiclm-has-google-solved-ai-music-generation-c6859e76bc3c&user=Max+Hilsdorf&userId=d0c085a74ae8&source=-----c6859e76bc3c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc6859e76bc3c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmusiclm-has-google-solved-ai-music-generation-c6859e76bc3c&source=-----c6859e76bc3c---------------------bookmark_footer-----------)![](../Images/d9c2dc9f2b4448695343134c707b1f1d.png)

图片由 [Placidplace](https://pixabay.com/de/illustrations/sprecher-lautsprecher-7459276/) 提供。

# 介绍

在2023年1月25日的PowerPoint演示中，我描述了生成长时间段高质量音乐作为音频AI领域需要在不久的将来解决的主要挑战之一。一天后，我的幻灯片就过时了。

> MusicLM，由谷歌研究开发，能够根据简单的自然语言文本查询生成一分钟长的高质量音乐，涵盖所有风格和流派。

最好你自己亲自体验一下，并查看包含大量音乐示例的[演示页面](https://google-research.github.io/seanet/musiclm/examples/)。如果你对细节感兴趣，也可以查看研究论文，尽管这篇文章也会涵盖所有相关话题。

![](../Images/4f5ba45fc72cd2e2716f904790cd3c8e.png)

来自MusicLM演示页面的摘录。图片由作者提供。

那么，是什么让MusicLM成为如此重大的技术突破？它解决了哪些困扰AI研究者过去十年的问题？而我为什么仍然认为MusicLM是一种过渡技术——进入音乐制作不同世界的桥梁？这些…
