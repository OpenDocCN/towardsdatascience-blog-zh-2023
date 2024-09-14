# 在 Kubernetes 上运行代码的智能灵活方式

> 原文：[`towardsdatascience.com/the-smart-flexible-way-to-run-code-on-kubernetes-94d1ae6c46f3`](https://towardsdatascience.com/the-smart-flexible-way-to-run-code-on-kubernetes-94d1ae6c46f3)

[](https://pascaljanetzky.medium.com/?source=post_page-----94d1ae6c46f3--------------------------------)![Pascal Janetzky](https://pascaljanetzky.medium.com/?source=post_page-----94d1ae6c46f3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----94d1ae6c46f3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----94d1ae6c46f3--------------------------------) [Pascal Janetzky](https://pascaljanetzky.medium.com/?source=post_page-----94d1ae6c46f3--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----94d1ae6c46f3--------------------------------) ·6 分钟阅读·2023 年 1 月 23 日

--

当我还是一个 Kubernetes 初学者时，我主要关心的是如何让代码在集群上运行。被投身于一个全新的世界，我看到所有这些令人困惑的 YAML 文件，每一行和缩进都带来新的意义。

![](img/77a4045112438f4e57e21a6fb9839dc5.png)

照片由[Thais Morais](https://unsplash.com/@tata_morais?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

一旦我学会了将代码最快速度放入文件中，我很快就用绝对路径填满了它。你可以看到下面的截断示例，我称之为*初学者的方式*。请注意，这是一种完全有效的运行代码的方式；只是缺乏即将到来的部分所关注的特性。

## 快速但容易出错的方式：代码目录的完整路径

让我们深入探讨一下.yaml 文件，并重点突出最相关的部分，以便将代码（和数据）放入容器中。简单来说，容器只是一个包含相关内容的盒子。这个盒子由 Pod 使用，我们可以将 Pod 视为一个虚拟计算机。这个虚拟计算机帮助我们运行容器中的内容。

（如果你对容器化更熟悉，你可能会注意到这个描述是简化的。a) 这是正确的，b) 你可能已经知道比这篇博客文章教你的更多内容）

在 YAML 文件中与本文相关的部分是我们决定运行哪些代码的“*command:*”部分。这已经是个棘手的地方。假设你的本地开发机器使用以下虚构路径：*/local/user/path/script.py*。假设我们将此代码发布到计算集群并存储在*/remote/cluster/path/script.py*下。既然我们知道脚本在 Kubernetes 集群中的位置，我们可以转到 YAML 文件中并粘贴它的路径。如果运气好，它会工作。

但很可能不会，特别是如果我们在*script.py*中使用了来自其他用户定义的 Python 文件的导入时。我们的代码很可能会因*“ModuleNotFoundError: No module named ‘xyz’”*而失败。这种错误发生是因为当我们运行 script.py 时，Python 在已知的位置搜索要导入的模块。如果我们希望读取/导入的文件不在这些位置，我们的代码将无法运行。

为了说明这一点，设想你站在烈日下，夏天的热浪让你汗流浃背。你想喝点水，哪怕是自来水，现在也非常棒，于是你走进屋里。

从厨房水槽的水龙头中，你倒了一杯水，急切地喝下去。水很不错，所以你也想在外面喝点东西。因为水来自水龙头，所以你简单地撕下水龙头并带着它。再次走到外面，你会发现：“WaterNotFoundError: No water。”

在这个构造的示例中，很明显为什么仅仅撕下水龙头并不能魔法般地获得无限的水供应。问题在 Python 示例中也是一样的：我们操作的点不知道所需文件的位置。在本地开发环境中，所需的代码可能与主脚本在同一文件夹中。但当我们——类似于水龙头的情况——从集群上的另一个文件夹（即从其他位置调用 Python 脚本*）操作时，代码会发生什么？答案是*“ModuleNotFoundError: No module named ‘xyz’”*

这里有一个快速但粗糙的修复方法（我之前也用过）：在所有从其他用户创建的 Python 文件中导入的 Python 文件中，将这些辅助文件所在的目录添加到 Python 的搜索路径中：

```py
sys.path.append("directory/with/auxiliary/files")
```

对所有目录重复这一步，你就完成了。

这个修复能解决问题，但不是最好的方法。代码膨胀；我们不需要本地设置中的额外行，如果我们把辅助文件完全移动到文件系统上呢？我们将不得不更新所有硬编码路径！这太麻烦了。幸运的是，还有更好的方法。

## 智能且灵活的方法：相对路径和包含代码

运行代码的更好方法是使用相对路径。在运行的示例中，我们将 Python 文件存储在集群的*/remote/cluster/path/script.py*下。

我将假设，根据良好的惯例，所有辅助代码（例如，“utils.py”）都位于此目录或其子目录中。考虑到这一点，采取更智能的方法有两个步骤，一个涉及 Docker 镜像，另一个涉及 .yaml 文件。

**改进后的 Docker 镜像** 在展示改进版本之前，这里有一个相当标准的 Dockerfile：

尽管这个默认的 Dockerfile 能完成工作并安装所有包，但我们可以通过稍微改变*内部*文件夹结构来进一步改进它：

在改进后的 Dockerfile 中，我们首先安装所需的 Python 包，然后将代码直接存储在镜像中。这一步非常重要，和之前的方法相比差别巨大：之前我们从底层文件系统调用 Python 脚本；现在我们从**Dockerfile 内部**调用它们。

为此，我们创建一个名为*code*的文件夹，将其设置为工作目录，并通过运行“*COPY . .*”将所有代码/数据等复制到 Docker 镜像中的此目录中。之后，我们授予自己在容器中运行代码的权限。

**改进后的 .yaml 文件** 使用这个设置，可以构建改进后的 .yaml 文件。我们之前调用脚本的完整路径，例如，

```py
command: [ "python3", "/home/scripts/research/audio/preprocessing.py" ]
```

现在我们可以将其简化为

```py
command: [ "python3", "preprocessing.py" ]
```

再次，差异可能看起来很小：毕竟，我们只是移除了脚本的路径（* /remote/cluster/path*）。

但在幕后，我们现在正在读取并运行镜像中的 Python 文件。换句话说，我们有了一个可移植的环境。

**不过有一个警告**：如果我们将新代码推送到远程服务器，从而更新我们的脚本，这一变化会反映在镜像中吗？不会，镜像保持为我们最后构建的状态。

## 两个 Dockerfile 以加快构建时间

幸运的是，对于这种常见情况，有一个简单的技巧：维护两个 Dockerfile。在这种情况下，我的常规步骤如下。

1.  我有一个主要的 Dockerfile，恰当地叫做*Dockerfile_base*。

我首先用这个文件构建一个镜像，文件中包含足够的命令来创建一个项目的基础。例如，这可能是这样的一个起始点：

细看，你会发现我在这一步并没有复制任何代码相关的文件。相反，我只安装了 pip 包，并通过 apt-get 获取一些额外的需求。这个结构不太可能发生变化，所以我将生成的镜像存储为，例如，*project-audio-analysis-base:0.0.1*。注意镜像名称中的 *-base* 标签。

2\. 构建基础镜像后，我构建第二个 Docker 镜像。

对于这个镜像，我拉取了之前创建的*project-audio-analysis-base:0.0.1*。这个新镜像是从一个单独的 Dockerfile 构建的，我通常称之为*Dockerfile_update*或类似名称。其内容示例如下：

关键是，我将生成的镜像**不是**存储在与之前基础脚本相同的位置——这会覆盖我们干净的起点。相反，我将其存储在一个不同的位置；通常，我只是省略 *base tag*，如：*project-audio-analysis:0.0.1*。每当代码有已发布的更改时，我使用较薄的 *Dockerfile_update*。这种方法的好处是始终有一个干净的起点（对于你破坏了代码的情况）并显著减少构建时间。

如果我们每次创建新的 Docker 镜像时都重新安装所有软件包，我们每次都需要等待。这是不必要的。只需存储一个已预装所有这些软件包的固定基础，并在此基础上构建镜像即可。

# 总结

在开始使用 Kubernetes 时，常常会遇到这些巨大的、复杂的 YAML 文件。可以理解的是，一旦代码按预期运行，人们会感到相当释然。然而，一个错误——或错误的设计决策——是使用绝对路径并从文件系统中运行代码。

为了缓解这个问题，我提出了一种更好、更清洁、更便携的在 Kubernetes 集群中运行代码的方法。它包括两个部分：首先，将代码包含在镜像内，其次，维护一个基础和更新的 Dockerfile。拥有这些工具后，可以快速生成改进的镜像。

* 从另一个位置调用 Python 脚本：通常，我们使用命令行进入 Python 脚本所在的文件夹，然后执行 *python script.py*。但是，当在另一个文件夹中时，我们也可以运行 *python /path/to/script.py*。在这种情况下，博客文章中描述的问题会出现。
