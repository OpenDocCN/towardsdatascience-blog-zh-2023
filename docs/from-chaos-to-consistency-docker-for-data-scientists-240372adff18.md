# 数据科学中的 Docker

> 原文：[`towardsdatascience.com/from-chaos-to-consistency-docker-for-data-scientists-240372adff18`](https://towardsdatascience.com/from-chaos-to-consistency-docker-for-data-scientists-240372adff18)

## 数据科学家用 Docker 的介绍和应用

[](https://medium.com/@egorhowell?source=post_page-----240372adff18--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----240372adff18--------------------------------)[](https://towardsdatascience.com/?source=post_page-----240372adff18--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----240372adff18--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----240372adff18--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----240372adff18--------------------------------) ·阅读时间 7 分钟·2023 年 5 月 24 日

--

![](img/b9ddc7ca1478ae7fd299d341e9610337.png)

图片由 [Ian Taylor](https://unsplash.com/@carrier_lost?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

> 但在我的机器上运行正常？

这是技术社区中的经典梗，尤其对于那些想要发布他们惊人的机器学习模型的数据科学家来说，结果却发现生产机器上有不同的操作系统。远非理想。

然而…

多亏了这些被称为[**容器**](https://www.docker.com/resources/what-container/)的奇妙事物和控制它们的工具，如[**Docker**](https://en.wikipedia.org/wiki/Docker_%28software%29)**，**这才有了解决方案。

在这篇文章中，我们将深入探讨容器是什么，以及如何使用 Docker 构建和运行它们。容器和 Docker 的使用已成为行业标准和数据产品的常见实践。作为数据科学家，学习这些工具是你武器库中宝贵的工具。

# 什么是 Docker？

> Docker 是一种服务，可以在容器中构建、运行和执行代码及应用程序。

现在你可能在想，什么是容器？

表面上看，容器非常类似于[**虚拟机 (VM)**](https://en.wikipedia.org/wiki/Virtual_machine)。它是一个小型隔离环境，一切都自‘包含’，可以在任何机器上运行。容器和虚拟机的主要卖点是它们的便携性，允许你的应用程序或模型在任何本地服务器、个人计算机或如[**AWS**](https://aws.amazon.com/)这样的云平台上无缝运行。

容器和虚拟机之间的主要区别在于它们如何使用主机计算机的资源。容器要轻得多，因为它们不会主动分割主机计算机的硬件资源。我这里不深入探讨所有技术细节，但如果你想了解更多，我在[这里](https://www.freecodecamp.org/news/a-beginner-friendly-introduction-to-containers-vms-and-docker-79a9e3e119b/)链接了一篇很棒的文章解释它们的区别。

Docker 简单来说是我们用来轻松创建、管理和运行这些容器的工具。这是容器变得非常流行的主要原因之一，因为它使开发者能够轻松部署可以在任何地方运行的应用程序和模型。

![](img/248db54f5672b41ad6c9f1deb2b8ee83.png)

作者提供的图示。

# Docker 技术特性

我们运行容器所需的三个主要元素是：

+   **Dockerfile：** *一个包含如何构建 Docker 镜像的指令的文本文件。*

+   **Docker 镜像：** *一个创建 Docker 容器的蓝图或模板。*

+   **Docker 容器：** *一个提供应用程序或机器学习模型运行所需的一切的隔离环境。包括依赖项和操作系统版本等。*

![](img/e10fb19dc6b93ce00b58c6d168f669ed.png)

作者提供的图示。

还有一些其他关键点需要注意：

+   **Docker 守护进程：** *一个后台进程（[*守护进程*](https://en.wikipedia.org/wiki/Daemon_(computing))），处理对 Docker 的传入请求。*

+   **Docker 客户端：** *一个允许用户通过其守护进程与 Docker 进行交互的 shell 接口。*

+   [**DockerHub**](https://hub.docker.com/)**：** *类似于 GitHub，开发者可以在这里分享他们的 Docker 镜像。*

# 安装 Docker

## Hombrew

首先你应该安装的是 **Homebrew** ([link here](https://brew.sh/))。它被称为“MacOS 的缺失包管理器”，对任何在 Mac 上编码的人都非常有用。

要安装 Homebrew，只需运行他们网站上给出的命令：

```py
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

通过运行`brew help`来验证 Homebrew 是否已安装。

## Docker

现在 Homebrew 已安装，你可以通过运行`brew install docker`来安装 Docker。通过运行`which docker`来验证 Docker 是否已安装，输出结果不应出现任何错误，应该如下所示：

```py
/opt/homebrew/bin/docker
```

## Colima

最后一步是安装 [**Colima**](https://github.com/abiosoft/colima)**。** 只需运行`install colima`，并通过`which colima`验证它是否已安装。再次，输出结果应如下所示：

```py
/opt/homebrew/bin/colima
```

现在你可能会想，Colima 到底是什么？

Colima 是一个软件包，能够在 MacOS 上启用[**容器运行时**](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)。简单来说，Colima 为容器在我们的系统上工作创建了环境。为此，它运行一个带有[**守护进程**](https://en.wikipedia.org/wiki/Daemon_(computing))的 Linux 虚拟机，Docker 可以通过[**客户端-服务器模型**](https://en.wikipedia.org/wiki/Client%E2%80%93server_model)与其通信。

或者，你也可以安装[**Docker 桌面版**](https://www.docker.com/products/docker-desktop/)来替代 Colima。然而，我更喜欢 Colima，原因有几个：它免费、更轻量，而且我喜欢在终端中工作！

> 有关 Colima 的更多论点，请查看这个[博客文章](https://alexpearce.me/2022/07/colima-docker-desktop-replacement/)。

# 使用 Docker 的示例

## 工作流

以下是数据科学家和机器学习工程师如何使用 Docker 部署其模型的示例：

![](img/e3c5bb63da7980c4b0e39bfd3298c5ce.png)

作者的图示。

第一步显然是构建他们的惊人模型。然后，你需要将运行模型所需的所有内容打包起来，比如 Python 版本和包依赖。最后一步是使用 Dockerfile 中的 requirements 文件。

如果你现在觉得这完全随意，不用担心，我们会一步一步讲解这个过程！

## 基本模型

我们先从构建一个基本模型开始。提供的代码片段展示了在著名的[Iris 数据集](https://www.kaggle.com/datasets/uciml/iris)上实现[**随机森林**](https://en.wikipedia.org/wiki/Random_forest)分类模型的简单实现。

> [来自 Kaggle 的 CC0 许可数据集。](https://www.kaggle.com/datasets/uciml/iris)

作者的 GitHub Gist。

这个文件叫做`basic_rf_model.py`，供参考。

## 创建需求文件

现在我们有了模型，我们需要创建一个`requirement.txt`文件，来容纳支撑我们模型运行的所有依赖。在这个简单的例子中，我们幸运地只依赖于`scikit-learn`包。因此，我们的`requirement.txt`文件看起来会是这样：

```py
scikit-learn==1.2.2
```

你可以通过`scikit-learn --version`命令检查你计算机上运行的版本。

## 创建 Dockerfile

现在我们终于可以创建我们的 Dockerfile 了！

因此，在与`requirement.txt`和`basic_rf_model.py`相同的目录下，创建一个名为`Dockerfile`的文件。在`Dockerfile`中，我们将包含以下内容：

作者的 GitHub Gist。

让我们逐行分析，看看这是什么意思：

+   `FROM python:3.9`：*这是我们镜像的基础镜像*

+   `MAINTAINER egor@some.email.com`：*这表示谁维护这个镜像*

+   `WORKDIR /src`：*将镜像的工作目录设置为 src*

+   `COPY . .`：*将当前目录的文件复制到 Docker 目录*

+   `RUN pip install -r requirements.txt`：*将`requirement.txt`文件中的要求安装到 Docker 环境中*

+   `CMD ["python", "basic_rf_model.py"]`：*告诉容器执行命令* `python basic_rf_model.py` *并运行模型*

## 启动 Colima 和 Docker

下一步是设置 Docker 环境：首先我们需要启动 Colima：

```py
colima start
```

Colima 启动后，通过运行以下命令检查 Docker 命令是否有效：

```py
docker ps
```

它应该返回类似这样的内容：

```py
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

这很好，意味着 Colima 和 Docker 按预期工作！

> **注意**：`docker ps` 命令列出所有当前运行的容器。

## 构建镜像

现在是时候从我们上面创建的 `Dockerfile` 构建我们的第一个 Docker 镜像了：

```py
docker build . -t docker_medium_example
```

`-t` 标志表示镜像的名称，`*.*` 告诉我们从当前目录构建。

如果我们现在运行`docker images`，我们应该会看到类似这样的内容：

![](img/e614fabc2418cc1e05331aee2b3dce81.png)

作者提供的图片。

恭喜，镜像已构建完成！

## 运行容器

在镜像创建完成后，我们可以使用上面列出的 `IMAGE ID` 作为容器运行它：

```py
docker run bb59f770eb07
```

输出：

```py
Accuracy: 0.9736842105263158
```

因为它所做的只是运行了`basic_rf_model.py`脚本！

## 额外信息

本教程只是触及了 Docker 的基本功能和用途。要真正理解 Docker，还有许多功能和命令需要学习。Docker 网站上提供了一个详细的教程，你可以在[这里](https://docker-curriculum.com/#docker-run)找到。

一个很酷的功能是你可以以交互模式运行容器并进入其 shell。例如，如果我们运行：

```py
docker run -it bb59f770eb07 /bin/bash
```

你将进入 Docker 容器，应该会看到类似这样的内容：

![](img/18257aca7eddeef508e5f81ae6050810.png)

作者提供的图片。

我们还使用了`ls`命令来显示 Docker 工作目录中的所有文件。

# 总结与进一步思考

Docker 和容器是确保数据科学家的模型可以随时随地运行的绝佳工具。它们通过创建包含模型有效运行所需的所有内容的小型隔离计算环境来实现这一点。这被称为容器。它易于使用且轻量化，使其成为如今常见的工业实践。本文介绍了如何使用 Docker 将模型打包到容器中的基本示例。这个过程简单而无缝，数据科学家可以很快学习和掌握。

本文使用的完整代码可以在我的 GitHub 上找到：

[](https://github.com/egorhowell/Medium-Articles/tree/main/Software%20Engineering%20/docker-example?source=post_page-----240372adff18--------------------------------) [## Medium-Articles/Software Engineering /docker-example at main · egorhowell/Medium-Articles

### 你现在无法执行此操作。你在另一个标签页或窗口中登录。你在另一个标签页或窗口中退出了……

github.com](https://github.com/egorhowell/Medium-Articles/tree/main/Software%20Engineering%20/docker-example?source=post_page-----240372adff18--------------------------------)

# 另一个事项！

我有一个免费的通讯，[**Dishing the Data**](https://dishingthedata.substack.com/)，在其中我每周分享成为更优秀数据科学家的技巧。没有“浮夸”或“点击诱饵”，只有来自实践中的数据科学家的纯粹可操作见解。

[](https://newsletter.egorhowell.com/?source=post_page-----240372adff18--------------------------------) [## Dishing The Data | Egor Howell | Substack

### 如何成为更好的数据科学家。点击阅读由 Egor Howell 编写的 Dishing The Data，一份 Substack 出版物。

[newsletter.egorhowell.com](https://newsletter.egorhowell.com/?source=post_page-----240372adff18--------------------------------)

# 与我联系！

+   [**YouTube**](https://www.youtube.com/@egorhowell)

+   [**LinkedIn**](https://www.linkedin.com/in/egor-howell-092a721b3/)

+   [**Twitter**](https://twitter.com/EgorHowell)

+   [**GitHub**](https://github.com/egorhowell)

# 参考资料与进一步阅读

+   *Docker 网站*: [`www.docker.com/`](https://www.docker.com/)

+   *Docker 教程*: [`docker-curriculum.com/`](https://docker-curriculum.com/)

+   *Docker 常见技巧*: [`github.com/veggiemonk/awesome-docker`](https://github.com/veggiemonk/awesome-docker)
