# 如何在 Python 中设置 GOOGLE_APPLICATION_CREDENTIALS

> 原文：[`towardsdatascience.com/google-application-credentials-python-ace518208a7`](https://towardsdatascience.com/google-application-credentials-python-ace518208a7)

## 配置应用程序默认凭据并修复 `oauth2client.client.ApplicationDefaultCredentialsError`

[](https://gmyrianthous.medium.com/?source=post_page-----ace518208a7--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----ace518208a7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ace518208a7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ace518208a7--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----ace518208a7--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ace518208a7--------------------------------) ·阅读时间 3 分钟·2023 年 1 月 19 日

--

![](img/481b848f924c744cbd3a67c44800e6fb.png)

照片由 [Daniel K Cheung](https://unsplash.com/@danielkcheung?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/ZqqlOZyGG7g?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

欢迎来到我们的教程，了解如何为 Google Cloud 和 Python 配置应用程序默认凭据。在本文中，我们将介绍如何在 Python 中正确设置 `GOOGLE_APPLICATION_CREDENTIALS`。

为了能够以编程方式与 Google Cloud Platform 服务（如 Google BigQuery）进行交互，首先需要正确认证应用程序并授予所有所需权限。这是通过定义应用程序默认凭据来指向包含所需凭据的文件来实现的。

当漏掉此步骤时，常见的错误是：

```py
oauth2client.client.ApplicationDefaultCredentialsError: The Application Default Credentials are not available. They are available if running in Google Compute Engine. Otherwise, the environment variable GOOGLE_APPLICATION_CREDENTIALS must be defined pointing to a file defining the credentials. See https://developers.google.com/accounts/docs/application-default-credentials for more information.
```

[**订阅数据管道**](https://thedatapipeline.substack.com/welcome)**，这是一个专注于数据工程的新闻通讯**

## Google Cloud 中的应用程序默认凭据如何工作

应用程序默认凭据 (ADC) 是 Google Cloud 上用于根据应用程序环境推断凭据的策略。这意味着应用程序代码可以在不同的环境中运行，而无需更改代码认证 GCP 服务或应用程序编程接口 (APIs) 的方式。

对于本地开发，通常有两种不同的方法来提供 ADC 凭据：

+   用户凭据

+   服务帐户密钥

## 创建凭据 JSON 文件

为了创建包含所需凭据的 JSON 文件，首先需要确保在主机机器上安装了 `[gcloud](https://cloud.google.com/sdk/docs/install)` [CLI](https://cloud.google.com/sdk/docs/install)。

对于本地开发，您最好使用与您个人 Google Cloud 帐户相关联的*用户凭据*。为此，您需要运行以下命令，这将会在您的（默认）浏览器中显示一个登录提示：

```py
gcloud auth application-default login
```

登录到 Google Cloud 后，您的凭据将存储在一个 JSON 文件中，位于以下默认位置：

+   Mac/Linux: `$HOME/.config/gcloud/application_default_credentials.json`

+   Windows: `%APPDATA%\gcloud\application_default_credentials.json`

如果您使用的是服务帐户，可以通过访问[Google Cloud 上的服务帐户服务](http://console.cloud.google.com/iam-admin/serviceaccounts)生成 JSON 令牌。**不过请注意，服务帐户密钥存在安全风险，不推荐使用。更强大且可能更安全的方法包括冒充和** [**工作负载身份池**](https://cloud.google.com/iam/docs/workload-identity-federation#providers)**。**

## 设置 GOOGLE_APPLICATION_CREDENTIALS 环境变量

为了提供凭据 JSON 文件的位置，您需要使用 `GOOGLE_APPLICATION_CREDENTIALS` 环境变量。

因此，在使用 Python 时，您可以使用以下代码片段以编程方式设置环境变量：

```py
import os 

os.environ['GOOGLE_APPLICATION_CREDENTIALS'] ='$HOME/.config/gcloud/application_default_credentials.json'
```

另外，您还可以创建 `google.oath2.service_account.Credentials` 的实例，然后在开始与 Google 客户端交互之前将其传递给 Google 客户端。

以下示例演示了如何在 Python 中对 Gmail 客户端进行身份验证：

```py
from google.oauth2 import service_account
from googleapiclient.discovery import build

credentials = service_account.Credentials.from_service_account_file(
  '$HOME/.config/gcloud/application_default_credentials.json'
)

service = build('gmail', 'v1', credentials=credentials)
```

*请注意，上述代码片段假设您的 JSON 凭据文件在创建时存储在默认目录下。如果与默认目录不同，请确保指向正确的目录。*

## 最终想法

总之，本教程介绍了如何为 Google Cloud 和 Python 正确设置应用程序默认凭据 (ADC)，以便对应用程序进行身份验证，并授予对 Google Cloud Platform 服务的所有必需权限。

ADC 是 Google Cloud 上的一种策略，用于根据应用程序环境推断凭据，使代码可以在不同的环境中运行而无需更改身份验证过程。

在本教程中，我们还介绍了如何创建所需的 JSON 凭据文件，无论是使用用户凭据还是服务帐户，以及如何设置 `GOOGLE_APPLICATION_CREDENTIALS` 环境变量以提供文件的位置。

[**订阅数据管道**](https://thedatapipeline.substack.com/welcome)**，这是一个致力于数据工程的新闻通讯**

**您可能也喜欢的相关文章**

## Python 中的图示代码

### 使用 Python 创建云系统架构图

[图表作为代码（Python）](https://towardsdatascience.com/diagrams-as-code-python-d9cbaa959ed5?source=post_page-----ace518208a7--------------------------------) [## SQL 反模式在 BigQuery 中](https://towardsdatascience.com/bigquery-anti-patterns-dacb61f8a3f?source=post_page-----ace518208a7--------------------------------)

### 在 Google Cloud BigQuery 上运行 SQL 时的最佳实践和需要避免的事项

[标准 SQL 与遗留 SQL 在 BigQuery 中的比较](https://towardsdatascience.com/standard-vs-legacy-sql-bigquery-6d01fa3046a9?source=post_page-----ace518208a7--------------------------------) [## 标准 SQL 与遗留 SQL 在 BigQuery 中的比较](https://towardsdatascience.com/standard-vs-legacy-sql-bigquery-6d01fa3046a9?source=post_page-----ace518208a7--------------------------------)

### 在 Google Cloud BigQuery 上理解标准 SQL 和遗留 SQL 的区别

[SQL 反模式在 BigQuery 中](https://towardsdatascience.com/bigquery-anti-patterns-dacb61f8a3f?source=post_page-----ace518208a7--------------------------------)
