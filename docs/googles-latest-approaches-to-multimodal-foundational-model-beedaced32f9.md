# 谷歌最新的多模态基础模型方法

> 原文：[https://towardsdatascience.com/googles-latest-approaches-to-multimodal-foundational-model-beedaced32f9?source=collection_archive---------5-----------------------#2023-08-12](https://towardsdatascience.com/googles-latest-approaches-to-multimodal-foundational-model-beedaced32f9?source=collection_archive---------5-----------------------#2023-08-12)

## 多模态基础模型比大型语言模型更加令人兴奋。让我们回顾一下谷歌研究在这方面的最新进展，以窥探前沿。

[](https://eileen-code4fun.medium.com/?source=post_page-----beedaced32f9--------------------------------)[![Eileen Pangu](../Images/cbdab572af709b6e6b52cb3a078f220d.png)](https://eileen-code4fun.medium.com/?source=post_page-----beedaced32f9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----beedaced32f9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----beedaced32f9--------------------------------) [Eileen Pangu](https://eileen-code4fun.medium.com/?source=post_page-----beedaced32f9--------------------------------)

·

[跟进](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F893d6b8a519f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoogles-latest-approaches-to-multimodal-foundational-model-beedaced32f9&user=Eileen+Pangu&userId=893d6b8a519f&source=post_page-893d6b8a519f----beedaced32f9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----beedaced32f9--------------------------------) ·7 分钟阅读·2023 年 8 月 12 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbeedaced32f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoogles-latest-approaches-to-multimodal-foundational-model-beedaced32f9&source=-----beedaced32f9---------------------bookmark_footer-----------)![](../Images/4c992bcc9d7d065534e2c5fec5e58b79.png)

图片来源：[https://unsplash.com/photos/U3sOwViXhkY](https://unsplash.com/photos/U3sOwViXhkY)

> **背景**

尽管大型语言模型（LLM）的热度在业界依然高涨，领先的研究组织已经将目光转向多模态基础模型——这些模型具有与LLM相同的规模和多样性特征，但能够处理超出文本的数据，如图像、音频、传感器信号等。许多人认为，多模态基础模型是解锁人工智能（AI）下一阶段发展的关键。

在这篇博客文章中，我们将深入探讨谷歌如何处理多模态基础模型。本文内容来源于谷歌最近论文的关键方法和见解，相关参考资料见文章末尾。

> **为什么你需要关注**

多模态基础模型令人兴奋，但你为什么需要关注？你可能是：

+   一个希望跟上领域最新研究进展的AI/ML从业者，但你却…
