# 为数据科学设置 Flask 应用程序

> 原文：[`towardsdatascience.com/setting-up-a-flask-application-for-data-science-7522fc9f771e?source=collection_archive---------3-----------------------#2023-03-10`](https://towardsdatascience.com/setting-up-a-flask-application-for-data-science-7522fc9f771e?source=collection_archive---------3-----------------------#2023-03-10)

## Flask 应用程序的基本结构以支持模块化开发

[](https://philip-wilkinson.medium.com/?source=post_page-----7522fc9f771e--------------------------------)![Philip Wilkinson, Ph.D.](https://philip-wilkinson.medium.com/?source=post_page-----7522fc9f771e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7522fc9f771e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7522fc9f771e--------------------------------) [Philip Wilkinson, Ph.D.](https://philip-wilkinson.medium.com/?source=post_page-----7522fc9f771e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fec0e018f30da&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsetting-up-a-flask-application-for-data-science-7522fc9f771e&user=Philip+Wilkinson%2C+Ph.D.&userId=ec0e018f30da&source=post_page-ec0e018f30da----7522fc9f771e---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----7522fc9f771e--------------------------------) ·9 分钟阅读·2023 年 3 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7522fc9f771e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsetting-up-a-flask-application-for-data-science-7522fc9f771e&user=Philip+Wilkinson%2C+Ph.D.&userId=ec0e018f30da&source=-----7522fc9f771e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7522fc9f771e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsetting-up-a-flask-application-for-data-science-7522fc9f771e&source=-----7522fc9f771e---------------------bookmark_footer-----------)![](img/7e9b18fe28a0c2adf0b7354b9a41f272.png)

照片由[KOBU Agency](https://unsplash.com/@kobuagency?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上。

数据科学工作流程通常涉及使用笔记本和 Python 脚本。这些都是很好的工具，但这往往意味着你的输出可能会停留在这些文件中而未被外界看到。然而，一个好的改变方法是创建一个网站来展示和讨论你的发现，或者创建一个 API 将你的模型提供给全世界。在这方面，一个有用的框架是 Flask。

Flask 允许你构建网站和 API，使你能够更广泛地分享你的成果。无论是通过一个展示你工作和结果的界面，还是通过一个其他人可以调用以获取模型预测的 API。Flask 是一个轻量级的框架，易于学习和使用，适合那些希望专注于构建模型和分析数据的**数据科学家**，而不是学习复杂的 web 开发框架。它也使用 Python，因此数据科学工作流中的许多步骤可以轻松迁移到 Flask 框架中。

在这篇文章中，我将展示如何设置一个 Flask 应用程序的基本框架，你可以在此基础上进行扩展。这将包括解释应用程序的基本结构以及你需要了解的内容……
