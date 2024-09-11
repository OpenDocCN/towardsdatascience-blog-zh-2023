# 数据管道设计模式

> 原文：[https://towardsdatascience.com/data-pipeline-design-patterns-100afa4b93e3?source=collection_archive---------0-----------------------#2023-01-02](https://towardsdatascience.com/data-pipeline-design-patterns-100afa4b93e3?source=collection_archive---------0-----------------------#2023-01-02)

## 选择合适的架构并提供示例

[](https://mshakhomirov.medium.com/?source=post_page-----100afa4b93e3--------------------------------)[![💡Mike Shakhomirov](../Images/bc6895c7face3244d488feb97ba0f68e.png)](https://mshakhomirov.medium.com/?source=post_page-----100afa4b93e3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----100afa4b93e3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----100afa4b93e3--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----100afa4b93e3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe06a48b3dd48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-pipeline-design-patterns-100afa4b93e3&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=post_page-e06a48b3dd48----100afa4b93e3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----100afa4b93e3--------------------------------) ·9分钟阅读·2023年1月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F100afa4b93e3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-pipeline-design-patterns-100afa4b93e3&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=-----100afa4b93e3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F100afa4b93e3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-pipeline-design-patterns-100afa4b93e3&source=-----100afa4b93e3---------------------bookmark_footer-----------)![](../Images/074dc1570ad17f76947283225050b8db.png)

由 [israel palacio](https://unsplash.com/@othentikisra?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供的照片

通常，数据在步骤中被处理、提取和转换。因此，一系列的数据处理阶段可以被称为**数据管道**。

> *选择哪种设计模式？*

有很多因素需要考虑，例如 **使用哪种数据栈？需要考虑哪些工具？如何从概念上设计数据管道？ETL还是ELT？也许是ETLT？什么是变更数据捕获？**

我将尝试在这里涵盖这些问题。

# 数据管道

因此，它是一个数据处理步骤的序列。由于这些阶段之间的***逻辑数据流连接***，每个阶段生成一个**输出**，作为下一个阶段的**输入**。

> 只要在 A 点和 B 点之间有数据处理，就存在数据管道。

数据管道的三个主要部分是**源、处理步骤或步骤，以及目标**。从外部 API（源）提取的数据可以加载到数据仓库（目标）中。这是一个最常见的数据管道示例，其中源和目标是不同的。然而，并非总是如此，因为*目标到目标*的管道也存在。
