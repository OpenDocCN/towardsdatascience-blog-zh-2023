# Python 项目设置：第六部分

> 原文：[https://towardsdatascience.com/setting-up-python-projects-part-vi-cbdbf28eff53?source=collection_archive---------14-----------------------#2023-04-10](https://towardsdatascience.com/setting-up-python-projects-part-vi-cbdbf28eff53?source=collection_archive---------14-----------------------#2023-04-10)

## 掌握 Python 项目设置的艺术：逐步指南

[](https://johschmidt42.medium.com/?source=post_page-----cbdbf28eff53--------------------------------)[![约翰内斯·施密特](../Images/e0cacf7ff37f339a9bf8bd33c7c83a4d.png)](https://johschmidt42.medium.com/?source=post_page-----cbdbf28eff53--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cbdbf28eff53--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cbdbf28eff53--------------------------------) [约翰内斯·施密特](https://johschmidt42.medium.com/?source=post_page-----cbdbf28eff53--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb5022ff2e428&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsetting-up-python-projects-part-vi-cbdbf28eff53&user=Johannes+Schmidt&userId=b5022ff2e428&source=post_page-b5022ff2e428----cbdbf28eff53---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cbdbf28eff53--------------------------------) · 26分钟阅读 · 2023年4月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcbdbf28eff53&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsetting-up-python-projects-part-vi-cbdbf28eff53&user=Johannes+Schmidt&userId=b5022ff2e428&source=-----cbdbf28eff53---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcbdbf28eff53&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsetting-up-python-projects-part-vi-cbdbf28eff53&source=-----cbdbf28eff53---------------------bookmark_footer-----------)![](../Images/1093eac93ddb53f3ce2570a3fa7c32bf.png)

照片由 [Amira El Fohail](https://unsplash.com/@amirasartistry?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

无论你是经验丰富的开发者还是刚刚开始接触 🐍 **Python**，了解如何构建稳健且易于维护的项目都是很重要的。本教程将指导你使用一些业内最受欢迎和有效的工具来设置一个 Python 项目。你将学习如何使用 [GitHub](https://github.com/) 和 [GitHub Actions](https://github.com/features/actions) 进行版本控制和持续集成，以及其他用于测试、文档、打包和分发的工具。该教程受到 [Hypermodern Python](https://medium.com/@cjolowicz/hypermodern-python-d44485d9d769) 和 [新 Python 项目的最佳实践](https://mitelman.engineering/blog/python-best-practice/automating-python-best-practices-for-a-new-project/) 等资源的启发。然而，这并不是唯一的做法，你可能有不同的偏好或意见。本教程旨在适合初学者，同时也涵盖一些高级主题。在每个部分，你将自动化一些任务，并为你的项目添加徽章以展示你的进展和成就。

该系列的仓库可以在 [github.com/johschmidt42/python-project-johannes](https://github.com/johschmidt42/python-project-johannes) 找到。

# 需求

+   **操作系统**: Linux, Unix, macOS, Windows（例如 Ubuntu 20.04 LTS 下的 WSL2）

+   **工具**: python3.10, bash, git, tree

+   **版本控制系统（VCS）主机**: [GitHub](https://github.com/)
