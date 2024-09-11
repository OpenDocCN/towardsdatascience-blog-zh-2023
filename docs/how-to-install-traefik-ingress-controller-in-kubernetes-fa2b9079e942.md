# 如何在 Kubernetes 中安装 Traefik Ingress Controller

> 原文：[https://towardsdatascience.com/how-to-install-traefik-ingress-controller-in-kubernetes-fa2b9079e942?source=collection_archive---------18-----------------------#2023-01-04](https://towardsdatascience.com/how-to-install-traefik-ingress-controller-in-kubernetes-fa2b9079e942?source=collection_archive---------18-----------------------#2023-01-04)

## 提供负载均衡、基于名称的虚拟主机和 SSL 终止

[](https://medium.knulst.de/?source=post_page-----fa2b9079e942--------------------------------)[![Paul Knulst](../Images/9fcb767d927a1fe53ee739c584fdf92c.png)](https://medium.knulst.de/?source=post_page-----fa2b9079e942--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fa2b9079e942--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fa2b9079e942--------------------------------) [Paul Knulst](https://medium.knulst.de/?source=post_page-----fa2b9079e942--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1282c85b5cbc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-install-traefik-ingress-controller-in-kubernetes-fa2b9079e942&user=Paul+Knulst&userId=1282c85b5cbc&source=post_page-1282c85b5cbc----fa2b9079e942---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fa2b9079e942--------------------------------) ·5 分钟阅读·2023年1月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffa2b9079e942&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-install-traefik-ingress-controller-in-kubernetes-fa2b9079e942&user=Paul+Knulst&userId=1282c85b5cbc&source=-----fa2b9079e942---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffa2b9079e942&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-install-traefik-ingress-controller-in-kubernetes-fa2b9079e942&source=-----fa2b9079e942---------------------bookmark_footer-----------)![](../Images/2c753cb3418bb0cc7907fc66fb5f7b5c.png)

图片由 [Growtika Developer Marketing Agency](https://unsplash.com/@growtika?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) / [Unsplash](https://unsplash.com/s/photos/kubernetes-k8s?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供

# 介绍

本教程将展示如何在 Kubernetes（或 k8s）中使用 Traefik 作为 Ingress Controller 以提供负载均衡、基于名称的虚拟主机和 SSL 终止。

要跟随本教程，你需要：

+   一个正在运行的 Kubernetes 集群或一个托管的 Kubernetes

+   一个负载均衡器会动态分配你的流量到任何标记为 [LoadBalancer](https://kubernetes.io/docs/tasks/access-application-cluster/create-external-load-balancer/) 的 Kubernetes 资源。

+   一个 PRIMARY_DOMAIN

注意：我在这篇文章中使用的域名是 `PRIMARY_DOMAIN`，请根据需要更改。如果你希望使用的域名是 `paulsblog.dev`，请将 `PRIMARY_DOMAIN` 替换为 `paulsblog.dev`。

# 什么是 Ingress Controller？

Ingress Controller 是一个 API 对象，它会管理对 Kubernetes 集群中任何已部署服务的外部访问。通常使用 HTTP 或 HTTPS。此外，它还提供负载均衡、基于名称的虚拟主机和 SSL 终止。

# 你为什么需要一个 Ingress Controller？
