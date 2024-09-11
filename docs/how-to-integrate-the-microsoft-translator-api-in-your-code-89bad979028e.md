# 如何将 Microsoft Translator API 集成到您的代码中

> 原文：[`towardsdatascience.com/how-to-integrate-the-microsoft-translator-api-in-your-code-89bad979028e?source=collection_archive---------13-----------------------#2023-01-24`](https://towardsdatascience.com/how-to-integrate-the-microsoft-translator-api-in-your-code-89bad979028e?source=collection_archive---------13-----------------------#2023-01-24)

## 一份全面的初学者友好指南

[](https://namiyousef96.medium.com/?source=post_page-----89bad979028e--------------------------------)![Yousef Nami](https://namiyousef96.medium.com/?source=post_page-----89bad979028e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----89bad979028e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----89bad979028e--------------------------------) [Yousef Nami](https://namiyousef96.medium.com/?source=post_page-----89bad979028e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F63d10d93acd8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-integrate-the-microsoft-translator-api-in-your-code-89bad979028e&user=Yousef+Nami&userId=63d10d93acd8&source=post_page-63d10d93acd8----89bad979028e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----89bad979028e--------------------------------) ·13 分钟阅读·2023 年 1 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F89bad979028e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-integrate-the-microsoft-translator-api-in-your-code-89bad979028e&user=Yousef+Nami&userId=63d10d93acd8&source=-----89bad979028e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F89bad979028e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-integrate-the-microsoft-translator-api-in-your-code-89bad979028e&source=-----89bad979028e---------------------bookmark_footer-----------)![](img/af8adbcc9c869a0985b8e5a4eed8d533.png)

图片来自[Unsplash](https://unsplash.com/photos/5Z8mR4vqJD4)，感谢[Edurne](https://unsplash.com/@edurnetx)提供。

目前有许多优秀的翻译服务，但其中一种最通用且最易于设置的服务是**Microsoft Translator** [[1](https://www.google.com/search?q=microsoft+translator&oq=microsoft+translator&aqs=chrome.0.35i39j69i59l2j0i512l2j69i60l3.2307j0j7&sourceid=chrome&ie=UTF-8)]，它为您提供对大量低资源和高资源语言的免费翻译服务（受每月翻译限额的限制）。

在本教程中，我将详细介绍如何在 Azure 上设置翻译实例，以及如何在你的代码中编写接口与之连接并遵循最佳实践。如果你对 Azure 熟悉并且已经设置了翻译实例，可以直接访问项目 [仓库](https://github.com/namiyousef/ml-utils) 获取代码。

本教程将涵盖：

1.  设置 Azure 翻译实例

1.  发送你的第一次翻译请求

1.  清理你的代码和结构化你的项目

1.  使用 Jupyter Notebooks 的考虑事项

1.  结论

# 在 Azure 上设置翻译实例

## 创建 Azure 帐户

第一步是创建一个 Microsoft Azure [帐户](https://azure.microsoft.com/en-us/free/)。这将要求你拥有：

+   一个有效的地址
