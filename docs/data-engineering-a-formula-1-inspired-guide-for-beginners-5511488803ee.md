# 数据工程：一份受一级方程式启发的初学者指南

> 原文：[https://towardsdatascience.com/data-engineering-a-formula-1-inspired-guide-for-beginners-5511488803ee?source=collection_archive---------1-----------------------#2023-12-04](https://towardsdatascience.com/data-engineering-a-formula-1-inspired-guide-for-beginners-5511488803ee?source=collection_archive---------1-----------------------#2023-12-04)

## 数据工程入门者用例词汇表

[](https://matteo-consoli.medium.com/?source=post_page-----5511488803ee--------------------------------)[![Matteo Consoli](../Images/20613484ba53baf5920608799339c8e0.png)](https://matteo-consoli.medium.com/?source=post_page-----5511488803ee--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5511488803ee--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5511488803ee--------------------------------) [Matteo Consoli](https://matteo-consoli.medium.com/?source=post_page-----5511488803ee--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3dda4e92359f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-engineering-a-formula-1-inspired-guide-for-beginners-5511488803ee&user=Matteo+Consoli&userId=3dda4e92359f&source=post_page-3dda4e92359f----5511488803ee---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5511488803ee--------------------------------) ·6分钟阅读·2023年12月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5511488803ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-engineering-a-formula-1-inspired-guide-for-beginners-5511488803ee&user=Matteo+Consoli&userId=3dda4e92359f&source=-----5511488803ee---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5511488803ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-engineering-a-formula-1-inspired-guide-for-beginners-5511488803ee&source=-----5511488803ee---------------------bookmark_footer-----------)![](../Images/07a6c325f61608f673752892fe443dea.png)

*在工作中的快乐数据工程师*

> **你是一个对现代数据基础设施感兴趣的数据工程新手吗？** 我敢打赌你是的，这篇文章就是为你准备的！
> 
> 在本指南中，**数据工程遇见了一级方程式**。不过，我们会保持简单明了。

# 介绍

我坚信，描述一个概念的最佳方式是通过实例，尽管我的一些大学教授曾经说过，*“如果你需要一个例子来解释它，说明你没有理解”*。

无论如何，我在大学课堂上没有足够专注，今天我将通过——*猜猜看*——**一个例子**来带你了解数据层。

# 业务场景与数据架构

想象一下：明年，一个新的车队，**红雷赛车**，会联系我们（是的，就是你和我）来建立他们的新数据基础设施。

在今天的一级方程式中，数据是核心，比20或30年前重要得多。赛车队通过惊人的数据驱动方法逐毫秒地提升性能。
