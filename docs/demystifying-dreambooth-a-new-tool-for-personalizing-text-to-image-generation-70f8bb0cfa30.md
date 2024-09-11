# 解密 DreamBooth：个性化文本到图像生成的新工具

> 原文：[https://towardsdatascience.com/demystifying-dreambooth-a-new-tool-for-personalizing-text-to-image-generation-70f8bb0cfa30?source=collection_archive---------15-----------------------#2023-06-13](https://towardsdatascience.com/demystifying-dreambooth-a-new-tool-for-personalizing-text-to-image-generation-70f8bb0cfa30?source=collection_archive---------15-----------------------#2023-06-13)

## 探索将枯燥图像转化为创意杰作的技术

[](https://mnslarcher.medium.com/?source=post_page-----70f8bb0cfa30--------------------------------)[![Mario Larcher](../Images/b5b443807fe06f096ed4fc5139b3cb42.png)](https://mnslarcher.medium.com/?source=post_page-----70f8bb0cfa30--------------------------------)[](https://towardsdatascience.com/?source=post_page-----70f8bb0cfa30--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----70f8bb0cfa30--------------------------------) [Mario Larcher](https://mnslarcher.medium.com/?source=post_page-----70f8bb0cfa30--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcd2b72f39ad4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdemystifying-dreambooth-a-new-tool-for-personalizing-text-to-image-generation-70f8bb0cfa30&user=Mario+Larcher&userId=cd2b72f39ad4&source=post_page-cd2b72f39ad4----70f8bb0cfa30---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----70f8bb0cfa30--------------------------------) · 13 分钟阅读 · 2023年6月13日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F70f8bb0cfa30&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdemystifying-dreambooth-a-new-tool-for-personalizing-text-to-image-generation-70f8bb0cfa30&user=Mario+Larcher&userId=cd2b72f39ad4&source=-----70f8bb0cfa30---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F70f8bb0cfa30&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdemystifying-dreambooth-a-new-tool-for-personalizing-text-to-image-generation-70f8bb0cfa30&source=-----70f8bb0cfa30---------------------bookmark_footer-----------)![](../Images/cd417c1c9a70877d284ffb874caaea6a.png)

作者用 DreamBooth 创建的新个性 Dougie。你能猜到提示是什么吗？

# 介绍

想象一下，轻松生成你心爱的幼犬在雅典卫城背景下的新图像的喜悦。还不满足，你希望看到梵高是如何绘制你最好的朋友的，或者如果他是由一只狮子构想出来的会是什么样子 😱！感谢DreamBooth，这一切都成为现实，今天只需几张图片，就可以让任何动物、物体或我们自己在幻想中旅行。

虽然我们中的许多人已经在社交媒体上看到了这项技术可以实现的令人惊叹的结果，并且有大量教程可以让我们尝试在自己的照片上进行应用，但很少有人尝试回答这样一个问题：是的，那么它到底是如何工作的呢？

在这篇文章中，我将尽力解析由Ruiz等人撰写的科学论文[DreamBooth: Fine Tuning Text-to-Image Diffusion Models for Subject-Driven Generation](https://arxiv.org/abs/2208.12242)，这是一切的起点。但不用担心，我会简化复杂的部分，并在可能需要一些先验知识的地方给出解释。现在，提醒一下，这是一个高级话题，所以我假设……
