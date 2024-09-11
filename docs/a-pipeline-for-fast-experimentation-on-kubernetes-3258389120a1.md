# 一个用于在Kubernetes上快速实验的管道

> 原文：[https://towardsdatascience.com/a-pipeline-for-fast-experimentation-on-kubernetes-3258389120a1?source=collection_archive---------26-----------------------#2023-01-31](https://towardsdatascience.com/a-pipeline-for-fast-experimentation-on-kubernetes-3258389120a1?source=collection_archive---------26-----------------------#2023-01-31)

## 仅使用原生Python包

[](https://pascaljanetzky.medium.com/?source=post_page-----3258389120a1--------------------------------)[![Pascal Janetzky](../Images/43d68509b63c5f9b3fc9cef3cbfc1a88.png)](https://pascaljanetzky.medium.com/?source=post_page-----3258389120a1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3258389120a1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3258389120a1--------------------------------) [Pascal Janetzky](https://pascaljanetzky.medium.com/?source=post_page-----3258389120a1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F672b95fdf976&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-pipeline-for-fast-experimentation-on-kubernetes-3258389120a1&user=Pascal+Janetzky&userId=672b95fdf976&source=post_page-672b95fdf976----3258389120a1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3258389120a1--------------------------------) ·6 min read·2023年1月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3258389120a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-pipeline-for-fast-experimentation-on-kubernetes-3258389120a1&user=Pascal+Janetzky&userId=672b95fdf976&source=-----3258389120a1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3258389120a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-pipeline-for-fast-experimentation-on-kubernetes-3258389120a1&source=-----3258389120a1---------------------bookmark_footer-----------)

为每个新实验手动创建一个新的配置文件是一个繁琐的过程。特别是当你希望在Kubernetes集群上快速部署大量作业时，自动化设置是必不可少的。使用Python，构建一个简单的调度脚本来读取实验的配置（如批量大小）、将其写入YAML文件，并创建一个新作业是非常直接的。在这篇文章中，我们将讨论如何实现。最棒的是，我们不需要额外的包！

![](../Images/042919420529caa70761f3408d3c6ec9.png)

照片由 [JJ Ying](https://unsplash.com/@jjying?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

管道由四个文件组成：两个 bash 脚本（一个用于创建 Kubernetes 作业，一个用于删除 Kubernetes 作业），一个 Python 脚本和一个 `.yaml` 文件模板。让我们更详细地了解它们，从 Python 脚本开始。你可以在[这个 GitHub 仓库](https://github.com/phrasenmaeher/kubernetes_yaml_pipeline)找到完整代码。

## Python 脚本

Python 代码被结构化为两个方法。第一个方法生成实验配置；它们填充了示例值。第二个方法进行实际调度，包括解析 `.yaml` 文件和与 Kubernetes 通信。让我们从第一个，更简单的函数开始：
