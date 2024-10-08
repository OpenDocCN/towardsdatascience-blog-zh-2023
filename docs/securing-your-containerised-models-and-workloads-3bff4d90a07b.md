# 保护你的容器化模型和工作负载

> 原文：[`towardsdatascience.com/securing-your-containerised-models-and-workloads-3bff4d90a07b`](https://towardsdatascience.com/securing-your-containerised-models-and-workloads-3bff4d90a07b)

## 切换到非根用户！

[](https://medium.com/@teosiyang?source=post_page-----3bff4d90a07b--------------------------------)![Jake Teo](https://medium.com/@teosiyang?source=post_page-----3bff4d90a07b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3bff4d90a07b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3bff4d90a07b--------------------------------) [Jake Teo](https://medium.com/@teosiyang?source=post_page-----3bff4d90a07b--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3bff4d90a07b--------------------------------) ·阅读时间 8 分钟·2023 年 10 月 24 日

--

容器化现在是部署许多应用程序的*事实上的*方法，其中 Docker 是推动其采用的前沿软件。随着其流行，也带来了攻击风险的增加 [1]。因此，我们需要确保保护我们的 Docker 应用程序。实现这一目标的最基本方法是确保我们在容器内设置非根用户。

```py
CONTENTS
========

Why use non-root?

What you can & cannot do as a default non-root user

The Four Scenarios
  1) Serve a model from host (Read Only)
  2) Run data processing pipelines (Write within Container)
  3) Libraries automatically writing files (Write within Container)
  4) Save trained models (Write to Host)

Summary
```

# 为什么要使用非根用户？

或者说，为什么不使用根用户？让我们以下面的虚拟架构为例。

![](img/f13742acc9199c79964e30e62bc8eac1.png)

黑客进入一个具有根权限的容器。图片由作者提供

安全通常被视为多层防御。如果攻击者设法进入容器，作为用户的权限将是第一道防线。如果容器用户被分配了根权限，攻击者就可以自由控制容器内的一切。凭借如此广泛的访问权限，攻击者还可以利用任何潜在的漏洞，并可能通过这些漏洞逃脱到主机，获得对所有连接系统的完全访问权限。后果非常严重，包括以下几点：

+   检索存储的机密

+   拦截并扰乱你的流量

+   运行恶意服务，如加密挖矿

+   访问任何连接的敏感服务，如数据库

![](img/1698de8e7b318fe8852897900e5f7418.png)

攻击者可能通过根权限横向渗透你的基础设施服务。图片由作者提供

真是太可怕了！不过解决方案很简单，将你的容器改为非根用户！

*在继续阅读本文之前，如果你对 Linux 权限和访问控制没有很好的理解，请查看我之前的* [*文章*](https://medium.com/@teosiyang/securing-linux-servers-with-two-commands-de5b565dc104) *[2]。*

# 作为默认的非根用户，你可以做什么和不能做什么

让我们尝试创建一个具有默认非 root 用户的简单 Docker 应用程序。使用下面的`Dockerfile`。

```py
# Dockerfile
FROM python:3.11-slim

WORKDIR /app

# create a dummy py file
RUN echo "print('I can run an existing py file')" > example.py

# create & switch to non-root user
RUN adduser --no-create-home nonroot
USER nonroot
```

构建镜像并使用它创建一个容器。

```py
docker build -t test .
docker run -it test bash
```

现在你在容器内，让我们尝试几个命令。那么你不能做哪些事情呢？你可以看到各种写入和安装权限都是不允许的。

![](img/495c8c4df0bda8034359b37a6672190e.png)

作为默认的非 root 用户你不能做的事情。作者截图。

在相反的情况下，我们可以运行各种读取权限。

![](img/376c2dc54b91b1e90b06e88235e902d2.png)

读取命令是可以的。作者图片

由于我们安装了 python，它有些独特。如果我们`ls -l $(which python)`，可以看到 python 解释器具有完全权限。因此，它可以执行像我们在`Dockerfile`中最初创建的`example.py`文件这样的现有 python 文件。我们甚至可以进入 python 控制台并运行简单命令。然而，当我们切换到非 root 用户时，其他系统写入权限已被移除，因此你会看到我们无法创建和修改脚本，或使用 python 执行写入命令。

![](img/a73482545f95e2efa3fc2e68d391bf71.png)

可以执行现有的 python 脚本，但其他操作则不可行。作者图片

虽然系统范围的限制对安全性有好处，但在许多情况下，特定文件和目录的写入权限是必需的，我们需要考虑这些许可。

在接下来的章节中，我将给出机器学习操作生命周期中的四种场景的示例。通过这些示例，可以了解如何实现大多数其他情况。

# 四种场景

## 1) 从主机服务模型 — 只读

服务模型时，它涉及到一个推理和服务脚本，用于加载模型并通过 API（例如 Flask、FastAPI）暴露它以接受输入。模型有时从主机加载，并与镜像分开，以使镜像大小尽可能小，任何重新加载镜像都将尽可能快速而不重复下载模型。然后，模型通过一个[bind-mount](https://docs.docker.com/storage/bind-mounts/)卷传递到容器中，以便加载和服务。

![](img/d2429a66f49352779768b059e20b622d.png)

服务模型只需要读取权限。作者图片

这可能是实现非 root 用户的最简单方法，因为只需要读取权限，而所有用户默认都被授予此权限。下面是一个如何完成的示例`Dockerfile`。

```py
# Dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip3 install --no-cache-dir --upgrade pip~=23.2.1 \
    && pip3 install --no-cache-dir -r requirements.txt

COPY ./project/ /app

# add non-root user ---------------------
RUN adduser --no-create-home nonroot
# switch from root to non-root user -----
USER nonroot

CMD ["python", "inference.py"]
```

它有两个简单的命令，首先创建一个名为`nonroot`的新系统用户。其次，在最后一行`CMD`之前，从 root 切换到`nonroot`用户。这一点很重要，因为默认的非 root 用户没有任何写入和执行权限，因此不能安装、复制或操作在早期步骤中所需的文件。

现在我们知道如何在 Docker 中分配非 root 用户，接下来我们进入下一步。

## **2) 运行数据处理管道 — 在容器内写入**

有时，我们只是想存储临时文件以执行一些任务，例如，一些数据预处理工作。这包括添加和删除文件。我们可以在容器内完成这些任务，因为文件不是持久的。

![](img/9c3a138c2b9fa01285a44f72be81bbae.png)

写入文件。作者图片

如果我们使用非 root 用户，我们需要写入权限。为此，我们需要使用命令`chown`（更改所有者）并将特定文件夹的所有权分配给`nonroot`用户。完成后，我们可以切换到`nonroot`用户。

```py
# Dockerfile

# ....

# add non-root user & grant ownership to processing folder
RUN adduser --no-create-home nonroot && \
    mkdir processing && \
    chown nonroot processing

# switch from root to non-root user
USER nonroot

CMD ["python", "preprocess.py"]
```

## 3) 库自动写入文件 — 在容器内写入

之前的示例展示了如何写入我们自己创建的文件。然而，库自动创建文件和目录也是很常见的。你只有在尝试运行容器时才会知道它们已被创建，并且被拒绝写入权限。

我将给你展示两个这样的示例，一个来自`supervisor`，用于管理多个进程，另一个来自`huggingface-hub`，用于从 huggingface 下载模型。如果我们切换到非 root 用户，将会看到类似的权限错误。

![](img/17a5f1d2032ec06efbec72679a83a162.png)

**Supervisor** 阻止了创建日志文件。作者截图。

![](img/d3417206526641e6649a202d416e4b65.png)

**Supervisor** 阻止了将 PID 存储在文件中。作者截图。

![](img/c337cb2e7554b0f0f10d4b048a734c1b.png)

**Huggingface Hub** 阻止了下载模型文件。作者截图。

对于两个`supervisor`文件，我们可以先将它们创建为空文件，并赋予所有权。对于`huggingface-hub`的下载问题，错误日志中已提示我们可以通过`TRANSFORMERS_CACHE`变量更改下载目录，因此我们可以先设置目录变量，创建目录，然后分配所有权。

```py
# Dockerfile

# ....

# add non-root user ................
# change huggingface dl dir
ENV TRANSFORMERS_CACHE=/app/model

RUN adduser --no-create-home nonroot && \
    # create supervisor files & huggingfacehub dir
    touch /app/supervisord.log /app/supervisord.pid && \
    mkdir $TRANSFORMERS_CACHE && \
    # grant supervisor & huggingfacehub write access
    chown nonroot /app/supervisord.log && \
    chown nonroot /app/supervisord.pid && \
    chown nonroot $TRANSFORMERS_CACHE
USER nonroot

CMD ["supervisord", "-c", "conf/supervisord.conf"]
```

当然，还会有其他示例可能与我这里展示的略有不同，但允许最少写入权限的概念将是相同的。

## 4) 保存训练好的模型 — 写入主机

假设我们使用容器训练模型，并希望将该模型写入主机，例如，以便被另一个任务用于基准测试或部署。在这种情况下，我们需要通过将容器目录链接到主机目录来写入模型文件，这也称为绑定挂载。

![](img/3df3be7a331f921dfa24f3ec31b6f674.png)

将模型文件写入主机。作者图片

首先，我们需要为`nonroot`创建一个组和用户，并为每个指定唯一的 ID，在此情况下，我们使用`1001`（1000 以上的任意数字都可以）。然后，创建一个模型目录来存储模型。

与情境 2 相比的不同之处在于，模型目录的写入不需要`chown`。为什么？

```py
# Dockerfile

# ....
# add non-root group/user & create model folder
ENV UID=1001
RUN addgroup --gid $UID nonroot && \
    adduser --uid $UID --gid $UID --no-create-home nonroot && \
    mkdir model

# switch from root to non-root user
USER nonroot

CMD ["python", "train.py"]
```

这是因为**绑定挂载目录的权限由主机目录决定**。因此，我们需要在主机中再次创建相同的用户，确保用户 ID 相同。然后在主机中创建模型目录，并将`nonroot`用户授予所有者权限。

```py
# in host terminal

# add the same user & group
addgroup --gid 1001
adduser --uid 1001 --gid 1001 --no-create-home nonroot
# create model dir to bind-mount & make nonroot an owner
mkdir /home/model
chown nonroot /home/model
```

绑定挂载通常在`docker-compose.yml`文件或`docker run`命令中指定，以启用更多灵活性。以下是前者的一个示例。

```py
version: "3.5"

services:
    modeltraining:
        container_name: modeltraining
        build:
            dockerfile: Dockerfile
        volumes:
            - type: bind
              source: /home/model # host dir
              target: /app/model  # container dir
```

对于后者：

```py
docker run -d --name modeltraining -v /home/model:/app/model <image_name>
```

运行其中之一，你会看到你的非根用户可以毫无问题地执行脚本。

# 摘要

我们已经看到如何分配非根用户并仍然使容器完成其所需任务。这主要在需要特定写入权限时相关。我们只需了解两个基本概念。

+   在容器中进行写入权限的设置，可以使用`Dockerfile`中的`chown`。

+   对于绑定挂载的写入权限，请在主机中创建相同的非根用户，并在主机目录中使用`chown`。

如果你需要进入 Docker 容器并以 root 用户身份运行一些测试，我们可以使用以下命令。

```py
docker exec -it -u 0 <container_id/name> bash
```

# 参考文献

+   [1] Wong et al. (2023) [关于容器的安全性：威胁建模、攻击分析和缓解策略](https://www.sciencedirect.com/science/article/abs/pii/S0167404823000500)。*Computers & Security*，第 128 卷。

+   [2] 我之前关于 Linux 权限和访问控制的文章：[`medium.com/@teosiyang/securing-linux-servers-with-two-commands-de5b565dc104`](https://medium.com/@teosiyang/securing-linux-servers-with-two-commands-de5b565dc104)
