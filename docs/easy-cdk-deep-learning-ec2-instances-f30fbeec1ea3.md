# 简单的CDK深度学习EC2实例

> 原文：[https://towardsdatascience.com/easy-cdk-deep-learning-ec2-instances-f30fbeec1ea3?source=collection_archive---------11-----------------------#2023-01-18](https://towardsdatascience.com/easy-cdk-deep-learning-ec2-instances-f30fbeec1ea3?source=collection_archive---------11-----------------------#2023-01-18)

## 使用CDK部署用于深度学习的EC2实例非常简单

[](https://nbertagnolli.medium.com/?source=post_page-----f30fbeec1ea3--------------------------------)[![Nicolas Bertagnolli](../Images/6f2ee2f49fae47f1dd89d8484303b20c.png)](https://nbertagnolli.medium.com/?source=post_page-----f30fbeec1ea3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f30fbeec1ea3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f30fbeec1ea3--------------------------------) [Nicolas Bertagnolli](https://nbertagnolli.medium.com/?source=post_page-----f30fbeec1ea3--------------------------------)

·

[阅读更多](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa998d10e34d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feasy-cdk-deep-learning-ec2-instances-f30fbeec1ea3&user=Nicolas+Bertagnolli&userId=a998d10e34d8&source=post_page-a998d10e34d8----f30fbeec1ea3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f30fbeec1ea3--------------------------------) ·10分钟阅读·2023年1月18日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff30fbeec1ea3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feasy-cdk-deep-learning-ec2-instances-f30fbeec1ea3&user=Nicolas+Bertagnolli&userId=a998d10e34d8&source=-----f30fbeec1ea3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff30fbeec1ea3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feasy-cdk-deep-learning-ec2-instances-f30fbeec1ea3&source=-----f30fbeec1ea3---------------------bookmark_footer-----------)![](../Images/5946aec4ae6ed299708df43b4bca69d2.png)

我们今天构建内容的大致概览。

# 简介

作为数据科学家，我常常需要在云中运行 GPU 作业，并且对点击界面感到非常厌烦。过去当我需要一个 GPU 实例进行一些临时模型训练时，我会费劲心力地在 AWS 的 UI 中导航。不再是这样了！现在创建一个 EC2 堆栈，知道如何安装和实例化你需要的一切，非常简单。在本教程中，我们将使用 AWS CDK 创建一个 EC2 实例。我们将把这个 EC2 实例保护在 VPC 的私有子网中，只允许入站流量。然后，我们将使用 AWS [安全会话管理器](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html)（SSM）连接到这个实例。这很酷，因为过去如果你想让你的机器在一个完全私有的网络中，你需要一个堡垒主机，而有了 SSM，这种情况就不再存在了！本教程的所有代码可以在这里找到。在这一切结束后，你将能够运行几个命令，拥有一个功能完整的、数据科学准备好的安全 EC2 实例，并配备 JupyterLab 界面。

本教程的所有代码可以在 [这里](https://github.com/nbertagnolli/ds-ec2) 找到。

# CDK

CDK 是 AWS 创建的云开发工具包。它允许我们使用如 python 或 typescript 等语言在云中创建资源。它真的很容易使用，而且是免费的…
