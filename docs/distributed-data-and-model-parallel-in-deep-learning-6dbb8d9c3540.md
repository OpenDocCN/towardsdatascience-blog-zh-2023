# PyTorch 中的分布式数据并行和分布式模型并行

> 原文：[`towardsdatascience.com/distributed-data-and-model-parallel-in-deep-learning-6dbb8d9c3540?source=collection_archive---------11-----------------------#2023-05-08`](https://towardsdatascience.com/distributed-data-and-model-parallel-in-deep-learning-6dbb8d9c3540?source=collection_archive---------11-----------------------#2023-05-08)

## 学习分布式数据并行和分布式模型并行如何在随机梯度下降中工作，使您能够在庞大的数据集上训练庞大的模型

[](https://jasonweiyi.medium.com/?source=post_page-----6dbb8d9c3540--------------------------------)![Wei Yi](https://jasonweiyi.medium.com/?source=post_page-----6dbb8d9c3540--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6dbb8d9c3540--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6dbb8d9c3540--------------------------------) [Wei Yi](https://jasonweiyi.medium.com/?source=post_page-----6dbb8d9c3540--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1b4bd5317a6e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdistributed-data-and-model-parallel-in-deep-learning-6dbb8d9c3540&user=Wei+Yi&userId=1b4bd5317a6e&source=post_page-1b4bd5317a6e----6dbb8d9c3540---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6dbb8d9c3540--------------------------------) ·14 min read·2023 年 5 月 8 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6dbb8d9c3540&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdistributed-data-and-model-parallel-in-deep-learning-6dbb8d9c3540&user=Wei+Yi&userId=1b4bd5317a6e&source=-----6dbb8d9c3540---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6dbb8d9c3540&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdistributed-data-and-model-parallel-in-deep-learning-6dbb8d9c3540&source=-----6dbb8d9c3540---------------------bookmark_footer-----------)![](img/ecced4c819e71328af444db89219dc4f.png)

摄影师 [Olga Zhushman](https://unsplash.com/ja/@ori_photostory?utm_source=medium&utm_medium=referral)，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

你一定听说过最近成功的模型，如 ChatGPT，有数万亿个参数，并且使用了数兆字节的数据进行训练。同时，您可能已经经历过，您的深度学习模型只有数千万个参数，甚至无法适应一个 GPU，并且仅使用数千兆字节的数据进行了数天的训练。

如果你想知道为什么其他人在同一生涯中能够取得如此大的成就，并且想要成为他们，请了解这两种技术，它们使得使用海量数据集训练大型深度学习模型成为可能：

+   **分布式数据并行** 将一个小批量数据分配到多个 GPU 上。这使你可以更快地训练。

+   **分布式模型并行** 将模型的参数、梯度和优化器的内部状态分配到多个 GPU 上。这使你可以在 GPU 上加载更大的模型。

分布式数据并行和分布式模型并行有许多 API 实现，例如 [DDP](https://pytorch.org/tutorials/intermediate/ddp_tutorial.html)、[FSDP](https://pytorch.org/docs/stable/fsdp.html) 和 [DeepSpeed](https://github.com/microsoft/DeepSpeed)。它们都有一个共同的主题：将训练数据或模型分成多个部分…
