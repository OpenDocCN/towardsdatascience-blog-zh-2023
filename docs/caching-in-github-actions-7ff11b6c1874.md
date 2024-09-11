# 在 GitHub Actions 中使用缓存

> 原文：[`towardsdatascience.com/caching-in-github-actions-7ff11b6c1874?source=collection_archive---------14-----------------------#2023-11-21`](https://towardsdatascience.com/caching-in-github-actions-7ff11b6c1874?source=collection_archive---------14-----------------------#2023-11-21)

## 加速您的 CI/CD 流水线

[](https://medium.com/@hrmnmichaels?source=post_page-----7ff11b6c1874--------------------------------)![Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----7ff11b6c1874--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7ff11b6c1874--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7ff11b6c1874--------------------------------) [Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----7ff11b6c1874--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff2daf6260cca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcaching-in-github-actions-7ff11b6c1874&user=Oliver+S&userId=f2daf6260cca&source=post_page-f2daf6260cca----7ff11b6c1874---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----7ff11b6c1874--------------------------------) ·7 分钟阅读·2023 年 11 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7ff11b6c1874&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcaching-in-github-actions-7ff11b6c1874&user=Oliver+S&userId=f2daf6260cca&source=-----7ff11b6c1874---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7ff11b6c1874&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcaching-in-github-actions-7ff11b6c1874&source=-----7ff11b6c1874---------------------bookmark_footer-----------)

在本篇文章中，我们将探讨如何缓存[GitHub Actions](https://docs.github.com/en/actions)。GitHub Actions 是 GitHub 的一个平台，可以用来自动化工作流程，通常用于 CI/CD（持续集成/持续交付）流水线，例如在想要合并新的 PR 时自动运行单元测试。由于这些流水线经常运行，其执行时间可能显著增加，因此查找节省时间的方法非常有必要——缓存操作输出就是其中之一。

![](img/60ed35c51b86a7f2379b590ccccb0a4b.png)

照片由[Possessed Photography](https://unsplash.com/@possessedphotography?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)在[Unsplash](https://unsplash.com/photos/sodimm-ram-stick-nuc3NFB_6po?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)上拍摄。

在这篇文章中，我们将讨论缓存。我觉得官方文档比较简略，留下了一些未解答的问题——因此我在这里想要多解释一些。我们从对 Github Actions 的简要介绍及缓存工作原理开始，然后通过两个例子来演示这一点：第一个例子跟随了原始的关于生成质数的玩具示例，而第二个则更具实际性——我们缓存了整个 Python 环境。

# Github Actions 简介

在[之前的文章](https://medium.com/gitconnected/introduction-to-github-actions-e742b5370bfa)中，我更详细地介绍了这个话题——因此这里我们只是简要覆盖一下，并建议阅读链接文章以获取详细信息。然而，总的来说，Github Actions 允许自动化工作流程，通常用于 CI/CD 管道，例如运行单元测试、检查样式指南等……
