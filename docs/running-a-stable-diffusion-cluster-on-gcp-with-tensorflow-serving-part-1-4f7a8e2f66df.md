# 在 GCP 上运行稳定扩散集群并使用 tensorflow-serving（第 1 部分）

> 原文：[https://towardsdatascience.com/running-a-stable-diffusion-cluster-on-gcp-with-tensorflow-serving-part-1-4f7a8e2f66df?source=collection_archive---------10-----------------------#2023-03-07](https://towardsdatascience.com/running-a-stable-diffusion-cluster-on-gcp-with-tensorflow-serving-part-1-4f7a8e2f66df?source=collection_archive---------10-----------------------#2023-03-07)

## 第 1 部分：使用 Terraform 设置基础设施

[](https://thushv89.medium.com/?source=post_page-----4f7a8e2f66df--------------------------------)[![Thushan Ganegedara](../Images/3fabfa37132f7d3a9e7679c3b8d7e061.png)](https://thushv89.medium.com/?source=post_page-----4f7a8e2f66df--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4f7a8e2f66df--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4f7a8e2f66df--------------------------------) [Thushan Ganegedara](https://thushv89.medium.com/?source=post_page-----4f7a8e2f66df--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6f0b045d5681&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frunning-a-stable-diffusion-cluster-on-gcp-with-tensorflow-serving-part-1-4f7a8e2f66df&user=Thushan+Ganegedara&userId=6f0b045d5681&source=post_page-6f0b045d5681----4f7a8e2f66df---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4f7a8e2f66df--------------------------------) ·11 分钟阅读·2023年3月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4f7a8e2f66df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frunning-a-stable-diffusion-cluster-on-gcp-with-tensorflow-serving-part-1-4f7a8e2f66df&user=Thushan+Ganegedara&userId=6f0b045d5681&source=-----4f7a8e2f66df---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4f7a8e2f66df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frunning-a-stable-diffusion-cluster-on-gcp-with-tensorflow-serving-part-1-4f7a8e2f66df&source=-----4f7a8e2f66df---------------------bookmark_footer-----------)![](../Images/113074e89bc6c210b08c2df7f0659860.png)

图片由 [Kier in Sight](https://unsplash.com/@kierinsight?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这个两部分教程的第一部分中，我们将学习在GCP上创建部署稳定扩散模型的Kubernetes集群。[Stable Diffusion](https://stability.ai/blog/stable-diffusion-public-release)（一种生成式AI）是新晋中的潮流。稳定扩散允许我们从给定的文本提示生成逼真的图像。由于稳定扩散模型带来的新颖性和计算负载，它提供了解决一些独特挑战的宝贵机会。

> **注意**：即使您是免费用户（只要您还有免费套餐余额），您也可以全程跟随本教程。
> 
> Github：[https://github.com/thushv89/tf-serving-gke/tree/master/infrastrcture](https://github.com/thushv89/tf-serving-gke/tree/master/infrastrcture)

但是，要创建完美的风暴（或完美的产品），仅有最新版本的模型权重不足以应对。需要努力构建一个可靠的生产系统，以支持用户请求并以合理的延迟可靠地提供服务。

![](../Images/029cc38eb4a6dca452cd71a4f95ebe6a.png)

我从部署模型中获取的一些图像示例。你能猜到提示是什么吗？（作者提供的图像）

为此，我们将学习如何在GKE集群上运行一个稳定扩散模型。这个2部分教程将包括4部分：

+   设置帐户和角色（第1部分）

+   设置集群（第1部分）

+   在配置的集群中部署预测服务（第2部分）

+   部署终端节点生成新图片（第2部分）

开始之前，请确保您已创建了一个GCP项目并通过`gcloud auth login`登录到您的用户帐户。您可以使用`gcloud config set project <project_id>`和`gcloud config set region <region>`来确保您在正确的项目和区域中。

> **注意**：我在这里讨论的大多数IAM（身份和访问管理）都基于我（有限）个人的经验。如果您发现任何可以改进的地方，请告诉我！

# terraform：时尚地管理基础架构

> 如果您已经熟悉`terraform`，请直接跳到“定义帐户和角色（IAM）”部分。

## 概述

对于GCP上所有基础架构的设置，我们将使用`terraform`；一种IaaS（基础设施即服务）工具，允许我们将所有基础架构需求编码化。为什么要通过代码管理云资源，而不是容易出错和痛苦的手动操作呢？还有许多其他原因：

+   人类可读的代码使架构更容易理解，提高了可重用性等。

+   `terraform`自动管理依赖关系并按正确顺序执行操作

+   版本控制代码使您能够在某个特定时间点获取系统状态的快照（用于故障排除）

`terraform` 提供了一个全面的开箱即用 API，可以快速构建所有常见提供者的基础设施，如 GCP、AWS、Azure 等。

## terraform 概念

`terraform` 术语将代码组织成配置。`terraform` 配置在一个工作目录中操作，该目录下的配置文件以 `.tf` 或 `.tf.json` 扩展名结尾；

+   `variables.tf` — 包含配置使用的所有变量定义

+   `outputs.tf` — 任何需要写出的输出

+   除此之外，你可以包含任意数量的 `.tf` 文件，包含资源定义、提供者等。在我们简单的场景中，我们只需要一个文件，称之为 `main.tf`。

接下来，让我们看看 `terraform` 如何实现代码的模块化。

> `terraform` 是一种声明性语言，这意味着你告诉 `terraform` 要做什么（像 SQL），而不是怎么做（像 Python）。由 `terraform` 来构建计划（例如图形形式）并执行。

然后我们可以使用 [模块](https://developer.hashicorp.com/terraform/language/modules) 来组成我们的 `terraform` 配置。模块化是可选的，但它将复杂的基础设施拆分为逻辑组件/子系统，并大大增强了可重用性。在我们的案例中，我们将定义三个模块；

1.  管理账户和角色 (`modules/iam`)

1.  管理 GKE 集群 (`modules/gke_cluster`)

1.  管理存储 — 设置 GCS 存储桶 (`modules/storage`)

当你进入代码中的这些模块时，你会看到以下基本构建块和谐地用于达到所需的基础设施状态（有关具体示例，请参见附录）。

+   资源块 — 描述基础设施对象（例如 VM、集群、VPC）

+   数据源 / 数据块 — 代表数据源（例如文件）及其相关数据

+   提供者插件 — 提供对某个提供者相关的资源类型和数据源的访问。

+   模块的输入和输出变量

一旦定义了配置，你可以运行 `terraform plan` 来查看 `terraform` 将执行什么。接下来可以使用 `terraform apply` 来应用这些更改。应用后，`terraform` 会在 `terraform.tfstate` 文件中记录所做的更改。因此，如果你想进行更改（或销毁），`terraform` 会了解基础设施的当前状态，从而为所需的更改创建计划。

如果你需要进一步巩固 `terraform` 概念，你可以阅读文档 [这里](https://developer.hashicorp.com/terraform/language) 或查看这个 [GCP 教程](https://developer.hashicorp.com/terraform/tutorials/gcp-get-started/google-cloud-platform-build)。现在我们了解了 `terraform` 的基础知识，接下来让我们理解逻辑。

# 定义账户和角色（IAM）

对于我们设置 GKE 集群的操作，我们将创建一个服务账号。顾名思义，服务账号通常由应用程序和工作负载使用，而不是实际的人。例如，GKE 节点可以使用服务账号来执行应用程序。服务账号可以被分配权限和角色（即以有意义的方式汇总的权限集合），就像用户账号一样。服务账号的几个优势包括，

+   我们可以快速绑定/移除用户与服务账号的绑定，允许我们为用户提供必要的权限，而无需重复分配角色/权限给各个用户。

+   服务账号可以通过 [设置短期凭据](https://cloud.google.com/iam/docs/create-short-lived-credentials-direct) 来提高安全性。

我们将设置两个具有以下 ID 的服务账号：

+   `gke-admin` — 具有创建 GKE 集群和配置节点所需的权限

+   `gke-node` — 具有成功执行工作负载所需的权限（例如，从 GCS 存储桶中读取）

虽然服务账号不直接由人使用或附加，但可以 [模拟服务账号](https://cloud.google.com/iam/docs/impersonating-service-accounts)，允许用户像服务账号一样执行命令。这是我们将用于设置集群的方法。

![](../Images/58ae52b348f33fcf618f46e76bfb5c81.png)

身份和资源的高层次视图（图像由作者提供）

这是我们将在 `terraform` 代码中概述的过程，

+   创建服务账号：`gke-admin` 和 `gke-node`

+   为创建的账号分配所需的角色

    — `gke-admin`：`container.admin`（例如创建集群）、`compute.viewer`（例如创建节点池）、`iam.serviceAccountUser`

    — `gke-node`：`container.nodeServiceAccount`（适用于典型的 Kubernetes 工作负载的权限）

    — 你可以在 **GCP 控制台 → IAM → 角色** 中查看每个角色提供了哪些权限。

+   分配所需的角色给用户账号以创建短期访问令牌（`iam.serviceAccountTokenCreator`）

+   从用户账号创建一个绑定到服务账号，以便用户可以模拟服务账号

最后，我们将在 `outputs.tf` 中声明我们创建的两个服务账号的名称作为输出，以便配置和其他子模块可以引用。

为了提供基础设施，我们将使用两种形式的身份验证，

+   通过运行 `gcloud auth login` 获取的典型身份验证将用于创建服务账号和绑定。

+   之后，我们将使用模拟服务账号来设置集群

> **注意 1**：我在用户账号上附加了 `owner` 角色（即项目所有者），如果你没有，你需要获得创建服务账号等所需的权限。
> 
> **注意 2**：即使你拥有所有者角色，进行所有这些服务账户创建和绑定可能看起来有些冗余，但是在一个项目中，与团队协作（或在组织中）时，你需要以最小权限用户的心态来思考和设置权限，以避免安全漏洞。

我们暂时不会运行`terraform apply`，因为我们将一次性创建服务账户和GKE集群。

# 定义GKE集群

我们将创建一个即使是免费用户也可以设置的GKE集群。一个集群由一个控制平面和一个或多个工作节点组成。控制平面提供对集群的访问，使你能够检查节点、Pods、服务等。每个节点可以运行一个或多个Pods（具有特定资源要求——例如CPU/内存）。一个Pod（可能运行一个或多个容器）将运行指定的工作负载（例如`tensorflow-serving`镜像以提供模型）。你可以参考[这里](https://cloud.google.com/kubernetes-engine/docs/concepts/cluster-architecture)了解GKE架构。

![](../Images/6d5016c1dd6ddb36ae71acc4e3c27009.png)

GKE集群的高级架构（作者提供的图片）

我们将以标准模式创建一个集群，配置如下：

+   `machine_type`：n2-standard-4

+   `max_node_count`（最大节点数）：2

+   `preemptible`：true（你也可以使用比`preemptible`实例更便宜的`spot`实例。了解这些差异请点击[这里](https://cloud.google.com/kubernetes-engine/docs/concepts/spot-vms#how-it-works)）

注意，我们没有使用GPU，因为作为免费用户，你可能没有GPU配额的资格。但是如果有，请随意按照[这里](https://cloud.google.com/kubernetes-engine/docs/how-to/gpus)的过程设置带有GPU的节点池。

> **注意 1**：如果你是免费用户，你将受到两个重要配额的限制：
> 
> `all_regions_cpus`：默认为`12`
> 
> `all_n2_cpus`：默认为`8`
> 
> `all_regions_gpus`：默认为`0`
> 
> 由于我们使用的是N2类型实例，每个实例具有4个vCPUs，因此在配额范围内我们只能启动2个这样的实例。如果你想要在集群中拥有更多节点，可以尝试其他实例类型，如`n2-standard-2`或`n1`实例。
> 
> **注意 2**：这些是全球配额，这意味着，例如，如果你有一个启动了其他`n2`类型实例的Vertex AI笔记本，它也会计入该配额。
> 
> 如果不遵守这些，你在应用这些基础设施时可能会遇到`Quota exceeded`类型的错误。

你可以在[这里](https://github.com/thushv89/tf-serving-gke/tree/master/infrastrcture/modules/gke_cluster)查看完整的配置。我在这里不会详细说明，因为它很简单。然而，我想提出一个警告，即区域集群和区域集群的概念。忽略这种区别可能会导致一些神秘的错误，比如[这个 Stackoverflow 问题](https://stackoverflow.com/questions/74836213/error-403-insufficient-regional-quota-to-satisfy-request-resource-ssd-total/75378781#75378781)。

# 在 GCP 上创建基础设施和资源

在应用讨论过的`terraform`更改之前，我们需要进行一些整理。首先，运行

```py
./setup.sh -u <user name> -p <project id> -r <region>
```

这将创建一个配置文件，其中包含定义的参数，以便将它们导入到`terraform`代码中。接下来，运行，

```py
terraform init 
```

这将安装提供的插件以及我们定义的本地模块。接下来，我们可以运行以下命令来了解`terraform`将要执行的操作。

```py
terraform plan [-var="include_module_storage=<true or false>"]
```

计划将是这样的。

```py
Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the
following symbols:
  + create
 <= read (data resources)

Terraform will perform the following actions:

  # data.google_service_account_access_token.default will be read during apply
  # (config refers to values not yet known)
 <= data "google_service_account_access_token" "default" {
      + access_token           = (sensitive value)
      + id                     = (known after apply)
      + scopes                 = [
          + "cloud-platform",
          + "userinfo-email",
        ]
      + target_service_account = (known after apply)
    }

  ...

  # module.iam.google_service_account_iam_binding.admin_account_iam will be created
  + resource "google_service_account_iam_binding" "admin_account_iam" {
      + etag               = (known after apply)
      + id                 = (known after apply)
      + members            = [
          + "user:thushv@gmail.com",
        ]
      + role               = "roles/iam.serviceAccountTokenCreator"
      + service_account_id = (known after apply)
    }

Plan: 9 to add, 0 to change, 0 to destroy.
```

如果我们对计划满意，我们可以运行以下命令来应用更改。

```py
terraform apply [-var="include_module_storage=<true or false>"]
```

如果一切成功，你应该会在工作目录中看到一个`terraform.tfstate`文件，列出所有应用的更改。访问[README](https://github.com/thushv89/tf-serving-gke/blob/master/infrastrcture/README.md)以获取详细说明。你可以前往**GCP 控制台 → IAM → 服务帐户**，确保服务帐户已正确创建。

![](../Images/48e969bb6de810d564a888ac282bf95e.png)

应用 terraform 转换后创建的服务帐户（图像由作者提供）

你还会在**GCP 控制台 → Kubernetes 引擎 → 集群**中看到一个名为`sd-cluster`的集群。

![](../Images/909cd447982e445a069657a3dd89a95d.png)

集群已经用单个节点初始化（图像由作者提供）

![](../Images/6026e8bb8d28c36040071647c3fb5ece.png)

一旦进入集群，你可以看到有关节点池和节点的更多信息（图像由作者提供）

很好，现在我们拥有了部署 ML 模型作为服务所需的一切。我们将在[教程的下一部分](/running-a-stable-diffusion-cluster-on-gcp-with-tensorflow-serving-part-2-c421ecb7472a)中查看如何完成这项工作。

到目前为止，你，

+   了解了什么是`terraform`以及它如何简化基础设施管理

+   创建了身份（服务帐户）并将其设置为正确的角色

+   理解了什么是 GKE 集群并通过模拟所需的服务帐户创建了一个

# 故障排除与注意事项

+   **错误**：`无法连接到服务器：x509: 证书已过期或尚未生效`

+   **解决方案 1**：这可能是由于`gcloud`会话过期。只需运行`gcloud auth login`并完成登录过程。

+   **解决方案 2**：WSL中存在[一个 bug](https://stackoverflow.com/questions/65086856/wsl2-clock-is-out-of-sync-with-windows)，其中 WSL 内的时钟与 Windows 时钟不同步。你可以运行`sudo hwclock -s`来触发同步。

**警告**：如果你在Powershell中使用bash（由[WSL](https://learn.microsoft.com/en-us/windows/wsl/install)支持），可能无法导出环境变量（供`terraform`使用）。因此，如果你依赖环境变量，建议不要使用它们。

# 附录

## 资源块

描述一个或多个基础设施对象（例如，虚拟机、集群、VPC）。每个资源通过*资源类型*和*唯一名称*来标识。

```py
resource "google_service_account" "sa_gke_admin" {
  account_id   = "gke-admin"
  display_name = "GKE Service Account (Admin)"
}
```

## 数据源 / 数据块

表示数据源及其关联的数据

```py
data "google_service_account_access_token" "default" {
 provider                = google.impersonation_helper
 target_service_account  = module.iam.service_account_gke_admin
 scopes                  = ["userinfo-email", "cloud-platform"]
 depends_on = [module.iam]
}
```

## 提供者插件

提供对某个提供者关联的资源类型和数据源的访问。

```py
terraform {
  required_providers {
    google = {
      source  = "hashicorp/google"
      version = "3.5.0"
    }
  }
}
```

## 输入和输出变量

作为模块的参数和返回类型。

```py
variable "gcp_user" {
  type = string
  description = "Your username for GCP"
}
```

```py
output "service_account_gke_node" {
  description = "GKE node service account"
  value       = google_service_account.sa_gke_node.email
}
```

```py
variable "gcp_user" {
  type = string
  description = "Your username for GCP"
}
```

```py
output "service_account_gke_node" {
  description = "GKE node service account"
  value       = google_service_account.sa_gke_node.email
}
```

# 致谢

我想感谢[ML开发者计划](https://developers.google.com/community/experts)以及团队提供的GCP积分，使这个教程取得了成功。
