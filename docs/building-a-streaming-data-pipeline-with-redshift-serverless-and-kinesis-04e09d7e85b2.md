# 使用 Redshift Serverless 和 Kinesis 构建流数据管道

> 原文：[https://towardsdatascience.com/building-a-streaming-data-pipeline-with-redshift-serverless-and-kinesis-04e09d7e85b2?source=collection_archive---------7-----------------------#2023-10-06](https://towardsdatascience.com/building-a-streaming-data-pipeline-with-redshift-serverless-and-kinesis-04e09d7e85b2?source=collection_archive---------7-----------------------#2023-10-06)

## 初学者的端到端教程

[](https://mshakhomirov.medium.com/?source=post_page-----04e09d7e85b2--------------------------------)[![💡Mike Shakhomirov](../Images/bc6895c7face3244d488feb97ba0f68e.png)](https://mshakhomirov.medium.com/?source=post_page-----04e09d7e85b2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----04e09d7e85b2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----04e09d7e85b2--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----04e09d7e85b2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe06a48b3dd48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-streaming-data-pipeline-with-redshift-serverless-and-kinesis-04e09d7e85b2&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=post_page-e06a48b3dd48----04e09d7e85b2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----04e09d7e85b2--------------------------------) ·9 分钟阅读·2023年10月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F04e09d7e85b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-streaming-data-pipeline-with-redshift-serverless-and-kinesis-04e09d7e85b2&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=-----04e09d7e85b2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F04e09d7e85b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-streaming-data-pipeline-with-redshift-serverless-and-kinesis-04e09d7e85b2&source=-----04e09d7e85b2---------------------bookmark_footer-----------)![](../Images/d1700c0485714244a17aec09305461e6.png)

照片由 [Sebastian Pandelache](https://unsplash.com/@pandelache?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇文章中，我将讨论一种最受欢迎的数据管道设计模式——事件流。除了其他好处，它使得数据分析速度极快，我们可以创建实时更新结果的报告仪表板。我将通过构建一个基于 AWS Kinesis 和 Redshift 的流数据管道来演示如何实现这一点，该管道可以通过基础设施即代码轻松部署。我们将使用 AWS CloudFormation 来描述我们的数据平台架构并简化部署。

想象一下，作为数据工程师，你的任务是创建一个将服务器事件流与数据仓库解决方案（Redshift）连接的数据管道，以转换数据并创建一个分析仪表板。

![](../Images/19d9e86d0773049b03bcd3c4ea66b9ab.png)

管道基础设施。图像由作者提供。

> 什么是数据管道？
> 
> 这是一系列数据处理步骤。由于***逻辑数据流连接***在这些阶段之间，每个阶段生成一个**输出**，作为下一个阶段的**输入**。
