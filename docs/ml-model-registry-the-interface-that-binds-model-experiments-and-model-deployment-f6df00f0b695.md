# ML 模型注册表——绑定模型实验与模型部署的“接口”

> 原文：[`towardsdatascience.com/ml-model-registry-the-interface-that-binds-model-experiments-and-model-deployment-f6df00f0b695?source=collection_archive---------8-----------------------#2023-02-16`](https://towardsdatascience.com/ml-model-registry-the-interface-that-binds-model-experiments-and-model-deployment-f6df00f0b695?source=collection_archive---------8-----------------------#2023-02-16)

## MLOps 实践——深入探讨 ML 模型注册表、模型版本控制与模型生命周期管理

[](https://medium.com/@weiyunna91?source=post_page-----f6df00f0b695--------------------------------)![YUNNA WEI](https://medium.com/@weiyunna91?source=post_page-----f6df00f0b695--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f6df00f0b695--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f6df00f0b695--------------------------------) [YUNNA WEI](https://medium.com/@weiyunna91?source=post_page-----f6df00f0b695--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4b47aa84fc4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fml-model-registry-the-interface-that-binds-model-experiments-and-model-deployment-f6df00f0b695&user=YUNNA+WEI&userId=4b47aa84fc4&source=post_page-4b47aa84fc4----f6df00f0b695---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f6df00f0b695--------------------------------) ·8 分钟阅读·2023 年 2 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff6df00f0b695&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fml-model-registry-the-interface-that-binds-model-experiments-and-model-deployment-f6df00f0b695&user=YUNNA+WEI&userId=4b47aa84fc4&source=-----f6df00f0b695---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff6df00f0b695&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fml-model-registry-the-interface-that-binds-model-experiments-and-model-deployment-f6df00f0b695&source=-----f6df00f0b695---------------------bookmark_footer-----------)

## **背景**

在我之前的文章中：

MLOps 实践——将 ML 解决方案架构拆解为 10 个组件

我讨论了管理模型元数据和 ML 实验运行生成的工件的架构重要性。我们都知道，模型训练过程会产生许多工件，以便进一步调整 ML 模型的性能，以及后续的 ML 模型部署。这些工件包括训练好的模型本身，以及模型参数和超参数、指标、代码、笔记本、配置等。集中管理和利用这些模型工件和元数据对构建一个稳健的 MLOps 架构至关重要。因此，在今天的文章中，我将讨论 ML 模型注册库，它作为绑定模型实验和模型部署的“接口”。

具体来说，今天的文章将重点讨论以下几个方面：

+   关于……什么是 ML 注册库以及 ML 注册库执行的关键功能的问题

+   ML 模型注册库带来的主要好处

+   如何将 ML 模型注册库集成到端到端的 MLOps 解决方案中
