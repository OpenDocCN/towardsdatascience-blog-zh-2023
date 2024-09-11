# 使用PyTorch和SHAP进行图像分类：你能信任自动驾驶汽车吗？

> 原文：[https://towardsdatascience.com/image-classification-with-pytorch-and-shap-can-you-trust-an-automated-car-4d8d12714eea?source=collection_archive---------7-----------------------#2023-03-21](https://towardsdatascience.com/image-classification-with-pytorch-and-shap-can-you-trust-an-automated-car-4d8d12714eea?source=collection_archive---------7-----------------------#2023-03-21)

## 构建一个对象检测模型，将其与强度阈值进行比较，评估并使用DeepSHAP进行解释。

[](https://conorosullyds.medium.com/?source=post_page-----4d8d12714eea--------------------------------)[![Conor O'Sullivan](../Images/2dc50a24edb12e843651d01ed48a3c3f.png)](https://conorosullyds.medium.com/?source=post_page-----4d8d12714eea--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4d8d12714eea--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4d8d12714eea--------------------------------) [Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----4d8d12714eea--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ae48256fb37&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimage-classification-with-pytorch-and-shap-can-you-trust-an-automated-car-4d8d12714eea&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=post_page-4ae48256fb37----4d8d12714eea---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4d8d12714eea--------------------------------) ·14 min read·2023年3月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4d8d12714eea&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimage-classification-with-pytorch-and-shap-can-you-trust-an-automated-car-4d8d12714eea&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=-----4d8d12714eea---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4d8d12714eea&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimage-classification-with-pytorch-and-shap-can-you-trust-an-automated-car-4d8d12714eea&source=-----4d8d12714eea---------------------bookmark_footer-----------)![](../Images/e0f878484110a05545a3b8a001d4b868.png)

（来源：作者）

如果世界不那么混乱，自驾车将会很简单。但事实并非如此。为了避免严重的伤害，人工智能必须考虑许多变量——速度限制、交通状况以及道路上的障碍物（例如分心的人）。人工智能需要能够检测这些障碍物，并在遇到时采取适当的行动。

幸运的是，我们的应用程序并没有那么复杂。更幸运的是，我们将使用铁罐而不是人类。我们将建立一个模型，用于检测迷你自动化小车前方的障碍物。如果障碍物太近，小车应该**停止**，否则应该**前进**。

从本质上来说，这是一个二分类问题。为了解决这个问题，我们将：

+   使用强度阈值创建基准

+   使用PyTorch构建卷积神经网络（CNN）

+   使用准确率、精确率和召回率来评估模型

+   使用SHAP来解释模型

我们将看到模型不仅表现良好，而且它的*预测方式*似乎也很合理。在过程中，我们将讨论……
