# 开始使用 Databricks

> 原文：[`towardsdatascience.com/getting-started-with-databricks-11af3db4f595?source=collection_archive---------14-----------------------#2023-05-08`](https://towardsdatascience.com/getting-started-with-databricks-11af3db4f595?source=collection_archive---------14-----------------------#2023-05-08)

## 初学者指南：Databricks

[](https://spierre91.medium.com/?source=post_page-----11af3db4f595--------------------------------)![Sadrach Pierre, Ph.D.](https://spierre91.medium.com/?source=post_page-----11af3db4f595--------------------------------)[](https://towardsdatascience.com/?source=post_page-----11af3db4f595--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----11af3db4f595--------------------------------) [Sadrach Pierre, Ph.D.](https://spierre91.medium.com/?source=post_page-----11af3db4f595--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F120b86134681&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgetting-started-with-databricks-11af3db4f595&user=Sadrach+Pierre%2C+Ph.D.&userId=120b86134681&source=post_page-120b86134681----11af3db4f595---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----11af3db4f595--------------------------------) ·9 分钟阅读·2023 年 5 月 8 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F11af3db4f595&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgetting-started-with-databricks-11af3db4f595&source=-----11af3db4f595---------------------bookmark_footer-----------)![](img/c2c2e8095e951f4e978e8ed16e16747c.png)

[Alexander Grey](https://www.pexels.com/@mccutcheon/) 在 [Pexels](https://www.pexels.com/photo/assorted-color-bricks-1148496/) 上的图片

[Databricks](https://www.databricks.com/) 允许数据科学家轻松创建和管理笔记本，用于研究、实验和部署。Databricks 平台的吸引力包括与云服务的无缝集成、模型维护工具和可扩展性。

Databricks 对于模型实验和维护非常有用。Databricks 拥有一个名为 [MLflow](https://mlflow.org/docs/latest/index.html) 的机器学习库，它提供了有用的工具来支持模型开发和部署。通过 MLflow，你可以记录模型及其相关的元数据，如性能指标和超参数。这使得运行实验和分析结果变得非常直接。

Databricks 的许多功能对于扩展机器学习工作流中的步骤非常有用，例如数据加载、模型训练和模型日志记录。[Koalas](https://koalas.readthedocs.io/en/latest/user_guide/index.html) 是 Databricks 中一个比 pandas 更高效的库。[Pandas 用户自定义函数](https://spark.apache.org/docs/3.1.2/api/python/reference/api/pyspark.sql.functions.pandas_udf.html)（UDF）允许你以分布式方式应用自定义函数，这些函数通常计算成本较高，从而显著减少运行时间。Databricks 还允许你在更大的机器上配置作业，这对于处理大数据和重计算非常有用。此外，模型注册表允许你运行和存储数百或甚至上千个实验结果……
