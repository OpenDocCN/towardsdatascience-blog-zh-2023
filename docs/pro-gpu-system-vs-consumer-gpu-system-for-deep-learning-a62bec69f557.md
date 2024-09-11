# 专业 GPU 系统 vs 消费级 GPU 系统在深度学习中的应用

> 原文：[https://towardsdatascience.com/pro-gpu-system-vs-consumer-gpu-system-for-deep-learning-a62bec69f557?source=collection_archive---------2-----------------------#2023-04-19](https://towardsdatascience.com/pro-gpu-system-vs-consumer-gpu-system-for-deep-learning-a62bec69f557?source=collection_archive---------2-----------------------#2023-04-19)

## 硬件

## 为什么你可能考虑使用专业设备

[](https://medium.com/@maclayton?source=post_page-----a62bec69f557--------------------------------)[![Mike Clayton](../Images/2d37746b13b7d2ff1c6515893914da97.png)](https://medium.com/@maclayton?source=post_page-----a62bec69f557--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a62bec69f557--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a62bec69f557--------------------------------) [Mike Clayton](https://medium.com/@maclayton?source=post_page-----a62bec69f557--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F51dce1c5bc03&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpro-gpu-system-vs-consumer-gpu-system-for-deep-learning-a62bec69f557&user=Mike+Clayton&userId=51dce1c5bc03&source=post_page-51dce1c5bc03----a62bec69f557---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a62bec69f557--------------------------------) ·21 分钟阅读·2023年4月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa62bec69f557&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpro-gpu-system-vs-consumer-gpu-system-for-deep-learning-a62bec69f557&user=Mike+Clayton&userId=51dce1c5bc03&source=-----a62bec69f557---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa62bec69f557&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpro-gpu-system-vs-consumer-gpu-system-for-deep-learning-a62bec69f557&source=-----a62bec69f557---------------------bookmark_footer-----------)![](../Images/f24ad59210450dc7f7fab7e6456b0d01.png)

*本文将使用的专业工作站。图片来源于* [*Exxact Corporation*](https://www.exxactcorp.com/category/Deep-Learning-Solutions?page=1&utm_source=web+referral&utm_medium=backlink&utm_campaign=Michael+Clayton&utm_term=Medium+Towards+Data+Science) *，由 Michael Clayton 授权使用*

**在系统中拥有 GPU（或显卡）几乎是训练神经网络，特别是深度神经网络时的必要条件。与 CPU 相比，使用一块相对普通的 GPU 在训练速度上有着天壤之别。**

**……但在什么情况下你可能会考虑跳入专业级而非消费者级的 GPU 领域？在训练和推理速度上是否存在巨大差异？还是说其他因素使得这个跳跃具有吸引力？**

# 引言

本文的目的是让你了解普通消费者（或刚开始学习机器/深度学习）使用的 GPU 与那些用于高端系统的 GPU 之间的主要区别。这些高端系统可能用于开发和/或推理高级深度学习模型。

除了作为了解前沿专业设备与消费者级硬件在纯处理速度方面区别的有趣练习外，它还将突出一些消费者级 GPU 和相关系统在处理前沿深度学习模型时存在的其他限制。
