# Kubernetes中的动态MIG分区

> 原文：[https://towardsdatascience.com/dynamic-mig-partitioning-in-kubernetes-89db6cdde7a3?source=collection_archive---------4-----------------------#2023-01-26](https://towardsdatascience.com/dynamic-mig-partitioning-in-kubernetes-89db6cdde7a3?source=collection_archive---------4-----------------------#2023-01-26)

## 最大化GPU利用率并降低基础设施成本。

[](https://medium.com/@telemaco019?source=post_page-----89db6cdde7a3--------------------------------)[![Michele Zanotti](../Images/6350ad98e5f057991b2e6f1a86a5c350.png)](https://medium.com/@telemaco019?source=post_page-----89db6cdde7a3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----89db6cdde7a3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----89db6cdde7a3--------------------------------) [Michele Zanotti](https://medium.com/@telemaco019?source=post_page-----89db6cdde7a3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9b7af839d7e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-mig-partitioning-in-kubernetes-89db6cdde7a3&user=Michele+Zanotti&userId=9b7af839d7e&source=post_page-9b7af839d7e----89db6cdde7a3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----89db6cdde7a3--------------------------------) ·9分钟阅读·2023年1月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F89db6cdde7a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-mig-partitioning-in-kubernetes-89db6cdde7a3&user=Michele+Zanotti&userId=9b7af839d7e&source=-----89db6cdde7a3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F89db6cdde7a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-mig-partitioning-in-kubernetes-89db6cdde7a3&source=-----89db6cdde7a3---------------------bookmark_footer-----------)![](../Images/faced2bd5b9da0f06d26dcaf220db464.png)

图片来源：[Growtika](https://unsplash.com/@growtika?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

为了减少基础设施开支，使用GPU加速器的高效方式至关重要。一种实现方法是将GPU划分为更小的分区，即切片，以便容器仅请求必要的资源。一些工作负载可能只需要GPU计算和内存的一小部分，因此在Kubernetes中能够将单个GPU分成多个切片，并允许个别容器请求这些切片是非常重要的。

这对用于运行人工智能 (AI) 和高性能计算 (HPC) 工作负载的大型Kubernetes集群特别相关，因为GPU利用效率低下可能对基础设施费用产生重大影响。这些低效通常是由于没有充分利用GPU的轻量级任务，如推理服务器和用于初步数据和模型探索的[Jupyter Notebooks](https://jupyter.org/)。

例如，[欧洲核子研究组织 (CERN)](https://home.cern/)的研究人员发布了一篇[博客文章](https://kubernetes.web.cern.ch/blog/2023/01/09/efficient-access-to-shared-gpu-resources-part-1/?utm_id=FAUN_Kaptain356_Link_title)，介绍了他们如何使用MIG GPU分区来解决由波动性工作负载运行高能物理 (HEP) 模拟和代码效率低下导致的低GPU利用率问题。

NVIDIA GPU Operator支持在Kubernetes中使用MIG，但仅凭它不足以确保高效的GPU分区。在本文中，我们将探讨原因，并提供在Kubernetes中使用MIG的更有效解决方案：**动态MIG分区**。

# Kubernetes中的MIG支持

Kubernetes中的MIG支持由[NVIDIA设备插件](https://github.com/NVIDIA/k8s-device-plugin)提供，该插件允许将MIG设备（即隔离的GPU分区）暴露为通用的`nvidia.com/gpu`资源或特定资源类型，例如`nvidia.com/mig-1g.10gb`。

通过`nvidia-smi`手动管理MIG设备是不切实际的，因此NVIDIA提供了一种名为[nvidia-mig-parted](https://github.com/NVIDIA/mig-parted)的工具。该工具允许集群管理员声明性地定义节点上所有GPU所需的MIG设备集。该工具自动管理GPU分区状态，以匹配所需的配置。例如，以下是从nvidia-mig-parted GitHub存储库中提取的配置示例：

```py
version: v1
mig-configs:
  all-disabled:
    - devices: all
      mig-enabled: false

  all-enabled:
    - devices: all
      mig-enabled: true
      mig-devices: {}

  all-1g.5gb:
    - devices: all
      mig-enabled: true
      mig-devices:
        "1g.5gb": 7

  all-2g.10gb:
    - devices: all
      mig-enabled: true
      mig-devices:
        "2g.10gb": 3

  all-3g.20gb:
    - devices: all
      mig-enabled: true
      mig-devices:
        "3g.20gb": 2
```

在Kubernetes中，集群管理员通常不会直接使用nvidia-mig-parted，而是通过[NVIDIA GPU Operator](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/gpu-operator-mig.html)来使用它。

这个操作程序进一步简化了MIG配置的应用。创建定义了一组允许的MIG配置的ConfigMap之后，NVIDIA GPU Operator只需要你用`nvidia.com/mig.config`标记节点，并指定作为值你想在该节点上应用的具体配置的名称。

例如，参考上述定义的配置，我们可以将配置`all-3g.20gb`应用于节点`node-1`，如下所示：

```py
kubectl label nodes node1 "nvidia.com/mig.config=all-2g.20gb"
```

## 静态MIG配置会导致较差的可用性

NVIDIA GPU Operator有一个显著的限制：MIG设备是通过静态配置创建的。

这意味着集群管理员必须首先定义他们认为可能需要的所有 MIG 配置，然后根据需要手动将其应用于每个节点。

这种管理 MIG 设备的方式很容易导致 GPU 利用率低下以及集群管理员花费大量时间更改 MIG 配置。实际上，GPU 内存和计算需求因 Pod 而异，并且会随着时间变化。为了在创建具有不同 MIG 资源请求的新 Pod 时实现最佳 GPU 利用率，集群管理员必须不断花时间寻找和应用最合适的配置，这对每个节点来说非常不切实际。

作为一个简单的例子，假设我们需要调度多个需要 20GB GPU 内存的 Pods。因此，我们将创建并应用一个配置，该配置在集群中的所有 GPU 上提供 `nvidia.com/mig-3.20gb` 配置，因为它可以完美地利用所有 GPU 资源。然而，稍后，服务器收到创建一些需要较少资源的 Pods 的请求，例如 10GB GPU 内存，对应于 MIG 配置 `nvidia.com/mig-2g.10gb`。这些 Pods 直到集群管理员更改至少一个节点的标签并应用提供所请求配置的 MIG 配置之前都不会被调度。

问题并没有在这里结束。虽然某个配置可能提供所需的 MIG 资源，但它同时可能会移除一些当前由容器使用的设备。在这种情况下，集群管理员需要寻找或创建最合适的配置，并确保不会删除任何正在使用的设备，从而引入显著的操作成本。

> *这种方法根本无法扩展。仅靠 NVIDIA GPU 操作员，不可能根据工作负载需求不断调整 MIG 配置，从而导致未使用的 MIG 设备和待处理的 Pods。*

让我们看看如何通过动态 MIG 分区解决这个问题。

# 动态 MIG 分区

动态 MIG 分区根据集群中工作负载的实时需求自动创建和删除 MIG 配置，确保始终将最佳的 MIG 配置应用于可用的 GPU。

要应用动态分区，我们需要使用 `[nos](https://github.com/nebuly-ai/nos)`，这是一个开源模块，它与 NVIDIA GPU 操作员一起运行，使 MIG 分区变得动态。

你可以把 `nos` 看作是 GPUs 的 [集群自动扩展器](https://github.com/kubernetes/autoscaler)：它不是增加节点和 GPU 的数量，而是动态地分区它们以最大化其利用率，从而导致闲置的 GPU 容量。然后，你可以调度更多的 Pods 或减少所需的 GPU 节点数量，从而降低基础设施成本。

> *有了* `*nos*` *，无需手动创建和管理 MIG 配置。只需将你的 Pods 提交到集群，请求的 MIG 设备将自动配置。*

让我们深入了解 `nos` 和动态 MIG 分区在实际中的工作方式。

## 先决条件

如前所述，`nos` 并不会替代 NVIDIA GPU Operator，而是与其一起工作。因此，你需要首先安装它，并满足两个要求：

1.  `mig.strategy` 必须设置为 `mixed`，这样每个不同的 MIG 配置文件就会作为特定的资源类型暴露给 Kubernetes。

1.  `migManager` 必须被禁用

如果尚未完成，你可以通过 Helm 安装 NVIDIA GPU Operator，如下所示：

```py
helm install --wait \
  --generate-name \
  -n gpu-operator \   
  --create-namespace nvidia/gpu-operator \
  --set mig.strategy=mixed \ 
  --set migManager.enabled=false
```

默认情况下，MIG 模式在 NVIDIA GPU 上未激活。因此，首先你需要在所有你希望进行分区的 GPU 上启用 MIG。你可以通过 SSH 进入节点并为每个 GPU 运行以下命令，其中 `<index>` 对应于各自的索引：

```py
sudo nvidia-smi -i <index> -mig 1
```

根据你使用的机器类型，可能需要在此操作后重启节点。有关更多信息和故障排除，请参阅 [NVIDIA MIG 用户指南](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/index.html#enable-mig-mode)。

## 安装

一旦你安装了 NVIDIA GPU Operator 并启用了你的 GPU 上的 MIG 模式，你可以简单地按如下方式安装 `nos`：

```py
helm install oci://ghcr.io/nebuly-ai/helm-charts/nos \
  --version 0.1.0 \
  --namespace nebuly-nos \
  --generate-name \
  --create-namespace
```

就是这样！现在你可以在你的节点上激活动态 MIG 分区。

## 动态分区操作中

首先，你需要向 `nos` 指定它应该管理哪些节点的 GPU 分区。将这些节点标记如下：

```py
kubectl label nodes <node-names> "nos.nebuly.com/gpu-partitioning=mig"
```

这个标签将节点标记为“MIG 节点”，将所有节点 GPU 的 MIG 设备管理委托给 `nos`。

之后，你可以提交请求 MIG 资源的工作负载。`nos` 将自动找到并应用你之前标记为“MIG 节点”的节点上的最佳 MIG 配置，创建 Pods 请求的缺失 MIG 设备，并删除不必要的未使用设备。

让我们看一个 `nos` 实际操作的简单示例。

假设我们操作一个简单的集群，其中一个节点有一个 NVIDIA A100 80GB。我们已经在该 GPU 上启用了 MIG 模式，因此我们可以为该节点启用自动分区：

```py
kubectl label nodes aks-gpua100-24975740-vmss000000 "nos.nebuly.com/gpu-partitioning=mig"
```

`kubectl describe node aks-gpua100–24975740-vmss000000` 的输出显示节点没有任何可用的 MIG 资源，因为尚未请求或创建任何 MIG 设备：

```py
Capacity:
  cpu:                24
  ephemeral-storage:  129886128Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             226783616Ki
  pods:               30
Allocatable:
  cpu:                23660m
  ephemeral-storage:  119703055367
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             214314368Ki
  pods:               30
```

让我们创建一些请求 MIG 资源的 Pods。在这种情况下，我们创建一个部署，包含 5 个 Pod 副本，每个 Pod 的容器请求 10 GB 内存的 GPU 切片。

```py
kubectl apply -f - <<EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deployment-1
  namespace: demo
spec:
  replicas: 5
  selector:
    matchLabels:
      app: dummy
  template:
    metadata:
      labels:
        app: dummy
    spec:
      containers:
        - name: sleepy
          image: busybox:latest
          command: ["sleep", "120"]
          resources:
            limits:
              nvidia.com/mig-1g.10gb: 1
EOF 
```

现在，命名空间 `demo` 中有 5 个待处理的 Pods，请求总共五个 `nvidia.com/mig-1g.10gb` 资源，而这些资源在集群中尚不可用：

```py
❯ kubectl get pods -n demo
NAME                           READY   STATUS    RESTARTS   AGE
deployment-1-f677f7555-7j7f6   0/1     Pending   0          5s
deployment-1-f677f7555-j9hdx   0/1     Pending   0          5s
deployment-1-f677f7555-lpg28   0/1     Pending   0          5s
deployment-1-f677f7555-lwpz5   0/1     Pending   0          5s
deployment-1-f677f7555-nj489   0/1     Pending   0          5s
```

几秒钟内，`nos` 将检测到这些待处理 Pods。它将尝试创建所请求的资源，选择最合适的 MIG 配置。在这个例子中，`nos` 应用了一种提供五个 1g.10gb 和一个 2g.20gb 设备的配置：

```py
Capacity:
  cpu:                     24
  ephemeral-storage:       129886128Ki
  hugepages-1Gi:           0
  hugepages-2Mi:           0
  memory:                  226783616Ki
  nvidia.com/mig-1g.10gb:  5
  nvidia.com/mig-2g.20gb:  1
  pods:                    30
Allocatable:
  cpu:                     23660m
  ephemeral-storage:       119703055367
  hugepages-1Gi:           0
  hugepages-2Mi:           0
  memory:                  214314368Ki
  nvidia.com/mig-1g.10gb:  5
  nvidia.com/mig-2g.20gb:  1
  pods:                    30
```

如果我们再次检查 Pods 的状态，可以看到这次它们现在处于运行状态：

```py
❯ kubectl get pods -n demo
NAME                           READY   STATUS    RESTARTS   AGE
deployment-1-f677f7555-7j7f6   1/1     Running   0          92s
deployment-1-f677f7555-j9hdx   1/1     Running   0          92s
deployment-1-f677f7555-lpg28   1/1     Running   0          92s
deployment-1-f677f7555-lwpz5   1/1     Running   0          92s
deployment-1-f677f7555-nj489   1/1     Running   0          92s
```

请注意，除了 1g.10gb 设备外，`nos` 还创建了额外的 2g.20gb 设备。这是因为每种 MIG GPU 模型仅支持 [特定的配置集合](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/#supported-profiles)，在这种情况下，满足所需设备的最佳配置也包括 2g.20gb 设备。请记住：

+   `nos` 选择允许调度最多待处理 Pods 的配置，这一计算是通过 `nos` 内部调度器进行的调度模拟完成的。

+   已经在使用中的 MIG 设备不会被删除。任何需要删除这些设备的 MIG 配置都会被拒绝。

# 结论

请求 GPU 切片的可能性对提高 GPU 利用率和降低基础设施成本至关重要。

NVIDIA MIG 允许创建完全隔离的 GPU 实例，配备专用的内存和计算资源，但如果我们希望实现卓越的操作，NVIDIA GPU 操作员提供的 Kubernetes 支持是不够的。静态配置不能自动调整以适应工作负载的变化需求，因此无法为每个 Pod 提供所需的 GPU 切片，特别是在需要不同内存和计算切片的工作负载场景中，这些需求随着时间的推移而变化。

> `*nos*` *通过动态 GPU 分区克服了 NVIDIA GPU 操作员静态配置的限制，这种方法提高了 GPU 利用率，减少了在集群节点上手动定义和应用 MIG 配置的操作负担。*

值得注意的是，NVIDIA MIG 有其局限性，并不是唯一的分区技术，也不是提高 Kubernetes 集群利用率的唯一方法。具体而言，MIG 仅支持较新的架构（Ampere 和 Hopper），且不提供细粒度的 GPU 分区，这意味着无法创建具有任意内存和计算资源的 GPU 切片。

为了克服这些限制，`nos` 还提供了 [通过 NVIDIA 多进程服务 (MPS) 的动态 GPU 分区](https://docs.nebuly.com/nos/overview/)，这是一种支持几乎所有 NVIDIA GPU 的分区技术，允许创建任意数量的内存切片。你可以在 [这里](https://docs.nebuly.com/nos/dynamic-gpu-partitioning/getting-started-mps/) 找到有关动态 MPS 分区的更多信息。

# 资源

+   [动态 GPU 分区文档](https://docs.nebuly.com/nos/dynamic-gpu-partitioning/overview/)

+   [Nos 源代码](https://github.com/nebuly-ai/nos)

+   [NVIDIA GPU 操作员文档](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/getting-started.html)

+   [NVIDIA MIG 用户指南](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/)

# 版权

特别感谢[Emile Courthoud](https://medium.com/u/2c53981c8a83?source=post_page-----89db6cdde7a3--------------------------------)对本文的审阅和贡献。
