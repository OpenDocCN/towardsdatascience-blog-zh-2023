# 现在是时候告别“git checkout”了

> 原文：[`towardsdatascience.com/its-finally-time-to-say-goodbye-to-git-checkout-fe95182c6100?source=collection_archive---------4-----------------------#2023-05-01`](https://towardsdatascience.com/its-finally-time-to-say-goodbye-to-git-checkout-fe95182c6100?source=collection_archive---------4-----------------------#2023-05-01)

## “Git switch” 和 “git restore” 将会留存下来

[](https://medium.com/@tomergabay?source=post_page-----fe95182c6100--------------------------------)![Tomer Gabay](https://medium.com/@tomergabay?source=post_page-----fe95182c6100--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fe95182c6100--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fe95182c6100--------------------------------) [Tomer Gabay](https://medium.com/@tomergabay?source=post_page-----fe95182c6100--------------------------------)

·

[-   追随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc9c352dba00a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fits-finally-time-to-say-goodbye-to-git-checkout-fe95182c6100&user=Tomer+Gabay&userId=c9c352dba00a&source=post_page-c9c352dba00a----fe95182c6100---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fe95182c6100--------------------------------) ·4 分钟阅读·2023 年 5 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffe95182c6100&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fits-finally-time-to-say-goodbye-to-git-checkout-fe95182c6100&user=Tomer+Gabay&userId=c9c352dba00a&source=-----fe95182c6100---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffe95182c6100&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fits-finally-time-to-say-goodbye-to-git-checkout-fe95182c6100&source=-----fe95182c6100---------------------bookmark_footer-----------)![](img/fa4cef177784a13c0c524ea2c5fb5a36.png)

[Dim Hou](https://unsplash.com/@dimhou?utm_source=medium&utm_medium=referral)的照片，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Git 是开发者们最广泛使用的版本控制系统之一。其中与 Git 一起使用最频繁的命令之一是`git checkout`，它允许用户在不同分支之间切换并将文件恢复到之前的某个状态。

然而，在 2019 年，随着 Git 2.23 的发布，两个新命令被引入以取代 `git checkout`，以实现更直观和流畅的工作流程：`git switch` 和 `git restore`。尽管它们发布已经快 4 年了，但开发者们仍然不愿意放弃使用 `git checkout`（旧习惯难以改变，我想）。

在这篇文章中，我们将探讨 `git checkout` 的哪些缺点是 Git 团队通过引入 `git switch` 和 `git restore` 试图解决的，以及这为何意味着不再使用 `git checkout`。

# git checkout 的问题

`git checkout` 是一个具有两个核心功能的命令：

+   在分支之间切换

+   将文件恢复到以前的状态

然而，这两个功能在命令语法中没有明确区分，这可能导致混淆和错误。例如，如果你不小心输入了 `git checkout <commit>` 而不是 `git`…
