# 变压器能学会制定策略吗？

> 原文：[`towardsdatascience.com/can-transformers-learn-to-strategize-862770c996ea?source=collection_archive---------6-----------------------#2023-09-08`](https://towardsdatascience.com/can-transformers-learn-to-strategize-862770c996ea?source=collection_archive---------6-----------------------#2023-09-08)

## TicTacGPT 用于玩简单的棋盘游戏

[](https://charlieoneill.medium.com/?source=post_page-----862770c996ea--------------------------------)![查理·奥尼尔](https://charlieoneill.medium.com/?source=post_page-----862770c996ea--------------------------------)[](https://towardsdatascience.com/?source=post_page-----862770c996ea--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----862770c996ea--------------------------------) [查理·奥尼尔](https://charlieoneill.medium.com/?source=post_page-----862770c996ea--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2742adff9d49&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-transformers-learn-to-strategize-862770c996ea&user=Charlie+O%27Neill&userId=2742adff9d49&source=post_page-2742adff9d49----862770c996ea---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----862770c996ea--------------------------------) ·27 分钟阅读·2023 年 9 月 8 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F862770c996ea&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-transformers-learn-to-strategize-862770c996ea&user=Charlie+O%27Neill&userId=2742adff9d49&source=-----862770c996ea---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F862770c996ea&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-transformers-learn-to-strategize-862770c996ea&source=-----862770c996ea---------------------bookmark_footer-----------)![](img/105981c00690836c0dd2d1ea74863f8b.png)

由[乔恩·泰森](https://unsplash.com/@jontyson?utm_source=medium&utm_medium=referral)拍摄，图片来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

尽管大多数棋盘游戏倾向于使用卷积神经网络或其他几何灵感的架构，但实际上我们可以将棋盘状态表示为字符串，这引出了一个问题，即变压器（transformers）是否可以自然地应用于游戏。在这里，我们将看看是否能在简单的井字棋游戏的背景下回答这个问题。虽然这看起来可能不太实际（几乎每个人都知道这个游戏中存在一种简单易学的封闭式纳什均衡策略），但它是我们问题的一个有用测试平台。原因在于，这个游戏足够简单，我们可以轻松训练一个变压器来玩，但又足够复杂，不会立刻显而易见什么是最佳策略。

# 实现游戏

我们将从实现一个`TicTacToe`类开始。这是相当直接的。我们希望能够将棋盘表示为一个由 9 个字符组成的字符串，每个字符代表一个格子。我们将使用`X`表示第一玩家，`O`表示第二玩家，`-`表示空格。我们还将跟踪轮到谁来走棋，以及游戏是否结束。我们还会跟踪是否有赢家。最后，我们将包含一个打印棋盘的好方法，这样我们就不必在...
