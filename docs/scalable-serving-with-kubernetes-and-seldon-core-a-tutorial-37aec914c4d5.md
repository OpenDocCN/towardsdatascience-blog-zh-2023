# 使用 Kubernetes 和 Seldon Core 进行可扩展服务：教程

> 原文：[https://towardsdatascience.com/scalable-serving-with-kubernetes-and-seldon-core-a-tutorial-37aec914c4d5?source=collection_archive---------20-----------------------#2023-01-09](https://towardsdatascience.com/scalable-serving-with-kubernetes-and-seldon-core-a-tutorial-37aec914c4d5?source=collection_archive---------20-----------------------#2023-01-09)

## 学习如何在 Kubernetes 集群中部署机器学习模型，并使用 HPA 和 KEDA 实现自动扩展

[](https://medium.com/@tintn03?source=post_page-----37aec914c4d5--------------------------------)[![Tin Nguyen](../Images/f5a69125e3d42be7906c8cd51f827854.png)](https://medium.com/@tintn03?source=post_page-----37aec914c4d5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----37aec914c4d5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----37aec914c4d5--------------------------------) [Tin Nguyen](https://medium.com/@tintn03?source=post_page-----37aec914c4d5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F78d51d946a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fscalable-serving-with-kubernetes-and-seldon-core-a-tutorial-37aec914c4d5&user=Tin+Nguyen&userId=78d51d946a3&source=post_page-78d51d946a3----37aec914c4d5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----37aec914c4d5--------------------------------) ·11 分钟阅读·2023年1月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F37aec914c4d5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fscalable-serving-with-kubernetes-and-seldon-core-a-tutorial-37aec914c4d5&user=Tin+Nguyen&userId=78d51d946a3&source=-----37aec914c4d5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F37aec914c4d5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fscalable-serving-with-kubernetes-and-seldon-core-a-tutorial-37aec914c4d5&source=-----37aec914c4d5---------------------bookmark_footer-----------)![](../Images/b9678f7f5d613266724cc19254ea38e4.png)

图片由 [Adam Kool](https://unsplash.com/@adamkool) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在大多数机器学习应用中，将训练好的模型部署到生产环境是一个关键阶段。在这一阶段，模型通过为客户或其他系统提供预测来展示其价值。

部署模型可以像实现一个 Flask 服务器然后导出其端点供用户调用一样简单。然而，构建一个能够稳健且可靠地处理大量请求且有严格响应时间或吞吐量要求的系统并不容易。

对于中型和大型企业，系统必须能够扩展以处理更重的工作负载，而无需显著改变代码库。也许公司正在扩展，需要一个可扩展的系统来处理不断增长的请求数量（这种特性叫做可扩展性）。业务需要系统能够适应流量波动（这种特性叫做弹性）。如果系统能够根据流量量进行自动扩展，这些特性是可以实现的。

在这个教程中，我们将学习如何使用 Seldon Core 在 Kubernetes 集群中部署 ML 模型。我们还将学习如何通过 HPA 和 KEDA 实现自动扩展。这个教程的代码可以在这个[仓库](https://github.com/tintn/ml-model-deployment-tutorials)中找到。

# 训练一个 PyTorch 模型

要完成部署过程，我们需要一个模型。我们使用来自官方 PyTorch 网站的这个[教程](https://pytorch.org/tutorials/beginner/blitz/cifar10_tutorial.html)中的模型。它是一个简单的图像分类模型，能够轻松运行于 CPU，因此我们可以在像你的笔记本电脑这样的本地机器上测试整个部署过程。

假设你在这个[仓库](https://github.com/tintn/ml-model-deployment-tutorials)的 `toy-model` 文件夹中。你可以使用以下命令在 CIFAR10 数据集上训练模型：

```py
python train.py
# Output: model.pth -> the trained weights of the model
```

Seldon Core 使用[Triton Inference Server](https://github.com/triton-inference-server/server)来服务 PyTorch 模型，因此我们需要将模型准备成可以用 Triton 服务的格式。首先，我们需要将模型导出为 TorchScript（也可以通过 Triton 的[python 后端](https://github.com/triton-inference-server/python_backend)服务 PyTorch 模型，但通常效率较低且部署更复杂）。

跟踪和脚本是将模型导出到 TorchScript 的两种方法。两者的选择仍然存在争议，这篇[文章](https://ppwwyyxx.com/blog/2022/TorchScript-Tracing-vs-Scripting/)探讨了这两种方法的优缺点。我们将使用跟踪方法来导出模型：

```py
python export_to_ts.py -c model.pth -o model.ts
# Output: model.ts 
# -> serialized model containing both trained weights and the model's architecture
```

Triton 从模型仓库加载模型。它必须包含服务器需要的服务模型的信息，如模型的输入/输出信息、使用的后端等。模型仓库必须遵循以下结构：

```py
<model-repository-path>/
    <model-name>/
      [config.pbtxt]
      [<output-labels-file> ...]
      <version>/
        <model-definition-file>
      <version>/
        <model-definition-file>
      ...
    <model-name>/
      [config.pbtxt]
      [<output-labels-file> ...]
      <version>/
        <model-definition-file>
      <version>/
        <model-definition-file>
      ...
    ...
```

在我们的例子中，我们只有一个模型。我们称这个模型为 `cifar10-pytorch`，我们的模型仓库应该具有以下结构：

```py
cifar10-model
└── cifar10-pytorch
    ├── 1
    │   └── model.ts
    └── config.pbtxt
```

`cifar10-model` 是仓库的名称，`cifar10-pytorch` 是模型名称，而 `model.ts` 是我们刚刚导出的 TorchScript 模型。`config.pdtxt` 定义了如何使用 Triton 服务模型：

```py
platform: "pytorch_libtorch"
default_model_filename: "model.ts"
max_batch_size: 0
input [
  {
    name: "image__0"
    data_type: TYPE_UINT8
    dims: [-1, 3, -1, -1]
  }
]
output [
  {
    name: "probs__0"
    data_type: TYPE_FP32
    dims: [-1, 10]
  }
]
```

你可以在[这里](https://github.com/tintn/ml-model-deployment-tutorials/tree/main/toy-model/cifar10-model)找到最终的代码库。Triton支持几种可能用于调整模型性能的特性。你还可以将多个步骤或多个模型组合成一个推理管道，以实现你的业务逻辑。然而，我故意保持模型配置简单，以展示模型部署的整个过程，而不是专注于性能。

如果你想查看如何使用Triton导出和服务PyTorch模型的更实际示例，可以查看这个[帖子](https://tintn.github.io/deploy-detectron2-with-triton/)。它展示了如何使用Triton服务来自Detectron2的MaskRCNN模型，这是一个用于实例分割的流行模型，并且在许多实际的计算机视觉系统中使用。

Triton可以访问本地文件系统或云存储服务（如S3、Google Storage或Azure Storage）中的模型。由于我们将要在Kubernetes中部署模型，使用云存储服务更为方便，因为Kubernetes集群中的所有节点都可以访问相同的模型。在本教程中，我们将使用AWS S3作为模型库。假设你已经有了AWS账户，让我们创建一个S3存储桶并上传我们准备好的文件夹：

```py
aws s3 cp --recursive cifar10-model s3://<YOUR_BUCKET>/cifar10-model
```

将`<YOUR_BUCKET>`替换为你的存储桶名称。我们现在已经在AWS S3上有了模型库，可以开始部署模型了。

# 使用Seldon Core部署模型

我们将使用Seldon Core将模型部署到Kubernetes集群中，Seldon Core是一个专注于ML模型部署和监控的框架。让我们创建一个本地Kubernetes集群，以便使用我们的本地计算机测试部署过程。

[Kind](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)可以用于创建本地集群。在撰写本文时，Seldon Core在k8s ≥ 1.25上有一个[问题](https://github.com/SeldonIO/seldon-core/issues/4339)，所以我们必须使用1.24或更旧版本。要使用Kind指定k8s版本，只需选择带有相应版本的镜像来启动集群。以下命令创建一个名为`kind-seldon`的本地集群，使用`k8s==1.24.7`：

```py
kind create cluster --name seldon --image kindest/node:v1.24.7
```

同时，请确保你在本地计算机上安装了`docker`、`kubectl`和`helm`。将`kubectl`的上下文切换到`kind-seldon`指示`kubectl`默认连接到新创建的集群：

```py
kubectl cluster-info --context kind-seldon
```

## 安装Seldon Core

我们将使用[Istio](https://istio.io/)作为集群的Ingress，Seldon Core作为服务平台。你可以在[这里](https://docs.seldon.io/projects/seldon-core/en/latest/install/kind.html)找到安装说明。安装了Istio和Seldon Core后，运行这些命令检查它们是否都已正确安装：

```py
kubectl get svc -n istio-system
# NAME                   TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)                                                                      AGE
# istio-egressgateway    ClusterIP      10.96.90.103   <none>        80/TCP,443/TCP                                                               3m29s
# istio-ingressgateway   LoadBalancer   10.96.229.8    <pending>     15021:30181/TCP,80:32431/TCP,443:30839/TCP,31400:32513/TCP,15443:32218/TCP   3m28s
# istiod                 ClusterIP      10.96.195.7    <none>        15010/TCP,15012/TCP,443/TCP,15014/TCP                                        8m48s
```

检查Istio网关是否正在运行：

```py
kubectl get gateway -n istio-system
# NAME             AGE
# seldon-gateway   5m17s
```

检查Seldon控制器是否正在运行：

```py
kubectl get pods -n seldon-system
# NAME                                        READY   STATUS    RESTARTS   AGE
# seldon-controller-manager-b74d66684-qndf6   1/1     Running   0          4m18Create an Istio gateway to manage the cluster’s traffic:
```

如果你还没做过，请确保启用了标签`istio-injection`：

```py
kubectl label namespace default istio-injection=enabled
```

Istio 网关在集群中的端口 80 上运行，我们需要将本地机器的端口转发到该端口，以便我们可以从外部访问：

```py
kubectl port-forward -n istio-system svc/istio-ingressgateway 8080:80
```

## 使用 Seldon Core 提供服务

如果你的模型库存储在私有存储桶中，你需要授予集群内部访问存储桶的权限。这可以通过创建一个秘密并在创建部署时引用它来完成。这是为 S3 存储桶创建秘密的模板：

```py
apiVersion: v1
kind: Secret
metadata:
  name: seldon-rclone-secret
type: Opaque
stringData:
  RCLONE_CONFIG_S3_TYPE: s3
  RCLONE_CONFIG_S3_PROVIDER: aws
  RCLONE_CONFIG_S3_ENV_AUTH: "false"
  RCLONE_CONFIG_S3_ACCESS_KEY_ID: "<AWS_ACCESS_KEY_ID>"
  RCLONE_CONFIG_S3_SECRET_ACCESS_KEY: "<AWS_SECRET_ACCESS_KEY>"
```

将 `AWS_ACCESS_KEY_ID` 和 `AWS_SECRET_ACCESS_KEY` 替换为你的实际 AWS 访问密钥 ID 和秘密访问密钥。创建秘密：

```py
kubectl apply -f secret.yaml
```

我们可以使用这个 [清单](https://github.com/tintn/ml-model-deployment-tutorials/blob/main/scalable-serving/cifar10-deploy.yaml) 部署模型，注意清单中创建的秘密通过 `envSecretRefName` 键进行引用。确保 `spec.predictors[].graph.name` 与你上传到模型库中的模型名称匹配。应用清单以创建部署：

```py
kubectl apply -f cifar10-deploy.yaml
```

如果这是你在该集群中的第一次部署，那么下载必要的 Docker 镜像将需要一些时间。检查模型是否成功部署：

```py
kubectl get deploy
# NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
# cifar10-default-0-cifar10-pytorch   1/1     1            1           41m
```

我创建了一个 [脚本](https://github.com/tintn/ml-model-deployment-tutorials/blob/main/testing/test.py)，使用 Locust 来测试已部署的模型。你需要首先安装脚本运行所需的 [依赖](https://github.com/tintn/ml-model-deployment-tutorials/blob/main/testing/requirements.txt)：

```py
pip install -r requirements.txt
```

给定`localhost:8080`已经被端口转发到集群的网关，运行以下命令以向部署了 Seldon 的模型发送请求：

```py
locust -f test.py --headless -u 100 -r 10 --run-time 180 -H http://localhost:8080
```

如果你的部署名称或模型名称不同，你可以在脚本中相应地调整部署的 URL。已部署模型的 URL 遵循 Seldon Core 的 [推理协议](https://docs.seldon.io/projects/seldon-core/en/latest/reference/apis/v2-protocol.html)：

```py
/seldon/{namespace}/{model_repo}/v2/models/{model_name}/versions/{model_version}/infer
```

我们已经使用 Seldon Core 部署了自定义模型，并通过向模型发送推理请求进行了测试。在下一部分中，我们将深入探讨如何扩展部署，以处理更多用户或更高的流量。

# 使用 HPA 进行 Pod 自动扩缩

说到可扩展性，Kubernetes 提供了 HPA（水平 Pod 自动扩缩）。当某些指标达到资源的阈值（例如 CPU 或内存）时，HPA 可以添加更多的 Pods 来处理更重的负载。

## 安装指标服务器

HPA 需要从聚合的 API 中获取指标，这通常通过一个 [Metrics Server](https://github.com/kubernetes-sigs/metrics-server) 提供。你可以使用以下命令为你的集群安装一个指标服务器：

```py
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

如果你的集群是本地的，你还需要通过将参数 `-kubelet-insecure-tls` 传递给服务器来禁用证书验证：

```py
kubectl patch -n kube-system deployment metrics-server --type=json \
  -p '[{"op":"add","path":"/spec/template/spec/containers/0/args/-","value":"--kubelet-insecure-tls"}]'
```

## 使用 HPA 部署模型

我们可以通过在部署清单中添加 `hpaSpec` 来启用 HPA 以适应相应的组件：

```py
hpaSpec:
  maxReplicas: 2
  metrics:
  - resource:
      name: cpu  # This can be either "cpu" or "memory"
      targetAverageUtilization: 50
    type: Resource
  minReplicas: 1
```

HPA 规范告诉部署当当前指标值（在本例中为 CPU 使用率）高于期望值的 50% 时进行扩展，并且部署可能具有的最大副本数为 2。

应用此[清单](https://github.com/tintn/ml-model-deployment-tutorials/blob/main/scalable-serving/cifar10-deploy-hpa.yaml)以创建一个具有 HPA 的部署，确保您将 `<YOUR_BUCKET>` 替换为您的存储桶名称，并且已经创建了访问存储桶的密钥（如前一节中所述）：

```py
kubectl apply -f cifar10-deploy-hpa.yaml
```

您可以通过以下命令查看当前指标值：

```py
kubectl get hpa
# NAME                                REFERENCE                                      TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
# cifar10-default-0-cifar10-pytorch   Deployment/cifar10-default-0-cifar10-pytorch   0%/50%    1         2         1          98m
```

让我们检查运行中的 Pod。您应该看到部署模型的正在运行的 Pod：

```py
kubectl get pods
# NAME                                                 READY   STATUS    RESTARTS   AGE
# cifar10-default-0-cifar10-pytorch-7744fdc4dd-vzx4x   3/3     Running   0          101m
```

现在，我们可以使用我们的[测试脚本](https://github.com/tintn/ml-model-deployment-tutorials/blob/main/testing/test.py)（在前一节中提到过）测试已部署的模型：

```py
locust -f test.py --headless -u 100 -r 10 --run-time 180 -H http://localhost:8080
```

通过 `kubectl get hpa -w` 监控当前指标值，一段时间后可以看到指标值超过阈值，HPA 将触发新 Pod 的创建：

```py
kubectl get pods
# NAME                                                 READY   STATUS    RESTARTS   AGE
# cifar10-default-0-cifar10-pytorch-7744fdc4dd-pgpxm   3/3     Running   0          10s
# cifar10-default-0-cifar10-pytorch-7744fdc4dd-vzx4x   3/3     Running   0          108m
```

如果当前指标值在一定时期内低于阈值（默认为 5 分钟），HPA 将缩减部署。可以使用参数 `--horizontal-pod-autoscaler-downscale-stabilization` 标志配置时期为 `kube-controller-manager`：

```py
kubectl get pods
# NAME                                                 READY   STATUS        RESTARTS   AGE
# cifar10-default-0-cifar10-pytorch-7744fdc4dd-pgpxm   3/3     Terminating   0          7m3s
# cifar10-default-0-cifar10-pytorch-7744fdc4dd-vzx4x   3/3     Running       0          114m
```

在本节中，我们已经学会了如何根据 CPU 使用率上下扩展 Pod 的数量。在下一节中，我们将使用 KEDA 根据自定义指标更灵活地扩展我们的部署。

# 使用 KEDA 进行 Pod 自动扩展

KEDA 可以从许多来源获取指标（称为扩展器），请参见支持的扩展器列表[这里](https://keda.sh/docs/2.9/scalers/)。我们将设置 KEDA 从 Prometheus 服务器获取指标，并监视指标以触发 Pod 的扩展。Prometheus 服务器收集集群中 Seldon 部署的指标。

## 安装 Seldon 监控和 KEDA

按照[说明](https://docs.seldon.io/projects/seldon-core/en/latest/analytics/analytics.html)安装 Seldon 的监控堆栈，其中包括 Prometheus 服务器。现在 `seldon-monitoring` 命名空间中应该存在以下 Pod：

```py
kubectl get pods -n seldon-monitoring
# NAME                                                    READY   STATUS    RESTARTS     AGE
# alertmanager-seldon-monitoring-alertmanager-0           2/2     Running   3 (8h ago)   26h
# prometheus-seldon-monitoring-prometheus-0               2/2     Running   2 (8h ago)   26h
# seldon-monitoring-blackbox-exporter-dbbcd845d-qszj8     1/1     Running   1 (8h ago)   26h
# seldon-monitoring-kube-state-metrics-7588b77796-nrd9g   1/1     Running   1 (8h ago)   26h
# seldon-monitoring-node-exporter-fmlh6                   1/1     Running   1 (8h ago)   26h
# seldon-monitoring-operator-6dc8898f89-fkwx8             1/1     Running   1 (8h ago)   26h
```

检查 Seldon Core 的 Pod 监视器是否已创建：

```py
kubectl get PodMonitor -n seldon-monitoring
# NAME                AGE
# seldon-podmonitor   26h
```

运行以下命令在 Seldon Core 中启用 KEDA：

```py
helm upgrade seldon-core seldon-core-operator \
    --repo https://storage.googleapis.com/seldon-charts \
    --set keda.enabled=true \
    --set usageMetrics.enabled=true \
    --set istio.enabled=true \
    --namespace seldon-system
```

将 KEDA 安装到集群中，并确保先前安装的 KEDA（如果有）已完全卸载：

```py
kubectl delete -f https://github.com/kedacore/keda/releases/download/v2.9.1/keda-2.9.1.yaml
kubectl apply -f https://github.com/kedacore/keda/releases/download/v2.9.1/keda-2.9.1.yaml
```

# 使用 KEDA 部署模型

我们已经设置好了一切。让我们使用 KEDA 创建一个 Seldon 部署。与 HPA 类似，在部署中启用 KEDA，我们只需要在部署清单中包含 `kedaSpec`。考虑以下规范：

```py
kedaSpec:
  pollingInterval: 15
  minReplicaCount: 1
  maxReplicaCount: 2
  triggers:
  - type: prometheus
    metadata:
      serverAddress: http://seldon-monitoring-prometheus.seldon-monitoring.svc.cluster.local:9090
      metricName: access_frequency
      threshold: '20'
      query: avg(rate(seldon_api_executor_client_requests_seconds_count{deployment_name=~"cifar10"}[1m]))
```

`serverAddress` 是集群中 Prometheus 服务器的地址，它应该是 Prometheus 服务的 URL，我们可以通过 `kubectl get svc -n seldon-monitoring` 来检查服务。当指标值超过 `threshold` 时，将触发缩放。`query` 是运行中副本的每秒平均请求数，这是我们要监控的指标。

应用这个 [清单](https://github.com/tintn/ml-model-deployment-tutorials/blob/main/scalable-serving/cifar10-deploy-keda.yaml) 来部署模型：

```py
kubectl apply -f cifar10-deploy-keda.yaml
```

通过向部署发送请求来触发自动缩放：

```py
locust -f test.py --headless -u 100 -r 10 --run-time 180 -H http://localhost:8080
```

几秒钟后，你可以看到一个新的 pod 被创建：

```py
kubectl get pods
# NAME                                                 READY   STATUS     RESTARTS      AGE
# cifar10-default-0-cifar10-pytorch-5dc484599c-2zrv8   3/3     Running    3 (18m ago)   35h
# cifar10-default-0-cifar10-pytorch-5dc484599c-ljk74   0/3     Init:0/2   0             9s
```

类似于 HPA，在一段低流量时间（默认5分钟）后，将触发缩放。

```py
kubectl get pods -w
# NAME                                                 READY   STATUS    RESTARTS      AGE
# cifar10-default-0-cifar10-pytorch-5dc484599c-2zrv8   3/3     Running   3 (22m ago)   35h
# cifar10-default-0-cifar10-pytorch-5dc484599c-ljk74   3/3     Running   0             3m55s
# cifar10-default-0-cifar10-pytorch-5dc484599c-ljk74   3/3     Terminating   0             5m49s
# cifar10-default-0-cifar10-pytorch-5dc484599c-ljk74   2/3     Terminating   0             6m
# cifar10-default-0-cifar10-pytorch-5dc484599c-ljk74   1/3     Terminating   0             6m1s
# cifar10-default-0-cifar10-pytorch-5dc484599c-ljk74   0/3     Terminating   0             6m3s
```

# 结论

我们已经学习了如何使用 Seldon Core 将机器学习模型部署到 Kubernetes 集群中。虽然我们主要关注于部署 PyTorch 模型，但本指南中展示的程序也可以用于部署其他框架的模型。

我们还使用 HPA 和 KEDA 使部署具有可扩展性。与 HPA 相比，KEDA 提供了更多基于 Prometheus 指标（或 KEDA 支持的其他扩展器）灵活的缩放方式。从技术上讲，我们可以实现从 Prometheus 服务器获取的任何指标的缩放规则。

*原文发表于* [*https://tintn.github.io*](https://tintn.github.io/Scalable-Serving-with-Kubernetes-and-Seldon-Core/) *2023年1月9日。*
