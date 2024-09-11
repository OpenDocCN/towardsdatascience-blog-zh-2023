# Sklearn 教程：模块 3

> 原文：[`towardsdatascience.com/sklearn-tutorial-module-3-08c9ae5cb8fa?source=collection_archive---------14-----------------------#2023-12-01`](https://towardsdatascience.com/sklearn-tutorial-module-3-08c9ae5cb8fa?source=collection_archive---------14-----------------------#2023-12-01)

## 我参加了官方的 sklearn MOOC 教程。以下是我的收获。

[](https://mocquin.medium.com/?source=post_page-----08c9ae5cb8fa--------------------------------)![Yoann Mocquin](https://mocquin.medium.com/?source=post_page-----08c9ae5cb8fa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----08c9ae5cb8fa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----08c9ae5cb8fa--------------------------------) [Yoann Mocquin](https://mocquin.medium.com/?source=post_page-----08c9ae5cb8fa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F173731d06320&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsklearn-tutorial-module-3-08c9ae5cb8fa&user=Yoann+Mocquin&userId=173731d06320&source=post_page-173731d06320----08c9ae5cb8fa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----08c9ae5cb8fa--------------------------------) ·9 分钟阅读·2023 年 12 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F08c9ae5cb8fa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsklearn-tutorial-module-3-08c9ae5cb8fa&user=Yoann+Mocquin&userId=173731d06320&source=-----08c9ae5cb8fa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F08c9ae5cb8fa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsklearn-tutorial-module-3-08c9ae5cb8fa&source=-----08c9ae5cb8fa---------------------bookmark_footer-----------)

这是我 scikit-learn 教程系列的第三篇文章。如果你没有看到前两篇，我强烈推荐你先阅读这两篇 —— 这样会更容易跟上：

[](/sklearn-tutorial-module-1-f31b3964a3b4?source=post_page-----08c9ae5cb8fa--------------------------------) [## Sklearn 教程：模块 1

### 我参加了官方的 sklearn MOOC 教程。以下是我的收获。

[`towardsdatascience.com/sklearn-tutorial-module-1-f31b3964a3b4?source=post_page-----08c9ae5cb8fa--------------------------------`](https://towardsdatascience.com/sklearn-tutorial-module-1-f31b3964a3b4?source=post_page-----08c9ae5cb8fa--------------------------------) [](/sklearn-tutorial-module-2-0739c44f595a?source=post_page-----08c9ae5cb8fa--------------------------------) [## Sklearn 教程：模块 2

### 我参加了官方的 sklearn MOOC 教程。以下是我的收获。

[`towardsdatascience.com/sklearn-tutorial-module-2-0739c44f595a?source=post_page-----08c9ae5cb8fa--------------------------------`](https://towardsdatascience.com/sklearn-tutorial-module-2-0739c44f595a?source=post_page-----08c9ae5cb8fa--------------------------------)

在这个第三模块中，我们将深入探讨超参数是什么，以及为什么和如何优化它们。

![](img/78274fa0cd73bdb11c003e4596c49c6a.png)

照片由[Glenn Carstens-Peters](https://unsplash.com/@glenncarstenspeters?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

# 什么是超参数

目前在设置我们的模型时，我们只更改了预处理、模型种类或两者兼而有之——但我们实际上还没有调整模型的超参数。

模型的超参数是由我们数据科学家在创建模型/管道时设定的参数。这些参数在模型接收任何数据之前定义了模型。你可以说它们…
