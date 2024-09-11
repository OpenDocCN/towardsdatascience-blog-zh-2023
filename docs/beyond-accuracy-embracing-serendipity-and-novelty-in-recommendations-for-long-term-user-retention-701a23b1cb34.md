# 超越准确性：在长期用户留存中接受偶然性和新颖性的推荐

> 原文：[https://towardsdatascience.com/beyond-accuracy-embracing-serendipity-and-novelty-in-recommendations-for-long-term-user-retention-701a23b1cb34?source=collection_archive---------7-----------------------#2023-06-26](https://towardsdatascience.com/beyond-accuracy-embracing-serendipity-and-novelty-in-recommendations-for-long-term-user-retention-701a23b1cb34?source=collection_archive---------7-----------------------#2023-06-26)

## 推荐系统

## 对于良好推荐和长期用户留存的因素进行考察

[](https://medium.com/@christabellecp?source=post_page-----701a23b1cb34--------------------------------)[![Christabelle Pabalan](../Images/24187865b6e9d03ae1aabf873ce1e67c.png)](https://medium.com/@christabellecp?source=post_page-----701a23b1cb34--------------------------------)[](https://towardsdatascience.com/?source=post_page-----701a23b1cb34--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----701a23b1cb34--------------------------------) [Christabelle Pabalan](https://medium.com/@christabellecp?source=post_page-----701a23b1cb34--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4200eb8e8b26&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-accuracy-embracing-serendipity-and-novelty-in-recommendations-for-long-term-user-retention-701a23b1cb34&user=Christabelle+Pabalan&userId=4200eb8e8b26&source=post_page-4200eb8e8b26----701a23b1cb34---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----701a23b1cb34--------------------------------) ·10分钟阅读·2023年6月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F701a23b1cb34&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-accuracy-embracing-serendipity-and-novelty-in-recommendations-for-long-term-user-retention-701a23b1cb34&user=Christabelle+Pabalan&userId=4200eb8e8b26&source=-----701a23b1cb34---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F701a23b1cb34&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-accuracy-embracing-serendipity-and-novelty-in-recommendations-for-long-term-user-retention-701a23b1cb34&source=-----701a23b1cb34---------------------bookmark_footer-----------)![](../Images/e8c54a4ea7faab00502f8dfcac239192.png)

作者使用 DALL-E 创作

## 在咖啡馆建立的信任纽带

你坐在咖啡馆里，品尝着你最喜欢的咖啡变种（*当然*是卡布奇诺），与朋友聊得非常投入。随着谈话的进行，话题转向了你们都一直迷上的最新引人入胜的电视系列剧。共同的兴奋感建立了一种信任，直到你的朋友迫切地转向你问道：“我接下来应该看什么？你有什么推荐？”

在那一刻，你成为了他们娱乐体验的策展人。你感到有责任维护他们的信任，并提供保证能吸引他们的建议。此外，你也对有机会向他们介绍一个他们之前未曾探索过的新类型或故事情节感到兴奋。

**但是，考虑到为你的朋友推荐完美的内容时，哪些因素会影响你的决策过程？**

# 什么造就了一个好的……
