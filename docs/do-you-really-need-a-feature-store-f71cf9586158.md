# 你真的需要一个特征存储吗？

> 原文：[`towardsdatascience.com/do-you-really-need-a-feature-store-f71cf9586158?source=collection_archive---------7-----------------------#2023-03-17`](https://towardsdatascience.com/do-you-really-need-a-feature-store-f71cf9586158?source=collection_archive---------7-----------------------#2023-03-17)

## 特征存储——原始数据和机器学习模型之间的接口

[](https://medium.com/@weiyunna91?source=post_page-----f71cf9586158--------------------------------)![YUNNA WEI](https://medium.com/@weiyunna91?source=post_page-----f71cf9586158--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f71cf9586158--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f71cf9586158--------------------------------) [YUNNA WEI](https://medium.com/@weiyunna91?source=post_page-----f71cf9586158--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4b47aa84fc4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdo-you-really-need-a-feature-store-f71cf9586158&user=YUNNA+WEI&userId=4b47aa84fc4&source=post_page-4b47aa84fc4----f71cf9586158---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f71cf9586158--------------------------------) · 8 分钟阅读 · 2023 年 3 月 17 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff71cf9586158&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdo-you-really-need-a-feature-store-f71cf9586158&user=YUNNA+WEI&userId=4b47aa84fc4&source=-----f71cf9586158---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff71cf9586158&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdo-you-really-need-a-feature-store-f71cf9586158&source=-----f71cf9586158---------------------bookmark_footer-----------)

“特征存储”已经存在了几年。既有开源解决方案（如[Feast](https://feast.dev/)和[Hopsworks](https://www.hopsworks.ai/open-source-hopsworks)），也有商业产品（如[Tecton](https://www.tecton.ai/)、[Hopsworks](https://www.hopsworks.ai/open-source-hopsworks)、[Databricks Feature Store](https://docs.databricks.com/machine-learning/feature-store/index.html)）。围绕“特征存储”的定义和价值，已经发表了大量的文章和博客。有些组织也已经将“特征存储”作为其机器学习应用的一部分。然而，需要指出的是，“特征存储”是你整体机器学习基础设施中增加的另一个组件，这需要额外的投资和努力来构建和运营。因此，确实有必要理解和讨论***“特征存储对于每个组织是否真的必要？”***。在我看来，答案一如既往，取决于具体情况。

因此，今天文章的重点是分析何时需要一个特征存储，以便组织可以明智地投入精力和资源于能够真正为业务增加价值的机器学习技术。

为了回答这个问题，以下是一些关键的考虑因素：

+   你的机器学习应用程序需要什么样的特征？

+   你们组织管理的机器学习应用程序是什么类型的？

+   是否需要在你们组织中的各个团队之间共享和重用功能？

+   训练-服务偏差是否经常是一个问题……
