# Kubernetes 的无限可扩展存储

> 原文：[https://towardsdatascience.com/infinitely-scalable-storage-for-kubernetes-9a20393e37e4?source=collection_archive---------3-----------------------#2023-05-07](https://towardsdatascience.com/infinitely-scalable-storage-for-kubernetes-9a20393e37e4?source=collection_archive---------3-----------------------#2023-05-07)

## 一项破坏性实验，确保我们的数据能够恢复

[](https://medium.com/@flavienb?source=post_page-----9a20393e37e4--------------------------------)[![Flavien Berwick](../Images/545083139e3cd14233a9a758b4ba762c.png)](https://medium.com/@flavienb?source=post_page-----9a20393e37e4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9a20393e37e4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9a20393e37e4--------------------------------) [Flavien Berwick](https://medium.com/@flavienb?source=post_page-----9a20393e37e4--------------------------------)

·

[跟进](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F615f38f3989a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finfinitely-scalable-storage-for-kubernetes-9a20393e37e4&user=Flavien+Berwick&userId=615f38f3989a&source=post_page-615f38f3989a----9a20393e37e4---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----9a20393e37e4--------------------------------) ·6 分钟阅读·2023 年 5 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9a20393e37e4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finfinitely-scalable-storage-for-kubernetes-9a20393e37e4&user=Flavien+Berwick&userId=615f38f3989a&source=-----9a20393e37e4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9a20393e37e4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finfinitely-scalable-storage-for-kubernetes-9a20393e37e4&source=-----9a20393e37e4---------------------bookmark_footer-----------)

有时候，你只需要一个可靠的存储解决方案。享有云服务提供商存储类的奢侈并非总是可能的，有时你必须自己全权管理。这是我在医疗保健领域的客户的挑战。

在本文中，您将了解为什么以及如何安装 Rook Ceph，为您的 Kubernetes 集群提供易于使用的复制存储类。

然后我们将部署一个文件共享应用程序，摧毁部署它的节点，然后看看会发生什么。Ceph 是否会让我们的文件再次可访问？

![](../Images/0adf23b4539f2fc1d127b4ebb0e5336f.png)

看向地平线的容器。[Kelly 摄于 Pexels](https://www.pexels.com/photo/rows-of-colorful-containers-in-industrial-area-6595781/)。

# 选择存储解决方案

存储在 Kubernetes 中一直是一个挑战，因为它没有原生提供冗余和分布式存储解决方案。在原生 Kubernetes 中，你只能附加一个 hostPath 卷以实现持久存储。

我的客户拥有自己的本地基础设施，并希望确保如果其中一台服务器出现故障，其数据不会丢失。大多数应用程序是单体应用，并且没有原生的数据复制机制。

所以我必须从[各种存储解决方案](https://vitobotta.com/2019/08/06/kubernetes-storage-openebs-rook-longhorn-storageos-robin-portworx/)中进行选择。我的客户不需要超高性能，但希望有一个稳定的解决方案。我选择了 Rook Ceph，原因如下：

+   这是一个 CNCF 毕业项目（[稳定性和质量的保证](https://github.com/cncf/toc/blob/main/process/graduation_criteria.md#graduation-stage)）

+   是开源的，具有出色的文档和[社区支持](https://github.com/rook/rook/issues?)

+   它易于部署和使用

+   性能[是公平的](https://vitobotta.com/2019/08/06/kubernetes-storage-openebs-rook-longhorn-storageos-robin-portworx/)（章节“基准测试”）

# 准备你的集群

我们需要一个最少包含 3 个节点的 Kubernetes 集群，每个节点有一个空的附加磁盘。

我推荐使用[Scaleway Kapsule](https://www.scaleway.com/en/kubernetes-kapsule)来轻松实例化 Kubernetes 集群并分配未格式化的磁盘。一旦 Kubernetes 集群启动，我们将为每个节点创建一个附加卷（磁盘）：

+   进入“实例”

+   选择你的节点

+   点击“附加卷”标签

+   点击“+”（创建卷），并创建一个新磁盘

下载你的*kubeconf*文件，并将其放置在`~/.kube/config`中。你现在应该可以使用你的*kubectl* CLI 访问集群。

# 安装 Rook Ceph

**1\.** 本博客文章有一个[GitHub 上的配套仓库](https://github.com/flavienbwk/ceph-kubernetes)，让我们克隆它以获取我们需要的所有资源

```py
git clone https://github.com/flavienbwk/ceph-kubernetes
cd ceph-kubernetes
```

**2\.** 克隆 Rook 仓库并部署 Rook Ceph 操作员

```py
git clone --single-branch --branch release-1.11 https://github.com/rook/rook.git
kubectl create -f ./rook/deploy/examples/crds.yaml
kubectl create -f ./rook/deploy/examples/common.yaml
kubectl create -f ./rook/deploy/examples/operator.yaml
```

**3\.** 创建 Ceph 集群

```py
kubectl create -f ./rook/deploy/examples/cluster.yaml -n rook-ceph
```

等待几分钟，直到 Ceph 配置完磁盘。健康状态应为 `HEALTH_OK`：

```py
kubectl get cephcluster -n rook-ceph
```

**4\.** 创建存储类

Rook Ceph 可以为你提供两个主要的存储类。一个是 RBD，允许你在 `ReadWriteOnce` 模式下拥有复制存储。第二个是我们将要安装的 CephFS，允许你在 `ReadWriteMany` 模式下拥有复制存储。RBD 代表 *RADOS Block Device*，允许你在 Kubernetes 集群中配置卷。它仅支持 `ReadWriteOnce` 卷（RWO）。CephFS 像一个复制的 NFS 服务器。这将使我们能够在 `ReadWriteMany` 模式下创建卷（RWX）。

```py
kubectl create -f ./rook/deploy/examples/csi/rbd/storageclass.yaml -n rook-ceph
kubectl create -f ./rook/deploy/examples/filesystem.yaml -n rook-ceph
kubectl create -f ./rook/deploy/examples/csi/cephfs/storageclass.yaml -n rook-ceph
```

**5\.** 部署 Ceph 控制面板

```py
kubectl create -f ./rook/deploy/examples/dashboard-external-https.yaml -n rook-ceph
```

转发控制面板的 HTTP 访问：

```py
kubectl port-forward service/rook-ceph-mgr-dashboard -n rook-ceph 8443:8443
```

使用用户名 `admin` 和以下密码进行连接：

```py
kubectl -n rook-ceph get secret rook-ceph-dashboard-password -o jsonpath="{['data']['password']}"
```

你应该访问以下页面：[*https://localhost:8443*](https://localhost:8443)

![](../Images/3fbc9a0b4427c3e2c71116133ce5d448.png)

作者提供的图像：Ceph 控制面板

# 部署应用

我们将部署一个自托管的文件共享应用（[psitransfer](https://github.com/psi-4ward/psitransfer)）来检查我们的卷是否正确绑定。

**1.** 部署文件共享应用（NodePort 30080）

```py
kubectl create -f ./psitransfer-deployment-rwx.yaml
```

**2.** 查看它被部署在哪个节点上

```py
kubectl get pods -o wide -l app=psitransfer
```

获取该节点的 IP（通过 Scaleway 界面）并检查应用是否在 *http://nodeip:30080* 上运行。

**3\.** 上传一些文件

从 [xcal1.vodafone.co.uk 网站](http://xcal1.vodafone.co.uk/) 下载 [5MB](http://212.183.159.230/5MB.zip)、[10MB](http://212.183.159.230/10MB.zip) 和 [20MB](http://212.183.159.230/20MB.zip) 文件。

将文件上传到我们的文件传输应用中。点击屏幕上出现的链接。

现在您应该能看到导入的树形文件。点击它并**保持浏览器标签中的链接**，我们稍后会用到它。

上传了大约 400MB 的文件后，我们可以看到数据在磁盘之间的复制是一致的。我们看到 3 个磁盘在文件上传时被同时写入。在以下截图中，每个磁盘的使用率为 1%：尽管我在同一主机上上传文件，但似乎复制按预期工作，数据在 3 个磁盘（OSD）之间均匀持久化。磁盘 2 有大量的“读取”活动，因为另外两个磁盘从它那里同步数据。

![](../Images/d5eac05176d2cea5167815e58df40a20.png)

Ceph 的仪表盘现在应该是这样的：

![](../Images/f85692aa66c5d10385c2ed4e7953ba15.png)

# C. 销毁并查看

我们将停止托管 Web 应用的节点，以确保数据已复制到其他节点。

1.  查看应用部署在哪个节点上

```py
kubectl get pods -o wide -l app=psitransfer
```

2\. 从 Scaleway 控制台关闭节点

这模拟了节点上的电力故障。它应该在几分钟后变为 `NotReady`：

```py
$> kubectl get node
NAME                                             STATUS     ROLES    AGE    VERSION
scw-ceph-test-clustr-default-5f02f221c3814b47a   Ready      <none>   3d1h   v1.26.2
scw-ceph-test-clustr-default-8929ba466e404a00a   Ready      <none>   3d1h   v1.26.2
scw-ceph-test-clustr-default-94ef39ea5b1f4b3e8   NotReady   <none>   3d1h   v1.26.2
```

而且 Node 3 在我们的 Ceph 仪表盘上不可用：

![](../Images/44c6bfae7fa77f59ce43d5a5e387e61b.png)

Ceph 的仪表盘现在应该是这样：

![](../Images/b6e839f15f8cadbd73b5a670420d3a84.png)

**3\.** 重新调度我们的 Pod

调度的 Pod 节点不可用。然而，我们的 Pod 仍然认为它是活动的：

```py
$> kubectl get pods -o wide -l app=psitransfer
NAME                                      READY   STATUS    RESTARTS   AGE   IP            NODE
psitransfer-deployment-8448887c9d-mt6wm   1/1     Running   0          19h   100.64.1.19   scw-ceph-test-clustr-default-94ef39ea5b1f4b3e8
```

删除它以便在另一个节点上重新调度：

```py
kubectl delete pod psitransfer-deployment-8448887c9d-mt6wm
```

检查刚刚重新启动的 Pod 的状态。您的应用程序应该可以通过之前保留的链接再次访问。

![](../Images/24657c632e2ca8c88632d90bd220c2fd.png)

为了避免在节点变为“NotReady”时需要手动删除 Pod 以便重新调度，建议将应用的副本数默认扩展到至少 3。

现在您可以重新启动之前关闭的节点。

# 什么时候使用 *rook-ceph-block* 或 *rook-cephfs*？

如果您的应用程序需要更好的性能并且需要 RWO 访问模式的块存储，请使用 *rook-ceph-block* (RBD) 存储类。另一方面，如果您的应用程序需要具有 RWX (CephFS) 访问模式和 POSIX 兼容的共享文件系统，请使用 *rook-cephfs* 存储类。

如果选择 RBD 并尝试在原节点离线时重新调度一个 pod，就像我们在 CephFS 中做的那样，你会收到来自 PVC 的错误，内容为："*卷已独占地附加到一个节点，无法附加到另一个节点*”。在这种情况下，你只需等待 PVC 重新绑定（我的集群自动重新分配 PVC 给我的 pod，花了大约 6 分钟，使其可以启动）。

尝试这种行为 [按照相关仓库章节](https://github.com/flavienbwk/ceph-kubernetes#when-to-use-rook-ceph-block-or-rook-cephfs-)。

# 最后一句

你已经学会了如何使用 Ceph 安装和部署应用程序。你甚至证明了它能够复制数据。恭喜你 ✨

*除非另有说明，否则所有图片均由作者提供。*
