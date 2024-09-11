# DVC 指南：适用于所有数据科学项目的数据版本控制

> 原文：[`towardsdatascience.com/the-dvc-guide-data-version-control-for-all-your-data-science-projects-382d5b5aab00?source=collection_archive---------6-----------------------#2023-02-17`](https://towardsdatascience.com/the-dvc-guide-data-version-control-for-all-your-data-science-projects-382d5b5aab00?source=collection_archive---------6-----------------------#2023-02-17)

## 让数据版本控制像代码版本控制一样熟悉

[](https://ipom.medium.com/?source=post_page-----382d5b5aab00--------------------------------)![Yash Prakash](https://ipom.medium.com/?source=post_page-----382d5b5aab00--------------------------------)[](https://towardsdatascience.com/?source=post_page-----382d5b5aab00--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----382d5b5aab00--------------------------------) [Yash Prakash](https://ipom.medium.com/?source=post_page-----382d5b5aab00--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9ba949960063&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-dvc-guide-data-version-control-for-all-your-data-science-projects-382d5b5aab00&user=Yash+Prakash&userId=9ba949960063&source=post_page-9ba949960063----382d5b5aab00---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----382d5b5aab00--------------------------------) ·6 分钟阅读·2023 年 2 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F382d5b5aab00&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-dvc-guide-data-version-control-for-all-your-data-science-projects-382d5b5aab00&user=Yash+Prakash&userId=9ba949960063&source=-----382d5b5aab00---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F382d5b5aab00&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-dvc-guide-data-version-control-for-all-your-data-science-projects-382d5b5aab00&source=-----382d5b5aab00---------------------bookmark_footer-----------)![](img/aa7bec53d0532117c63382ccdc27c733.png)

图片由 [Dmitri Sobolevski](https://unsplash.com/@dbrenan?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

作为数据科学家，我们会对不同版本的代码、模型和数据进行实验。此外，我们甚至使用像 Git 这样的版本控制系统来管理我们的代码、跟踪版本、前进和后退，并与团队共享代码。

代码的版本控制很重要，因为它有助于在更大范围内重现软件。数据的版本控制也很重要，因为它帮助开发出具有相似指标的机器学习模型，使得团队或组织中的任何开发者在任何时间都能做到这一点。

因此，对你的模型和数据进行版本控制至关重要。但资深软件工程师会知道，使用 Git 存储大文件是一个大忌。

Git 对大文件的处理效率低下，而且它也不是存储大数据文件的标准化环境。大多数数据存储在 AWS S3 桶、Google Cloud Storage 或任何机构的远程存储服务器中。

那么我们如何对数据进行版本控制呢？这时就要用到 DVC。

# 介绍 DVC

[DVC](https://dvc.org) 是一个数据版本控制系统，与 Git 密切配合来跟踪我们的数据文件。它甚至有类似的…
