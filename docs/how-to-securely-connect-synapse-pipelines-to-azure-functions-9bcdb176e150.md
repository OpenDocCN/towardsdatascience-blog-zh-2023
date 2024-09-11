# 如何安全地将 Synapse 管道连接到 Azure Functions

> 原文：[`towardsdatascience.com/how-to-securely-connect-synapse-pipelines-to-azure-functions-9bcdb176e150?source=collection_archive---------14-----------------------#2023-01-04`](https://towardsdatascience.com/how-to-securely-connect-synapse-pipelines-to-azure-functions-9bcdb176e150?source=collection_archive---------14-----------------------#2023-01-04)

## 学习如何使用 Synapse 外泄保护、私有端点和 Azure AD 认证来创建安全连接

[](https://rebremer.medium.com/?source=post_page-----9bcdb176e150--------------------------------)![René Bremer](https://rebremer.medium.com/?source=post_page-----9bcdb176e150--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9bcdb176e150--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9bcdb176e150--------------------------------) [René Bremer](https://rebremer.medium.com/?source=post_page-----9bcdb176e150--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F11e5e7fb3771&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-securely-connect-synapse-pipelines-to-azure-functions-9bcdb176e150&user=Ren%C3%A9+Bremer&userId=11e5e7fb3771&source=post_page-11e5e7fb3771----9bcdb176e150---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9bcdb176e150--------------------------------) ·4 min read·2023 年 1 月 4 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9bcdb176e150&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-securely-connect-synapse-pipelines-to-azure-functions-9bcdb176e150&user=Ren%C3%A9+Bremer&userId=11e5e7fb3771&source=-----9bcdb176e150---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9bcdb176e150&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-securely-connect-synapse-pipelines-to-azure-functions-9bcdb176e150&source=-----9bcdb176e150---------------------bookmark_footer-----------)![](img/0d78113d8329f6775a3e9d8a438128b5.png)

图片由 [Alina Grubnyak](https://unsplash.com/@alinnnaaaa) 提供，来源于 [Unsplash](https://unsplash.com/)

# 1\. 引言

Azure Functions 是一个流行的工具，用于创建 REST API。团队可以使用 API 暴露他们的应用程序，然后其他团队可以使用这些 API。一个常见的模式是将 Synapse 管道连接到 Azure Functions，例如，运行由其他团队提供的小型计算，创建元数据或发送通知。在本博客中，讨论了将 Synapse 连接到 Azure Functions 的安全方面，如下所述：

+   **数据外泄保护**以防止内部攻击

+   使用**私有端点**将 Azure Function 的暴露限制为仅内部

+   使用**Azure AD 身份验证**来访问 Azure Function

+   将**Synapse 托管身份**列为唯一允许访问 Azure Function 的身份

在这篇博客文章和 git 仓库`[securely-connect-synapse-azure-function](https://github.com/rebremer/securely-connect-synapse-to-azure-functions)`中，讨论了如何安全地将 Synapse 连接到 Azure Functions，请参阅下面的概述。
