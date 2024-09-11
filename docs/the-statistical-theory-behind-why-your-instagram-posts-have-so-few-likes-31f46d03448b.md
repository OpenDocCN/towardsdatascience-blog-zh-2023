# 为什么你的 Instagram 帖子这么少赞的统计理论

> 原文：[https://towardsdatascience.com/the-statistical-theory-behind-why-your-instagram-posts-have-so-few-likes-31f46d03448b?source=collection_archive---------6-----------------------#2023-12-28](https://towardsdatascience.com/the-statistical-theory-behind-why-your-instagram-posts-have-so-few-likes-31f46d03448b?source=collection_archive---------6-----------------------#2023-12-28)

## *一个计算不可计数事物的实用技巧——从 Instagram 影响力到企鹅种群，再到北伦敦的曼城球迷*

[](https://tuannguyen-doan.medium.com/?source=post_page-----31f46d03448b--------------------------------)[![Tuan Nguyen Doan](../Images/a7f477026bd101bfe74a25c02f9ee1a8.png)](https://tuannguyen-doan.medium.com/?source=post_page-----31f46d03448b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----31f46d03448b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----31f46d03448b--------------------------------) [Tuan Nguyen Doan](https://tuannguyen-doan.medium.com/?source=post_page-----31f46d03448b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F87144e3252f6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-statistical-theory-behind-why-your-instagram-posts-have-so-few-likes-31f46d03448b&user=Tuan+Nguyen+Doan&userId=87144e3252f6&source=post_page-87144e3252f6----31f46d03448b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----31f46d03448b--------------------------------) ·6分钟阅读·2023年12月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F31f46d03448b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-statistical-theory-behind-why-your-instagram-posts-have-so-few-likes-31f46d03448b&user=Tuan+Nguyen+Doan&userId=87144e3252f6&source=-----31f46d03448b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F31f46d03448b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-statistical-theory-behind-why-your-instagram-posts-have-so-few-likes-31f46d03448b&source=-----31f46d03448b---------------------bookmark_footer-----------)![](../Images/31b16ea3c54f4a33f1c56db4884ce45c.png)

图片来自 [StockSnap](https://pixabay.com/users/stocksnap-894430/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2570925) 的 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2570925)

我们都经历过这样的情况……

带着希望的目光，我们通过 iPhone 15 镜头捕捉了圣诞节的精神，用艺术家的执着和诗人的灵魂记录了晚餐桌，每张照片都是一个充满朋友的节日小插曲，有着闪闪发光的火鸡和装饰品的光辉。我们精心为每张快照配上了积极向上的说明文字，这些文字小心编织，期望每个帖子能在 Instagram 繁忙的节日流量中引起共鸣。

尽管我们努力将节日的氛围展现给数字世界，但我们的 Instagram 帖子仅获得了 15 个赞（如果计算在 Facebook 上的转发，总共 20 个）。

![](../Images/e46bbee26a913e48c5ca73bad41216fe.png)

图片由 [Gerd Altmann](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=6038943) 提供，来源于 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=6038943)

也许期待社交媒体的轰动效果在只有 300 个 Instagram 关注者的情况下是不现实的，但这些充满真挚情感的圣诞帖子肯定会让超过 5% 的朋友做出反应，对吧？问题是，你的…
