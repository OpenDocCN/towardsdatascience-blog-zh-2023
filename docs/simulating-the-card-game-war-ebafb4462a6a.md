# 模拟纸牌游戏《战争》

> 原文：[https://towardsdatascience.com/simulating-the-card-game-war-ebafb4462a6a?source=collection_archive---------23-----------------------#2023-01-24](https://towardsdatascience.com/simulating-the-card-game-war-ebafb4462a6a?source=collection_archive---------23-----------------------#2023-01-24)

## 一个关于具有无限变数的简单游戏的编程故事

[](https://medium.com/@Jake_Mitchell?source=post_page-----ebafb4462a6a--------------------------------)[![Jake Mitchell](../Images/08d2f4af3a58a419b0f9547ca64bbb07.png)](https://medium.com/@Jake_Mitchell?source=post_page-----ebafb4462a6a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ebafb4462a6a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ebafb4462a6a--------------------------------) [Jake Mitchell](https://medium.com/@Jake_Mitchell?source=post_page-----ebafb4462a6a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3d9ea428abd9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimulating-the-card-game-war-ebafb4462a6a&user=Jake+Mitchell&userId=3d9ea428abd9&source=post_page-3d9ea428abd9----ebafb4462a6a---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ebafb4462a6a--------------------------------) ·6 分钟阅读·2023年1月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Febafb4462a6a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimulating-the-card-game-war-ebafb4462a6a&user=Jake+Mitchell&userId=3d9ea428abd9&source=-----ebafb4462a6a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Febafb4462a6a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimulating-the-card-game-war-ebafb4462a6a&source=-----ebafb4462a6a---------------------bookmark_footer-----------)

我模拟了很多游戏——那些需要技能、欺骗或策略的游戏。

**这不是其中之一。**

![](../Images/d27d824ad8b753bec0bbb0a08df402b8.png)

图片由 [Ivan Slade](https://unsplash.com/@flowinteractive?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 引言：

“战争”非常简单。两名玩家各被发到一副标准52张牌的牌组的一半。然后，两人对峙，翻开各自的顶牌。谁的牌最大，谁就拿走这两张牌。如果牌值相同，玩家们将冒险拿出自己牌组中的接下来的3张牌，并用第4张牌进行对决。谁的第4张牌最大，谁就赢得这堆牌（包括自己之前的3张牌和对手的3张牌）。当一名玩家没有剩余的牌时，游戏结束。

赢得胜利没有秘密。你只能希望你的牌比对手的牌价值更高。**你只能听天由命。**

当我开始编写这个游戏代码时，我对牌的起始值是否可以在游戏开始前预测胜者感到兴趣。我没有预料到的是，在适当的情况下，这个游戏实际上可以*永远*进行下去。

## **游戏编码：**

我首先“洗牌”：

```py
deal = randperm(52,52);
  for i = 1:13…
```
