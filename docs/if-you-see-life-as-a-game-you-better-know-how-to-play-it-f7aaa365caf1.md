# 如果你把生活看作游戏，你最好知道怎么玩它

> 原文：[https://towardsdatascience.com/if-you-see-life-as-a-game-you-better-know-how-to-play-it-f7aaa365caf1?source=collection_archive---------3-----------------------#2023-12-02](https://towardsdatascience.com/if-you-see-life-as-a-game-you-better-know-how-to-play-it-f7aaa365caf1?source=collection_archive---------3-----------------------#2023-12-02)

## 博弈论如何帮助你做出每日决策

[](https://medium.com/@manfred.james?source=post_page-----f7aaa365caf1--------------------------------)[![迭戈·曼弗雷](../Images/2189d8e63df449a869526bf8b6c50440.png)](https://medium.com/@manfred.james?source=post_page-----f7aaa365caf1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f7aaa365caf1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f7aaa365caf1--------------------------------) [迭戈·曼弗雷](https://medium.com/@manfred.james?source=post_page-----f7aaa365caf1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6e3d8f9df1a5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fif-you-see-life-as-a-game-you-better-know-how-to-play-it-f7aaa365caf1&user=Diego+Manfre&userId=6e3d8f9df1a5&source=post_page-6e3d8f9df1a5----f7aaa365caf1---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f7aaa365caf1--------------------------------) · 13分钟阅读 · 2023年12月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff7aaa365caf1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fif-you-see-life-as-a-game-you-better-know-how-to-play-it-f7aaa365caf1&user=Diego+Manfre&userId=6e3d8f9df1a5&source=-----f7aaa365caf1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff7aaa365caf1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fif-you-see-life-as-a-game-you-better-know-how-to-play-it-f7aaa365caf1&source=-----f7aaa365caf1---------------------bookmark_footer-----------)![](../Images/5a7467228767af3e775dcdf50280118a.png)

图片由作者使用 [Midjourney](https://www.midjourney.com/home/?callbackUrl=%2Fapp%2F) 制作

在一个未知的银河系中，你和你的朋友被一群神秘的外星生物俘获。他们承诺，如果你们能在他们的一场游戏中战胜他们，就会放你们离开。你坐在一个宇宙桌旁，与一个外星人对面坐着，那个外星人分发包含黑色和白色石头的袋子。当游戏开始时，每个玩家都会选择一块石头并将其放在桌子上。获胜者是选择颜色不同的石头的人。这意味着，当其他两个人选择白色而你选择黑色，或者当其他两个人选择黑色而你选择白色时，你就会赢。每次有人获胜时，他们会给你一个硬币，将其堆放在你桌子的一侧。你们将玩一百万次游戏，最终获胜者是硬币最多的人。除了游戏规则，你唯一知道的其他信息是，外星人总是以50%的概率选择黑色。在游戏开始的几秒钟前，你的伙伴低声告诉你应该遵循的策略。一百万场游戏结束后，最终得分宣布你们两个都是获胜者。这是纯粹的运气吗？你伙伴的策略有多重要？

# 介绍

你最终陷入类似描述的情况的可能性极低……
