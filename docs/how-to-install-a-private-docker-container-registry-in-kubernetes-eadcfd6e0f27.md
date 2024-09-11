# 如何在 Kubernetes 中安装私有 Docker 容器注册表

> 原文：[https://towardsdatascience.com/how-to-install-a-private-docker-container-registry-in-kubernetes-eadcfd6e0f27?source=collection_archive---------16-----------------------#2023-02-01](https://towardsdatascience.com/how-to-install-a-private-docker-container-registry-in-kubernetes-eadcfd6e0f27?source=collection_archive---------16-----------------------#2023-02-01)

## 完全掌控你的镜像存储位置

[](https://medium.knulst.de/?source=post_page-----eadcfd6e0f27--------------------------------)[![Paul Knulst](../Images/9fcb767d927a1fe53ee739c584fdf92c.png)](https://medium.knulst.de/?source=post_page-----eadcfd6e0f27--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eadcfd6e0f27--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----eadcfd6e0f27--------------------------------) [Paul Knulst](https://medium.knulst.de/?source=post_page-----eadcfd6e0f27--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1282c85b5cbc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-install-a-private-docker-container-registry-in-kubernetes-eadcfd6e0f27&user=Paul+Knulst&userId=1282c85b5cbc&source=post_page-1282c85b5cbc----eadcfd6e0f27---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eadcfd6e0f27--------------------------------) ·7 分钟阅读·2023年2月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Feadcfd6e0f27&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-install-a-private-docker-container-registry-in-kubernetes-eadcfd6e0f27&user=Paul+Knulst&userId=1282c85b5cbc&source=-----eadcfd6e0f27---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Feadcfd6e0f27&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-install-a-private-docker-container-registry-in-kubernetes-eadcfd6e0f27&source=-----eadcfd6e0f27---------------------bookmark_footer-----------)![](../Images/7845977d57ebf55058ab85daf608d88c.png)

[图片由 Growtika](https://unsplash.com/@growtika?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/wLknZfsKmxQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

托管 Docker 私有注册表让你完全控制镜像的存储位置和访问方式。如果你开发的是不应公开在 Docker Hub 上的私人项目，这尤其有用。

在本教程中，你将学习如何在任何 Kubernetes 集群中安装一个私有 Docker 注册表。这是一个 [之前在博客上发布的教程的后续](https://www.paulsblog.dev/how-to-install-traefik-ingress-controller-in-kubernetes/)，因为它将使用 Traefik Ingress Controller 来暴露 Docker 注册表。

# 准备工作

## 创建 Kubernetes 命名空间

第一步是创建一个 Kubernetes 命名空间，在本教程中所有资源都将在其中应用：

```py
kubectl create namespace docker-registry
```

## PersistentVolumeClaim

在这一部分，你将通过使用 PersistentVolumeClaim 将一个卷挂载到一个专用的 Kubernetes Pod 中。PersistentVolumeClaim (`PVC`) 是一种 Kubernetes 资源，用于使用预定义的抽象 PersistentVolume (`PV`) 存储，而无需暴露这些卷的实现方式。
