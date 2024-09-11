# 管理基于云的机器学习训练的简单解决方案

> 原文：[https://towardsdatascience.com/a-simple-solution-for-managing-cloud-based-ml-training-c80a69c6939a?source=collection_archive---------10-----------------------#2023-12-21](https://towardsdatascience.com/a-simple-solution-for-managing-cloud-based-ml-training-c80a69c6939a?source=collection_archive---------10-----------------------#2023-12-21)

## 如何使用基础（非托管）云服务 API 实现自定义训练解决方案

[](https://chaimrand.medium.com/?source=post_page-----c80a69c6939a--------------------------------)[![Chaim Rand](../Images/c52659c389f167ad5d6dc139940e7955.png)](https://chaimrand.medium.com/?source=post_page-----c80a69c6939a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c80a69c6939a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c80a69c6939a--------------------------------) [Chaim Rand](https://chaimrand.medium.com/?source=post_page-----c80a69c6939a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9440b37e27fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-simple-solution-for-managing-cloud-based-ml-training-c80a69c6939a&user=Chaim+Rand&userId=9440b37e27fe&source=post_page-9440b37e27fe----c80a69c6939a---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c80a69c6939a--------------------------------) · 18 分钟阅读 · 2023年12月21日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc80a69c6939a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-simple-solution-for-managing-cloud-based-ml-training-c80a69c6939a&source=-----c80a69c6939a---------------------bookmark_footer-----------)![](../Images/708f7fdb7efc84080b4ea5662f37e3dd.png)

`照片由 [Aditya Chinchure](https://unsplash.com/@adityachinchure?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)`

在之前的帖子中（例如，[这里](/6-steps-to-migrating-your-machine-learning-project-to-the-cloud-6d9b6e4f18e0)），我们已经扩展了在云中开发AI模型的好处。机器学习项目，特别是大型项目，通常需要**访问**专用设备（例如，训练加速器）、**按需扩展**的能力、用于维护大量数据的适当**基础设施**以及用于管理大规模实验的**工具**。像[Amazon Web Services (AWS)](https://aws.amazon.com/)、[Google Cloud Platform (GCP)](https://cloud.google.com/)和[Microsoft Azure](https://azure.microsoft.com/)这样的云服务提供商提供了大量针对AI开发的服务，从低级基础设施（例如，GPU和几乎无限的对象存储）到用于创建自定义ML模型的高度自动化工具（例如，[AWS AutoML](https://aws.amazon.com/machine-learning/automl/)）。特别是，管理培训服务（如[Amazon SageMaker](https://aws.amazon.com/sagemaker/)、[Google Vertex AI](https://cloud.google.com/vertex-ai)和[Microsoft Azure ML](https://azure.microsoft.com/en-us/products/machine-learning)）使得在云中进行培训变得特别容易，并提高了潜在ML工程师的可及性。要使用管理的ML服务，您只需指定所需的实例类型，选择ML框架和版本，并指向您的训练脚本，服务将自动启动所选实例并配置所需环境，运行脚本以训练AI模型，将结果保存到持久存储位置，并在完成后拆除一切。

虽然管理培训服务可能是许多ML开发人员的理想解决方案，但正如我们将看到的，有些情况下需要直接在“未管理”机器实例上运行并以非中介方式训练您的模型。在这些情况下，即在没有官方管理层的情况下，可能需要包含一个自定义解决方案来控制和监控您的培训实验。在这篇文章中，我们将提出一些步骤，用于使用低级未管理云服务的API构建一个非常简单的“穷人”解决方案来管理培训实验。

我们将首先说明在未管理机器上进行训练而不是通过管理服务进行训练的一些动机。接下来，我们将识别一些我们期望的基本训练管理功能。最后，我们将演示一种使用GCP API来[创建虚拟机实例](https://cloud.google.com/compute/docs/instances/create-start-instance)的简单管理系统的实现方法。

尽管我们将在 GCP 上演示我们的解决方案，但类似的解决方案也可以在其他云平台上开发。请不要将我们选择 GCP 或任何其他工具、框架或服务视为对其使用的推荐。云端训练有多种选择，每种都有其优缺点。对你来说，最佳选择将很大程度上取决于你项目的具体细节。请务必在阅读时对照最新版本的 API 和文档重新评估本文内容。

感谢我的同事 [Yitzhak Levi](https://www.linkedin.com/in/yitzhak-levi-49a217201/) 对本文的贡献。

# 动机 — 管理训练服务的局限性

高级解决方案通常会优先考虑易用性和更高的可访问性，虽然这意味着对底层流程的控制减少。基于云的管理训练服务也不例外。除了上述的便利性之外，还会有对训练启动和执行细节的一定控制丧失。这里我们将提到几个例子，希望能够代表你可能遇到的限制类型。

## 1\. 机器类型选择的限制

管理训练服务中提供的机器类型的种类并不总是涵盖云服务提供商支持的所有机器类型。例如，Amazon SageMaker 不支持其 [DL1](/training-on-aws-with-habana-gaudi-3126e183048) 和 [Inf2](/dl-training-on-aws-inferentia-53e103597a03) 系列的实例类型（截至本文撰写时）。由于各种原因（如节省成本），你可能需要或希望在这些实例类型上进行训练。在这种情况下，你将不得不放弃使用 Amazon SageMaker 进行训练的奢华。

## 2\. 对底层机器镜像的控制有限

管理训练工作负载通常在专用的 [Docker](https://www.docker.com/) 容器内运行。你可以依赖服务选择最合适的容器，从服务提供商预定义的容器列表中选择特定容器（例如 [AWS Deep Learning Containers](https://github.com/aws/deep-learning-containers)），或者定义适合你特定需求的自定义 Docker 镜像（例如见 [这里](/customizing-your-cloud-based-machine-learning-training-environment-part-2-b65a6cf91812)）。但是，虽然你对 Docker 镜像容器有相当大的控制权，但你对底层的 [机器镜像](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) *没有* 控制权。在大多数情况下，这不会成为问题，因为 Docker 旨在减少对主机系统的依赖。然而，在处理专用硬件（如训练时）的情况下，我们需要依赖主机镜像中的特定驱动程序安装。

## 3\. 对多节点部署控制的限制

通常为了提高训练速度，会在多个机器实例上训练大型模型。在这种情况下，机器之间的网络容量和延迟会对训练的速度（和成本）产生关键影响——因为它们之间会不断交换数据。理想情况下，我们希望这些实例位于同一数据中心。不幸的是，（截至本文撰写时），像 Amazon SageMaker 这样的托管服务限制了你对设备位置的控制。因此，你可能会发现机器分布在两个或多个[可用区](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html)，这可能会对你的训练时间产生负面影响。

## 4\. 用户权限限制

有时，训练工作负载可能需要对系统主机进行根级访问。例如，AWS 最近宣布了[Mountpoint for Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/mountpoint.html)，这是一种基于[FUSE](https://en.wikipedia.org/wiki/Filesystem_in_Userspace)的新解决方案，用于高吞吐量的数据存储访问，并可能优化数据流入你的训练循环。不幸的是，这个解决方案只能在[Docker 环境](https://github.com/awslabs/mountpoint-s3/blob/main/docker/README.md)中使用，如果你的容器运行时带有`*--cap-add SYS_ADMIN*`标志，这实际上阻止了在托管服务设置中使用它。

## 5\. Docker 启动设置的限制

一些训练工作负载需要能够配置特定的 *docker run* 设置。例如，如果你的工作负载将训练数据存储在共享内存中（例如，在 */dev/shm* 中），并且你的数据样本特别大（例如，高分辨率的 3D 点云），你可能需要指定增加分配给 Docker 容器的共享内存量。虽然 Docker 通过 *shm-size* 标志来实现这一点，但你的托管服务可能会阻止你控制这一点。

## 6\. 训练环境访问受限

托管训练的副作用之一是对训练环境的访问性降低。有时你需要直接连接到你的训练环境，例如，调试故障时。当运行托管训练任务时，你实际上将所有控制权交给了服务，包括你随意访问机器的能力。请注意，一些解决方案支持对托管环境的有限访问，前提是适当配置训练任务（例如，见[这里](https://github.com/aws-samples/sagemaker-ssh-helper)）。

## 7\. 缺乏 Spot 实例终止的通知

[Spot 实例](https://aws.amazon.com/ec2/spot/) 是 CSPs 通常以折扣价格提供的未使用机器。权衡是这些机器可能会被突然撤回。当使用未管理的 Spot 实例时，你会收到 [终止通知](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-instance-termination-notices.html)，这使你有一点时间优雅地停止应用程序。如果你正在运行培训工作负载，你可以利用这一提前通知捕获模型的最新状态，并将其复制到持久存储中，以便稍后恢复使用。

托管训练服务的一个引人注目的特性是它们 [管理 Spot 生命周期](https://docs.aws.amazon.com/sagemaker/latest/dg/model-managed-spot-training.html) ，即你可以选择在成本较低的 Spot 实例上进行训练，并依赖托管服务在可能的情况下自动恢复中断的作业。然而，当通过 Amazon SageMaker 等托管服务使用 Spot 实例时，你不会收到终止通知。因此，当你恢复训练作业时，将从你捕获的最新模型状态开始，而不是预占时模型的状态。根据你捕获中间检查点的频率，这种差异可能会影响你的训练成本。

## 成本考虑

需要注意的另一个托管训练服务的属性是通常与其使用相关的额外成本。AI 开发可能是一项昂贵的任务，你可能会选择通过放弃托管训练服务的便利，换取一个更简单的自定义解决方案，从而减少成本。

## Kubernetes 替代方案

容器编排系统有时被提出作为托管训练服务的替代方案。在撰写本文时，最受欢迎的选择是 [Kubernetes](https://kubernetes.io/)。凭借其高度的自动扩展性以及对将应用拆分为*微服务*的支持，Kubernetes 已成为许多现代应用开发人员的首选平台。这对于包含复杂流程及多个相互依赖组件的应用尤为如此，有时称为有向无环图**(**DAG)工作流。一些端到端的 ML 开发管道可以视为 DAG 工作流（例如，从数据准备和处理开始，到模型部署结束）。实际上，基于 Kubernetes 的解决方案通常应用于 ML 管道。然而，就仅培训阶段而言（如本文所述），我们通常知道实例的确切数量和类型，可以认为 [Kubernetes](https://kubernetes.io/) 提供的附加价值有限。Kubernetes 的主要缺点是它通常需要依赖专门的基础设施团队进行持续的支持和维护。

# 训练管理最小要求

在没有 CSP 训练管理服务的情况下，让我们定义一些我们希望的基本管理功能。

1.  **自动启动** — 我们希望训练脚本在实例启动后自动运行。

1.  **自动实例终止** — 我们希望机器实例在训练脚本完成后自动终止，以便我们不会为机器不使用时支付费用。

1.  **支持多实例训练** — 我们需要能够启动一个集群，以便进行多节点训练。

1.  **持久日志** — 我们希望训练日志输出写入持久存储，以便在训练机器终止后仍然可以随时访问。

1.  **检查点捕获** — 我们希望将训练成果，包括定期检查点，保存到持久存储中。

1.  **训练任务总结** — 我们希望有一种方法来审查（和比较）训练实验。

1.  **在抢占性终止时重启（高级功能）** — 我们希望能够利用折扣 [抢占实例](https://aws.amazon.com/ec2/spot/) 的容量，以降低训练成本而不影响开发的连续性。

在下一节中，我们将展示如何在 GCP 中实现这些功能。

# 穷人的 GCP 训练管理

在本节中，我们将展示一个基本的训练管理解决方案，使用 Google 的 [gcloud 命令行工具](https://cloud.google.com/sdk/gcloud/reference/compute/instances/create)（基于 Google Cloud SDK 版本 424.0.0）来 [创建 VM 实例](https://cloud.google.com/compute/docs/instances/create-start-instance)。我们将从一个简单的 VM 实例创建命令开始，并逐步补充额外的组件，以纳入我们所需的管理功能。请注意，gcloud [compute-instances-create](https://cloud.google.com/sdk/gcloud/reference/compute/instances/create) 命令包含一长串可选标志，这些标志控制实例创建的许多元素。为了演示的目的，我们将仅关注与我们的解决方案相关的控制。我们假设：1）您的环境已 [设置](https://cloud.google.com/sdk/gcloud/reference/auth) 以使用 gcloud 连接到 GCP，2）默认网络已适当配置，3）存在具有访问权限的 [托管服务账户](https://cloud.google.com/compute/docs/access/create-enable-service-accounts-for-instances)。

## 1\. 创建 VM 实例

以下命令将启动一个[g2-standard-48](https://gcloud-compute.com/g2-standard-48.html)虚拟机实例（包含四个[NVIDIA L4](https://www.nvidia.com/en-us/data-center/l4/) GPU），并使用公共[M112](https://cloud.google.com/deep-learning-vm/docs/release-notes#October_10_2023)虚拟机镜像。或者，您可以选择使用[自定义镜像](https://cloud.google.com/compute/docs/images/create-custom)。

```py
gcloud compute instances create test-vm \
    --zone=us-east1-d \
    --machine-type=g2-standard-48 \
    --image-family=common-cu121-debian-11-py310 \
    --image-project=deeplearning-platform-release \
    --service-account=my-account@my-project.iam.gserviceaccount.com \
    --maintenance-policy=TERMINATE
```

## 2\. 自动启动训练

一旦机器实例启动并运行，我们希望自动开始训练工作负载。在下面的示例中，我们将通过将[startup script](https://cloud.google.com/compute/docs/instances/startup-scripts/linux)传递给gcloud [compute-instances-create](https://cloud.google.com/sdk/gcloud/reference/compute/instances/create)命令来演示这一点。我们的启动脚本将执行一些基本的环境设置步骤，然后运行训练作业。我们首先调整*PATH*环境变量以指向我们的conda环境，然后从Google Storage下载包含源代码的tarball，解压它，安装项目依赖项，最后运行我们的训练脚本。演示假设tarball已经创建并上传到云中，并且它包含两个文件：一个要求文件（*requirements.txt*）和一个独立的训练脚本（*train.py*）。实际上，启动脚本的具体内容将取决于项目。

```py
gcloud compute instances create test-vm \
    --zone=us-east1-d \
    --machine-type=g2-standard-48 \
    --image-family=common-cu121-debian-11-py310 \
    --image-project=deeplearning-platform-release \
    --service-account=my-account@my-project.iam.gserviceaccount.com \
    --maintenance-policy=TERMINATE \
    --metadata=startup-script='#! /bin/bash \
    export PATH="/opt/conda/bin:$PATH" \
    gsutil cp gs://my-bucket/test-vm/my-code.tar . \
    tar -xvf my-code.tar \
    python3 -m pip install -r requirements.txt \
    python3 train.py'
```

## 3\. 完成后自毁

管理培训的一个引人注目的特点是您只需为所需的部分付费。更具体地说，一旦您的训练作业完成，训练实例将自动拆除。实现这一点的一种方法是在训练脚本的末尾附加一个自毁命令。请注意，为了启用自毁功能，实例需要使用适当的*scopes*设置进行创建。有关更多详细信息，请参阅[API文档](https://cloud.google.com/sdk/gcloud/reference/compute/instances/create)。

```py
gcloud compute instances create test-vm \
    --zone=us-east1-d \
    --machine-type=g2-standard-48 \
    --image-family=common-cu121-debian-11-py310 \
    --image-project=deeplearning-platform-release \
    --service-account=my-account@my-project.iam.gserviceaccount.com \
    --maintenance-policy=TERMINATE \
    --scopes=https://www.googleapis.com/auth/cloud-platform \
    --metadata=startup-script='#! /bin/bash \
    export PATH="/opt/conda/bin:$PATH" \
    gsutil cp gs://my-bucket/test-vm/my-code.tar . \
    tar -xvf my-code.tar \
    python3 -m pip install -r requirements.txt \
    python3 train.py \
    yes | gcloud compute instances delete $(hostname) --zone=zone=us-east1-d'
```

需要记住的是，尽管我们有意这样做，但有时实例可能不会被正确删除。这可能是由于特定错误导致启动脚本提前退出，或由于进程停滞而阻止其完成运行。我们强烈建议引入后端机制，以验证未使用的实例是否被识别并终止。实现这一点的一种方法是调度定期的[Cloud Function](https://cloud.google.com/functions?hl=en)。请参阅我们的[近期文章](https://medium.com/towards-data-science/using-server-less-functions-to-govern-and-monitor-cloud-based-training-experiments-755c43fba26b)，我们在其中提出了使用无服务器函数来解决在Amazon SageMaker上训练时出现的问题。

## 4\. 将应用程序日志写入持久存储

鉴于我们训练时的实例在完成后会被终止，我们需要确保系统输出写入持久日志。这对监控作业进度、调查错误等非常重要。在 GCP 中，这是通过 [Cloud Logging](https://cloud.google.com/logging?hl=en) 提供的。默认情况下，会为每个 VM 实例收集输出日志。可以使用与 VM 关联的*实例 ID*通过 [Logs Explorer](https://cloud.google.com/logging/docs/view/logs-explorer-interface) 访问这些日志。以下是访问训练作业日志的示例查询：

```py
resource.type="gce_instance"
resource.labels.instance_id="6019608135494011466"
```

为了能够查询日志，我们需要确保捕获并存储每个 VM 的*实例 ID*。这必须在实例终止之前完成。在下面的代码块中，我们使用 [compute-instances-describe](https://cloud.google.com/sdk/gcloud/reference/compute/instances/describe) API 来检索我们的 VM 的实例 ID。我们将 ID 写入文件，并将其上传到 Google Storage 中与我们项目关联的路径下的专用*元数据*文件夹中以供将来参考：

```py
gcloud compute instances describe test-vm \
    --zone=us-east1-d --format="value(id)" > test-vm-instance-id
gsutil cp test-vm-instance-id gs://my-bucket/test-vm/metadata/
```

我们进一步将*元数据*文件夹填充了我们 [compute-instances-create](https://cloud.google.com/sdk/gcloud/reference/compute/instances/create) 命令的全部内容。这将在后续过程中派上用场：

```py
gsutil cp create-instance.sh gs://my-bucket/test-vm/metadata/
```

## 5\. 将工件保存到持久存储中

重要的是，我们必须确保所有训练作业的工件在实例终止之前上传到持久存储中。最简单的方法是将一个命令附加到我们的启动脚本中，该命令将整个输出文件夹与 Google Storage 同步：

```py
gcloud compute instances create test-vm \
    --zone=us-east1-d \
    --machine-type=g2-standard-48 \
    --image-family=common-cu121-debian-11-py310 \
    --image-project=deeplearning-platform-release \
    --service-account=my-account@my-project.iam.gserviceaccount.com \
    --maintenance-policy=TERMINATE \
    --scopes=https://www.googleapis.com/auth/cloud-platform \
    --metadata=startup-script='#! /bin/bash \
    export PATH="/opt/conda/bin:$PATH" \
    gsutil cp gs://my-bucket/test-vm/my-code.tar . \
    tar -xvf my-code.tar \
    python3 -m pip install -r requirements.txt \
    python3 train.py \
    gsutil -m rsync -r output gs://my-bucket/test-vm/output \
    yes | gcloud compute instances delete $(hostname) --zone=zone=us-east1-d'
```

这个解决方案的问题在于它只会在训练结束时同步输出。如果由于某种错误我们的机器崩溃，我们可能会丢失所有中间工件。一个更具容错性的解决方案是，在整个训练作业过程中，将工件上传到 Google Storage（例如，在固定的训练步骤间隔）：

## 6\. 支持多节点训练

要运行多节点训练作业，我们可以使用 [compute-instances-bulk-create](https://cloud.google.com/sdk/gcloud/reference/compute/instances/bulk/create) API 来 [创建一组 GPU VM](https://cloud.google.com/compute/docs/gpus/create-gpu-vm-bulk)。下面的命令将创建两个 [g2-standard-48](https://gcloud-compute.com/g2-standard-48.html) VM 实例。

```py
gcloud compute instances bulk create \
    --name-pattern="test-vm-#" \
    --count=2 \
    --region=us-east1 \
    --target-distribution-shape=any_single_zone \
    --image-family=common-cu121-debian-11-py310 \
    --image-project=deeplearning-platform-release \
    --service-account=my-account@my-project.iam.gserviceaccount.com \
    --on-host-maintenance=TERMINATE \
    --scopes=https://www.googleapis.com/auth/cloud-platform \
    --metadata=startup-script='#! /bin/bash \
    export MASTER_ADDR=test-vm-1 \
    export MASTER_PORT=7777 \
    export NODES="test-vm-1 test-vm-2" \
    export PATH="/opt/conda/bin:$PATH" \
    gsutil cp gs://my-bucket/test-vm/my-code.tar . \
    tar -xvf my-code.tar \
    python3 -m pip install -r requirements.txt \
    python3 train.py \
    gsutil -m rsync -r output gs://my-bucket/test-vm/output \
    HN="$(hostname)" \
    ZN="$(gcloud compute instances list --filter=name=${HN} --format="value(zone)")" \
    yes | gcloud compute instances delete $HN --zone=${ZN}'

gcloud compute instances describe test-vm-1 \
    --zone=us-east1-d --format="value(id)" > test-vm-1-instance-id
gsutil cp test-vm-1-instance-id gs://my-bucket/test-vm/metadata/

gcloud compute instances describe test-vm-2 \
    --zone=us-east1-d --format="value(id)" > test-vm-2-instance-id
gsutil cp test-vm-2-instance-id gs://my-bucket/test-vm/metadata/
```

与单个 VM 创建命令相比，有一些重要的区别：

1.  对于批量创建，我们指定一个*区域*而不是*区域*。我们选择通过将 [*target-distribution-shape*](https://cloud.google.com/compute/docs/instance-groups/regional-mig-distribution-shape) 标志设置为 *any_single_zone* 来强制所有实例位于一个区域内。

1.  我们在启动脚本中添加了多个环境变量定义。这些将用于正确配置训练脚本以在所有节点上运行。

1.  要删除VM实例，我们需要知道它创建时的*zone*。由于在运行批量创建命令时我们并不知道这一点，我们需要在启动脚本中以编程方式提取它。

1.  我们现在捕获并存储*both*创建的VM实例的ID。

在下面的脚本中，我们演示如何使用环境设置在[PyTorch](https://pytorch.org/)中配置[data parallel training](https://pytorch.org/tutorials/intermediate/ddp_tutorial.html)。

```py
import os, ast, socket
import torch
import torch.distributed as dist
import torch.multiprocessing as mp

def mp_fn(local_rank, *args):
    # discover topology settings
    gpus_per_node = torch.cuda.device_count()
    nodes = os.environ['NODES'].split()
    num_nodes = len(nodes)
    world_size = num_nodes * gpus_per_node
    node_rank = nodes.index(socket.gethostname())
    global_rank = (node_rank * gpus_per_node) + local_rank
    print(f'local rank {local_rank} '
          f'global rank {global_rank} '
          f'world size {world_size}')
    dist.init_process_group(backend='nccl',
                            rank=global_rank, 
                            world_size=world_size)
    torch.cuda.set_device(local_rank)
    # Add training logic

if __name__ == '__main__':
    mp.spawn(mp_fn,
             args=(),
             nprocs=torch.cuda.device_count())
```

## 7. 实验总结报告

托管服务通常会暴露一个API和/或一个仪表盘，以查看训练作业的状态和结果。你可能会想在自定义管理解决方案中包含类似功能。实现此功能的方法有很多，包括使用[Google Cloud Database](https://cloud.google.com/products/databases?hl=en)来维护训练作业的元数据，或者使用第三方工具（如 [neptune.ai](https://neptune.ai/), [Comet](https://www.comet.com/site/), 或 [Weights & Biases](https://wandb.ai/site)）来记录和比较训练结果。以下函数假设训练应用程序的返回代码已被上传（来自启动脚本）到我们的专用*metadata*文件夹，并简单地遍历Google Storage中的作业：

```py
import os
import subprocess
from tabulate import tabulate
from google.cloud import storage

# get job list
p=subprocess.run(["gsutil","ls","gs://my-bucket/"], capture_output=True)
output = p.stdout.decode("utf-8").split("\n")
jobs = [i for i in output if i.endswith("/")]

storage_client = storage.Client()
bucket_name = "my-bucket"
bucket = storage_client.get_bucket(bucket_name)

entries = [] 
for job in jobs:
    blob = bucket.blob(f'{job}/metadata/{job}-instance-id')
    inst_id = blob.download_as_string().decode('utf-8').strip()
    try:
        blob = bucket.blob(f'{job}/metadata/status-code')
        status = blob.download_as_string().decode('utf-8').strip()
        status = 'SUCCESS' if status==0 else 'FAILED'
    except:
        status = 'RUNNING'
    print(inst_id)
    entries.append([job,status,inst_id,f'gs://my-bucket/{job}/my-code.tar'])

print(tabulate(entries,
            headers=['Job', 'Status', 'Instance/Log ID', 'Code Location']))
```

上述代码将生成以下格式的表格：

![](../Images/3753e9bf01d0e1dea9d613cacf4a58f1.png)

训练实验表（按作者分类）

## 8. 支持Spot实例使用

我们可以通过将[*provisioning-model*](https://cloud.google.com/compute/docs/instances/create-use-spot#create)标志设置为*SPOT*，以及将[*instance-termination-action*](https://cloud.google.com/compute/docs/instances/create-use-spot#create)标志设置为*DELETE*，轻松修改我们的单节点创建脚本以使用[Spot VM](https://cloud.google.com/compute/docs/instances/spot)。不过，我们希望我们的训练管理解决方案能够管理Spot VM使用的完整生命周期，即识别Spot抢占并在可能时重新启动未完成的作业。支持此功能的方法有很多。在下面的示例中，我们定义了一个专用的Google Storage路径 *gs://my-bucket/preempted-jobs/* ，该路径维护一个未完成训练作业名称的列表，并定义一个关闭脚本以识别Spot抢占并将当前作业名称添加到列表中。我们的关闭脚本是[文档](https://cloud.google.com/compute/docs/instances/create-use-spot#handle-preemption)中推荐脚本的精简版本。实际上，你可能希望包含捕获和上传最新模型状态的逻辑。

```py
#!/bin/bash

MY_PROGRAM="python"
# Find the newest copy of $MY_PROGRAM
PID="$(pgrep -n "$MY_PROGRAM")"
if [[ "$?" -ne 0 ]]; then
  echo "${MY_PROGRAM} not running shutting down immediately."
  exit 0
fi

echo "Termination in progress registering job..."
gsutil cp /dev/null gs://my-bucket/preempted-jobs/$(hostname)
echo "Registration complete shutting down."
```

我们将关闭脚本的内容复制到一个*shutdown.sh*文件中，并添加[*metadata-from-file*](https://cloud.google.com/compute/docs/shutdownscript#provide_a_shutdown_script_file)标志，指明脚本的位置。

```py
gcloud compute instances create test-vm \
    --zone=us-east1-d \
    --machine-type=g2-standard-48 \
    --image-family=common-cu121-debian-11-py310 \
    --image-project=deeplearning-platform-release \
    --service-account=my-account@my-project.iam.gserviceaccount.com \
    --maintenance-policy=TERMINATE \
    --scopes=https://www.googleapis.com/auth/cloud-platform \
    --provisioning-model=SPOT \
    --instance-termination-action=DELETE \
    --metadata-from-file=shutdown-script=shutdown.sh \
    --metadata=startup-script='#! /bin/bash \
    export PATH="/opt/conda/bin:$PATH" \
    gsutil cp gs://my-bucket/test-vm/my-code.tar . \
    tar -xvf my-code.tar \
    python3 -m pip install -r requirements.txt \
    python3 train.py \
    gsutil -m rsync -r output gs://my-bucket/test-vm/output \
    yes | gcloud compute instances delete $(hostname) --zone=zone=us-east1-d'
```

在 GCP 中使用关机脚本有[一些注意事项](https://cloud.google.com/compute/docs/shutdownscript#limitations)，你应该确保了解。特别是，具体行为可能会根据机器镜像和环境中的其他组件有所不同。在某些情况下，你可能会选择将关机行为直接编程到机器镜像中（例如，参见[这里](https://stackoverflow.com/questions/61110921/shutdown-script-not-executing-on-a-google-cloud-vm)）。

最终的解决方案组件需要一个[云函数](https://cloud.google.com/functions?hl=en)，它遍历*gs://my-bucket/preempted-jobs/*中的项目列表，并为每个项目检索相关的初始化命令（例如，*gs://my-bucket/test-vm/metadata/create-instance.sh*），并尝试重新运行。如果成功，它会从列表中移除作业名称。

```py
import os
import subprocess
from google.cloud import storage
storage_client = storage.Client()
bucket = storage_client.get_bucket('my-bucket')
jobs=bucket.list_blobs(prefix='users/preempted-jobs/')
for job in jobs:
    job_name=job.name.split('/')[-1]
    job_name='test-vm'
    script_path=f'{job_name}/metadata/create-instance.sh'
    blob=bucket.blob(script_path)
    blob.download_to_filename('script.sh')
    os.chmod('script.sh', 0o0777)
    p = subprocess.Popen('script.sh', stdout=subprocess.PIPE)
    p.wait()
    if(p.returncode==0):
        job.delete()
```

云函数可以编程定期运行和/或我们可以设计一个机制来[触发](https://cloud.google.com/functions/docs/calling)它从关机脚本。

**多节点 Spot 使用**：当将 Spot 使用应用于多节点作业时，我们需要解决可能只有一部分节点被终止的问题。处理这个问题最简单的方法是编程让应用程序识别当一些节点变得无响应时，停止训练循环，并继续关机。

# 受管理训练定制

构建自己的受管理训练解决方案的一个优势是可以根据特定需求进行定制。在上一节中，我们演示了如何设计一个自定义解决方案来管理 Spot VM 生命周期。我们还可以类似地使用[云监控警报](https://cloud.google.com/monitoring/alerts)、[云订阅/发布](https://cloud.google.com/pubsub/docs/overview)消息服务和无服务器[云函数](https://cloud.google.com/functions?hl=en)等工具，来量身定制其他挑战的解决方案，例如清理停滞作业、识别利用不足的 GPU 资源、限制 VM 的总体运行时间以及管理开发者使用模式。请参见我们的[近期帖子](https://chaimrand.medium.com/using-server-less-functions-to-govern-and-monitor-cloud-based-training-experiments-755c43fba26b)，我们演示了如何将无服务器函数应用于受管理训练环境中的这些挑战。

请注意，我们定义的管理解决方案解决了我们上述列出的受管理训练的每个限制：

1.  gcloud [compute-instances-create](https://cloud.google.com/sdk/gcloud/reference/compute/instances/create) API 公开了 GCP 提供的所有 VM 实例类型，并允许你指定你选择的机器镜像。

1.  gcloud [compute-instances-bulk-create](https://cloud.google.com/sdk/gcloud/reference/compute/instances/bulk/create) API 允许我们以确保所有节点在同一*区域*中共同部署的方式启动多个节点。

1.  我们的解决方案支持在非容器化环境中运行。如果你选择使用容器，你可以用任何设置和用户权限进行配置。

1.  GCP VMs 支持 SSH 访问（例如通过 gcloud [compute-ssh](https://cloud.google.com/compute/docs/connect/standard-ssh) 命令）。

1.  我们描述的 Spot VM 生命周期支持捕获和处理[抢占通知](https://cloud.google.com/compute/docs/instances/spot#preemption-process)。

# 说了这么多……

使用托管训练服务（例如[Amazon SageMaker](https://aws.amazon.com/pm/sagemaker/)、[Google Vertex AI](https://cloud.google.com/vertex-ai)和[Microsoft Azure ML](https://azure.microsoft.com/en-us/products/machine-learning/)）的便利性毋庸置疑。它们不仅处理了我们上面列出的所有托管需求，还通常提供额外功能，如[超参数优化](https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning.html)、[平台优化的分布式训练库](https://docs.aws.amazon.com/sagemaker/latest/dg/distributed-training.html)、[专用开发环境](https://docs.aws.amazon.com/sagemaker/latest/dg/machine-learning-environments.html)等。确实，有时开发自己的管理解决方案是有很好的理由的，但在走这条路之前，完全探索使用现有解决方案的所有机会可能是个好主意。

# 总结

尽管云计算可以为开发 AI 模型提供理想的领域，但合适的训练管理解决方案对于使其使用有效和高效至关重要。尽管 CSP 提供专门的托管训练服务，但它们并不总是与我们的项目要求对齐。在这篇文章中，我们展示了如何通过使用一些高级控制的低级、未管理的机器实例创建 API 设计一个简单的管理解决方案。自然，一刀切的方案*不适用于所有情况*；最理想的设计将高度依赖于你 AI 项目的具体细节。但我们希望我们的文章能为你提供一些开始的思路。
