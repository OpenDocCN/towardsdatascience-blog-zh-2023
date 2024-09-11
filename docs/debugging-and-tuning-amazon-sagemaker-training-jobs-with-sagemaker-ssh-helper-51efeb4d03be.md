# 调试和调整 Amazon SageMaker 训练任务与 SageMaker SSH 帮助工具

> 原文：[https://towardsdatascience.com/debugging-and-tuning-amazon-sagemaker-training-jobs-with-sagemaker-ssh-helper-51efeb4d03be?source=collection_archive---------6-----------------------#2023-12-27](https://towardsdatascience.com/debugging-and-tuning-amazon-sagemaker-training-jobs-with-sagemaker-ssh-helper-51efeb4d03be?source=collection_archive---------6-----------------------#2023-12-27)

## 一个新工具，提升了托管训练工作负载的调试能力

[](https://chaimrand.medium.com/?source=post_page-----51efeb4d03be--------------------------------)[![Chaim Rand](../Images/c52659c389f167ad5d6dc139940e7955.png)](https://chaimrand.medium.com/?source=post_page-----51efeb4d03be--------------------------------)[](https://towardsdatascience.com/?source=post_page-----51efeb4d03be--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----51efeb4d03be--------------------------------) [Chaim Rand](https://chaimrand.medium.com/?source=post_page-----51efeb4d03be--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9440b37e27fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdebugging-and-tuning-amazon-sagemaker-training-jobs-with-sagemaker-ssh-helper-51efeb4d03be&user=Chaim+Rand&userId=9440b37e27fe&source=post_page-9440b37e27fe----51efeb4d03be---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----51efeb4d03be--------------------------------) ·10分钟阅读·2023年12月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F51efeb4d03be&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdebugging-and-tuning-amazon-sagemaker-training-jobs-with-sagemaker-ssh-helper-51efeb4d03be&user=Chaim+Rand&userId=9440b37e27fe&source=-----51efeb4d03be---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F51efeb4d03be&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdebugging-and-tuning-amazon-sagemaker-training-jobs-with-sagemaker-ssh-helper-51efeb4d03be&source=-----51efeb4d03be---------------------bookmark_footer-----------)![](../Images/480849dada98f2550913adfd745d3ba9.png)

照片由 [James Wainscoat](https://unsplash.com/@tumbao1949?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

考虑到过去一年（2023）宣布的所有新 Amazon SageMaker 功能，包括最近的 [AWS re:invent](https://press.aboutamazon.com/2023/11/aws-announces-five-new-amazon-sagemaker-capabilities-for-scaling-with-models)，很容易忽视 [SageMaker SSH Helper](https://github.com/aws-samples/sagemaker-ssh-helper) — 一种用于连接到远程 SageMaker 培训环境的新工具。但 **有时正是这些安静的增强功能有潜力对你的日常开发产生最大的影响。** 在这篇文章中，我们将回顾 [SageMaker SSH Helper](https://github.com/aws-samples/sagemaker-ssh-helper) 并展示它如何提高你 1) 调查和解决培训应用程序中出现的错误的能力，以及 2) 优化其运行时性能。

在 [之前的帖子](/6-steps-to-migrating-your-machine-learning-project-to-the-cloud-6d9b6e4f18e0) 中，我们详细讨论了云端培训的好处。基于云的管理培训服务，如 [Amazon SageMaker](https://aws.amazon.com/sagemaker/)，简化了围绕 AI 模型开发的许多复杂问题，并大大提高了对 AI 特定机器和 [预训练 AI 模型](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-jumpstart.html) 的可访问性。要在 Amazon SageMaker 中进行培训，你只需定义一个培训环境（包括实例类型）并指向你希望运行的代码，培训服务将 1) 设置请求的环境，2) 将你的代码传送到培训机器，3) 运行你的培训脚本，4) 将培训输出复制到持久存储中，5) 在培训完成后拆除一切（以便你只需为你需要的部分付费）。听起来很简单…对吧？然而，管理培训也并非没有缺陷，其中之一 — 它对培训环境的有限访问性 — 将在本文中讨论。

## 免责声明

1.  请不要将我们对 Amazon SageMaker、[SageMaker SSH Helper](https://github.com/aws-samples/sagemaker-ssh-helper) 或任何其他框架或工具的使用解读为对它们使用的支持。开发 AI 模型有许多不同的方法。对你而言，最佳解决方案将取决于你的项目细节。

1.  请务必在阅读时核实本文内容，特别是代码示例，与当时可用的最新软件和文档。AI 开发工具的环境在不断变化，我们提到的一些 API 可能会随着时间而变化。

# 管理培训的缺点 — 培训环境的不可访问性

正如经验丰富的开发者所知道的，应用开发时间的很大一部分实际上花在了调试上。我们的程序很少会“开箱即用”；更多时候，它们需要经过数小时的繁琐调试才能按预期运行。当然，要有效调试，你需要对应用环境有直接访问权限。尝试在没有环境访问权限的情况下调试应用程序，就像尝试修理水龙头却没有扳手一样。

AI模型开发中的另一个重要步骤是调整训练应用的运行时性能。训练AI模型可能成本高昂，我们最大化计算资源利用的能力可能对训练成本产生决定性影响。在[之前的一篇文章](/cloud-ml-performance-checklist-caa51e798002)中，我们描述了分析和优化训练性能的迭代过程。类似于调试，直接访问运行时环境将大大提高并加速我们获得最佳结果的能力。

不幸的是，SageMaker训练的“开火即忘”特性带来了一个副作用，那就是无法自由连接到训练环境。当然，你可以通过训练作业输出日志和调试打印（即，添加打印、研究输出日志、修改代码，并重复直到解决所有错误并达到所需性能）来调试和优化性能，但这将是一个非常原始且耗时的解决方案。

有一些最佳实践可以解决管理训练工作负载时的调试问题，每种方法都有其优缺点。我们将回顾其中的三种，讨论它们的局限性，然后展示新的[SageMaker SSH Helper](https://github.com/aws-samples/sagemaker-ssh-helper)如何彻底改变游戏规则。

## 在本地环境中调试

建议在将任务提交到云端之前，先在本地环境中运行几步训练。尽管这可能需要对代码进行一些修改（例如，为了在CPU设备上进行训练），但通常是值得的，因为它使你能够识别并修复一些简单的编码错误。显然，这比在云端昂贵的GPU机器上发现这些错误更具成本效益。理想情况下，你的本地环境应尽可能类似于SageMaker训练环境（例如，使用相同版本的Python和Python包），但在大多数情况下，这种相似性是有限的。

## 在SageMaker Docker容器中本地调试

第二个选项是拉取 SageMaker 使用的 [深度学习容器 (DLC) 镜像](https://docs.aws.amazon.com/deep-learning-containers/latest/devguide/deep-learning-containers-images.html)，并在你的本地 PC 上的容器中运行你的训练脚本。这种方法可以让你很好地了解 SageMaker 训练环境，包括已安装的包（和包版本）。它在识别缺失的依赖项和解决依赖项冲突方面极为有用。请参见 [文档](https://github.com/aws/deep-learning-containers/blob/master/available_images.md) 以了解如何登录并拉取适当的镜像。请注意，SageMaker 的 API 支持通过其 [本地模式](https://sagemaker.readthedocs.io/en/stable/overview.html#local-mode) 功能拉取和训练 DLC。然而，自己运行镜像将使你能够更自由地探索和研究镜像。

## 在未管理的实例上在云中调试

另一个选择是在云中使用未管理的 [Amazon EC2](https://aws.amazon.com/ec2/) 实例进行训练。这个选项的优势在于可以在与你在 SageMaker 中使用的相同实例类型上运行。这将使你能够重现可能在本地环境中无法重现的问题，例如，与你使用 GPU 资源相关的问题。最简单的方法是使用与 SageMaker 环境（例如相同的操作系统、Python 和 Python 包版本）最相似的 [机器镜像](https://aws.amazon.com/machine-learning/amis/) 来运行你的实例。或者，你可以拉取 SageMaker 的 DLC 并在远程实例上运行它。然而，尽管这也在云中运行，但运行时环境可能与 SageMaker 的环境有显著差异。SageMaker 在初始化期间配置了一整套系统设置。尝试重现相同环境可能需要相当多的努力。鉴于在云中调试比前两种方法更昂贵，我们的目标应该是在求助于这一选项之前尽可能清理我们的代码。

## 调试限制

尽管上述每个选项对解决某些类型的错误都很有用，但没有一个能完美地复制 SageMaker 环境。因此，当在 SageMaker 中运行时，你可能会遇到这些方法无法复现的问题，从而无法进行修正。特别是，有许多功能仅在 SageMaker 环境中受支持（例如，SageMaker 的 [Pipe input](https://aws.amazon.com/blogs/machine-learning/using-pipe-input-mode-for-amazon-sagemaker-algorithms/) 和 [Fast File](https://aws.amazon.com/about-aws/whats-new/2021/10/amazon-sagemaker-fast-file-mode/) 模式来访问 Amazon S3 中的数据）。如果你的问题与这些功能相关，你将 *无法* 在 SageMaker 之外复现它。

## 调优限制

此外，上述选项未提供有效的性能调优解决方案。运行时性能对环境中的细微变化非常敏感。虽然模拟环境可能提供一些一般优化提示（例如，不同数据增强的比较性能开销），但准确的性能分析只能在 SageMaker 运行时环境中进行。

# SageMaker SSH Helper

[SageMaker SSH Helper](https://github.com/aws-samples/sagemaker-ssh-helper) 提供了连接到远程 SageMaker 训练环境的功能。这是通过 [AWS SSM](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html) 上的 SSH 连接实现的。正如我们将演示的，设置这个功能的步骤非常简单，值得花费精力。 [官方文档](https://github.com/aws-samples/sagemaker-ssh-helper/tree/v2.1.0) 包含了关于此工具的价值及其使用方法的详细信息。

## 示例

在下面的代码块中，我们演示了如何使用 [sagemaker-ssh-helper](https://pypi.org/project/sagemaker-ssh-helper/)（版本 2.1.0）启用对 SageMaker 训练作业的远程连接。我们传入完整的代码源目录，但将通常的 *entry_point*（*train.py*）替换为一个新的 *run_ssh.py* 脚本，该脚本放置在 *source_dir* 的根目录中。请注意，我们将 *SSHEstimatorWrapper* 添加到项目依赖项列表中，因为我们的 *start_ssh.py* 脚本将需要它。或者，我们也可以将 [sagemaker-ssh-helper](https://pypi.org/project/sagemaker-ssh-helper/) 添加到 *requirements.txt* 文件中。这里我们将 *connection_wait_time_seconds* 设置为两分钟。正如我们将看到的，这会影响训练脚本的行为。

```py
from sagemaker.pytorch import PyTorch
from sagemaker_ssh_helper.wrapper import SSHEstimatorWrapper
MINUTE = 60

estimator = PyTorch(
    role='<sagemaker role>',
    entry_point='run_ssh.py',
    source_dir='<path to source dir>',
    instance_type='ml.g5.xlarge',
    instance_count=1,
    framework_version='2.0.1',
    py_version='py310',
    dependencies=[SSHEstimatorWrapper.dependency_dir()]
)

# configure the SSH wrapper. Set the wait time for the connection.
ssh_wrapper = SSHEstimatorWrapper.create(estimator.framework, 
                                    connection_wait_time_seconds=2*MINUTE)

# start job
estimator.fit()

# wait to receive an instance id for the connection over SSM
instance_ids = ssh_wrapper.get_instance_ids()

print(f'To connect run: aws ssm start-session --target {instance_ids[0]}')
```

和往常一样，SageMaker 服务将分配一个机器实例，建立所请求的环境，下载并解压我们的源代码，并安装所请求的依赖项。此时，运行环境将与我们通常运行训练脚本的环境完全相同。只是现在，我们将运行我们的*start_ssh.py*脚本，而不是训练：

```py
import sagemaker_ssh_helper
from time import sleep

# setup SSH and wait for connection_wait_time_seconds seconds
# (to give opportunity for the user to connect before script resumes)
sagemaker_ssh_helper.setup_and_start_ssh()

# place any code here... e.g. your training code
# we choose to sleep for two hours to enable connecting in an SSH window
# and running trials there
HOUR = 60*60
sleep(2*HOUR)
```

[*setup_and_start_ssh*](https://github.com/aws-samples/sagemaker-ssh-helper/blob/main/sagemaker_ssh_helper/__init__.py#L10) 函数将启动 SSH 服务，然后在我们上面定义的分配时间（*connection_wait_time_seconds*）内阻塞，以允许 SSH 客户端连接，然后继续执行脚本的其余部分。在我们的例子中，它将睡眠两小时，然后退出训练作业。在这段时间里，我们可以使用[*aws ssm start-session*](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-sessions-start.html) 命令和由*ssh_wrapper* 返回的 instance-id（通常以“*mi-”*前缀开头，代表“*管理实例*”）连接到机器，尽情玩耍。特别是，我们可以显式运行我们原始的训练脚本（该脚本作为*source_dir*的一部分上传）并监控训练行为。

我们描述的方法使我们能够在识别和修复错误的同时，迭代运行我们的训练脚本。它还提供了一个优化性能的理想环境——在这个环境中，我们可以 1) 运行几个训练步骤，2) 识别性能瓶颈（例如，使用[PyTorch Profiler](https://pytorch.org/tutorials/recipes/recipes/profiler_recipe.html)），3) 调整我们的代码以解决这些瓶颈，然后 4) 重复这个过程，直到我们实现所需的运行时性能。

重要的是，请记住，一旦*start_ssh.py* 脚本完成，实例将被终止。在为时已晚之前，确保将所有重要文件（例如，代码修改、配置文件跟踪等）复制到持久存储中。

## 通过 AWS SSM 进行端口转发

我们可以扩展我们的*aws ssm start-session* 命令，以启用[端口转发](https://aws.amazon.com/blogs/aws/new-port-forwarding-using-aws-system-manager-sessions-manager/)。这允许您安全地连接到云实例上运行的服务器应用程序。这对于习惯使用[TensorBoard Profiler](https://pytorch.org/tutorials/intermediate/tensorboard_profiler_tutorial.html) 插件分析运行时性能的开发人员尤其令人兴奋（正如[我们所做的](/pytorch-model-performance-analysis-and-optimization-10c3c5822869)）。下面的命令演示了如何通过 AWS SSM 设置端口转发：

```py
aws ssm start-session \
  --target mi-0748ce18cf28fb51b \
  --document-name AWS-StartPortForwardingSession
  --parameters '{"portNumber":["6006"],"localPortNumber":["9999"]}'
```

## 使用的其他模式

[SageMaker SSH Helper 文档](https://github.com/aws-samples/sagemaker-ssh-helper/tree/v2.1.0)描述了几种使用 SSH 功能的方法。在[基础示例](https://github.com/aws-samples/sagemaker-ssh-helper/tree/v2.1.0#step-3-modify-your-training-script)中，将 *setup_and_start_ssh* 命令添加到现有训练脚本的顶部（而不是定义一个专门的脚本）。这使你有时间（根据 *connection_wait_time_seconds* 设置定义）在训练开始前连接到机器，以便你可以在训练运行时（通过一个独立的进程）监控其行为。

更加[高级的示例](https://github.com/aws-samples/sagemaker-ssh-helper/tree/v2.1.0#pycharm-debug-server)包括使用 SageMaker SSH Helper 从本地环境中的 IDE 调试运行在 SageMaker 环境中的训练脚本的不同方法。虽然设置更为复杂，但能够从本地 IDE 进行逐行调试的奖励可能非常值得。

其他用例包括[在 VPC 中训练](https://github.com/aws-samples/sagemaker-ssh-helper/blob/v2.1.0/FAQ.md#im-running-sagemaker-in-a-vpc-do-i-need-to-make-extra-configuration)、与[SageMaker Studio](https://github.com/aws-samples/sagemaker-ssh-helper/tree/v2.1.0#studio)的集成、[连接到 SageMaker 推理端点](https://github.com/aws-samples/sagemaker-ssh-helper/tree/v2.1.0#connecting-to-sagemaker-inference-endpoints-with-ssm)等。请务必查看文档以获取详细信息。

## 何时使用 SageMaker SSH Helper

考虑到使用 SageMaker SSH Helper 进行调试的优势，你可能会想知道是否有理由使用我们上述描述的三种调试方法。我们认为，尽管你*可以*在云端进行所有调试，但仍然强烈建议你在本地环境中进行初步开发和实验阶段——尽可能地使用我们描述的前两种方法。只有在你已耗尽本地调试的能力后，才应转到使用 SageMaker SSH Helper 在云端进行调试。你最不希望的就是在一个极其昂贵的云端 GPU 机器上花费数小时清理简单的语法错误。

与调试相反，性能分析和优化的价值不大*除非*它在目标训练环境中直接进行。因此，*建议*在使用 SageMaker SSH Helper 的 SageMaker 实例上进行优化工作。

# 总结

到现在为止，在 Amazon SageMaker 上训练的一个最痛苦的副作用就是失去了对训练环境的直接访问。这限制了我们有效调试和调整训练工作负载的能力。最近发布的 SageMaker SSH Helper 以及对训练环境的直接访问支持，为开发、调试和调整提供了大量新的机会。这些机会可以对你的 ML 开发生命周期的效率和速度产生显著影响。这就是为什么 SageMaker SSH Helper 是我们 2023 年最喜欢的新云 ML 功能之一。
