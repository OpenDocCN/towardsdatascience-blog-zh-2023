# 在 Kubernetes 上进行快速实验的流程

> 原文：[`towardsdatascience.com/a-pipeline-for-fast-experimentation-on-kubernetes-3258389120a1`](https://towardsdatascience.com/a-pipeline-for-fast-experimentation-on-kubernetes-3258389120a1)

## 仅使用原生 Python 包

[](https://pascaljanetzky.medium.com/?source=post_page-----3258389120a1--------------------------------)![Pascal Janetzky](https://pascaljanetzky.medium.com/?source=post_page-----3258389120a1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3258389120a1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3258389120a1--------------------------------) [Pascal Janetzky](https://pascaljanetzky.medium.com/?source=post_page-----3258389120a1--------------------------------)

·发布于[数据科学前沿](https://towardsdatascience.com/?source=post_page-----3258389120a1--------------------------------) ·6 分钟阅读·2023 年 1 月 31 日

--

手动为每个新实验创建一个新配置文件是一项繁琐的过程。特别是当你想要在 Kubernetes 集群上快速部署大量作业时，自动化设置是必须的。使用 python，构建一个简单的调度脚本，读取实验的配置，如批量大小，将其写入 YAML 文件，并创建新作业，非常简单。在这篇文章中，我们将讨论具体的方法。最棒的是，我们无需额外的包！

![](img/042919420529caa70761f3408d3c6ec9.png)

图片来源于[JJ Ying](https://unsplash.com/@jjying?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这个流程包含四个文件：两个 bash 脚本（一个用于创建 Kubernetes 作业，一个用于删除 Kubernetes 作业），一个 python 脚本，以及一个 .yaml 文件模板。我们将详细介绍它们，从 python 脚本开始。你可以在[这个 GitHub 仓库](https://github.com/phrasenmaeher/kubernetes_yaml_pipeline)中找到完整的代码。

## python 脚本

这个 python 代码被结构化为两个方法。第一个方法生成实验配置，这些配置填充了示例值。第二个方法进行实际的调度，包括解析 .yaml 文件和与 Kubernetes 通信。让我们从第一个、更简单的函数开始：

*get_experiments* 方法持有一个内部字典对象，在我的示例中，它包含了 4 个样本实验。每个实验都有一个唯一的编号，每个实验本身也是一个字典。这个字典包含实验的配置，包括标准的机器学习参数，如批量大小、训练轮数和模型。此外，我们指定了我们希望实验运行的数据集，例如 CIFAR10 数据集。这里列出的参数是为了说明目的，你不必局限于这些参数。例如，你可以包含更多的超参数（如学习率）、存储文件路径（如数据集目录）或包括环境变量。简而言之：根据你的需要进行调整。

如果我们调用该方法而不带参数，则等同于使用默认设置“experiment_number=-1”来调用它。使用该设置，会返回所有实验的配置。然而，如果你与 Kubernetes 一起工作了较长时间，你会发现一些作业会因为各种原因失败。如果发生这种情况，最好是修复代码并重新启动那个特定的实验。因此，除了获取所有实验的设置外，我还添加了选择特定实验进行重新运行的功能。当我们用特定实验编号调用该方法时，支持这种用例。一个例子是 4，它将返回第四个实验的配置。

## .yaml 模板

*get_experiments* 方法在脚本的主要函数 *schedule* 中被调用。在详细介绍之前，我们需要查看我们的模板文件。我包含的 .yaml 文件是以我在超级计算集群上运行机器学习实验时常写的文件为基础。然而，它并不是一个有效的文件，意思是你不能直接使用它。

相反，它旨在展示你在填写模板时的灵活性。说到填写模板，这就是它：

请注意那些有两个大括号：{} 的独特位置。每个这些位置都有一个编号，从零开始。我们在 python 脚本中填补这些空白，我们很快就会回到这一点。最适合我们的机器学习用例，“{}”可以放置在模板中的任何位置；它们并不局限于特定位置。

为了展示多样性，我在 .yaml 文件中放置了标记：我们可以使用它们传递命令行参数、挂载目录、填写环境变量或选择我们的 pod 镜像。此外，我们还可以重用占位符：在模板文件中，我使用了“{1}”占位符两次；一次是将作业分配到作业组中（*group-model-{1}*，第 11 行），另一次是将模型名称传递到命令行中（第 25 和 26 行）。

模板文件的填写通过 python 脚本的 *schedule* 方法完成。

在函数内部，我们首先按原样解析模板。然后，我们在第 10 行收集所有我们想运行的实验。接下来的三个变量，第 11 到 13 行，是为了激发你的灵感：你可以自动化的不仅仅是我提出的内容。有趣的部分从第 14 行开始，我们遍历所有我们想要创建的实验。正如我所写，实验配置存储在字典对象中。这意味着我们可以在第 17 到 26 行填写模板时查询字典。

为了更容易理解哪个槽位被填充，我在每一行后面留下了注释。例如，第 18 行填充了标记为“{0}”的位置，第 19 行填充了标记为“{1}”的位置，依此类推。为了查看我们在填写模板后创建了什么，我们在第 27 行打印出完成的版本。

到目前为止，我们已经在内存中创建了一个可以使用的 YAML 文件。接下来的步骤是创建相应的 Kubernetes 作业，从第 30 行开始。首先，我们检查是否只想删除旧的实验（例如，因为它由于某种原因失败了，我们需要先修复错误）。如果不是这种情况，我们在重新创建作业之前需要删除旧作业——不能有两个具有相同名称的作业。

如果不存在之前的作业，脚本在尝试终止它时不会失败，而是会打印一行空白并继续创建作业。

## Shell 脚本

作业的创建和删除都转发给两个小的 bash 脚本。第一个脚本如下所示，它使用 *kubectl* 命令根据传递的内容创建一个作业（*echo "$1"* 部分）。注意，我已经将 *kubectl* 设置为默认使用我的命名空间。如果你没有这样做，那么可以写 *kubectl -n your-namespace* 或将你的命名空间注册为默认命名空间：

删除作业的脚本几乎相同；我们只是将“create”标志换成“delete”：

## 回到 Python 脚本

将各个部分组合在一起后，我们需要一个驱动程序来启动代码。这个任务是通过“main”语句完成的，如下面的代码片段所示：

启动实验时，如前所述，我们可以选择仅（重新）创建一个特定的实验。默认情况下，我们运行 **所有** 实验；要运行个别实验，我们可以将它们的 ID 传递给命令行。

接下来，我们创建一个标志，告诉调度程序仅删除一个运行或重新启动运行。默认情况下，这个标志被设置为 false，意味着我们首先终止一个实验的现有运行，然后重新启动它。只有当我们在命令行上明确设置标志时，它才会被设置为 true，意味着我们仅终止现有作业而不重新创建它。此外，我们也可以完全不设置它；标志的缺失等同于标志为 false。最后，我们解析参数并运行调度。

## 总结

在这篇文章中，我们讨论了如何快速部署实验到 Kubernetes 集群。为实现这一目标，我们使用了四个文件：一个.yaml 模板文件、两个 bash 脚本和一个 python 文件。在 python 文件中，我们使用了两种方法来收集实验的配置，然后填充模板。

完成模板后，作业通过两个单行 bash 脚本创建和删除。在这些脚本中，我们使用了原生 Kubernetes 命令。*最终*，我们可以调用 python 脚本（传递可选的命令行参数），我们的实验将被自动调度。

这个自动化过程在尝试参数组合时非常有用：手动为每个实验创建一个.yaml 文件既繁琐又容易出错。因此，省去这些麻烦，构建一个自动化流水线，如本文所述。用于开始的完整代码可以在[我的代码库](https://github.com/phrasenmaeher/kubernetes_yaml_pipeline)中找到。
