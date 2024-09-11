# Git 101 — 从术语到架构与工作流

> 原文：[`towardsdatascience.com/git-101-from-terminologies-to-architecture-and-workflows-78cb6d735798?source=collection_archive---------6-----------------------#2023-05-21`](https://towardsdatascience.com/git-101-from-terminologies-to-architecture-and-workflows-78cb6d735798?source=collection_archive---------6-----------------------#2023-05-21)

## Git 的幕后和如何高效使用 Git

[](https://kayjanwong.medium.com/?source=post_page-----78cb6d735798--------------------------------)![Kay Jan Wong](https://kayjanwong.medium.com/?source=post_page-----78cb6d735798--------------------------------)[](https://towardsdatascience.com/?source=post_page-----78cb6d735798--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----78cb6d735798--------------------------------) [Kay Jan Wong](https://kayjanwong.medium.com/?source=post_page-----78cb6d735798--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffee8693930fb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgit-101-from-terminologies-to-architecture-and-workflows-78cb6d735798&user=Kay+Jan+Wong&userId=fee8693930fb&source=post_page-fee8693930fb----78cb6d735798---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----78cb6d735798--------------------------------) ·7 分钟阅读·2023 年 5 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F78cb6d735798&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgit-101-from-terminologies-to-architecture-and-workflows-78cb6d735798&user=Kay+Jan+Wong&userId=fee8693930fb&source=-----78cb6d735798---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F78cb6d735798&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgit-101-from-terminologies-to-architecture-and-workflows-78cb6d735798&source=-----78cb6d735798---------------------bookmark_footer-----------)![](img/abc3db02185eba249984afdc9e69a784.png)

照片由 [Roman Synkevych](https://unsplash.com/@synkevych?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

所以你已经了解了`git add`、`git commit`和`git push`，但每一步到底实现了什么——你的*本地*和*远程*仓库中发生了什么？你可以使用哪些不同的合并策略和分支策略来充分利用 Git 的强大功能？

在浏览了许多仅展示 Git 命令的文章后，我觉得了解 Git 的架构对真正欣赏和理解发生的事情是至关重要的。本文将涉及 Git 的基本术语，逐步建立 Git 的架构，最终，你可以考虑在下一个编码项目中采用的不同常见 Git 工作流程！

# 目录

+   [什么是 Git](https://medium.com/p/78cb6d735798/#34c1)

+   [Git 术语](https://medium.com/p/78cb6d735798/#d4b5)

+   [Git 架构](https://medium.com/p/78cb6d735798/#3d8c)

+   [分支、合并和合并冲突](https://medium.com/p/78cb6d735798/#c5e6)

+   [Git 工作流程](https://medium.com/p/78cb6d735798/#414a)

# 什么是 Git

简而言之，Git 是一个帮助进行版本控制的工具，可以帮助团队以敏捷的方式管理他们的代码——从跟踪代码更改历史到处理代码更改中的冲突等等。
