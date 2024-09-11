# 使用 Ray 在 Amazon SageMaker 上实现有效负载均衡

> 原文：[https://towardsdatascience.com/effective-load-balancing-with-ray-on-amazon-sagemaker-d3b9020679d3?source=collection_archive---------8-----------------------#2023-09-04](https://towardsdatascience.com/effective-load-balancing-with-ray-on-amazon-sagemaker-d3b9020679d3?source=collection_archive---------8-----------------------#2023-09-04)

## 一种提高 DNN 训练效率和降低训练成本的方法

[](https://chaimrand.medium.com/?source=post_page-----d3b9020679d3--------------------------------)[![Chaim Rand](../Images/c52659c389f167ad5d6dc139940e7955.png)](https://chaimrand.medium.com/?source=post_page-----d3b9020679d3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d3b9020679d3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d3b9020679d3--------------------------------) [Chaim Rand](https://chaimrand.medium.com/?source=post_page-----d3b9020679d3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9440b37e27fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feffective-load-balancing-with-ray-on-amazon-sagemaker-d3b9020679d3&user=Chaim+Rand&userId=9440b37e27fe&source=post_page-9440b37e27fe----d3b9020679d3---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d3b9020679d3--------------------------------) ·10 分钟阅读·2023年9月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd3b9020679d3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feffective-load-balancing-with-ray-on-amazon-sagemaker-d3b9020679d3&user=Chaim+Rand&userId=9440b37e27fe&source=-----d3b9020679d3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd3b9020679d3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feffective-load-balancing-with-ray-on-amazon-sagemaker-d3b9020679d3&source=-----d3b9020679d3---------------------bookmark_footer-----------)![](../Images/825ef1fcbc6836470358bbe0e79c0b7f.png)

图片由 [Fineas Anton](https://unsplash.com/@fineas_anton?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

在之前的文章中（例如，[这里](/cloud-ml-performance-checklist-caa51e798002)），我们扩展了对DNN训练工作负载性能分析和优化的重要性。训练深度学习模型——尤其是大型模型——可能是一项昂贵的工作。**最大化训练资源的利用率**，以加速模型收敛并最小化训练成本，是决定您项目成功的关键因素。**性能优化**是一个迭代过程，其中我们识别并解决应用程序中的**性能瓶颈**，即阻碍我们提高资源利用率和/或加速运行时间的部分。

本文是关于训练深度学习模型时遇到的一个常见性能瓶颈系列文章中的第三篇，涉及到[**数据预处理瓶颈**](/overcoming-data-preprocessing-bottlenecks-with-tensorflow-data-service-nvidia-dali-and-other-d6321917f851)。数据预处理瓶颈发生在我们的GPU（或其他加速器）——通常是我们训练设置中*最昂贵*的资源——在等待来自*过度繁忙*的CPU资源的数据输入时处于闲置状态。

![](../Images/b1cc394697bc34133fcceda418841e87.png)

来自[TensorBoard分析器](https://www.tensorflow.org/tensorboard/tensorboard_profiling_keras)标签页的一张图像，展示了数据输入管道中的瓶颈的典型特征。我们可以清楚地看到在每第七个训练步骤中都有长时间的GPU空闲时间。（作者提供）

在我们[第一篇文章](/overcoming-ml-data-preprocessing-bottlenecks-with-grpc-ca30fdc01bee)中，我们讨论并演示了应对这种瓶颈的不同方法，包括：

1.  选择一个与您的工作负载更为匹配的CPU与GPU计算比率的训练实例，

1.  通过将一些CPU操作转移到GPU，改善CPU和GPU之间的工作负载平衡，并且

1.  将一些CPU计算卸载到辅助CPU工作者设备上。

我们使用了[TensorFlow Data Service API](https://www.tensorflow.org/api_docs/python/tf/data/experimental/service)演示了第三种选择，这是一种专门针对TensorFlow的解决方案，其中部分输入数据处理可以通过使用gRPC作为底层通信协议来卸载到其他设备上。

在我们的[第二篇文章](/overcoming-ml-data-preprocessing-bottlenecks-with-grpc-ca30fdc01bee)中，我们提出了一种基于gRPC的更通用的解决方案，用于利用辅助CPU工作者，并在一个玩具PyTorch模型上进行了演示。尽管它需要比[TensorFlow Data Service API](https://www.tensorflow.org/api_docs/python/tf/data/experimental/service)更多的手动编码和调整，但该解决方案提供了更大的鲁棒性，并在训练性能上实现了相同的优化。

## 使用Ray进行负载均衡

在这篇文章中，我们将展示一种额外的方法，利用辅助 CPU 工作线程，这种方法旨在将通用解决方案的鲁棒性与 TensorFlow 特定 API 的简单性和易用性相结合。我们将演示的方法将使用 [**Ray 数据集**](https://docs.ray.io/en/latest/data/dataset.html) 来自 [Ray 数据](https://docs.ray.io/en/latest/data/dataset.html) 库。通过利用 Ray 的 [资源管理](https://docs.ray.io/en/latest/ray-core/scheduling/resources.html) 和 [分布式调度](https://docs.ray.io/en/latest/ray-core/scheduling/index.html) 系统的全部功能，Ray 数据能够以 **可扩展** 和 **分布式** 的方式运行我们的训练数据输入管道。特别是，我们将配置 Ray 数据集，使得该库能够自动检测和利用所有可用的 CPU 资源进行训练数据的预处理。我们还将用 [Ray AIR Trainer](https://docs.ray.io/en/latest/ray-air/trainer.html#air-trainers) 封装我们的模型训练循环，以便能够无缝扩展到多 GPU 设置。

## 在 Amazon SageMaker 上部署 Ray 集群

在多节点环境中使用 Ray 框架及其提供的工具的前提是部署 [Ray 集群](https://docs.ray.io/en/latest/cluster/getting-started.html)。一般来说，设计、部署、管理和维护这样的计算集群可能是一项艰巨的任务，并且通常需要专门的 DevOps 工程师（或工程师团队）。这可能对某些开发团队构成无法克服的障碍。在这篇文章中，我们将演示如何使用 AWS 的托管训练服务 Amazon SageMaker 来克服这一障碍。特别是，我们将创建一个包含 GPU 实例和 CPU 实例的 [SageMaker 异构集群](https://docs.aws.amazon.com/sagemaker/latest/dg/train-heterogeneous-cluster.html)，并在启动时使用它来部署 Ray 集群。然后，我们将在此 Ray 集群上运行 Ray AIR 训练应用程序，同时依赖 Ray 的后端在集群中的所有资源之间进行有效的负载均衡。当训练应用程序完成时，Ray 集群将自动拆除。以这种方式使用 SageMaker，使我们能够在没有与集群管理相关的额外开销的情况下部署和使用 Ray 集群。

Ray 是一个强大的框架，能够支持广泛的机器学习工作负载。在这篇文章中，我们将演示其一些功能和 API，使用 Ray 版本 2.6.1。本文不应作为 [Ray 文档](https://docs.ray.io/en/latest/) 的替代品。请务必查阅官方 [文档](https://docs.ray.io/en/latest/) 以获取有关 Ray 工具的最合适和最新的使用信息。

在我们开始之前，特别感谢[Boruch Chalk](https://www.linkedin.com/in/boruch-chalk-56b28097/?originalSubdomain=il)引导我了解 Ray Data 库及其独特的功能。

# 玩具示例

为了方便讨论，我们将定义并训练一个基于 PyTorch (2.0) 的[Vision Transformer](https://en.wikipedia.org/wiki/Vision_transformer)分类模型，该模型将在一个由随机图像和标签组成的合成数据集上进行训练。[Ray AIR 文档](https://docs.ray.io/en/latest/ray-air/getting-started.html)包括各种[示例](https://docs.ray.io/en/latest/ray-air/examples/index.html)，演示如何使用 Ray AIR 构建不同类型的训练工作负载。我们在这里创建的脚本大致遵循了[PyTorch 图像分类器示例](https://docs.ray.io/en/latest/ray-air/examples/torch_image_example.html#)中描述的步骤。

## 定义 Ray 数据集和预处理器

[Ray AIR Trainer API](https://docs.ray.io/en/latest/train/api/doc/ray.train.trainer.BaseTrainer.html#ray.train.trainer.BaseTrainer) 区分了原始数据集和在将数据集元素送入训练循环之前应用的预处理管道。对于我们的原始 Ray 数据集，我们创建一个大小为*num_records*的简单[整数范围](https://docs.ray.io/en/latest/data/api/doc/ray.data.range.html)。接下来，我们定义我们希望应用于数据集的[预处理器](https://docs.ray.io/en/latest/ray-air/preprocessors.html)。我们的 Ray 预处理器包含两个组件：第一个是[BatchMapper](https://docs.ray.io/en/latest/ray-air/api/doc/ray.data.preprocessors.BatchMapper.html)，它将原始整数映射到随机的图像-标签对。第二个是[TorchVisionPreprocessor](https://docs.ray.io/en/latest/ray-air/api/doc/ray.data.preprocessors.TorchVisionPreprocessor.html)，它对我们的随机批次执行[torchvision transform](https://pytorch.org/vision/0.9/transforms.html)，将其转换为 PyTorch 张量，并应用一系列[GaussianBlur](https://pytorch.org/vision/main/generated/torchvision.transforms.GaussianBlur.html)操作。这些[GaussianBlur](https://pytorch.org/vision/main/generated/torchvision.transforms.GaussianBlur.html)操作旨在模拟相对较重的数据预处理管道。这两个预处理器通过[Chain](https://docs.ray.io/en/latest/ray-air/api/doc/ray.data.preprocessors.Chain.html) 预处理器进行组合。Ray 数据集和预处理器的创建在下面的代码块中演示：

```py
import ray
from typing import Dict, Tuple
import numpy as np
import torchvision.transforms as transforms
from ray.data.preprocessors import Chain, BatchMapper, TorchVisionPreprocessor

def get_ds(batch_size, num_records):
    # create a raw Ray tabular dataset
    ds = ray.data.range(num_records)

    # map an integer to a random image-label pair
    def synthetic_ds(batch: Tuple[int]) -> Dict[str, np.ndarray]:
        labels = batch['id']
        batch_size = len(labels)
        images = np.random.randn(batch_size, 224, 224, 3).astype(np.float32)
        labels = np.array([label % 1000 for label in labels]).astype(
                                                               dtype=np.int64)
        return {"image": images, "label": labels}

    # the first step of the prepocessor maps batches of ints to
    # random image-label pairs
    synthetic_data = BatchMapper(synthetic_ds, 
                                 batch_size=batch_size, 
                                 batch_format="numpy")

    # we define a torchvision transform that converts the numpy pairs to 
    # tensors and then applies a series of gaussian blurs to simulate
    # heavy preprocessing   
    transform = transforms.Compose(
        [transforms.ToTensor()] + [transforms.GaussianBlur(11)]*10
    )

    # the second step of the prepocessor appplies the torchvision tranform
    vision_preprocessor = TorchVisionPreprocessor(columns=["image"], 
                                                  transform=transform)

    # combine the preprocessing steps
    preprocessor = Chain(synthetic_data, vision_preprocessor)
    return ds, preprocessor
```

请注意，Ray 数据管道将自动使用 Ray 集群中所有可用的 CPU。这包括 GPU 实例上的 CPU 资源以及集群中任何额外辅助实例的 CPU 资源。

## 定义训练循环

下一步是定义训练序列，该序列将在每个训练节点（例如，GPU）上运行。首先，我们使用流行的 [timm](https://pypi.org/project/timm/)（0.6.13 版本）Python 包定义模型，并使用 [train.torch.prepare_model](https://docs.ray.io/en/latest/train/api/doc/ray.train.torch.prepare_model.html#ray.train.torch.prepare_model) API 进行包装。接下来，我们从数据集中提取适当的分片，并定义一个迭代器，该迭代器以请求的批量大小生成数据批次，并将其复制到训练设备。然后是训练循环本身，其中包含标准的 PyTorch 代码。当我们退出循环时，我们报告结果损失指标。每个训练节点的训练序列在以下代码块中展示：

```py
import time
from ray import train
from ray.air import session
import torch.nn as nn
import torch.optim as optim
from timm.models.vision_transformer import VisionTransformer

# build a ViT model using timm
def build_model():
    return VisionTransformer()

# define the training loop per worker
def train_loop_per_worker(config):
    # wrap the PyTorch model with a Ray object
    model = train.torch.prepare_model(build_model())
    criterion = nn.CrossEntropyLoss()
    optimizer = optim.SGD(model.parameters(), lr=0.001, momentum=0.9)

    # get the appropriate dataset shard
    train_dataset_shard = session.get_dataset_shard("train")

    # create an iterator that returns batches from the dataset
    train_dataset_batches = train_dataset_shard.iter_torch_batches(
        batch_size=config["batch_size"],
        prefetch_batches=config["prefetch_batches"],
        device=train.torch.get_device()
    )

    t0 = time.perf_counter()

    for i, batch in enumerate(train_dataset_batches):
        # get the inputs and labels
        inputs, labels = batch["image"], batch["label"]

        # zero the parameter gradients
        optimizer.zero_grad()

        # forward + backward + optimize
        outputs = model(inputs)
        loss = criterion(outputs, labels)
        loss.backward()
        optimizer.step()

        # print statistics
        if i % 100 == 99:  # print every 100 mini-batches
            avg_time = (time.perf_counter()-t0)/100
            print(f"Iteration {i+1}: avg time per step {avg_time:.3f}")
            t0 = time.perf_counter()

    metrics = dict(running_loss=loss.item())
    session.report(metrics)
```

## 定义 Ray Torch Trainer

一旦我们定义了数据流水线和训练循环，我们可以继续设置 [Ray TorchTrainer](https://docs.ray.io/en/latest/train/api/doc/ray.train.torch.TorchTrainer.html)。我们根据集群中可用的资源配置 Trainer。具体来说，我们根据 GPU 的数量设置训练节点数，并根据目标 GPU 上可用的内存设置批量大小。我们构建我们的数据集，以确保训练精确进行 1000 步所需的记录数。

```py
from ray.train.torch import TorchTrainer
from ray.air.config import ScalingConfig

def train_model():
    # we will configure the number of workers, the size of our
    # dataset, and the size of the data storage according to the
    # available resources 
    num_gpus = int(ray.available_resources().get("GPU", 0))

    # set the number of training workers according to the number of GPUs
    num_workers = num_gpus if num_gpus > 0 else 1

    # we set the batch size based on the GPU memory capacity of the
    # Amazon EC2 g5 instance family
    batch_size = 64

    # create a synthetic dataset with enough data to train for 1000 steps
    num_records = batch_size * 1000 * num_workers
    ds, preprocessor = get_ds(batch_size, num_records)

    ds = preprocessor(ds) 
    trainer = TorchTrainer(
        train_loop_per_worker=train_loop_per_worker,
        train_loop_config={"batch_size": batch_size},
        datasets={"train": ds},
        scaling_config=ScalingConfig(num_workers=num_workers, 
                                     use_gpu=num_gpus > 0),
    )
    trainer.fit()
```

## 部署 Ray 集群并运行训练序列

现在，我们定义训练脚本的入口点。在这里，我们设置 Ray 集群并在主节点上启动训练序列。我们使用 [sagemaker-training](https://pypi.org/project/sagemaker-training/) 库中的 [Environment](https://github.com/aws/sagemaker-training-toolkit/blob/master/README.md#get-information-about-the-container-environment) 类来发现异构 SageMaker 集群中的实例，如 [此教程](https://docs.aws.amazon.com/sagemaker/latest/dg/train-heterogeneous-cluster.html#train-heterogeneous-cluster-modify-training-script) 中所述。我们将 GPU 实例组的第一个节点定义为 Ray 集群的 *head* 节点，并在所有其他节点上运行适当的命令，将它们连接到集群中（有关创建集群的更多详细信息，请参阅 [Ray 文档](https://docs.ray.io/en/latest/cluster/getting-started.html)）。我们编程主节点等待所有节点连接，然后开始训练序列。这确保了 Ray 在定义和分发底层任务时利用所有可用资源。

```py
import time
import subprocess
from sagemaker_training import environment

if __name__ == "__main__":
    # use the Environment() class to auto-discover the SageMaker cluster
    env = environment.Environment()
    if env.current_instance_group == 'gpu' and \
             env.current_instance_group_hosts.index(env.current_host) == 0:
        # the head node starts a ray cluster
        p = subprocess.Popen('ray start --head --port=6379',
                             shell=True).wait()
        ray.init()

        # calculate the total number of nodes in the cluster
        groups = env.instance_groups_dict.values()
        cluster_size = sum(len(v['hosts']) for v in list(groups))

        # wait until all SageMaker nodes have connected to the Ray cluster
        connected_nodes = 1
        while connected_nodes < cluster_size:
            time.sleep(1)
            resources = ray.available_resources().keys()
            connected_nodes = sum(1 for s in list(resources) if 'node' in s)

        # call the training sequence
        train_model()

        # tear down the ray cluster
        p = subprocess.Popen("ray down", shell=True).wait()
    else:
        # worker nodes attach to the head node
        head = env.instance_groups_dict['gpu']['hosts'][0]
        p = subprocess.Popen(
            f"ray start --address='{head}:6379'",
            shell=True).wait()

        # utility for checking if the cluster is still alive
        def is_alive():
            from subprocess import Popen
            p = Popen('ray status', shell=True)
            p.communicate()[0]
            return p.returncode

        # keep node alive until the process on head node completes
        while is_alive() == 0:
            time.sleep(10)
```

## 在 Amazon SageMaker 异构集群上进行训练

在完成我们的训练脚本后，我们现在需要将其部署到 Amazon SageMaker 异构集群。为此，我们遵循 [本教程](https://docs.aws.amazon.com/sagemaker/latest/dg/train-heterogeneous-cluster.html) 中描述的步骤。我们首先创建一个 *source_dir* 目录，将我们的 *train.py* 脚本和一个包含脚本所依赖的两个 pip 包的 *requirements.txt* 文件放入其中，分别是 [timm](https://pypi.org/project/timm/) 和 [ray[air]](https://pypi.org/project/ray/)。这些包会自动安装在 SageMaker 集群的每个节点上。我们定义了两个 SageMaker [实例组](https://sagemaker.readthedocs.io/en/stable/api/utility/instance_group.html)，第一个实例组包含一个 [ml.g5.xlarge](https://aws.amazon.com/ec2/instance-types/g5/) 实例（包含 1 个 GPU 和 4 个 vCPUs），第二个实例组包含一个 [ml.c5.4xlarge](https://aws.amazon.com/ec2/instance-types/c5/) 实例（包含 16 个 vCPUs）。然后我们使用 [SageMaker PyTorch 估算器](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/sagemaker.pytorch.html) 定义并将训练作业部署到云端。

```py
from sagemaker.pytorch import PyTorch
from sagemaker.instance_group import InstanceGroup
cpu_group = InstanceGroup("cpu", "ml.c5.4xlarge", 1)
gpu_group = InstanceGroup("gpu", "ml.g5.xlarge", 1)

estimator = PyTorch(
    entry_point='train.py',
    source_dir='./source_dir',
    framework_version='2.0.0',
    role='<arn role>',
    py_version='py310',
    job_name='hetero-cluster',
    instance_groups=[gpu_group, cpu_group]
)

estimator.fit()
```

# 结果

在下表中，我们比较了在两种不同设置下运行我们的训练脚本的运行时结果：一个单独的 ml.g5.xlarge GPU 实例和一个包含 ml.g5.xlarge 实例以及 ml.c5.4xlarge 实例的异构集群。我们使用 [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) 评估系统资源利用率，并根据编写本文时的 [Amazon SageMaker 定价](https://aws.amazon.com/sagemaker/pricing/?p=pm&c=sm&z=4) 估算训练成本（ml.c5.4xlarge 实例每小时 $0.816，ml.g5.xlarge 实例每小时 $1.408）。

![](../Images/3e024e2da81b7e609eb6fcc2d2112427.png)

性能比较结果（作者提供）

单个实例实验中相对较高的 CPU 利用率与较低的 GPU 利用率表明数据预处理管道存在性能瓶颈。这些问题在迁移到异构集群时得到了明显解决。GPU 利用率和训练速度都得到了提升。总体而言，训练的价格效率提高了 23%。

我们需要强调的是，这些实验纯粹是为了演示 Ray 生态系统启用的自动负载均衡功能而创建的。调整控制参数可能会导致性能的改善。选择不同的解决方案来解决 CPU 瓶颈问题（例如选择具有更多 CPU 的 [EC2 g5](https://aws.amazon.com/ec2/instance-types/g5/) 系列实例）也可能会带来更好的成本效益。

# 总结

在这篇文章中，我们展示了如何利用 Ray 数据集在集群中所有可用的 CPU 工作节点之间平衡繁重的数据预处理管道的负载。这使我们能够通过简单地向训练环境中添加辅助 CPU 实例来轻松解决 CPU 瓶颈问题。亚马逊 SageMaker 的异构集群支持是一种在云中运行 Ray 训练任务的有力方式，因为它处理了集群管理的所有方面，避免了对专门 DevOps 支持的需求。

请记住，这里展示的解决方案只是应对 CPU 瓶颈的众多不同方法之一。最适合您的解决方案将高度依赖于您项目的具体细节。

像往常一样，请随时提出意见、纠正和问题。
