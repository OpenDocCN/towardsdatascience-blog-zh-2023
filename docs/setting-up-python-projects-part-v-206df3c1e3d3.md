# 设置 Python 项目：第五部分

> 原文：[`towardsdatascience.com/setting-up-python-projects-part-v-206df3c1e3d3?source=collection_archive---------0-----------------------#2023-01-14`](https://towardsdatascience.com/setting-up-python-projects-part-v-206df3c1e3d3?source=collection_archive---------0-----------------------#2023-01-14)

## 掌握 Python 项目设置的艺术：逐步指南

[](https://johschmidt42.medium.com/?source=post_page-----206df3c1e3d3--------------------------------)![Johannes Schmidt](https://johschmidt42.medium.com/?source=post_page-----206df3c1e3d3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----206df3c1e3d3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----206df3c1e3d3--------------------------------) [Johannes Schmidt](https://johschmidt42.medium.com/?source=post_page-----206df3c1e3d3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb5022ff2e428&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsetting-up-python-projects-part-v-206df3c1e3d3&user=Johannes+Schmidt&userId=b5022ff2e428&source=post_page-b5022ff2e428----206df3c1e3d3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----206df3c1e3d3--------------------------------) ·20 分钟阅读·2023 年 1 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F206df3c1e3d3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsetting-up-python-projects-part-v-206df3c1e3d3&user=Johannes+Schmidt&userId=b5022ff2e428&source=-----206df3c1e3d3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F206df3c1e3d3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsetting-up-python-projects-part-v-206df3c1e3d3&source=-----206df3c1e3d3---------------------bookmark_footer-----------)![](img/c80bbfc10240ac4b4126a69759a6a36e.png)

照片由 [Zoya Loonohod](https://unsplash.com/@loonohod?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

无论你是经验丰富的开发者还是刚刚开始接触 🐍 **Python**，了解如何构建稳健且易于维护的项目都是重要的。本教程将引导你使用行业内一些最受欢迎和有效的工具来设置 Python 项目。你将学习如何使用 [GitHub](https://github.com/) 和 [GitHub Actions](https://github.com/features/actions) 进行版本控制和持续集成，以及其他工具来进行测试、文档编写、打包和分发。该教程受到 [Hypermodern Python](https://medium.com/@cjolowicz/hypermodern-python-d44485d9d769) 和 [新 Python 项目的最佳实践](https://mitelman.engineering/blog/python-best-practice/automating-python-best-practices-for-a-new-project/) 等资源的启发。然而，这并不是唯一的方法，你可能会有不同的偏好或观点。教程旨在对初学者友好，但也涵盖一些高级主题。在每一节中，你将自动化一些任务，并为你的项目添加徽章以展示你的进展和成就。

该系列的代码库可以在 [github.com/johschmidt42/python-project-johannes](https://github.com/johschmidt42/python-project-johannes) 找到

这一部分的灵感来自这篇博客文章：

[**使用 Python、Poetry 和 GitHub Actions 进行语义发布 🚀**

*我计划根据一些同事的兴趣向 Dr. Sven 添加一些功能。在这样做之前，我需要*](https://mestrak.com/blog/semantic-release-with-python-poetry-github-actions-20nn)…
