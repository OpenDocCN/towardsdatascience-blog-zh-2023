# 为什么我们有巨大的语言模型却有小型视觉变换器？

> 原文：[https://towardsdatascience.com/why-do-we-have-huge-language-models-and-small-vision-transformers-5d59ac36c1d6?source=collection_archive---------3-----------------------#2023-02-17](https://towardsdatascience.com/why-do-we-have-huge-language-models-and-small-vision-transformers-5d59ac36c1d6?source=collection_archive---------3-----------------------#2023-02-17)

## 人工智能 | 计算机视觉 | 视觉变换器

## Google ViT-22 为新的大型变换器铺平了道路，并彻底改变了计算机视觉

[](https://salvatore-raieli.medium.com/?source=post_page-----5d59ac36c1d6--------------------------------)[![Salvatore Raieli](../Images/6bb4520e2df40d20283e7283141b5e06.png)](https://salvatore-raieli.medium.com/?source=post_page-----5d59ac36c1d6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5d59ac36c1d6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5d59ac36c1d6--------------------------------) [Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----5d59ac36c1d6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff1a08d9452cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-do-we-have-huge-language-models-and-small-vision-transformers-5d59ac36c1d6&user=Salvatore+Raieli&userId=f1a08d9452cd&source=post_page-f1a08d9452cd----5d59ac36c1d6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5d59ac36c1d6--------------------------------) ·9 分钟阅读·2023年2月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5d59ac36c1d6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-do-we-have-huge-language-models-and-small-vision-transformers-5d59ac36c1d6&user=Salvatore+Raieli&userId=f1a08d9452cd&source=-----5d59ac36c1d6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5d59ac36c1d6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-do-we-have-huge-language-models-and-small-vision-transformers-5d59ac36c1d6&source=-----5d59ac36c1d6---------------------bookmark_footer-----------)![](../Images/e70451dc343d2e3cbc3ea2e6b5bbd3ea.png)

图片由 [Joshua Earle](https://unsplash.com/@joshuaearle) 提供，来源于 unsplash.com

近年来，我们见证了变换器参数数量的增长。然而仔细观察，这些是语言模型（LMs），参数多达令人难以置信的 [540 B](https://ai.googleblog.com/2022/04/pathways-language-model-palm-scaling-to.html)。 **为什么视觉模型却没有？**

至于文本模型，数据集规模的增加、可扩展架构和新的训练方法促进了参数数量的增长。这不仅提升了特定任务（如分类等）的性能，**而且随着参数数量的增加，我们看到了新兴的能力**。

![](../Images/57b77e6e745a2ea296895080a8738106.png)

“随着时间推移，最先进的自然语言处理（NLP）模型的规模趋势。这些模型的训练所需的浮点运算次数以指数级速度增加。” 来源：[这里](https://arxiv.org/abs/2104.04473)

此外，大型模型可以作为[迁移学习](https://en.wikipedia.org/wiki/Transfer_learning)和微调的基础，因此有兴趣开发越来越高性能的模型。尽管语言模型在许多任务中取得了成功，但在图像分析方面仍需要一个能够胜任的模型。
