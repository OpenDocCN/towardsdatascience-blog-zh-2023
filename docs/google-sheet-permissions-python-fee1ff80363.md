# 如何使用 Python 更改 Google 表格权限

> 原文：[`towardsdatascience.com/google-sheet-permissions-python-fee1ff80363?source=collection_archive---------5-----------------------#2023-05-04`](https://towardsdatascience.com/google-sheet-permissions-python-fee1ff80363?source=collection_archive---------5-----------------------#2023-05-04)

## 使用 Python API 编程共享 Google 表格给特定用户

[](https://gmyrianthous.medium.com/?source=post_page-----fee1ff80363--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----fee1ff80363--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fee1ff80363--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fee1ff80363--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----fee1ff80363--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoogle-sheet-permissions-python-fee1ff80363&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----fee1ff80363---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fee1ff80363--------------------------------) · 5 分钟阅读 · 2023 年 5 月 4 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffee1ff80363&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoogle-sheet-permissions-python-fee1ff80363&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----fee1ff80363---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffee1ff80363&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoogle-sheet-permissions-python-fee1ff80363&source=-----fee1ff80363---------------------bookmark_footer-----------)![](img/08ae206774c5ada897f91d3d70b78ba0.png)

图片由 [Artur Tumasjan](https://unsplash.com/es/@arturtumasjan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，发布在 [Unsplash](https://unsplash.com/photos/zM8sSfYFDww?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

与他人共享 Google 表格是一项简单直接的任务，可以通过用户界面完成。但是，如果你需要与特定用户或服务帐户共享多个 Google 表格，该怎么办呢？

想象一下，你已经在 BigQuery 上创建了数百个外部表，这些表从不同的 Google Sheets 中获取数据。如果另一个服务，如 Apache Airflow，执行查询引用这些表，你需要确保 Airflow 的服务账户在所有这些 Sheets 上都有足够的权限。但手动共享（即授予 `Viewer` 或 `Editor` 权限）数百个 Sheets 给特定主体几乎是不可能的，而且需要几个小时。

在本教程中，我们将演示如何使用 Python 和 Google Drive API 一次性更改数百个 Google Sheets 的权限。

## 先决条件

我们需要做的第一件事是确保我们已成功获得具有所需范围的用户访问凭据。为此，只需运行以下命令：

```py
gcloud auth application-default login…
```
