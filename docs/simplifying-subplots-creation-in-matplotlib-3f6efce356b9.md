# 在Matplotlib中简化子图的创建

> 原文：[https://towardsdatascience.com/simplifying-subplots-creation-in-matplotlib-3f6efce356b9?source=collection_archive---------6-----------------------#2023-05-23](https://towardsdatascience.com/simplifying-subplots-creation-in-matplotlib-3f6efce356b9?source=collection_archive---------6-----------------------#2023-05-23)

## 将马赛克魔法融入您的图表

[](https://pandeyparul.medium.com/?source=post_page-----3f6efce356b9--------------------------------)[![Parul Pandey](../Images/760b72a4feacfad6fc4224835c2e1f19.png)](https://pandeyparul.medium.com/?source=post_page-----3f6efce356b9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3f6efce356b9--------------------------------)[![数据科学的前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3f6efce356b9--------------------------------) [Parul Pandey](https://pandeyparul.medium.com/?source=post_page-----3f6efce356b9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7053de462a28&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimplifying-subplots-creation-in-matplotlib-3f6efce356b9&user=Parul+Pandey&userId=7053de462a28&source=post_page-7053de462a28----3f6efce356b9---------------------post_header-----------) 发表在 [数据科学的前沿](https://towardsdatascience.com/?source=post_page-----3f6efce356b9--------------------------------) · 5分钟阅读 · 2023年5月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3f6efce356b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimplifying-subplots-creation-in-matplotlib-3f6efce356b9&user=Parul+Pandey&userId=7053de462a28&source=-----3f6efce356b9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3f6efce356b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimplifying-subplots-creation-in-matplotlib-3f6efce356b9&source=-----3f6efce356b9---------------------bookmark_footer-----------)![](../Images/c62c7aa026a2002e8b02ca77fdeb78e2.png)

图片由 [charlesdeluvio](https://unsplash.com/@charlesdeluvio?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

最近，我在做一个项目，需要使用Python中的[Matplotlib](https://matplotlib.org/)库创建子图。如果你曾经使用过Matplotlib库，你很可能也使用过它的子图功能。子图是同时生成多个图形的有效工具，这在比较结果或多个图形共享相同坐标轴时非常有用。然而，有时Matplotlib中的子图语法对于许多人，包括我自己，来说都不是特别简单。实现期望的子图布局可能像是一场试错游戏，将注意力从实际项目上转移开。

# 公开隐藏，真的！！

![](../Images/2bba185b381ff006779b7142cb08f408.png)

公开隐藏 | [图片来自Pixabay](https://pixabay.com//?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=317041)

我知道R中的[patchwork库](https://cran.r-project.org/web/packages/patchwork/vignettes/patchwork.html)擅长处理子图创建。然而，我很惊讶地发现[**Matplotlib一直具备这种功能**](https://twitter.com/matplotlib/status/1382034095534931969)，这让我深刻认识到要彻底阅读文档。感到好奇，我决定深入探讨这一功能，以扩展我的理解，并通过博客文章与他人分享我的经验和见解。
