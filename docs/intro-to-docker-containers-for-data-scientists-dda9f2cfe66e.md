# 数据科学家的Docker容器简介

> 原文：[https://towardsdatascience.com/intro-to-docker-containers-for-data-scientists-dda9f2cfe66e?source=collection_archive---------4-----------------------#2023-12-20](https://towardsdatascience.com/intro-to-docker-containers-for-data-scientists-dda9f2cfe66e?source=collection_archive---------4-----------------------#2023-12-20)

## 一个实用的教程，教你如何使用Docker容器设置本地开发环境

[](https://leshem-ido.medium.com/?source=post_page-----dda9f2cfe66e--------------------------------)[![Ido Leshem](../Images/914fd1635e4c34876816956422c357e8.png)](https://leshem-ido.medium.com/?source=post_page-----dda9f2cfe66e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dda9f2cfe66e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----dda9f2cfe66e--------------------------------) [Ido Leshem](https://leshem-ido.medium.com/?source=post_page-----dda9f2cfe66e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F64b979a03bf7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintro-to-docker-containers-for-data-scientists-dda9f2cfe66e&user=Ido+Leshem&userId=64b979a03bf7&source=post_page-64b979a03bf7----dda9f2cfe66e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dda9f2cfe66e--------------------------------) ·7分钟阅读·2023年12月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdda9f2cfe66e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintro-to-docker-containers-for-data-scientists-dda9f2cfe66e&user=Ido+Leshem&userId=64b979a03bf7&source=-----dda9f2cfe66e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdda9f2cfe66e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintro-to-docker-containers-for-data-scientists-dda9f2cfe66e&source=-----dda9f2cfe66e---------------------bookmark_footer-----------)![](../Images/3fbf0a765877408a6ff283956455bbdd.png)

图片来源：Tom Fask，来自Pexels

# **动机**

数据科学家日常工作的重要组成部分是管理和维护开发环境。当开发环境保持更新并且紧密反映生产环境时，我们的工作会顺利很多；否则，事情就会变得混乱。在较大的环境中，对CI/CD管道和DevOps的熟练掌握非常有利。提供易于集成和投入生产的开发成果是数据科学家的首要任务。

这就是容器发挥作用的地方；通过封装我们的开发环境，它们可以帮助我们节省时间和精力。

在这篇博客文章中，我将提供逐步指南，说明如何设置 Docker 环境。我将使用一个 Linux 环境，Python 版本为 3.8，并连接到你选择的 Git 仓库。为了说明，我创建了一个包含一个算法的仓库，该算法可以判断给定的文本是更可能是 AI 生成的还是由人类编写的。

## **谁可以从使用 Docker 容器环境中受益？**

如果你是一个正在进行副项目的开发者，并且不关心将其部署到生产环境中，那么你可能不需要使用容器。然而，如果你是使用 CI/CD 流水线的团队的一部分，那么这就是一种必要性。

我能想到一些日常的例子，说明容器化是多么重要：

1.  假设你正在处理多个具有不同依赖项和环境设置的项目。如果不使用容器，在它们之间切换可能会非常具有挑战性。

1.  一个新的开发者加入团队，你希望他能轻松地设置工作环境。如果没有容器，他或她将不得不在本地机器上工作，而本地机器可能与其他团队成员的要求和属性不同。

1.  如果你对依赖项进行了某些测试或更改，而这些依赖项现在彼此不兼容，在这种情况下，回滚更改将是一个耗时的任务。另一种选择是创建一个新的容器并恢复正常操作。

## **什么是容器？**

容器的概念最早在 1970 年代被提出。可以把容器想象成一个隔离的工作环境——一个我们可以从零开始定义的服务器。我们可以决定这个服务器的属性，比如操作系统、Python 解释器版本和库依赖。服务器依靠你的机器资源来维持。容器的另一个属性是，它没有访问我们的存储，除非我们明确授予权限。一个好的做法是挂载一个我们希望包含在容器范围内的文件夹。

总而言之，使用容器包括以下步骤：

1\. 使用 Dockerfile 定义容器环境

2\. 基于 Dockerfile 创建 Docker 镜像

3\. 基于 Docker 镜像创建容器

这个过程可以很容易地在团队成员之间共享，容器可以根据需要反复创建。

**什么是 Dockerfile？**

如上所述，容器是运行我们算法的封装环境。这个环境由负责容器化的 Docker 扩展支持。

为了实现这一点，我们首先需要定义一个 Dockerfile，该文件指定我们的系统要求。把 Dockerfile 想象成一个文档或“配方”，它定义了我们的容器模板，也就是 Docker 镜像。

这里有一个我们将在本教程中使用的 Dockerfile 示例：

```py
FROM ubuntu:20.04

# Update and install necessary dependencies
RUN apt-get update && \
    apt-get install -y python3-pip python3.8 git && \
    apt-get clean

# Set working directory
WORKDIR /app

# Copy the requirements file and install dependencies
COPY requirements.txt .
RUN pip3 install --no-cache-dir -r requirements.txt

# Install transformers separately without its dependencies
RUN pip3 install --no-cache-dir transformers
```

这个 Dockerfile 包含几个重要步骤：

(1) 我们导入用于创建 Ubuntu 环境的基础镜像。

(2) 我们安装 pip、python 和 git。apt-get 是一个用于包管理的 Linux 命令。

(3) 我们设置工作目录名称（在此示例中为 /app）

(4) 我们安装 requirements.txt 文件中详细列出的依赖

Dockerfile 为我们提供了很多灵活性。例如，我的仓库依赖于 transformers 库及其依赖项，因此我单独安装了它（Dockerfile 的最后一行）。

**注意 —** 使用容器在速度和灵活性方面提供了许多好处，但也有缺点。其中之一就是安全性。由不可信资源上传的容器镜像可能包含恶意内容。确保你使用的是可信来源，并且容器配置正确。另一种选择是使用安全工具如 snyk，它可以扫描你的 Docker 镜像以查找潜在的漏洞。

![](../Images/08efeb6c5032f306d8e21c61f17f2368.png)

图片来源于 Tom Fask，来自 Pexels

# **设置容器**

**初步前提** 在我们创建 Docker 容器之前，首先需要确保我们的本地工作环境已准备好。让我们确保我们拥有以下检查清单：

1\. 使用 VS Code 作为我们的代码编辑器: [https://code.visualstudio.com/](https://code.visualstudio.com/)

2\. 版本控制管理的 Git: [https://git-scm.com/downloads](https://git-scm.com/downloads)

3\. Github 用户: [https://github.com/](https://github.com/)

4\. [https://www.docker.com/](https://www.docker.com/)

完成所有这些前提条件后，请确保登录你安装的 Docker 应用程序。这将使我们能够创建 Docker 容器并跟踪其状态。

**步骤 1 — 克隆仓库**

首先，让我们选择一个仓库进行操作。这里我提供了一个包含算法的仓库，该算法通过结合模型在给定文本下的困惑度值和拼写错误数量来估计文本是否由AI生成。较高的困惑度意味着LLM更难预测下一个词，因此不是由人类生成的。

仓库链接：

[](https://github.com/Idoleshem/setup_a_local_container?source=post_page-----dda9f2cfe66e--------------------------------) [## GitHub - Idoleshem/setup_a_local_container

### 通过在 GitHub 上创建帐户来贡献于 Idoleshem/setup_a_local_container 的开发。

[github.com](https://github.com/Idoleshem/setup_a_local_container?source=post_page-----dda9f2cfe66e--------------------------------)

在 GitHub 上，点击 **code** 并复制 HTTPS 地址，如下所示：

![](../Images/84a2b8dac6fab0d65425d44a6b542773.png)

图片作者

然后，打开 VS Code，克隆一个你希望包含在容器中的仓库。确保 VS Code 已连接到你的 GitHub 账户。或者，你也可以初始化一个新的 Git 仓库。

![](../Images/f7dea06269b48ae10074c3042cdf05ab.png)

图片作者

**步骤 2 — 创建 Docker 镜像**

通过打开终端并复制粘贴以下命令来完成此操作：

```py
docker build -t local_container_intro .
```

这可能需要几分钟时间，直到你看到你的 Docker 镜像被创建。点击 Docker 图标，变更将被反映出来。一旦我们创建了 Docker 镜像，我们就不再需要运行这个命令。我们将使用的唯一命令是 docker run。

local_container_intro 是 Docker 镜像的名称，你可以将其更改为你想要的任何名称。

![](../Images/94bafe936090f0cdf43a60b144572b89.png)

作者提供的图片

一旦创建了 Docker 镜像，你可以在 IMAGES 窗口中看到它。

**步骤 3 — 创建一个 Docker 容器**

为了授予容器访问你克隆的仓库的权限，请记得在 docker run 命令中包括你的项目路径。我们将使用以下命令创建容器，并将其命名为“local_container_instance”：

```py
 docker run -it --name local_container_instance -v /pate/to/your/project/folder :/project local_container_intro
```

![](../Images/b4e1edd0deb8c4c43eaf2d9e2975c799.png)

作者提供的图片

创建容器后，你可以在 CONTAINERS 窗口中查看它。为了实际使用它，点击“attach visual studio code”。这将打开一个新窗口，反映你的容器化环境。这个环境包括你的代码，你可以在左下角看到你的容器名称。打开终端并运行“pip list”，检查所有依赖项是否已安装。确保安装任何可能需要的 Python 扩展。

![](../Images/476e4bf7c70c32225e0204ef2d6bd2c5.png)

作者提供的图片

就这样，剩下的就是开始开发 :)

总结一下，这篇博客文章提供了设置本地容器的逐步指南。我强烈推荐使用容器，并利用它们在开发中的高灵活性和便利性。
