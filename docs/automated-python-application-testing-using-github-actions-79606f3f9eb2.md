# 使用 GitHub Actions 进行自动化 Python 应用测试

> 原文：[`towardsdatascience.com/automated-python-application-testing-using-github-actions-79606f3f9eb2?source=collection_archive---------8-----------------------#2023-03-21`](https://towardsdatascience.com/automated-python-application-testing-using-github-actions-79606f3f9eb2?source=collection_archive---------8-----------------------#2023-03-21)

## 如何在 `push` 命令上运行自动化测试

[](https://philip-wilkinson.medium.com/?source=post_page-----79606f3f9eb2--------------------------------)![Philip Wilkinson, Ph.D.](https://philip-wilkinson.medium.com/?source=post_page-----79606f3f9eb2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----79606f3f9eb2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----79606f3f9eb2--------------------------------) [Philip Wilkinson, Ph.D.](https://philip-wilkinson.medium.com/?source=post_page-----79606f3f9eb2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fec0e018f30da&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fautomated-python-application-testing-using-github-actions-79606f3f9eb2&user=Philip+Wilkinson%2C+Ph.D.&userId=ec0e018f30da&source=post_page-ec0e018f30da----79606f3f9eb2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----79606f3f9eb2--------------------------------) ·6 分钟阅读·2023 年 3 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F79606f3f9eb2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fautomated-python-application-testing-using-github-actions-79606f3f9eb2&user=Philip+Wilkinson%2C+Ph.D.&userId=ec0e018f30da&source=-----79606f3f9eb2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F79606f3f9eb2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fautomated-python-application-testing-using-github-actions-79606f3f9eb2&source=-----79606f3f9eb2---------------------bookmark_footer-----------)![](img/cda5a053f31851154a45ff8b1b4be350.png)

图片由 [Roman Synkevych 🇺🇦](https://unsplash.com/@synkevych?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

测试你的应用程序是任何软件开发或数据科学工作流程中的关键步骤。测试可以确保代码按预期运行，减少错误或漏洞的可能性，并提高软件的整体质量和可靠性。

不过，在某些情况下，测试可能会成为负担，你或其他人可能会忘记在提交到代码库之前运行测试套件。这就是通过 GitHub Actions 自动化测试工作流的作用，以减少手动操作，确保一致的测试，并能够及早发现问题。

在本文中，我将介绍如何设置 GitHub Action 工作流以自动化应用程序的测试。这包括创建 GitHub Action 工作流、设置触发事件、创建要运行的作业以及向你的代码库添加徽章以显示测试是否通过。到最后，你将拥有一个完全自动化的测试工作流，可以向世界展示你的代码按预期工作！

## 什么是 GitHub Action？

根据 GitHub 提供的描述：

> GitHub Actions 使自动化变得简单……
