# 如何在大众之前访问未来的 Python 版本，如 3.12

> 原文：[`towardsdatascience.com/how-to-access-future-python-versions-like-3-12-before-the-masses-eb05953ba599`](https://towardsdatascience.com/how-to-access-future-python-versions-like-3-12-before-the-masses-eb05953ba599)

## 并进行测试

[](https://ibexorigin.medium.com/?source=post_page-----eb05953ba599--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----eb05953ba599--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eb05953ba599--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb05953ba599--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----eb05953ba599--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb05953ba599--------------------------------) ·6 分钟阅读·2023 年 6 月 22 日

--

![](img/97b8818cf5623f3b04b622ac8e77d05a.png)

Image by me with Midjourney

新的 Python 版本总是带来显著改进的功能。例如 3.11——它承诺提升高达 60% 的性能，并且去年十月实现了这个承诺。

除了新功能的承诺，测试未发布的 Python 版本有助于开发者更快地发现 bug 并让其他人参与开发过程。利用新功能在大众之前也有获得 **竞争优势** 的实际可能。

即使这些理由听起来不够吸引人，炫耀你在大众之前检查了一个新版本的 Python 总是很酷的。

那么，让我们开始吧。

## 步骤 0：安装 Docker

无法访问新版 Python 的原因是因为它们被很好地隐藏了。它们在 Python.org 上没有下载链接。相反，它们托管在 [官方 Python Docker 镜像](https://hub.docker.com/_/python) 页面上：

![](img/5d6f6338bafa5bfda306718d4b6e0684.png)

Screenshot by me

如你所见，这个镜像已经有超过 10 亿次下载。如果你向下滚动一点，你会看到不同版本的 Python 镜像：

![](img/3489092a5d50f451c478638dfd7721d3.png)

Screenshot by me

我们想安装未发布的 Python 3.12，但有很多版本。我们该选择哪个？

在我们回答之前，确保你已经安装了 [Docker Desktop](https://www.docker.com/products/docker-desktop/) 并且能够在你的 CLI 上成功运行 `docker --version`。

我们使用 Docker 镜像和容器的原因是它们是安全和隔离的。Python 3.12 不会破坏你的环境，如果它在容器中。除此之外，我们别无选择，只能使用容器 :)

> 在这个教程中，你不需要了解 Docker 的任何知识。

## 步骤 1：选择镜像

所有这些词——`alpine`、`rc`、`bookworm`、`slim` 和 `bullseye` 在 Python 镜像标签中是什么意思？这些术语用来告知我们每个镜像所使用的基础操作系统。以下是它们的定义：

+   `alpine`：带有 `alpine` 标签的镜像使用 Alpine Linux 发行版，以其小巧的体积和安全性设计而闻名。

+   `bookworm`：Bookworm 是 Debian OS 12 的代号，这是一种因其稳定性和广泛的软件适配性而闻名的流行发行版。

+   `bullseye`（很酷的名字）：Bullseye 是 Debian（版本 11）的另一个代号。

+   `slim`：这些镜像的变体专注于较小的体积。它们被精简到仅包含运行 Python 应用程序所需的基本组件和依赖项，从而使其更轻量高效。

+   `0bn`：带有 `0b` 的标签代表测试版。例如，`0b2` 镜像是 Python 3.12 Beta 2 版本。Python 3.12 的最终测试版（`0b4`）将在 2023 年 7 月发布。

+   `rc`：带有此标签的镜像是候选版本。RC 版本被认为可能稳定并准备发布，但仍需进一步测试和反馈。

我们将选择这些候选版本中的一个，具体是`3.12-rc-bookworm`。就是 Debian 12。

> 查看 Python 3.12 版本发布计划 [这里](https://peps.python.org/pep-0693/)。

## 步骤 2：拉取镜像

首先，确保 Docker Desktop 正在运行，方法是启动应用程序并检查状态：

![](img/73df241eb070c51cdf7864753adb6672.png)

图片由我提供

然后，在任何终端中运行下面的命令并等待：

```py
$ docker pull python:3.12-rc-bookworm
```

该命令将从 Docker Hub 拉取官方镜像。

在等待时：Docker 镜像是一个轻量、独立且可执行的包，包含运行软件所需的所有内容，包括代码、运行时、系统工具、库和配置。

Docker 容器则是 Docker 镜像的运行实例。我们将在拉取完成后启动这样的容器。

[现代数据科学家的 Docker：2023 年不能忽视的 6 个概念](https://towardsdatascience.com/docker-for-the-modern-data-scientists-6-concepts-you-cant-ignore-in-2023-8c9477e1f4a5?source=post_page-----eb05953ba599--------------------------------) 

### 编辑描述

[查看现代数据科学家的 Docker：2023 年不能忽视的 6 个概念](https://towardsdatascience.com/docker-for-the-modern-data-scientists-6-concepts-you-cant-ignore-in-2023-8c9477e1f4a5?source=post_page-----eb05953ba599--------------------------------)

## 步骤 3：启动一个容器

请鼓掌！我们将第一次使用全新的 Python 3.12。执行的命令是…（此时请保持戏剧性的沉默）：

```py
$ docker run -it python:3.12-rc-bookworm

Python 3.12.0b2 (main, Jun 14 2023, 17:45:20) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

就这样，各位。命令从 3.12-rc-bookworm 镜像启动了一个容器，添加了 `-it` 标签后启动了 Python 解释器。现在，你可以在交互式 Shell 中做任何事情。

了解更多信息，请查看官方文档中的[3.12 中的新功能](https://docs.python.org/3.12/whatsnew/3.12.html)文章。我会首先尝试那些改进的错误信息。

> 要退出 shell 和容器，运行 `exit()`。

## 第 4 步：将 VSCode 连接到容器

你难道不觉得我们会在这里运行一堆在傻乎乎的 shell 中的表达式吗？

哦不。我们要将 Python 3.12 的容器与 VSCode 链接，并编写一些脚本以真正测试新版本。

因此，在你机器上的任何目录中，打开 VSCode（我希望你已经[为 Python 和数据科学配置了它](https://code.visualstudio.com/docs/datascience/data-science-tutorial)）。

```py
$ cd some_dir
$ code . # Launches VSCode
```

然后，安装 Remote Development 扩展：

![](img/b52a687c363c0bee7cff213f22f3c744.png)

图片由我提供

重新加载 VSCode。然后，跳转到你的 Docker Desktop，点击 Containers 菜单：

![](img/f5c7d8ce2333573930d8eb84f7a06ca5.png)

图片由我提供

你会看到一个运行中的容器列表。我版本的 Python 3.12 被称为 `adoring_dirac`。现在，它没有运行，因为我在 CLI 中退出了 Python shell。要运行它，我按下播放按钮来启动容器。

现在，再次打开 VSCode，打开命令面板（Ctrl + Shift + P），搜索“附加到运行中的容器”。这里有一个有用的 GIF：

![](img/43fb8eb62f3fb3b60da5651b2c20da2f.png)

一旦你点击一个以 Python 3.12 为基础镜像的正在运行的容器实例，一个新的 VSCode 窗口将弹出，并直接链接到容器。

请记住，这个容器与操作系统隔离，因此你创建或编辑的任何文件或启动的终端会话也会被隔离，且不可见。

所以，这就是你尝试在容器内测试 Python 3.12 的**绿灯**。除了创建脚本，

![](img/40e4996425a9d76ffd89912e1f8c150a.png)

你还可以安装其他软件，如 Conda、Git、DVC，基本上将容器视为一个只安装了 Python 3.12 的全新空计算机。

## 结论

你还可以将上述方法应用于安装未来的 Python 版本到其他工具或库中。例如，流行的框架如 TensorFlow 或 PyTorch 在 Docker Hub 上有它们的官方 Docker 镜像。

通过利用这些官方镜像，你可以轻松设置支持 GPU 的框架，消除任何复杂性或挑战。Docker 容器已预配置，确保了无忧的安装体验。

感谢阅读！

喜欢这篇文章和它奇怪的写作风格吗？想象一下，访问到更多类似的文章，所有文章都由一位才华横溢、迷人且风趣的作者（顺便说一句，就是我 :)）。

只需 4.99 美元的会员费，你不仅能访问我的故事，还能从 Medium 上最优秀的头脑那里获得丰富的知识宝藏。如果你使用 [我的推荐链接](https://ibexorigin.medium.com/membership)，你将获得我超级感激的心意和一个虚拟的击掌，以支持我的工作。

[](https://ibexorigin.medium.com/membership?source=post_page-----eb05953ba599--------------------------------) [## 使用我的推荐链接加入 Medium — Bex T。

### 作为 Medium 会员，你的一部分会员费会分配给你阅读的作者，你可以完全访问所有故事……

ibexorigin.medium.com](https://ibexorigin.medium.com/membership?source=post_page-----eb05953ba599--------------------------------)
