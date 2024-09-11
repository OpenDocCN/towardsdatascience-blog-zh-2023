# 从头构建 PCA

> 原文：[`towardsdatascience.com/building-pca-from-the-ground-up-434ac88b03ef?source=collection_archive---------6-----------------------#2023-08-07`](https://towardsdatascience.com/building-pca-from-the-ground-up-434ac88b03ef?source=collection_archive---------6-----------------------#2023-08-07)

## 使用逐步推导来提升你对主成分分析的理解

[](https://harrisonfhoffman.medium.com/?source=post_page-----434ac88b03ef--------------------------------)![哈里森·霍夫曼](https://harrisonfhoffman.medium.com/?source=post_page-----434ac88b03ef--------------------------------)[](https://towardsdatascience.com/?source=post_page-----434ac88b03ef--------------------------------)![数据科学的前沿](https://towardsdatascience.com/?source=post_page-----434ac88b03ef--------------------------------) [哈里森·霍夫曼](https://harrisonfhoffman.medium.com/?source=post_page-----434ac88b03ef--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F38889d0801d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-pca-from-the-ground-up-434ac88b03ef&user=Harrison+Hoffman&userId=38889d0801d0&source=post_page-38889d0801d0----434ac88b03ef---------------------post_header-----------) 发表在 [数据科学的前沿](https://towardsdatascience.com/?source=post_page-----434ac88b03ef--------------------------------) ·12 分钟阅读·2023 年 8 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F434ac88b03ef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-pca-from-the-ground-up-434ac88b03ef&user=Harrison+Hoffman&userId=38889d0801d0&source=-----434ac88b03ef---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F434ac88b03ef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-pca-from-the-ground-up-434ac88b03ef&source=-----434ac88b03ef---------------------bookmark_footer-----------)![](img/09f3176416e889b15a24ea18524b299a.png)

热气球。图片由作者提供。

主成分分析（PCA）是一种常用于降维的[古老](https://en.wikipedia.org/wiki/Principal_component_analysis#History:~:text=PCA%20was%20invented%20in%201901) 技术。尽管在数据科学家中已经是一个知名话题，但 PCA 的推导往往被忽视，这使得我们错过了关于数据本质以及微积分、统计学和线性代数之间关系的宝贵见解。

在本文中，我们将通过一个思想实验推导 PCA，从二维开始，并扩展到任意维度。随着每一步推导的进展，我们将看到数学看似不同分支的和谐互动，最终形成优雅的坐标变换。这一推导将揭示 PCA 的机制，并展示数学概念的迷人相互关联。让我们开始这段对 PCA 及其美的启迪探索吧。

# 在二维中热身

作为生活在三维世界中的人类，我们通常掌握二维概念，而这正是我们在本文中开始的地方。从二维开始将简化我们的首次思想实验，并使我们更好地理解问题的本质。
