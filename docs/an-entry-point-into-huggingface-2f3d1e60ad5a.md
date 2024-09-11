# 进入 HuggingFace 的入口点

> 原文：[https://towardsdatascience.com/an-entry-point-into-huggingface-2f3d1e60ad5a?source=collection_archive---------2-----------------------#2023-11-26](https://towardsdatascience.com/an-entry-point-into-huggingface-2f3d1e60ad5a?source=collection_archive---------2-----------------------#2023-11-26)

## 初学者的基础逐步指南

[](https://medium.com/@mina.ghashami?source=post_page-----2f3d1e60ad5a--------------------------------)[![Mina Ghashami](../Images/745f53b94f5667a485299b49913c7a21.png)](https://medium.com/@mina.ghashami?source=post_page-----2f3d1e60ad5a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2f3d1e60ad5a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2f3d1e60ad5a--------------------------------) [Mina Ghashami](https://medium.com/@mina.ghashami?source=post_page-----2f3d1e60ad5a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc99ed9ed7b9a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-entry-point-into-huggingface-2f3d1e60ad5a&user=Mina+Ghashami&userId=c99ed9ed7b9a&source=post_page-c99ed9ed7b9a----2f3d1e60ad5a---------------------post_header-----------) 发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----2f3d1e60ad5a--------------------------------) ·15分钟阅读·2023年11月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2f3d1e60ad5a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-entry-point-into-huggingface-2f3d1e60ad5a&user=Mina+Ghashami&userId=c99ed9ed7b9a&source=-----2f3d1e60ad5a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2f3d1e60ad5a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-entry-point-into-huggingface-2f3d1e60ad5a&source=-----2f3d1e60ad5a---------------------bookmark_footer-----------)![](../Images/a6112804c83872912af719222f58fd92.png)

图片来自[unsplash](https://unsplash.com/photos/person-holding-ac-receiver-_J3oTl6acVg)

HuggingFace 如果你不知道从哪里开始学习，可能会显得复杂而困难。进入 HuggingFace 仓库的一个入口点是[run_mlm.py](https://github.com/huggingface/transformers/blob/main/examples/pytorch/language-modeling/run_mlm.py)和[run_clm.py](https://github.com/huggingface/transformers/blob/main/examples/pytorch/language-modeling/run_clm.py)脚本。

在这篇文章中，我们将详细介绍[run_mlm.py](https://github.com/huggingface/transformers/blob/main/examples/pytorch/language-modeling/run_mlm.py)脚本。该脚本从HuggingFace选择一个掩码语言模型，并在数据集上进行微调（或从头开始训练）。如果你是初学者，对HuggingFace代码了解不多，这篇文章将帮助你理解基础知识。

> 我们将选择一个掩码语言模型，从HuggingFace加载数据集，并在数据集上对模型进行微调。最后，我们将评估模型。这一切都是为了理解代码结构，因此我们的重点不在于任何特定的使用案例。

让我们开始吧。

# 微调简介

微调是深度学习中的一种常见技术，用于对一个预训练的神经网络模型进行调整，使其更好地适应新的数据集或任务。

微调在你的数据集不够大，无法从头训练一个深度模型时效果很好！因此，你从一个已经学习过的基础模型开始。
