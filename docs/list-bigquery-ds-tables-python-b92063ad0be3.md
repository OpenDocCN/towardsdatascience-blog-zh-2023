# 如何使用 Python 列出所有 BigQuery 数据集和表

> 原文：[`towardsdatascience.com/list-bigquery-ds-tables-python-b92063ad0be3?source=collection_archive---------11-----------------------#2023-05-15`](https://towardsdatascience.com/list-bigquery-ds-tables-python-b92063ad0be3?source=collection_archive---------11-----------------------#2023-05-15)

## 使用 BigQuery API 和 Python 以编程方式列出所有数据集和表

[](https://gmyrianthous.medium.com/?source=post_page-----b92063ad0be3--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----b92063ad0be3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b92063ad0be3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b92063ad0be3--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----b92063ad0be3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flist-bigquery-ds-tables-python-b92063ad0be3&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----b92063ad0be3---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b92063ad0be3--------------------------------) · 5 分钟阅读 · 2023 年 5 月 15 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb92063ad0be3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flist-bigquery-ds-tables-python-b92063ad0be3&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----b92063ad0be3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb92063ad0be3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flist-bigquery-ds-tables-python-b92063ad0be3&source=-----b92063ad0be3---------------------bookmark_footer-----------)![](img/c7fe779b864a72382bc66ab76d39ab86.png)

图片由 [Joanna Kosinska](https://unsplash.com/@joannakosinska?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/PbgY3ptgA4A?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

BigQuery 是 Google Cloud Platform 上的托管数据仓库服务，它允许用户存储、管理和查询数据。数据和/或分析工程的一个重要部分是某些任务的自动化，包括与 BigQuery 的一些交互。这种自动化通常需要我们使用 BigQuery API，这使得开发者能够以编程方式与 GCP 上的服务进行交互。

在今天的文章中，我们将演示如何使用 BigQuery API 和 Google Python 客户端，以便以编程方式获取单个 GCP 项目下的所有表和数据集。

## 前提条件

为了跟随本教程中概述的步骤，确保安装 Google 提供的 Python 客户端库。为此，我们需要通过 `pip` 安装 `google-cloud-bigquery`：

```py
$ python3 -m pip install --upgrade google-cloud-bigquery
```

此外，你需要通过应用默认凭证（ADC）对客户端进行身份验证。为此，你首先需要确保已经[安装](https://cloud.google.com/sdk/gcloud) `[gcloud](https://cloud.google.com/sdk/gcloud)` [命令行界面（CLI）](https://cloud.google.com/sdk/gcloud)。现在，既然我们在本地机器上开发代码，我们需要…
