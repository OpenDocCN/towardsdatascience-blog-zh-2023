# 稳定扩散：掌握室内设计的艺术

> 原文：[https://towardsdatascience.com/stable-diffusion-mastering-the-art-of-interior-design-9fb4214544b0?source=collection_archive---------2-----------------------#2023-12-18](https://towardsdatascience.com/stable-diffusion-mastering-the-art-of-interior-design-9fb4214544b0?source=collection_archive---------2-----------------------#2023-12-18)

## 对于室内设计中的稳定扩散及其修补变体的深度探讨

[](https://medium.com/@rjguedes?source=post_page-----9fb4214544b0--------------------------------)[![拉斐尔·圭德斯](../Images/b3d000b3bce0113d2b2727e84db04870.png)](https://medium.com/@rjguedes?source=post_page-----9fb4214544b0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9fb4214544b0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9fb4214544b0--------------------------------) [拉斐尔·圭德斯](https://medium.com/@rjguedes?source=post_page-----9fb4214544b0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2789d1da9c75&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstable-diffusion-mastering-the-art-of-interior-design-9fb4214544b0&user=Rafael+Guedes&userId=2789d1da9c75&source=post_page-2789d1da9c75----9fb4214544b0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9fb4214544b0--------------------------------) ·9分钟阅读·2023年12月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9fb4214544b0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstable-diffusion-mastering-the-art-of-interior-design-9fb4214544b0&user=Rafael+Guedes&userId=2789d1da9c75&source=-----9fb4214544b0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9fb4214544b0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstable-diffusion-mastering-the-art-of-interior-design-9fb4214544b0&source=-----9fb4214544b0---------------------bookmark_footer-----------)

在我们生活的快节奏世界中以及疫情之后，我们中的许多人意识到，拥有一个如同家一样舒适的环境来逃离现实是无价的，也是一个值得追求的目标。

无论你是想要斯堪的纳维亚风格、简约风格还是迷人的装饰风格，将每一个物品如何与满是不同单品和颜色的空间融合在一起并不容易。因此，我们通常寻求专业帮助来创建那些令人惊叹的3D图像，以帮助我们理解未来的家会是什么样子。

然而，这些 3D 图像价格昂贵，如果我们的初步想法效果不如预期，获取新的图像将需要时间和更多的金钱，而这些在如今都变得稀缺。

在这篇文章中，我探讨了 Stable Diffusion 模型，从简要解释其是什么、如何训练以及适应图像修补所需的内容开始。最后，我将文章的结尾部分应用于我未来家的 3D 图像中，其中我将厨房岛台和橱柜更换为不同的颜色和材料。

![](../Images/2a2fbf68e14d0e4d69be676ac27fb9c3.png)

图 1: 室内设计 ([source](https://unsplash.com/photos/brown-wooden-framed-yellow-padded-chair-_HqHX3LBN18))
