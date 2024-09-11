# PyTorch 介绍 — 张量与张量计算

> 原文：[https://towardsdatascience.com/pytorch-introduction-tensors-and-tensor-calculations-412ff818bd5b?source=collection_archive---------1-----------------------#2023-11-30](https://towardsdatascience.com/pytorch-introduction-tensors-and-tensor-calculations-412ff818bd5b?source=collection_archive---------1-----------------------#2023-11-30)

## 了解张量以及如何在最著名的机器学习库之一 PyTorch 中使用它们

[](https://ivopbernardo.medium.com/?source=post_page-----412ff818bd5b--------------------------------)[![伊沃·贝尔纳多](../Images/39887b6f3e63a67c0545e87962ad5df0.png)](https://ivopbernardo.medium.com/?source=post_page-----412ff818bd5b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----412ff818bd5b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----412ff818bd5b--------------------------------) [伊沃·贝尔纳多](https://ivopbernardo.medium.com/?source=post_page-----412ff818bd5b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F74eec53531c0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpytorch-introduction-tensors-and-tensor-calculations-412ff818bd5b&user=Ivo+Bernardo&userId=74eec53531c0&source=post_page-74eec53531c0----412ff818bd5b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----412ff818bd5b--------------------------------) ·8分钟阅读·2023年11月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F412ff818bd5b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpytorch-introduction-tensors-and-tensor-calculations-412ff818bd5b&user=Ivo+Bernardo&userId=74eec53531c0&source=-----412ff818bd5b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F412ff818bd5b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpytorch-introduction-tensors-and-tensor-calculations-412ff818bd5b&source=-----412ff818bd5b---------------------bookmark_footer-----------)![](../Images/b8413e710aba7c20bbd31cf22008112b.png)

数学魔法 — 由 AI 生成的图像

在深度学习领域（包括 ChatGPT 构建的基础）中，最重要的库之一是 `pytorch`。与 Tensorflow 框架一起，`pytorch` 是软件开发者和数据科学家最著名的神经网络训练框架之一。除了其易用性和简单的 API 外，它在灵活性和内存使用方面表现出色，使其在多维微积分中极为快速（这是反向传播的重要组成部分，反向传播是用于优化神经网络权重的关键技术）——这些细节使其成为公司在构建深度学习模型时最受追捧的库之一。

在这篇博客文章中，我们将检查一些使用 `pytorch` 的基本操作，并了解如何使用 `tensor` 对象！张量是数据的数学表示，通常有不同的称谓：

+   1 元素张量：通常称为标量，由单一数学值组成。

+   1 维张量：由 *n* 个示例组成，通常称为 1 维向量，并在单一...
