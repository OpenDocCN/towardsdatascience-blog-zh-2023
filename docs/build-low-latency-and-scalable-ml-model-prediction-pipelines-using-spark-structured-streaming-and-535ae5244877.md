# 使用 Spark 结构化流处理和 MLflow 构建低延迟且可扩展的 ML 模型预测管道

> 原文：[`towardsdatascience.com/build-low-latency-and-scalable-ml-model-prediction-pipelines-using-spark-structured-streaming-and-535ae5244877?source=collection_archive---------23-----------------------#2023-01-10`](https://towardsdatascience.com/build-low-latency-and-scalable-ml-model-prediction-pipelines-using-spark-structured-streaming-and-535ae5244877?source=collection_archive---------23-----------------------#2023-01-10)

## 实践中的 MLOps 系列——分享关键 MLOps 组件的设计和实施模式。今天文章的重点是构建模型预测管道。

[](https://medium.com/@weiyunna91?source=post_page-----535ae5244877--------------------------------)![YUNNA WEI](https://medium.com/@weiyunna91?source=post_page-----535ae5244877--------------------------------)[](https://towardsdatascience.com/?source=post_page-----535ae5244877--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----535ae5244877--------------------------------) [YUNNA WEI](https://medium.com/@weiyunna91?source=post_page-----535ae5244877--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4b47aa84fc4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-low-latency-and-scalable-ml-model-prediction-pipelines-using-spark-structured-streaming-and-535ae5244877&user=YUNNA+WEI&userId=4b47aa84fc4&source=post_page-4b47aa84fc4----535ae5244877---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----535ae5244877--------------------------------) · 8 分钟阅读·2023 年 1 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F535ae5244877&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-low-latency-and-scalable-ml-model-prediction-pipelines-using-spark-structured-streaming-and-535ae5244877&user=YUNNA+WEI&userId=4b47aa84fc4&source=-----535ae5244877---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F535ae5244877&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-low-latency-and-scalable-ml-model-prediction-pipelines-using-spark-structured-streaming-and-535ae5244877&source=-----535ae5244877---------------------bookmark_footer-----------)

使机器学习模型在实际生产环境中工作，最关键的步骤之一是部署经过训练的模型进行预测。模型部署（发布）是一个过程，它使你能够将训练好的机器学习模型集成到生产环境中，以对实际数据做出决策。谈到模型部署，一般有两种类型。

+   一种是批量预测，其中经过训练的模型在某个间隔（如每天一次或每周一次，根据模型在特定业务环境中的使用情况）调用，并处理一批数据，以定期生成用于预测的结果。

+   另一种是在线预测，其中经过训练的模型被打包成 REST API 或容器化微服务，模型通过响应 API 请求返回预测结果（通常为 JSON 格式）。使用在线预测时，模型实时进行预测，即一旦调用 API，就会返回模型预测结果。此外，模型 REST API 通常集成在 Web 应用程序中，以便最终用户或下游应用程序进行交互。
