# 适用于神经网络的最佳优化算法

> 原文：[https://towardsdatascience.com/the-best-optimization-algorithm-for-your-neural-network-d16d87ef15cb?source=collection_archive---------2-----------------------#2023-10-14](https://towardsdatascience.com/the-best-optimization-algorithm-for-your-neural-network-d16d87ef15cb?source=collection_archive---------2-----------------------#2023-10-14)

## 如何选择优化算法并最小化神经网络训练时间。

[](https://medium.com/@riccardo.andreoni?source=post_page-----d16d87ef15cb--------------------------------)[![Riccardo Andreoni](../Images/5e22581e419639b373019a809d6e65c1.png)](https://medium.com/@riccardo.andreoni?source=post_page-----d16d87ef15cb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d16d87ef15cb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d16d87ef15cb--------------------------------) [Riccardo Andreoni](https://medium.com/@riccardo.andreoni?source=post_page-----d16d87ef15cb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76784541161c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-best-optimization-algorithm-for-your-neural-network-d16d87ef15cb&user=Riccardo+Andreoni&userId=76784541161c&source=post_page-76784541161c----d16d87ef15cb---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d16d87ef15cb--------------------------------) · 13 分钟阅读 · 2023年10月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd16d87ef15cb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-best-optimization-algorithm-for-your-neural-network-d16d87ef15cb&user=Riccardo+Andreoni&userId=76784541161c&source=-----d16d87ef15cb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd16d87ef15cb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-best-optimization-algorithm-for-your-neural-network-d16d87ef15cb&source=-----d16d87ef15cb---------------------bookmark_footer-----------)![](../Images/5d74e7bc343277d4b012a49a8ed2de2f.png)

图片来源：[unsplash.com](https://unsplash.com/photos/FfbVFLAVscw)。

开发任何机器学习模型都涉及一个严格的实验过程，该过程遵循[**创意-实验-评估循环**](/ai-ml-practicalities-the-cycle-of-experimentation-fd46fc1f3835)。

![](../Images/89f8be024c84d92865f49b0077b29126.png)

图片由作者提供。

上述周期会重复多次，直到达到满意的性能水平。“实验”阶段包括机器学习模型的编码和训练步骤。由于**模型变得越来越复杂**并且在更**大规模的数据集**上进行训练，训练时间不可避免地会增加。因此，训练一个大型深度神经网络可能会非常缓慢。

幸运的是，对于数据科学从业者来说，存在几种加速训练过程的技术，包括：

+   **迁移学习**。

+   **权重初始化**，如Glorot或He初始化。

+   **批量归一化**用于训练数据。

+   选择**可靠的激活函数**。

+   使用**更快的优化器**。
