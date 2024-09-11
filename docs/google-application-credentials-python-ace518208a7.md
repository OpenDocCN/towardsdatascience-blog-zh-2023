# 如何在 Python 中设置 `GOOGLE_APPLICATION_CREDENTIALS`

> 原文：[`towardsdatascience.com/google-application-credentials-python-ace518208a7?source=collection_archive---------1-----------------------#2023-01-19`](https://towardsdatascience.com/google-application-credentials-python-ace518208a7?source=collection_archive---------1-----------------------#2023-01-19)

## 配置应用程序默认凭据并修复 `oauth2client.client.ApplicationDefaultCredentialsError`

[](https://gmyrianthous.medium.com/?source=post_page-----ace518208a7--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----ace518208a7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ace518208a7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ace518208a7--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----ace518208a7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoogle-application-credentials-python-ace518208a7&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----ace518208a7---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ace518208a7--------------------------------) · 3 分钟阅读 · 2023 年 1 月 19 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Face518208a7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoogle-application-credentials-python-ace518208a7&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----ace518208a7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Face518208a7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoogle-application-credentials-python-ace518208a7&source=-----ace518208a7---------------------bookmark_footer-----------)![](img/481b848f924c744cbd3a67c44800e6fb.png)

照片由 [Daniel K Cheung](https://unsplash.com/@danielkcheung?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/ZqqlOZyGG7g?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

欢迎来到我们的教程，介绍如何为 Google Cloud 和 Python 配置应用程序默认凭据。在本文中，我们将详细讲解如何在 Python 中正确设置`GOOGLE_APPLICATION_CREDENTIALS`。

为了能够以编程方式与 Google Cloud Platform 服务（例如 Google BigQuery）进行交互，首先需要正确地验证应用程序并授予所有所需的权限。这通过定义应用程序默认凭据来实现，指定一个包含所需凭据的文件。

当遗漏这一步骤时，常见的错误是以下情况

```py
oauth2client.client.ApplicationDefaultCredentialsError: The Application Default Credentials are not available. They are available if running in Google Compute Engine. Otherwise, the environment variable GOOGLE_APPLICATION_CREDENTIALS must be defined pointing to a file defining the credentials. See https://developers.google.com/accounts/docs/application-default-credentials for more information.
```

[**订阅数据管道**](https://thedatapipeline.substack.com/welcome)**，这是一个专注于数据工程的新闻通讯**

## 应用程序如何…
