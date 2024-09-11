# 将分布式 Ray Serve 部署与 Kafka 集成

> 原文：[https://towardsdatascience.com/integrate-distributed-ray-serve-deployment-with-kafka-181403f4e194?source=collection_archive---------1-----------------------#2023-07-16](https://towardsdatascience.com/integrate-distributed-ray-serve-deployment-with-kafka-181403f4e194?source=collection_archive---------1-----------------------#2023-07-16)

## 学习如何简单地将 Ray Serve 部署与异步 Kafka 消费者结合起来

[](https://medium.com/@slanjr?source=post_page-----181403f4e194--------------------------------)[![Rostyslav Neskorozhenyi](../Images/7d8616b6a3c60bcc29fde5d0a72f13c9.png)](https://medium.com/@slanjr?source=post_page-----181403f4e194--------------------------------)[](https://towardsdatascience.com/?source=post_page-----181403f4e194--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----181403f4e194--------------------------------) [Rostyslav Neskorozhenyi](https://medium.com/@slanjr?source=post_page-----181403f4e194--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8c7f264e81e5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintegrate-distributed-ray-serve-deployment-with-kafka-181403f4e194&user=Rostyslav+Neskorozhenyi&userId=8c7f264e81e5&source=post_page-8c7f264e81e5----181403f4e194---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----181403f4e194--------------------------------) ·5 min read·2023年7月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F181403f4e194&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintegrate-distributed-ray-serve-deployment-with-kafka-181403f4e194&user=Rostyslav+Neskorozhenyi&userId=8c7f264e81e5&source=-----181403f4e194---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F181403f4e194&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintegrate-distributed-ray-serve-deployment-with-kafka-181403f4e194&source=-----181403f4e194---------------------bookmark_footer-----------)![](../Images/86f8c4b46bf6189e1dc7d0edbd8945ac.png)

图片由 [Midjourney](https://www.midjourney.com/) 生成

[Ray](https://github.com/ray-project/ray) 是一个现代的开源框架，允许你轻松创建 Python 的分布式应用程序。你可以创建简单的训练管道，进行超参数调整，数据处理和模型服务。

Ray 允许你使用 [Ray Serve](https://docs.ray.io/en/latest/serve/index.html) 创建在线推理 API。你可以在一个应用程序中轻松地将多个机器学习模型和自定义业务逻辑结合起来。Ray Serve 会自动为你的部署创建一个 HTTP 接口，处理故障容错和复制。

![](../Images/93c068fcae1fe686691387d9f713af16.png)

Ray 生态系统。来源：[https://docs.ray.io/en/latest/ray-air/getting-started.html](https://docs.ray.io/en/latest/ray-air/getting-started.html) （[APACHE 2.0](https://github.com/ray-project/ray/blob/releases/2.6.1/LICENSE) 许可）

但有一件事**Ray Serve**目前还缺失。许多现代分布式应用通过 [Kafka](https://kafka.apache.org/) 进行通信，但目前没有现成的方法将 Ray Serve 服务连接到 Kafka 主题。

但别担心。教 Ray Serve 与 Kafka 进行通信并不会花费太多精力。现在，开始吧。

首先，我们需要准备本地环境。我们将使用一个包含 Kafka 和 [Kafdrop UI](https://github.com/obsidiandynamics/kafdrop) Docker 容器的 docker-compose 文件来启动和探索我们的本地环境…
