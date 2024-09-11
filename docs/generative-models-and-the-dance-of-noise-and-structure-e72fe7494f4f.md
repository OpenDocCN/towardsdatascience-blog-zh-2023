# 生成模型与噪声与结构的舞蹈

> 原文：[https://towardsdatascience.com/generative-models-and-the-dance-of-noise-and-structure-e72fe7494f4f?source=collection_archive---------7-----------------------#2023-10-07](https://towardsdatascience.com/generative-models-and-the-dance-of-noise-and-structure-e72fe7494f4f?source=collection_archive---------7-----------------------#2023-10-07)

## 构建数字梦想家的指南

[](https://manuel-brenner.medium.com/?source=post_page-----e72fe7494f4f--------------------------------)[![Manuel Brenner](../Images/f62843c79a9b378494cb83caf3ddc792.png)](https://manuel-brenner.medium.com/?source=post_page-----e72fe7494f4f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e72fe7494f4f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e72fe7494f4f--------------------------------) [Manuel Brenner](https://manuel-brenner.medium.com/?source=post_page-----e72fe7494f4f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fde95441432&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerative-models-and-the-dance-of-noise-and-structure-e72fe7494f4f&user=Manuel+Brenner&userId=1fde95441432&source=post_page-1fde95441432----e72fe7494f4f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e72fe7494f4f--------------------------------) · 21分钟阅读 · 2023年10月7日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe72fe7494f4f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerative-models-and-the-dance-of-noise-and-structure-e72fe7494f4f&user=Manuel+Brenner&userId=1fde95441432&source=-----e72fe7494f4f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe72fe7494f4f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerative-models-and-the-dance-of-noise-and-structure-e72fe7494f4f&source=-----e72fe7494f4f---------------------bookmark_footer-----------)

我喜欢思考文艺复兴时期意大利的居民，他们对人类想象力和理性可能性的热情高涨，会对我们现在的技术感到最惊讶的是什么。[列奥纳多·达芬奇](https://medium.datadriveninvestor.com/there-is-no-clash-between-art-and-science-3028d0420fbe)，梦想着飞行器，肯定会对一架空中飞行的空客380印象深刻，乘客们舒适地躺在座位上，观看电影，抱怨Wi-Fi速度不够快。

但在所有在中世纪看起来像巫术的技术中，生成式AI的奇迹可能是最具巫术感的。如果我给**达芬奇**看一个能在几秒钟内绘制出他风格的女性肖像的设备，他会怎么说呢？请看：

![](../Images/f16d0b3e7077c2029a697d822ccbdf4e.png)

由DALL-E绘制的**达芬奇风格的女性肖像**。

虽然不可否认，这位女性的笑容没有真实的蒙娜丽莎那样诱惑和神秘（且经过进一步检查，看起来有些滑稽），但我们中的许多人已经遇到过令人惊讶的AI生成实例：从[超现实图像](https://twitter.com/mariswaran/status/1674505829456965633)到令人毛骨悚然的深度伪造声音，甚至是AI撰写的整篇文章。
