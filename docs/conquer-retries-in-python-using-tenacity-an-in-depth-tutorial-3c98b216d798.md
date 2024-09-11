# 使用 Tenacity 征服 Python 中的重试：一个端到端教程

> 原文：[`towardsdatascience.com/conquer-retries-in-python-using-tenacity-an-in-depth-tutorial-3c98b216d798?source=collection_archive---------5-----------------------#2023-07-01`](https://towardsdatascience.com/conquer-retries-in-python-using-tenacity-an-in-depth-tutorial-3c98b216d798?source=collection_archive---------5-----------------------#2023-07-01)

## [PYTHON 工具箱](https://medium.com/@qtalen/list/python-toolbox-4289824c6407)

## 提升你的 Python 项目，运用强大的重试机制和错误处理技巧

[](https://qtalen.medium.com/?source=post_page-----3c98b216d798--------------------------------)![Peng Qian](https://qtalen.medium.com/?source=post_page-----3c98b216d798--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3c98b216d798--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3c98b216d798--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----3c98b216d798--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e2fe735546d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconquer-retries-in-python-using-tenacity-an-in-depth-tutorial-3c98b216d798&user=Peng+Qian&userId=8e2fe735546d&source=post_page-8e2fe735546d----3c98b216d798---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3c98b216d798--------------------------------) ·5 分钟阅读·2023 年 7 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3c98b216d798&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconquer-retries-in-python-using-tenacity-an-in-depth-tutorial-3c98b216d798&user=Peng+Qian&userId=8e2fe735546d&source=-----3c98b216d798---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3c98b216d798&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconquer-retries-in-python-using-tenacity-an-in-depth-tutorial-3c98b216d798&source=-----3c98b216d798---------------------bookmark_footer-----------)![](img/7d3089c27ffefc03c909a68dcb0d46b3.png)

许多时候，再试一次会带来成功。图片来源：作者创建，[Canva](https://www.canva.com/)

本文将讨论 Tenacity 的基本用法和自定义功能。

这个实用的 Python 库提供了重试机制。我们还将通过一个实际示例来探索 Tenacity 的重试和异常处理功能。

# 介绍

想象一下，你管理着数百个网络服务，有些位于海外（延迟高），有些则相当老旧（不太稳定）。你会有什么感受？

我的同事王在这种困境中。他告诉我，他感到非常沮丧：

他每天需要检查这些远程服务的调用状态，常常遇到超时问题或其他异常。故障排除尤其具有挑战性。

此外，大部分客户端代码是他的前任编写的，这使得进行大规模重构非常困难。因此，这些服务必须继续按原样运行。

如果能够在出现异常后自动重新连接这些远程调用，那就太好了…
