# 预测 API：Django 和 Google Trends 示例

> 原文：[`towardsdatascience.com/forecasting-api-an-example-with-django-and-google-trends-9b55046bd578?source=collection_archive---------13-----------------------#2023-08-01`](https://towardsdatascience.com/forecasting-api-an-example-with-django-and-google-trends-9b55046bd578?source=collection_archive---------13-----------------------#2023-08-01)

## 构建一个 web 应用程序来预测 Google Trends 的发展。

[](https://medium.com/@davide.burba?source=post_page-----9b55046bd578--------------------------------)![Davide Burba](https://medium.com/@davide.burba?source=post_page-----9b55046bd578--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9b55046bd578--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9b55046bd578--------------------------------) [Davide Burba](https://medium.com/@davide.burba?source=post_page-----9b55046bd578--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9f58aaaeaed7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fforecasting-api-an-example-with-django-and-google-trends-9b55046bd578&user=Davide+Burba&userId=9f58aaaeaed7&source=post_page-9f58aaaeaed7----9b55046bd578---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9b55046bd578--------------------------------) ·14 分钟阅读·2023 年 8 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9b55046bd578&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fforecasting-api-an-example-with-django-and-google-trends-9b55046bd578&user=Davide+Burba&userId=9f58aaaeaed7&source=-----9b55046bd578---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9b55046bd578&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fforecasting-api-an-example-with-django-and-google-trends-9b55046bd578&source=-----9b55046bd578---------------------bookmark_footer-----------)![](img/8e462ec58405d206471214cd10c1d9ea.png)

“Google Trends”，由 [Giulia Roggia](https://www.instagram.com/giulia_roggia__/). 经许可使用。

+   介绍

+   Django 模型

+   服务：数据源，预处理，机器学习，任务

+   交互层：序列化器，视图，端点

+   结论

# 介绍

## 什么是 Django？

[Django](https://www.djangoproject.com) 是一个高级 Python 网络框架。它旨在快速、安全、可扩展，使其成为开发复杂且预期会增长的强大 web 应用程序的热门选择。有关 Django 的介绍，你可以查看 [这个教程](https://docs.djangoproject.com/en/4.2/intro/tutorial01/)。

在这个例子中，我们将使用 [Django Rest Framework](https://www.django-rest-framework.org)（DRF），这是 Django 的一个扩展，旨在简化 [REST API](https://www.redhat.com/en/topics/api/what-is-a-rest-api) 的开发。有关 DRF 的介绍，你可以查看 [这个教程](https://www.django-rest-framework.org/tutorial/quickstart/)。

## 需求

我们将通过列出一些假设需求来开始设计我们的应用程序：

+   **总体目标**：实施一个系统来预测未来的时间序列值。

+   **数据**：每周频率的 [Google Trends](https://trends.google.com/trends/) 数据，包括特征和目标，未来可能会扩展。数据应为…
