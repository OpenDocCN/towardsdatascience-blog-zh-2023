# 使用 Pants 组织机器学习单一代码库

> 原文：[https://towardsdatascience.com/organizing-a-machine-learning-monorepo-with-pants-8e0570de0c4c?source=collection_archive---------6-----------------------#2023-08-18](https://towardsdatascience.com/organizing-a-machine-learning-monorepo-with-pants-8e0570de0c4c?source=collection_archive---------6-----------------------#2023-08-18)

## MLOps

## 精简你的机器学习工作流管理

[](https://michaloleszak.medium.com/?source=post_page-----8e0570de0c4c--------------------------------)[![Michał Oleszak](../Images/61b32e70cec4ba54612a8ca22e977176.png)](https://michaloleszak.medium.com/?source=post_page-----8e0570de0c4c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8e0570de0c4c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8e0570de0c4c--------------------------------) [Michał Oleszak](https://michaloleszak.medium.com/?source=post_page-----8e0570de0c4c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc58320fab2a8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Forganizing-a-machine-learning-monorepo-with-pants-8e0570de0c4c&user=Micha%C5%82+Oleszak&userId=c58320fab2a8&source=post_page-c58320fab2a8----8e0570de0c4c---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8e0570de0c4c--------------------------------) ·20分钟阅读·2023年8月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8e0570de0c4c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Forganizing-a-machine-learning-monorepo-with-pants-8e0570de0c4c&user=Micha%C5%82+Oleszak&userId=c58320fab2a8&source=-----8e0570de0c4c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8e0570de0c4c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Forganizing-a-machine-learning-monorepo-with-pants-8e0570de0c4c&source=-----8e0570de0c4c---------------------bookmark_footer-----------)![](../Images/9a6317d603483978f49944578ae5d4f6.png)

你是否曾经在项目之间复制粘贴工具代码块，导致相同代码的多个版本存在于不同的代码库中？或者，也许你在更新存储数据的GCP桶名称后不得不向数十个项目提交拉取请求？

上述情况在机器学习团队中经常发生，其后果从单个开发人员的不满到团队无法按需交付代码不等。幸运的是，有解决办法。

让我们深入了解单一代码库的世界，这是一种被谷歌等主要科技公司广泛采用的架构，以及它们如何提升你的机器学习工作流。单一代码库提供了大量优势，尽管存在一些缺点，但它仍然是管理复杂机器学习生态系统的一个有吸引力的选择。

我们将简要讨论单一代码库（monorepos）的优点和缺点，探讨为什么它是机器学习团队的一个优秀架构选择，并了解大公司是如何使用它的。最后，我们将看看如何利用Pants构建系统的力量，将你的机器学习单一代码库组织成一个强大的CI/CD构建系统。
