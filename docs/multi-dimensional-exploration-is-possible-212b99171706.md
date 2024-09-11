# 多维探索是可能的！

> 原文：[https://towardsdatascience.com/multi-dimensional-exploration-is-possible-212b99171706?source=collection_archive---------5-----------------------#2023-10-05](https://towardsdatascience.com/multi-dimensional-exploration-is-possible-212b99171706?source=collection_archive---------5-----------------------#2023-10-05)

## （至少在数学上是这样）

[](https://medium.com/@manfred.james?source=post_page-----212b99171706--------------------------------)[![Diego Manfre](../Images/2189d8e63df449a869526bf8b6c50440.png)](https://medium.com/@manfred.james?source=post_page-----212b99171706--------------------------------)[](https://towardsdatascience.com/?source=post_page-----212b99171706--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----212b99171706--------------------------------) [Diego Manfre](https://medium.com/@manfred.james?source=post_page-----212b99171706--------------------------------)

·

[点击这里](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6e3d8f9df1a5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmulti-dimensional-exploration-is-possible-212b99171706&user=Diego+Manfre&userId=6e3d8f9df1a5&source=post_page-6e3d8f9df1a5----212b99171706---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----212b99171706--------------------------------) ·13分钟阅读·2023年10月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F212b99171706&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmulti-dimensional-exploration-is-possible-212b99171706&user=Diego+Manfre&userId=6e3d8f9df1a5&source=-----212b99171706---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F212b99171706&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmulti-dimensional-exploration-is-possible-212b99171706&source=-----212b99171706---------------------bookmark_footer-----------)![](../Images/76898d48bac7cf0039a33ff5a35111e7.png)

图片由作者使用 [Midjourney](https://www.midjourney.com/home/?callbackUrl=%2Fapp%2F) 制作

探索多维世界是科幻故事和电影中的一个常见主题。虽然穿越这些世界仍然是幻想，但数学可以帮助我们接近这样一个离奇的想法。如果你曾经想过，“又一天没有计算矩阵的行列式”，那就等一下。线性代数可能比你想象的更接近现实！在你的手机、电脑和流媒体应用中，复杂到简单的数据结构的转换每天都在发生。这些操作是真实的、数学上的多维门户。本文解释了[主成分分析](https://en.wikipedia.org/wiki/Principal_component_analysis)——它是什么，为什么重要，以及它是如何工作的。

# 卡尔·萨根解释……

有一段[视频](https://youtu.be/UnURElCzGc0?feature=shared)，其中[Carl Sagan](https://en.wikipedia.org/wiki/Carl_Sagan)在YouTube上解释了来自高维世界的访客在其低维世界中的同胞面前会是什么样子。这个美丽的课程取材于Sagan著名的系列节目[宇宙](https://en.wikipedia.org/wiki/Cosmos:_A_Personal_Voyage)。正如他节目中的许多片段一样，Sagan巧妙地解释了“平面世界”中的一些二维个体如何体验到来自三维苹果的访问。实际上，从高维到低维空间的数学原理并不超出线性代数课程的内容。正如Sagan在视频结束时所说，“虽然我们无法想象……
