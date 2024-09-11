# 你应该了解的 6 种与索引相关的常见操作

> 原文：[`towardsdatascience.com/6-common-index-related-operations-you-should-know-about-pandas-783fdba59768?source=collection_archive---------6-----------------------#2023-10-17`](https://towardsdatascience.com/6-common-index-related-operations-you-should-know-about-pandas-783fdba59768?source=collection_archive---------6-----------------------#2023-10-17)

## 在你的数据框中有效地处理索引

[](https://yongcui01.medium.com/?source=post_page-----783fdba59768--------------------------------)![Yong Cui](https://yongcui01.medium.com/?source=post_page-----783fdba59768--------------------------------)[](https://towardsdatascience.com/?source=post_page-----783fdba59768--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----783fdba59768--------------------------------) [Yong Cui](https://yongcui01.medium.com/?source=post_page-----783fdba59768--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F88ff1e2545d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F6-common-index-related-operations-you-should-know-about-pandas-783fdba59768&user=Yong+Cui&userId=88ff1e2545d0&source=post_page-88ff1e2545d0----783fdba59768---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----783fdba59768--------------------------------) · 9 分钟阅读 · 2023 年 10 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F783fdba59768&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F6-common-index-related-operations-you-should-know-about-pandas-783fdba59768&user=Yong+Cui&userId=88ff1e2545d0&source=-----783fdba59768---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F783fdba59768&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F6-common-index-related-operations-you-should-know-about-pandas-783fdba59768&source=-----783fdba59768---------------------bookmark_footer-----------)![](img/6020659939470dfe03adf748de117f0e.png)

图片来源：[Alejandro Luengo](https://unsplash.com/@aluengo91?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

想象一下你有一个满是成千上万本书的图书馆，每本书都蕴藏着丰富的信息。为了找到你需要的那本书，你会查阅图书馆的索引（如果有的话），对吧？在处理现实世界的数据时，拥有类似图书馆的索引对于你在海量数据中筛选、精确找到所需信息至关重要，而无需逐一翻查每一部分。

在本文中，我将分享一些常见但重要的与索引相关的操作，并通过简单的适用场景来进行拆解。无论你是数据新手还是经验丰富的专家，你都很快会发现这些操作如何成为你数据的最佳助手。

话不多说，我们开始吧。

> 简要说明一下，在数据框中，行和列都被视为索引，但在大多数数据操作中，我们通常将行视为关注的索引，因为许多数据集以宽格式呈现——每一行代表一个数据记录，而列代表数据记录的不同方面。
> 
> 在本文中，我们将重点讨论沿行操作索引。也就是说，每个索引项对应一行。

## 1\. 设置索引
