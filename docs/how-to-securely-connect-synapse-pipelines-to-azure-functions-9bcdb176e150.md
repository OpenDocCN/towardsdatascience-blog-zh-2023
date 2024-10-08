# 如何安全地将 Synapse Pipelines 连接到 Azure Functions

> 原文：[`towardsdatascience.com/how-to-securely-connect-synapse-pipelines-to-azure-functions-9bcdb176e150`](https://towardsdatascience.com/how-to-securely-connect-synapse-pipelines-to-azure-functions-9bcdb176e150)

## 学会使用 Synapse 外泄保护、私有端点和 Azure AD 认证来创建安全连接

[](https://rebremer.medium.com/?source=post_page-----9bcdb176e150--------------------------------)![René Bremer](https://rebremer.medium.com/?source=post_page-----9bcdb176e150--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9bcdb176e150--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9bcdb176e150--------------------------------) [René Bremer](https://rebremer.medium.com/?source=post_page-----9bcdb176e150--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9bcdb176e150--------------------------------) ·4 分钟阅读·2023 年 1 月 4 日

--

![](img/0d78113d8329f6775a3e9d8a438128b5.png)

照片由 [Alina Grubnyak](https://unsplash.com/@alinnnaaaa) 在 [Unsplash](https://unsplash.com/) 上提供

# 1. 介绍

Azure Functions 是一个流行的工具，用于创建 REST API。团队可以使用 API 暴露他们的应用程序，这些应用程序可以被其他团队使用。一个常见的模式是将 Synapse 管道连接到 Azure Functions，例如运行其他团队提供的小型计算、创建元数据或发送通知。在本博客中，连接 Synapse 到 Azure Functions 的安全性方面讨论如下：

+   Synapse **数据外泄保护**以防止内部攻击

+   **私有端点** 限制 Azure Function 仅对内部可见

+   **Azure AD 认证**使用身份访问 Azure Function

+   将 **Synapse 托管身份** 白名单设置为唯一允许访问 Azure Function 的身份

在本博客文章和 Git 仓库 `[securely-connect-synapse-azure-function](https://github.com/rebremer/securely-connect-synapse-to-azure-functions)` 中，讨论了如何安全地将 Synapse 连接到 Azure Functions，另见下文概述。

![](img/2444848155d3bf646f68b368594e0698.png)

1. 安全地在 Synapse Pipelines 中使用 Azure Functions

在本博客的其余部分中，将部署一个项目，在其中将 Synapse 管道连接到 Azure Function。在下一章中，将部署该项目

# 2. 部署项目：安全地将 Synapse 连接到 Azure Function

在本章中，执行以下步骤：

+   2.0 前提条件

+   2.1 部署 Synapse 和 Azure Functions

+   2.2 设置私有端点连接

+   2.3 设置 Azure AD 认证

+   2.4 部署 Synapse 管道

## 2.0 前提条件

本教程中需要以下资源：

+   [Azure 账户](https://azure.microsoft.com/en-us/free/)

+   [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)

+   [Azure PowerShell](https://learn.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-9.2.0)（仅 2.3 需要 Powershell，其他段落的 CLI 命令可以轻松修改为 bash）

最后，将下面的 Git 仓库克隆到本地计算机。如果您尚未安装 git，您可以从网页下载一个 zip 文件。

```py
https://github.com/rebremer/securely-connect-synapse-to-azure-functions
```

## 2.1 部署 Synapse 和 Azure Functions

在这一部分，创建了一个 Synapse 工作区和 Azure Functions，具有以下属性：

+   Synapse 工作区已部署具有管理的 VNET，允许团队创建指向 Azure 中其他 PaaS 服务（例如存储、SQL 以及 Azure Functions）的私有端点

+   Synapse 工作区已部署并启用了数据外流保护。这意味着数据只能通过事先批准的私有端点流动（例如，连接到 Synapse 部署所在的同一 Azure AD 租户中的服务的私有端点）

+   Azure Function 使用 Python 创建，并在基本 SKU 上部署

请参见 `[Scripts/1_deploy_resources.ps1](https://github.com/rebremer/securely-connect-synapse-to-azure-functions/blob/main/Scripts/1_deploy_resources.ps1)` 以获取此部分的 Azure CLI 脚本

## 2.2 设置私有端点连接

在这一部分，设置了一个 Synapse 工作区与 Azure Function 之间的私有链接连接，具有以下属性：

+   从 Synapse 托管 VNET 启动到 Azure Function 的私有端点

+   批准 Azure Function 中的私有端点。批准私有端点后，Azure Function 不再暴露于公共互联网。部署 scm 接口仍然对互联网开放，也可以通过添加此链接来决定是否限制此 fqdn 的暴露，见 [这里](https://github.com/rebremer/securely-connect-synapse-to-azure-functions/blob/main/ADF/AzureFunction_private_endpoint.json#L4)

请参见 `[Scripts/2_Setup_private_endpoint_Synapse_FunctionApp.ps1](https://github.com/rebremer/securely-connect-synapse-to-azure-functions/blob/main/Scripts/2_setup_private_endpoint_Synapse_FunctionApp.ps1)` 以获取此部分的 Azure PowerShell 脚本。部署后，您将在 Synapse 中找到一个已批准的私有端点，请参见下方。

![](img/45d5c68756d424d9080966a664331498.png)

2.2 批准 Synapse 到 Azure Function 的管理私有端点

## 2.3 设置 Azure AD 认证白名单 Synapse 托管身份

在这一部分，设置了 Synapse 和 Azure Function 之间的认证，具有以下属性：

+   Azure AD 认证已设置用于 Azure Function

+   Synapse 托管身份被列入白名单，只有 Azure AD 对象 ID 被允许触发 Azure Function

参见 `[Scripts/3_Setup_AzureAD_auth_Synapse_FunctionApp.ps1](https://github.com/rebremer/securely-connect-synapse-to-azure-functions/blob/main/Scripts/3_setup_AzureAD_auth_Synapse_FunctionApp.ps1)` 获取 Azure CLI 脚本。部署后，你会发现 Synapse 管理的身份被允许访问功能，详见下文。

![](img/672ed483f663e096f69730ffeea0699a.png)

2.3 Azure Function 的 Azure AD 认证，仅允许 Synapse 管理的身份访问

## 2.4 部署 Synapse 管道

在这一部分，部署了具有以下属性的 Synapse 管道：

+   Synapse 管道使用网络活动访问 Azure Function。

+   在网络活动中，使用私有端点连接功能，因此调用不会被 Synapse 数据泄露保护阻止

+   在网络活动中，使用系统分配的管理身份进行 Azure Function 的身份验证

参见 `[Scripts/4_deploy_synapse_pipeline.ps1](https://github.com/rebremer/securely-connect-synapse-to-azure-functions/blob/main/Scripts/4_deploy_synapse_pipeline.ps1)` 获取 Azure CLI 脚本。部署后，确保 Azure Function URL 和 Azure AD 资源 ID 正确填写，详见下文。

![](img/dab8ea661e6155679671cb74050628c7.png)

2.4 Synapse 管道通过 Azure AD 认证连接到 Azure Function

# 3\. 结论

Azure Functions 是创建 REST API 以暴露服务的热门工具，无论是内部还是外部。Synapse 工作区是一个可以利用其他团队 API 的示例。数据工程师可以使用 Synapse 管道来获取元数据、发送通知和/或运行其他团队暴露的小型计算。在本文中，连接 Synapse 到 Functions 的安全方面讨论如下：

+   Synapse **数据泄露保护** 以防止内部攻击

+   **私有端点** 限制 Azure Function 的暴露

+   **Azure AD 认证提供者** 限制凭证管理

+   Synapse **管理的身份** 被白名单列为访问 Azure Function 的权限

参见这个 Git 仓库 `[securely-connect-synapse-azure-function](https://github.com/rebremer/securely-connect-synapse-to-azure-functions)` 和下文的架构。

![](img/2444848155d3bf646f68b368594e0698.png)

3\. 安全地在 Synapse 管道中使用 Azure Functions
