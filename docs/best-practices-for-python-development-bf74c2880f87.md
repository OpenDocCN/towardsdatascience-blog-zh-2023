# Python 开发最佳实践

> 原文：[https://towardsdatascience.com/best-practices-for-python-development-bf74c2880f87?source=collection_archive---------3-----------------------#2023-02-08](https://towardsdatascience.com/best-practices-for-python-development-bf74c2880f87?source=collection_archive---------3-----------------------#2023-02-08)

## 设置并使用一个专业的 Python 存储库

[](https://medium.com/@hrmnmichaels?source=post_page-----bf74c2880f87--------------------------------)[![Oliver S](../Images/b5ee0fa2d5fb115f62e2e9dfcb92afdd.png)](https://medium.com/@hrmnmichaels?source=post_page-----bf74c2880f87--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bf74c2880f87--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bf74c2880f87--------------------------------) [Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----bf74c2880f87--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff2daf6260cca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbest-practices-for-python-development-bf74c2880f87&user=Oliver+S&userId=f2daf6260cca&source=post_page-f2daf6260cca----bf74c2880f87---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bf74c2880f87--------------------------------) · 8 分钟阅读 · 2023年2月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbf74c2880f87&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbest-practices-for-python-development-bf74c2880f87&user=Oliver+S&userId=f2daf6260cca&source=-----bf74c2880f87---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbf74c2880f87&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbest-practices-for-python-development-bf74c2880f87&source=-----bf74c2880f87---------------------bookmark_footer-----------)![](../Images/89bfd14b512226b92a7b815046f0d5c8.png)

图片由 [Christina @ wocintechchat.com](https://unsplash.com/@wocintechchat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/EkeThvO9VfM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

本文的目标是分享 Python 开发的最佳实践——特别是如何设置、使用和管理一个符合行业专业标准的 Github 仓库。我们将讨论保持代码整洁和无错误的有用工具，展示如何设置仓库并包含之前介绍的用于自动化 CI（持续集成）检查的工具——最后将所有这些整合到一个示例项目中。请注意，我并不声称这个列表是完整的或唯一的方法。然而，我想分享我作为软件工程师的专业经验，并可以确认许多大型软件公司遵循类似的模式。

话虽如此，让我们直接进入重点——希望你觉得这很有用！你可以在[这里](https://github.com/hermanmichaels/python-sample)找到本文的完整代码，并在我们进行过程中跟随。

# 使用的工具

在本节中，我们将介绍本文使用的工具。

## poetry

Poetry 是一个管理 Python 版本和依赖关系的整洁工具。它使得控制和修复 Python 版本以及以集中方式管理依赖关系变得简单。在所有的工具中，我推荐使用 poetry。我在[这里](https://medium.com/@hrmnmichaels/dependency-management-with-poetry-f1d598591161)写了一篇更详细的介绍。
