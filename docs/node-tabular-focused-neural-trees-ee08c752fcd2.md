# NODE：表格数据聚焦的神经树

> 原文：[https://towardsdatascience.com/node-tabular-focused-neural-trees-ee08c752fcd2?source=collection_archive---------6-----------------------#2023-07-04](https://towardsdatascience.com/node-tabular-focused-neural-trees-ee08c752fcd2?source=collection_archive---------6-----------------------#2023-07-04)

## 探索 NODE：一种用于表格数据的神经决策树架构

[](https://medium.com/@upadhyan?source=post_page-----ee08c752fcd2--------------------------------)[![Nakul Upadhya](../Images/336cb21272e9b1f098177adbde50e92e.png)](https://medium.com/@upadhyan?source=post_page-----ee08c752fcd2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ee08c752fcd2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ee08c752fcd2--------------------------------) [Nakul Upadhya](https://medium.com/@upadhyan?source=post_page-----ee08c752fcd2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4d9dddc62a80&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnode-tabular-focused-neural-trees-ee08c752fcd2&user=Nakul+Upadhya&userId=4d9dddc62a80&source=post_page-4d9dddc62a80----ee08c752fcd2---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ee08c752fcd2--------------------------------) ·7 min read·2023年7月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fee08c752fcd2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnode-tabular-focused-neural-trees-ee08c752fcd2&user=Nakul+Upadhya&userId=4d9dddc62a80&source=-----ee08c752fcd2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fee08c752fcd2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnode-tabular-focused-neural-trees-ee08c752fcd2&source=-----ee08c752fcd2---------------------bookmark_footer-----------)

近年来，机器学习的普及度激增，神经深度学习模型在图像和文本处理等复杂任务中远远超越了浅层模型，如 XGBoost [4]。然而，深度模型在处理表格数据时通常不如这些浅层模型有效，并且没有一种通用的深度学习方法能 consistently 超越梯度提升树。

为了填补这一空白，俄罗斯互联网服务公司 Yandex 的研究人员提出了一种新架构：神经遗忘决策集成（NODE）[1]。这个网络利用轻量级且可解释的神经决策树，并将其集成在神经网络框架中。这使得模型能够捕捉表格数据中的复杂交互和依赖关系，同时保持可解释性。

在这篇文章中，我旨在解释NODE是如何工作的，以及它的各种属性如何使其成为一个既强大又易于解释的预测模型。像往常一样，我鼓励大家阅读原始论文。如果你想使用NODE，请查看该模型的GitHub。

这篇文章是关于神经决策树系列的一部分，这些架构高度可解释，提供的预测能力等同于传统的深度网络。
