# 如何在 Kubernetes 中通过 NVIDIA MPS 提高 GPU 利用率

> 原文：[https://towardsdatascience.com/how-to-increase-gpu-utilization-in-kubernetes-with-nvidia-mps-e680d20c3181?source=collection_archive---------1-----------------------#2023-02-02](https://towardsdatascience.com/how-to-increase-gpu-utilization-in-kubernetes-with-nvidia-mps-e680d20c3181?source=collection_archive---------1-----------------------#2023-02-02)

## 在 Kubernetes 中集成 NVIDIA 多进程服务（MPS），以在工作负载之间共享 GPU，最大化利用率并降低基础设施成本

[](https://medium.com/@telemaco019?source=post_page-----e680d20c3181--------------------------------)[![Michele Zanotti](../Images/6350ad98e5f057991b2e6f1a86a5c350.png)](https://medium.com/@telemaco019?source=post_page-----e680d20c3181--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e680d20c3181--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e680d20c3181--------------------------------) [Michele Zanotti](https://medium.com/@telemaco019?source=post_page-----e680d20c3181--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9b7af839d7e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-increase-gpu-utilization-in-kubernetes-with-nvidia-mps-e680d20c3181&user=Michele+Zanotti&userId=9b7af839d7e&source=post_page-9b7af839d7e----e680d20c3181---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e680d20c3181--------------------------------) ·10 分钟阅读·2023 年 2 月 2 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe680d20c3181&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-increase-gpu-utilization-in-kubernetes-with-nvidia-mps-e680d20c3181&user=Michele+Zanotti&userId=9b7af839d7e&source=-----e680d20c3181---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe680d20c3181&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-increase-gpu-utilization-in-kubernetes-with-nvidia-mps-e680d20c3181&source=-----e680d20c3181---------------------bookmark_footer-----------)![](../Images/b62ec1e0ca0e76162ec896ccec566e06.png)

图片由 [Growtika](https://unsplash.com/@growtika?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

大多数工作负载并不需要每个 GPU 的全部内存和计算资源。因此，在多个进程之间共享 GPU 对于提高 GPU 利用率和降低基础设施成本至关重要。

在Kubernetes中，这可以通过将单个GPU暴露为多个资源（即切片），每个资源具有特定的内存和计算大小，供单独的容器请求来实现。通过创建每个容器严格需要的GPU切片，你可以释放集群中的资源。这些资源可以用于调度额外的Pod，或者减少集群中的节点数量。无论哪种情况，进程间的GPU共享可以帮助你降低基础设施成本。

Kubernetes中的GPU支持由[NVIDIA Kubernetes设备插件](https://github.com/NVIDIA/k8s-device-plugin)提供，目前仅支持两种共享策略：时间切片和多实例GPU（MIG）。然而，还有一种平衡时间切片和MIG优缺点的GPU共享策略：[**多进程服务（MPS）**](https://docs.nvidia.com/deploy/mps/index.html)。尽管MPS不受NVIDIA设备插件支持，但在Kubernetes中仍有使用它的方法。

在本文中，我们将首先审查这三种GPU共享技术的优缺点，然后提供如何在Kubernetes中使用MPS的逐步指南。此外，我们还提出了一种自动化管理MPS资源以优化利用率和降低运营成本的解决方案：**动态MPS分区**。

# GPU共享技术概述

共享GPU的三种方法如下：

1.  **时间切片**

1.  **多实例GPU（MIG）**

1.  **多进程服务（MPS）**

在深入了解动态MPS分区的演示之前，我们先概述这些技术。

# 时间切片

时间切片是一种机制，允许落在超额分配GPU上的工作负载彼此交替。时间切片利用GPU时间切片调度器，通过*时间共享*同时执行多个CUDA进程。

当时间切片被激活时，GPU以公平共享的方式在不同进程之间分配计算资源，通过在固定时间间隔内切换进程。这会产生与持续上下文切换相关的计算时间开销，导致抖动和更高的延迟。

> 时间切片几乎被所有GPU架构支持，是在Kubernetes集群中共享GPU的最简单解决方案。然而，进程间的不断切换会产生计算时间开销。此外，时间切片不提供进程间的内存隔离，也没有内存分配限制，这可能导致频繁的内存溢出（OOM）错误。

如果你想在Kubernetes中使用时间切片，你只需编辑NVIDIA设备插件配置。例如，你可以将以下配置应用到一个具有2个GPU的节点。该节点上运行的设备插件将向Kubernetes通告8个`nvidia.com/gpu`资源，而不是2个。这允许每个GPU最多由4个容器共享。

```py
version: v1
sharing:
  timeSlicing:
    resources:
    - name: nvidia.com/gpu
      replicas: 4
```

有关 Kubernetes 中时间切片分区的更多信息，请参阅 [NVIDIA GPU Operator 文档](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/gpu-sharing.html)。

# 多实例 GPU (MIG)

多实例 GPU (MIG) 是 NVIDIA [Ampere](https://www.nvidia.com/en-us/data-center/ampere-architecture/) 和 [Hopper](https://www.nvidia.com/en-us/data-center/technologies/hopper-architecture/) 架构上可用的一项技术，允许将一个 GPU 安全地分割成最多七个独立的 GPU 实例，每个实例都有自己独立的高带宽内存、缓存和计算核心。

隔离的 GPU 切片称为 MIG 设备，它们的命名采用一种格式，以指示设备的计算和内存资源。例如，2g.20gb 对应于一个具有 20 GB 内存的 GPU 切片。

MIG 不允许创建自定义大小和数量的 GPU 切片，因为每种 GPU 型号仅支持 [特定的 MIG 配置](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/#supported-profiles)。这降低了你对 GPU 分割的细粒度控制。此外，MIG 设备的创建必须遵循某些 [放置规则](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/#partitioning)，进一步限制了使用的灵活性。

> MIG 是一种提供最高级别进程隔离的 GPU 共享方式。然而，它缺乏灵活性，仅与少数几种 GPU 架构（Ampere 和 Hopper）兼容。

你可以使用 [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) CLI 手动创建和删除 MIG 设备，或使用 [NVML](https://developer.nvidia.com/nvidia-management-library-nvml) 进行编程操作。这些设备随后通过 NVIDIA Device Plugin 使用 [不同的命名策略](https://docs.google.com/document/d/1mdgMQ8g7WmaI_XVVRrCvHPFPOMCm5LQD5JefgAh6N8g/edit#bookmark=id.vj44q8ogvavv) 被暴露为 Kubernetes 资源。例如，使用 `mixed` 策略时，设备 `1g.10gb` 被暴露为 `nvidia.com/mig-1g.10gb`。而 `single` 策略则将设备暴露为通用的 `nvidia.com/gpu` 资源。

手动使用 nvidia-smi CLI 或 NVML 管理 MIG 设备相当不切实际：在 Kubernetes 中，NVIDIA GPU Operator 提供了一种更简单的 MIG 使用方式，尽管仍然存在一些限制。该操作程序使用一个 [ConfigMap](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/gpu-operator-mig.html#configuring-mig-profiles) 来定义一组允许的 MIG 配置，你可以通过为每个节点打标签来应用这些配置。

你可以编辑这个 ConfigMap 来定义你自己的自定义 MIG 配置，如下例所示。在这个例子中，一个节点被标记为 `nvidia.com/mig.config=all-1g.5gb`。因此，GPU Operator 将该节点的每个 GPU 分割成七个 1g.5gb MIG 设备，这些设备随后被暴露为 Kubernetes 中的 `nvidia.com/mig-1g.5gb` 资源。

```py
apiVersion: v1
kind: ConfigMap
metadata:
  name: default-mig-parted-config
data:
  config.yaml: |
    version: v1
    mig-configs:
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
```

要有效利用集群中的资源与NVIDIA GPU Operator，集群管理员必须持续修改ConfigMap，以适应不断变化的MIG大小和工作负载计算需求。

这非常不切实际。虽然这种方法确实比通过SSH登录节点并手动创建/删除MIG设备要好，但对于集群管理员来说，这非常费力和耗时。因此，MIG设备的配置往往很少随着时间的推移而变化，或者根本没有应用，这两种情况都会导致GPU利用率低下，从而提高基础设施成本。

这个挑战可以通过[动态GPU分区](https://docs.nebuly.com/nos/dynamic-gpu-partitioning/overview/)来克服。稍后在本文中，我们将看到如何使用开源模块`nos`动态分区GPU，这种方法也适用于MIG。

# 多进程服务（MPS）

多进程服务（MPS）是CUDA应用程序编程接口（API）的客户端-服务器实现，用于在同一GPU上并发运行多个进程。

服务器管理GPU访问，为客户端提供并发。客户端通过客户端运行时连接到它，该运行时内置于CUDA驱动程序库中，任何CUDA应用程序都可以透明地使用它。

> MPS与基本上所有现代GPU兼容，并提供了最高的灵活性，允许创建具有任意限制的GPU切片，包括分配内存量和可用计算量。然而，它不强制执行进程间的完全内存隔离。在大多数情况下，MPS是MIG和时间切片之间的良好折中方案。

与时间切片相比，MPS通过*空间共享*并行运行进程，消除了上下文切换的开销，因此能提供更好的计算性能。此外，MPS为每个进程提供自己的GPU内存地址空间。这允许对进程施加内存限制，克服了时间切片共享的限制。

然而，在MPS中，客户端进程之间并未完全隔离。实际上，即使MPS允许限制客户端的计算和内存资源，它也不提供错误隔离和内存保护。这意味着客户端进程可能崩溃并导致整个GPU重置，影响在GPU上运行的所有其他进程。

NVIDIA Kubernetes设备插件不支持MPS分区，这使得在Kubernetes中使用它并不简单。在接下来的部分，我们将探讨通过利用`nos`和另一种Kubernetes设备插件来利用MPS进行GPU共享的替代方法。

# Kubernetes中的多进程服务（MPS）

你可以通过使用Helm安装[NVIDIA设备插件的这个分支](https://github.com/nebuly-ai/k8s-device-plugin)来启用Kubernetes集群中的MPS分区：

```py
helm install oci://ghcr.io/nebuly-ai/helm-charts/nvidia-device-plugin \
  --version 0.13.0 \
  --generate-name \
  -n nebuly-nvidia \
  --create-namespace
```

默认情况下，Helm图表会在所有标记为`nos.nebuly.com/gpu-partitioning=mps`的节点上启用MPS模式部署设备插件。要在特定节点的GPU上启用MPS分区，你只需将标签`nos.nebuly.com/gpu-partitioning=mps`应用于该节点。

你的集群上很可能已经安装了一个版本的NVIDIA设备插件。如果你不想删除它，可以选择将这个分叉插件与原始NVIDIA设备插件一起安装，并仅在特定节点上运行。为此，重要的是确保在同一节点上同时运行的两个插件中只有一个在运行。如[安装指南](https://github.com/nebuly-ai/k8s-device-plugin#installation)所述，可以通过编辑**原始**NVIDIA设备插件的规格，并在其`spec.template.spec`中添加一个反亲和性规则，以确保它不会在分叉插件所针对的相同节点上运行：

```py
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: nos.nebuly.com/gpu-partitioning
          operator: NotIn
          values:
          - mps
```

安装设备插件后，你可以通过编辑其配置中的`sharing.mps`部分来配置它以将GPU暴露为多个MPS资源。例如，下面的配置告诉插件将索引为`0`的GPU暴露给Kubernetes，作为两个GPU资源（名为`nvidia.com/gpu-4gb`），每个资源具有4GB的内存：

```py
version: v1
sharing:
  mps: 
    resources:
      - name: nvidia.com/gpu
        rename: nvidia.com/gpu-4gb
        memoryGB: 4
        replicas: 2
        devices: ["0"]
```

广告给Kubernetes的资源名称、分区大小和副本数量可以根据需要进行配置。回到上面给出的示例，容器可以请求4GB GPU内存的一部分，如下所示：

```py
apiVersion: v1
kind: Pod
metadata:
  name: mps-partitioning-example
spec:
  hostIPC: true # 
  securityContext:
    runAsUser: 1000 # 
  containers:
    - name: sleepy
      image: "busybox:latest"
      command: ["sleep", "120"]
      resources:
        limits:
          nvidia.com/gpu-4gb: 1 #
```

请注意，容器请求MPS资源的Pods存在一些限制：

1.  容器必须以与设备插件部署的MPS服务器相同的用户ID运行，默认情况下为1000。你可以通过编辑设备插件安装图表中的`mps.userID`值来更改它。

1.  Pod规范必须包含`hostIPC: true`。由于MPS要求客户端和服务器共享相同的内存空间，我们需要允许Pods访问主机节点的IPC命名空间，以便它能够与运行在主机上的MPS服务器进行通信。

在这个例子中，容器最多只能在共享GPU上分配2GB的内存。如果它尝试分配更多内存，将会因内存不足（OOM）错误而崩溃，但不会影响其他Pods。

然而，需要指出的是，`nvidia-smi`访问NVIDIA驱动程序时会绕过MPS客户端运行时。因此，在容器内运行`nvidia-smi`将显示其输出中的整个GPU资源：

![](../Images/e71cd8b16606e92a4a45eb526a387fe3.png)

# 动态MPS分区

总体而言，通过设备插件配置来管理MPS资源是复杂且耗时的。与其如此，不如直接创建请求MPS资源的Pods，并让其他人自动配置和管理它们。

动态 MPS 分区正是这样做的：它根据集群中工作负载的实时需求自动创建和删除 MPS 资源，确保始终将最佳共享配置应用于可用的 GPU。

要应用动态分区，我们需要使用 `[nos](https://github.com/nebuly-ai/nos)`，这是一个开源模块，用于在 Kubernetes 上高效地运行 GPU 工作负载。

我已经介绍了如何使用 `nos` 基于多实例 GPU（MIG）进行动态 GPU 分区。因此，在这里我们不会详细讨论，因为 `nos` 以相同的方式管理 MPS 分区。有关更多信息，你可以参考文章 [Kubernetes 中的动态 MIG 分区](https://medium.com/towards-data-science/dynamic-mig-partitioning-in-kubernetes-89db6cdde7a3)，或查看 `nos` 的 [文档](https://docs.nebuly.com/nos/overview/)。

MPS 和 MIG 动态分区之间唯一的区别在于用于告诉`nos`应该管理哪些节点的 GPU 分区的标签值。在 MPS 的情况下，你需要按照以下方式对节点进行标记：

```py
kubectl label nodes <node-names> "nos.nebuly.com/gpu-partitioning=mps"
```

# 结论

请求 GPU 切片的可能性对于提高 GPU 利用率和降低基础设施成本至关重要。

有三种方式可以实现：时间切片、多实例 GPU（MIG）和多进程服务器（MPS）。时间切片是共享 GPU 的最简单技术，但它缺乏内存隔离，并且引入的开销会降低工作负载性能。另一方面，MIG 提供了最高级别的隔离，但其支持的配置和“切片”大小的有限集合使其缺乏灵活性。

> MPS 是 MIG 和时间切片之间的有效折中方案。与 MIG 不同，它允许创建任意大小的 GPU 切片。与时间切片不同，它允许强制内存分配限制，并减少多个容器争夺共享 GPU 资源时可能出现的内存不足（OOM）错误。

目前，NVIDIA 设备插件不支持 MPS。不过，只需安装支持 MPS 的其他设备插件即可启用 MPS。

然而，MPS 静态配置不会自动调整以应对工作负载需求的变化，因此无法为每个 Pod 提供所需的 GPU 资源，特别是在工作负载在内存和计算方面要求多种切片且随时间变化的情况下。

> `nos` 通过动态 GPU 分区克服了 MPS 静态配置的局限性，这增加了 GPU 利用率，并减少了在集群节点上运行的设备插件实例上手动定义和应用 MPS 配置的操作负担。

总结来说，我们必须指出，有些情况下 MPS 的灵活性并非必要，而 MIG 提供的完全隔离则至关重要。然而，在这些情况下，仍然可以通过 `nos` 利用动态 GPU 分区，因为它支持两种分区模式。

# 资源

+   [动态 GPU 分区文档](https://docs.nebuly.ai/nos/dynamic-gpu-partitioning/)

+   [Nos 源代码](https://github.com/nebuly-ai/nos)

+   [NVIDIA Kubernetes 设备插件](https://github.com/NVIDIA/k8s-device-plugin)

+   [NVIDIA Kubernetes 设备插件（分支版）](https://github.com/nebuly-ai/k8s-device-plugin)

+   [NVIDIA GPU 操作员文档](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/getting-started.html)

# 贡献者

特别感谢 [Emile Courthoud](https://medium.com/u/2c53981c8a83) 对本文的审阅和贡献。
