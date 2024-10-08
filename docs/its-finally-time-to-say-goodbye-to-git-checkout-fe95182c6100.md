# 终于是时候告别 “git checkout” 了

> 原文：[`towardsdatascience.com/its-finally-time-to-say-goodbye-to-git-checkout-fe95182c6100`](https://towardsdatascience.com/its-finally-time-to-say-goodbye-to-git-checkout-fe95182c6100)

## “Git switch”和“git restore”将会长期存在

[](https://medium.com/@tomergabay?source=post_page-----fe95182c6100--------------------------------)![Tomer Gabay](https://medium.com/@tomergabay?source=post_page-----fe95182c6100--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fe95182c6100--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fe95182c6100--------------------------------) [Tomer Gabay](https://medium.com/@tomergabay?source=post_page-----fe95182c6100--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fe95182c6100--------------------------------) ·4 分钟阅读·2023 年 5 月 1 日

--

![](img/fa4cef177784a13c0c524ea2c5fb5a36.png)

图片由 [Dim Hou](https://unsplash.com/@dimhou?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Git 是开发人员最广泛使用的版本控制系统。Git 中最常用的命令之一是 `git checkout`，它允许用户在分支之间切换并将文件恢复到先前的状态。

然而，在 2019 年，随着 Git 2.23 的发布，两个新命令被引入以取代 `git checkout`，以实现更直观和简化的工作流：`git switch` 和 `git restore`。尽管它们发布已有近四年，开发人员仍然难以放弃使用 `git checkout`（我猜老习惯很难改）。

在这篇文章中，我们将探讨 Git 团队通过引入 `git switch` 和 `git restore` 解决了 `git checkout` 的哪些缺点，以及为什么这应该导致不再使用 `git checkout`。

# git checkout 的问题

`git checkout` 是一个具有两个核心功能的命令：

+   在分支之间切换

+   将文件恢复到先前状态

然而，这两个功能在命令语法中没有明确区分，这可能导致混淆和错误。例如，如果你不小心输入 `git checkout <commit>` 而不是 `git checkout <branch>`，你将进入所谓的“分离的 HEAD” 状态，这意味着你所做的任何新提交将不会与任何分支相关联。当你在分离的 HEAD 中做了更改后切换到新分支时，这些更改将会丢失，且无法恢复。

# 引入 git switch 和 git restore

为了解决`git checkout`的一些问题，Git 团队在 Git 版本 2.23 中引入了两个新命令：`git switch`和`git restore`。这两个命令将`git checkout`之前提到的两个核心功能拆分为两个独立的命令。

`git switch`仅用于在分支间切换。语法很简单：`git switch <branch>`。如果你尝试使用`git switch`切换到一个提交，Git 会抛出一个错误，而不是将你置于脱离的 HEAD 状态。因此，你不太可能进行与任何分支无关的提交。

另一方面，`git restore`只能用于将文件恢复到之前的状态。它的语法也很简单：`git restore <file>`。如果你不小心尝试切换到一个分支而不是文件，Git 将会抛出一个错误。

# 使用`git switch`和`git restore`的好处

`git switch`和`git restore`的一个关键好处是提高了安全性。由于两个功能被分离成两个不同的命令，你无意中应用错误功能的机会减少了。这使得因为丢失工作而发生错误和挫败感的可能性降低了。

`git switch`和`git restore`的另一个好处是语法更直接，因为你不需要记住`git checkout`的不同用法。即使在使用 Git 多年之后，许多开发者仍然无法完全理解`git checkout`的工作原理和语法。

此外，`git switch`和`git restore`的命令更直观且准确。当你想创建一个新分支时，可以使用`git switch -c <branch>`，其中`-c`标志表示*创建*。在使用`checkout`时，你必须使用`git checkout -b <branch>`，其中`-b`标志表示*分支*。然而，你也可以使用`git checkout <branch>`在分支间切换，因此`-b`标志仅用于创建新分支，而一个名为*branch*的标志并不能很好地表明这是关于创建新分支而不是切换到一个已有的分支。因此，`git switch`和`git restore`的命令更直观，因此应当被优先使用。

# **总而言之**

虽然`git checkout`多年来一直是 Git 用户的首选命令，但随着`git switch`和`git restore`的引入，已经有了更好的替代方案。将`git checkout`的两个核心功能拆分成两个独立的命令后，Git 使得在分支间切换和恢复文件变得更加容易，而不会不小心使用错误的命令。如果你仍在使用`git checkout`而不是`git switch`和`git restore`，现在正是切换的最佳时机！

# 资源

[](https://git-scm.com/?source=post_page-----fe95182c6100--------------------------------) [## Git

### Git 易于学习，体积小，性能极快。它超越了像 Subversion 这样的 SCM 工具…

[Git 2.23 发布，新增两个命令 'git switch' 和 'git restore'，还有更多内容！](https://git-scm.com/?source=post_page-----fe95182c6100--------------------------------)

### 上周，Git 团队发布了包含实验性命令、向后兼容等更多功能的版本…

[Hub PacktPub](https://hub.packtpub.com/git-2-23-released-with-two-new-commands-git-switch-and-git-restore-a-new-tutorial-and-much-more/?source=post_page-----fe95182c6100--------------------------------) [](https://stackoverflow.com/questions/57265785/whats-the-difference-between-git-switch-and-git-checkout-branch?source=post_page-----fe95182c6100--------------------------------) [## git switch 和 git checkout 有什么区别

### `switch` 命令确实与 `checkout` 做了相同的事情，但仅限于切换分支的用法。在…

[Stack Overflow](https://stackoverflow.com/questions/57265785/whats-the-difference-between-git-switch-and-git-checkout-branch?source=post_page-----fe95182c6100--------------------------------)
