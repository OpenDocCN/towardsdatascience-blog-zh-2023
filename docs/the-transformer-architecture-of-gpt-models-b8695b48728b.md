# GPT 模型的 Transformer 架构

> 原文：[`towardsdatascience.com/the-transformer-architecture-of-gpt-models-b8695b48728b?source=collection_archive---------3-----------------------#2023-07-25`](https://towardsdatascience.com/the-transformer-architecture-of-gpt-models-b8695b48728b?source=collection_archive---------3-----------------------#2023-07-25)

## 了解 Transformer 架构的详细信息

[](https://medium.com/@bea_684?source=post_page-----b8695b48728b--------------------------------)![Beatriz Stollnitz](https://medium.com/@bea_684?source=post_page-----b8695b48728b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b8695b48728b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b8695b48728b--------------------------------) [Beatriz Stollnitz](https://medium.com/@bea_684?source=post_page-----b8695b48728b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1c8863892480&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-transformer-architecture-of-gpt-models-b8695b48728b&user=Beatriz+Stollnitz&userId=1c8863892480&source=post_page-1c8863892480----b8695b48728b---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----b8695b48728b--------------------------------) · 22 分钟阅读 · 2023 年 7 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb8695b48728b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-transformer-architecture-of-gpt-models-b8695b48728b&user=Beatriz+Stollnitz&userId=1c8863892480&source=-----b8695b48728b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb8695b48728b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-transformer-architecture-of-gpt-models-b8695b48728b&source=-----b8695b48728b---------------------bookmark_footer-----------)![](img/b27fc545e177f9112abf27cfac0ceebb.png)

图片由[fabio](https://unsplash.com/@fabioha?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

2017 年，谷歌的作者们发布了一篇名为 [Attention is All You Need](https://arxiv.org/abs/1706.03762) 的论文，在其中他们介绍了 Transformer 架构。这种新架构在语言翻译任务中取得了前所未有的成功，论文很快成为了从事这一领域的人员必读的文献。像许多人一样，当我第一次阅读这篇论文时，我能看出其创新想法的价值，但我没有意识到这篇论文会对 AI 更广泛领域产生多么深远的影响。在几年的时间里，研究人员将 Transformer 架构应用于除语言翻译之外的许多任务，包括图像分类、图像生成和蛋白质折叠问题。特别是，Transformer 架构彻底改变了文本生成，并为 GPT 模型和我们当前在 AI 领域经历的指数级增长铺平了道路。

鉴于当前 Transformer 模型在工业界和学术界的广泛应用，理解它们的工作细节对每一位 AI 从业者来说都是一项重要技能。本文将主要关注 GPT 模型的架构，这些模型是基于原始 Transformer 架构的一个子集构建的，但也会在最后涉及原始 Transformer 架构。关于模型代码……
