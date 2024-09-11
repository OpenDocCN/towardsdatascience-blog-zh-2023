# 自定义您的基于云的机器学习训练环境 — 第一部分

> 原文：[`towardsdatascience.com/customizing-your-cloud-based-machine-learning-training-environment-part-1-2622e10ed65a?source=collection_archive---------7-----------------------#2023-05-21`](https://towardsdatascience.com/customizing-your-cloud-based-machine-learning-training-environment-part-1-2622e10ed65a?source=collection_archive---------7-----------------------#2023-05-21)

## 如何利用云的强大功能而不妨碍开发灵活性

[](https://chaimrand.medium.com/?source=post_page-----2622e10ed65a--------------------------------)![Chaim Rand](https://chaimrand.medium.com/?source=post_page-----2622e10ed65a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2622e10ed65a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2622e10ed65a--------------------------------) [Chaim Rand](https://chaimrand.medium.com/?source=post_page-----2622e10ed65a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9440b37e27fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcustomizing-your-cloud-based-machine-learning-training-environment-part-1-2622e10ed65a&user=Chaim+Rand&userId=9440b37e27fe&source=post_page-9440b37e27fe----2622e10ed65a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2622e10ed65a--------------------------------) ·8 分钟阅读·2023 年 5 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2622e10ed65a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcustomizing-your-cloud-based-machine-learning-training-environment-part-1-2622e10ed65a&user=Chaim+Rand&userId=9440b37e27fe&source=-----2622e10ed65a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2622e10ed65a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcustomizing-your-cloud-based-machine-learning-training-environment-part-1-2622e10ed65a&source=-----2622e10ed65a---------------------bookmark_footer-----------)![](img/c0807cb1ec61c0e065e0fc72f8147157.png)

图片由 [Jeremy Thomas](https://unsplash.com/@jeremythomasphoto?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

基于云的机器学习（ML）服务为 AI 开发者提供了许多便利，其中可能没有比**访问**更多样化、完全配置、完全功能和完全维护的 ML 训练环境更重要的。例如，托管训练服务，如 [Amazon SageMaker](https://aws.amazon.com/pm/sagemaker/?trk=ps_a134p000007BxdvAAC&trkCampaign=acq_paid_search_brand&sc_channel=PS&sc_campaign=acquisition_IL&sc_publisher=Google&sc_category=Machine+Learning&sc_country=IL&sc_geo=EMEA&sc_outcome=acq&sc_detail=amazon+sagemaker&sc_content=Sagemaker_e&sc_matchtype=e&sc_segment=532435490322&sc_medium=ACQ-P%7CPS-GO%7CBrand%7CDesktop%7CSU%7CMachine+Learning%7CSagemaker%7CIL%7CEN%7CText&s_kwcid=AL%214422%213%21532435490322%21e%21%21g%21%21amazon+sagemaker&ef_id=Cj0KCQiAhMOMBhDhARIsAPVml-HxIwfeABmnxXbZ9ia_5DV_TckDGpMSH2mFhSpu8jrCgntII8hcHB4aAuhfEALw_wcB%3AG%3As) 和 [Google Vertex AI](https://cloud.google.com/vertex-ai)，使用户能够指定（1）所需的实例类型（例如，配备最新可用 GPU 的实例），（2）ML 框架及版本，（3）代码源目录，以及（4）训练脚本，并会自动启动所选实例，运行脚本以训练 AI 模型，并在完成后拆除所有内容。这些服务的优点之一是可以显著节省构建和维护自己的训练集群的时间和成本。有关云端训练的更多好处和注意事项，请参见 这里。

然而，使用预定义、完全配置、完全验证的 ML 环境的便利性也伴随着开发灵活性的潜在限制。这与可以随心所欲自由定义的本地“本地”环境相对。以下是几个示例，展示了可能的限制：

1.  **训练依赖**：很容易想到那些对特定软件包（例如，Linux 包或 Python 包）有依赖的训练流程，而这些软件包可能不包括在您选择的云服务提供的预定义环境中。

1.  **开发平台独立性**：您可能希望（或需要）一个独立于底层运行平台的开发环境。例如，您可能希望无论是在自己的 PC 上、在本地（“本地”）集群中，还是在云中运行，都能够使用相同的训练环境，并且无论选择哪个云服务提供商和云服务。这可以减少适应多个环境的开销，并可能有助于调试问题。

1.  **个人发展偏好**：工程师（特别是有经验的工程师）对他们的开发习惯和开发环境可能非常挑剔。即使是由于开发流程的变更而可能带来的限制，无论其对团队的价值和重要性如何（例如，迁移到云端机器学习），也可能引发显著的抵触情绪。

在这篇分为两部分的博客文章中，我们将介绍一些克服基于云的训练潜在开发限制的选项。具体来说，我们将假设训练环境由一个包含 Python 环境的 [Docker](https://www.docker.com/) 镜像定义，并演示几种方法来定制环境以满足我们的特定需求。

为了演示的目的，我们将假设所选的训练服务是 Amazon SageMaker。然而，请注意，一般方法同样适用于其他训练服务，并且这种选择不应被视为对某一云服务的偏爱。最适合你的选项可能取决于许多因素，包括项目的细节、预算考虑等。

在博客中，我们将引用和演示某些库的 API 和行为。请注意，这些信息在撰写时是准确的，并可能会在未来版本的库中有所更改。在信任我们所写的内容之前，请务必参考最新的官方文档。

我想感谢 [Yitzhak Levi](https://www.linkedin.com/in/yitzhak-levi-49a217201/?originalSubdomain=il)，他的实验为这篇博客文章奠定了基础。

# 托管训练示例

让我们开始使用托管服务在云端进行训练的简单演示。在下面的代码块中，我们使用 [Amazon SageMaker](https://aws.amazon.com/pm/sagemaker/?trk=ps_a134p000007BxdvAAC&trkCampaign=acq_paid_search_brand&sc_channel=PS&sc_campaign=acquisition_IL&sc_publisher=Google&sc_category=Machine+Learning&sc_country=IL&sc_geo=EMEA&sc_outcome=acq&sc_detail=amazon+sagemaker&sc_content=Sagemaker_e&sc_matchtype=e&sc_segment=532435490322&sc_medium=ACQ-P%7CPS-GO%7CBrand%7CDesktop%7CSU%7CMachine+Learning%7CSagemaker%7CIL%7CEN%7CText&s_kwcid=AL%214422%213%21532435490322%21e%21%21g%21%21amazon+sagemaker&ef_id=Cj0KCQiAhMOMBhDhARIsAPVml-HxIwfeABmnxXbZ9ia_5DV_TckDGpMSH2mFhSpu8jrCgntII8hcHB4aAuhfEALw_wcB%3AG%3As) 来运行 *train.py* PyTorch (1.13.1) 脚本，该脚本位于 *source_dir* 文件夹中，并在单个 [*ml.g5.xlarge*](https://aws.amazon.com/ec2/instance-types/g5/) GPU 实例类型上运行。请注意，这是使用 Amazon SageMaker 进行训练的一个相对简单的示例；有关更多详细信息，请参阅官方 [AWS 文档](https://aws.amazon.com/getting-started/hands-on/machine-learning-tutorial-train-a-model/)。

```py
from sagemaker.pytorch import PyTorch

# define the training job
estimator = PyTorch(
    entry_point='train.py',
    source_dir='./source_dir',
    framework_version='1.13.1',
    role='<arn role>',
    py_version='py39',
    job_name='demo',
    instance_type='ml.g5.xlarge',
    instance_count=1
)

# deploy the job to the cloud
estimator.fit()
```

为了更好地了解在云中训练时开发灵活性的潜在限制以及如何克服这些限制，让我们回顾一下使用诸如 Amazon SageMaker 的服务部署训练作业时发生的步骤。

## 背后的秘密

我们将在这里重点关注主要步骤；有关详细信息，请参见 [官方文档](https://docs.aws.amazon.com/sagemaker/index.html)。尽管我们将回顾 SageMaker API 执行的操作，但其他托管训练 API 也表现出类似的行为。

**第 1 步**：将代码源目录打包并上传到云存储。

**第 2 步**：配置所请求的实例类型。

**第 3 步**：将适当的 [预构建 Docker 镜像](https://docs.aws.amazon.com/sagemaker/latest/dg/pre-built-containers-frameworks-deep-learning.html) 拉取到实例上。适当的镜像由训练作业的属性决定。在上述示例中，我们请求了 Python 3.9、PyTorch 1.13.1 和一个 GPU 实例类型在 *us-east-1* [AWS 区域](https://aws.amazon.com/about-aws/global-infrastructure/regions_az/)，因此最终会得到 763104351884.dkr.ecr.**us-east-1**.amazonaws.com/**pytorch**-training:**1.13.1**-**gpu**-**py39**-cu117-ubuntu20.04-sagemaker 镜像。

**第 4 步**：运行 Docker 镜像。Docker *ENTRYPOINT* 是一个脚本（由服务定义），该脚本从云存储中下载并解压训练代码，并运行用户定义的训练脚本（下面有更多细节）。

**第 5 步**：当训练脚本完成时，停止并释放实例。

请注意，我们大大简化了服务执行的操作，并关注了与我们的讨论相关的那些操作。在实际操作中，还有许多其他“管理”活动在后台进行，包括 [访问训练数据](https://docs.aws.amazon.com/sagemaker/latest/dg/model-access-training-data.html)、[协调分布式训练](https://docs.aws.amazon.com/sagemaker/latest/dg/distributed-training.html)、[从点中断中恢复](https://docs.aws.amazon.com/sagemaker/latest/dg/model-managed-spot-training.html)、[监控和分析训练](https://docs.aws.amazon.com/sagemaker/latest/dg/training-metrics.html) 等等。

## 关于 Docker 镜像

[AWS 深度学习容器](https://github.com/aws/deep-learning-containers)（DLCs）GitHub 仓库包括了由 SageMaker 服务使用的预构建 AWS Docker 镜像。这些镜像可以用来更好地理解训练脚本运行的环境。特别是，定义上述示例中使用的镜像的 Dockerfile 位于 [这里](https://github.com/aws/deep-learning-containers/blob/master/pytorch/training/docker/1.13/py3/cu117/Dockerfile.gpu)。从对该文件的粗略审查中，我们可以看到，除了标准的 Linux 和 Python 包（例如，OpenSSH、pandas、scikit-learn 等），该镜像还包含几个 AWS 特定的包，例如 [PyTorch for AWS](https://github.com/aws/deep-learning-containers/blob/master/pytorch/training/docker/1.13/py3/cu117/Dockerfile.gpu#L385) 的增强版本和用于利用 [Amazon EFA](https://aws.amazon.com/hpc/efa/) 的库。AWS 特定的增强功能包括用于管理和监控训练任务的特性，更重要的是，当在 AWS 的训练基础设施上运行时，用于 **最大化资源利用率和运行时性能** 的优化。此外，通过对 Dockerfile 的更详细分析可以明确，其创建需要细致的工作、偶尔的变通和广泛的测试。除其他考虑因素外（见下文），我们在 AWS 上训练时的首选始终是使用官方 AWS DLCs。

[*Dockerfile*中的*ENTRYPOINT*](https://github.com/aws/deep-learning-containers/blob/master/pytorch/training/docker/1.13/py3/cu117/Dockerfile.gpu#L561) 指向一个名为[*start_with_right_hostname.sh*](https://github.com/aws/sagemaker-pytorch-training-toolkit/blob/master/docker/build_artifacts/start_with_right_hostname.sh) 的脚本。这个脚本调用了来自[sagemaker-training](https://pypi.org/project/sagemaker-training/) Python 包的[*train.py*](https://github.com/aws/sagemaker-training-toolkit/blob/1f4691ab98967bd32e2860a6afc42001d0945ec9/src/sagemaker_training/cli/train.py) 脚本，该脚本又调用一个解析[*SAGEMAKER_TRAINING_MODULE*](https://github.com/aws/deep-learning-containers/blob/master/pytorch/training/docker/1.13/py3/cu117/Dockerfile.gpu#L370) 环境变量的[函数](https://github.com/aws/sagemaker-training-toolkit/blob/1f4691ab98967bd32e2860a6afc42001d0945ec9/src/sagemaker_training/trainer.py#L61)，最终运行来自[sagemaker-pytorch-training](https://pypi.org/project/sagemaker-pytorch-training/) Python 包的 PyTorch 特定的[入口点](https://github.com/aws/sagemaker-pytorch-training-toolkit/blob/master/src/sagemaker_pytorch_container/training.py#L152)。正是这个入口点下载源代码并启动训练，如上面第 4 步所述。请记住，我们刚刚描述的流程和我们链接的代码在撰写时是有效的，随着 SageMaker API 的演变，可能会发生变化。虽然我们已经分析了上述示例中配置的 SageMaker PyTorch 1.13 训练作业的启动流程，但同样的分析也可以应用于其他类型的基于云的训练作业。

公开访问的 AWS DLC Dockerfile 和 SageMaker Python 包源代码使我们能够了解基于云的训练运行时环境。这使我们能够更好地理解在云环境中训练的能力和局限性，并且正如我们在接下来的部分中将看到的，了解我们可以用来引入更改和自定义的工具。

# 自定义 Python 环境

自定义你的训练环境的第一种也是最简单的方法是添加 Python 包。训练脚本通常依赖于 Python 包（或特定版本的包），这些包不包含在云服务默认 Docker 镜像的 Python 环境中。SageMaker API 通过允许你在传递给 SageMaker 估算器的 *source_dir* 文件夹的根目录中包含一个 *requirements.txt* 文件来解决这个问题（请参见上面的 API 调用示例）。在下载和解压源代码（上面的步骤 4）之后，SageMaker 脚本将搜索 *requirements.txt* 文件，并使用 [pip](https://pypi.org/project/pip/) 包安装程序安装其所有内容。有关此功能的更多详细信息，请参见 [这里](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/using_pytorch.html#using-third-party-libraries)。其他云服务也包括类似的机制来自动安装包依赖。通过在你自己的训练脚本开始时简单地包含一个指定的包安装程序，也可以轻松实现这种解决方案（尽管在运行多个进程（例如，使用 MPI）在单个实例上时，需要仔细协调）。

虽然这个解决方案简单易用，但确实存在一些限制：

## 安装时间

需要考虑的一点是安装包依赖所需的开销（时间）。如果我们有一个很长的依赖列表，或者其中任何依赖项需要很长时间才能安装，以这种方式自定义环境可能会在不合理的程度上增加总体时间和成本，这时我们可能会发现我们将讨论的其他方法更适合我们的需求。

## 访问存储库

另一个需要注意的是，这种方法依赖于访问 Python 包存储库。如果你的训练环境的网络配置允许自由访问互联网，那么这不是问题。然而，如果你在私有网络环境中进行训练，例如 [Amazon Virtual Private Cloud](http://aws.amazon.com/vpc)（Amazon VPC），那么这种访问可能会受到限制。在这种情况下，你需要创建一个私有包存储库。在 AWS 中，可以使用 [AWS CodeArtifact](https://aws.amazon.com/codeartifact/) 或创建一个私有 PyPI 镜像来完成此操作。（虽然用例有所不同，但你可能会发现 [这里](https://aws.amazon.com/blogs/machine-learning/private-package-installation-in-amazon-sagemaker-running-in-internet-free-mode/) 和 [这里](https://aws.amazon.com/blogs/machine-learning/hosting-a-private-pypi-server-for-amazon-sagemaker-studio-notebooks-in-a-vpc/) 描述的方案很有帮助。）如果没有可访问的包存储库，你需要考虑我们将介绍的其他自定义选项。

## Conda 包依赖

我们提出的定制解决方案对于所有依赖项都是 pip 包的情况非常好。但是，如果我们的依赖项仅作为[conda](https://docs.conda.io/en/latest/)包存在呢？例如，假设我们希望使用[s5cmd](https://anaconda.org/conda-forge/s5cmd)来加速从云存储中传输数据。当我们完全控制训练环境时，可以选择使用 conda 构建 Python 环境并自由安装 conda 包依赖项。如果我们使用由云服务提供的 Docker 容器，我们无法控制 Python 环境的创建，可能无法享受相同的自由。事实上，尝试在 SageMaker 上通过[subprocess](https://docs.python.org/3/library/subprocess.html)安装[s5cmd](https://anaconda.org/conda-forge/s5cmd)（*conda install -y s5cmd*）会失败。即使我们找到一种方法使其工作，以这种方式安装包可能会导致不希望的副作用，例如覆盖其他 AWS 优化包或普遍破坏 conda 环境。

## Linux 软件包依赖关系

理论上，这种方法也可以扩展以支持 Linux 软件包。例如，如果我们的训练脚本依赖于特定的 Linux 软件包或软件包版本，我们可以在脚本开始时使用[subprocess](https://docs.python.org/3/library/subprocess.html)调用来安装它。实际上，包括 Amazon SageMaker 在内的许多服务限制了 Linux 用户权限，从而阻止了这种选项。

# 总结

我们提出的第一个用于定制训练环境的解决方案易于使用，并允许我们充分利用由云服务提供商特别设计和优化的 Docker 镜像。然而，正如我们所看到的，它也有一些限制。在[我们文章的第二部分](https://chaimrand.medium.com/customizing-your-cloud-based-machine-learning-training-environment-part-2-b65a6cf91812)中，我们将讨论两种额外的方法来解决这些限制。
