# Linux 控制组的威力：容器如何控制其资源

> 原文：[`towardsdatascience.com/the-power-of-linux-cgroups-how-containers-take-control-of-their-resources-ba564fef13b0`](https://towardsdatascience.com/the-power-of-linux-cgroups-how-containers-take-control-of-their-resources-ba564fef13b0)

## 使用 Linux 控制组优化容器资源分配

[](https://dpoulopoulos.medium.com/?source=post_page-----ba564fef13b0--------------------------------)![Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----ba564fef13b0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ba564fef13b0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ba564fef13b0--------------------------------) [Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----ba564fef13b0--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----ba564fef13b0--------------------------------) ·阅读时间 8 分钟·2023 年 1 月 10 日

--

![](img/b0e5159a60f501ca4b4aa41481bdd327.png)

照片由[Joshua Hoehne](https://unsplash.com/@mrthetrain?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

上一篇文章探讨了如何使用 Linux 命名空间在单一 Linux 系统内创建隔离环境。本文是我们深入了解容器如何工作的努力的一部分，旨在揭示其背后的机制。

[](/containers-how-they-work-under-the-hood-and-why-theyre-taking-over-the-data-science-world-6b94702609aa?source=post_page-----ba564fef13b0--------------------------------) ## 容器：它们如何在幕后工作以及为何它们正在主导数据科学领域

### 初学者理解 Docker 魔法的指南

towardsdatascience.com

命名空间是我们旅程的第一步。我们看到如何创建一个`PID`命名空间，创建一个其中运行的进程假设自己是唯一存在的世界，但如何对它们可以消耗的资源数量施加限制呢？这就要引入 Linux cgroups 了。

Linux 控制组，或称 cgroups，是一种强大的工具，用于管理和分配 Linux 系统中的资源。它们允许管理员限制进程或进程组使用的资源，确保关键系统服务始终能够获得其正常运行所需的资源。

但 cgroups 不仅对系统管理员有用——它们还为容器提供了一种控制自身资源的方法，使其在共享主机环境中更高效、更可靠地运行。

在本文中，我们将探讨在容器上下文中使用 cgroups 的好处，并展示如何在自己的环境中开始使用 cgroups。首先，我们将创建一个控制组，以限制在其上下文中运行的进程的内存消耗，然后在其下运行一个完整的命名空间。让我们深入了解吧！

> [学习速率](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=cgroups) 是一份面向那些对 MLOps 世界充满好奇的人的新闻通讯。MLOps 是一个广泛的领域，致力于以高效和可重现的方式将 ML 模型投入生产。容器在这个流程中发挥着关键作用。如果你想了解更多类似的主题，请[点击这里订阅](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=cgroups)。每个月的第一个星期六，你将收到我关于最新 MLOps 新闻和文章的更新和见解！

# 什么是 Cgroups？

Linux 控制组或 cgroups 是一种内核特性，允许管理员将 CPU、内存和 I/O 带宽等资源分配给进程组。

Cgroups 提供了一种控制系统资源的使用量的方法。例如，管理员可以为与特定应用程序相关的进程组创建一个 cgroup（例如，运行在服务器上的 web 应用程序），然后设置这些进程允许使用的 CPU 和内存的限制。

Cgroups 对于各种目的都很有用，包括提高系统性能、隔离进程以提高安全性，以及简化在单个系统上管理多个应用程序。

在容器上下文中，cgroups 允许我们限制每个容器可以消耗的资源，以便我们的应用程序不会占用整个服务器，或者确保它拥有运行所需的资源。

例如，设置 Kubernetes 中 pod 的资源部分总是一个好主意，因为它帮助 Kubernetes 调度器决定将 pod 调度到哪个节点。这确保了我们的应用程序将拥有运行所需的一切。如果没有，应用程序甚至无法启动。

我们如何在实际中使用 cgroups？通过创建两个简单的示例来了解：首先，我们将创建一个简单的应用程序，并在特定的 cgroup 下运行它，然后在类似的上下文中运行一个完整的命名空间。

# 一个简单的示例

我们在 cgroups 世界的旅程从这里开始。首先，我们将创建一个简单的应用程序，并在内存限制的 cgroup 中运行它。然后，我们将在相同的上下文中创建一个 Linux 命名空间，并在该命名空间中运行相同的应用程序。

## Memhog

为了使这个示例有效，我们需要控制应用程序使用的内存量。为此，我们将使用一个名为 `memhog` 的 Debian 软件包。

`memhog` 是一个简单的软件包，用于分配我们指定的内存以供测试（有关更多详细信息，请参见 [这里](https://man7.org/linux/man-pages/man8/memhog.8.html) 的手册页）。

第一步是安装 `memhog`：

```py
sudo apt update && sudo apt install numactl
```

测试你是否正确安装了 `memhog`，让它分配 100 兆字节的内存：

```py
memhog 100M
```

输出应该类似于以下内容：

```py
..........
```

如果你得到了这个输出，那么你可以继续！让我们创建一个每 2 秒运行一次的 bash 脚本。因此，我们的应用程序是一个每两秒请求 100 兆字节的服务。创建一个新文件，命名为 `memhog.sh` 并将以下内容放入其中：

```py
#!/bin/bash
while true; do memhog 100M; sleep 2; done
```

最后，使文件可执行：

```py
sudo chmod +x memhog.sh
```

# 创建一个 Cgroup

要创建、管理和监控 cgroup，我们需要另一个名为 `cgroup-tools` 的软件包。所以，首先，你需要安装它：

```py
sudo apt install cgroup-tools
```

现在我们在工具箱中有了这个，过程如下：

1.  使用我们刚刚安装的软件包创建一个新的 cgroup

1.  为我们想要控制的资源在这个特定的 cgroup 中设置一个限制

1.  在这个 cgroup 下运行应用程序或命名空间

因此，让我们首先创建 cgroup。为此，请使用以下命令：

```py
sudo cgcreate -g memory:memhog-limiter
```

这个命令创建了一个新的 cgroup（`cgcreate`），类型为内存，并将其名称设置为 `memhog-limiter`。这个命令实际上做的是在 `/sys/fs/cgroup/memory` 下创建了一个新目录，你可以通过运行 `ls` 来查看其内容：

```py
ls -la /sys/fs/cgroup/memory/memhog-limiter/
drwxr-xr-x 2 root root 0 Jan  9 05:56 .
dr-xr-xr-x 8 root root 0 Jan  4 10:31 ..
-rw-r--r-- 1 root root 0 Jan  9 05:56 cgroup.clone_children
--w--w--w- 1 root root 0 Jan  9 05:56 cgroup.event_control
-rw-r--r-- 1 root root 0 Jan  9 05:56 cgroup.procs
-rw-r--r-- 1 root root 0 Jan  9 05:56 memory.failcnt
--w------- 1 root root 0 Jan  9 05:56 memory.force_empty
-rw-r--r-- 1 root root 0 Jan  9 05:56 memory.kmem.failcnt
-rw-r--r-- 1 root root 0 Jan  9 05:56 memory.kmem.limit_in_bytes
-rw-r--r-- 1 root root 0 Jan  9 05:56 memory.kmem.max_usage_in_bytes
-r--r--r-- 1 root root 0 Jan  9 05:56 memory.kmem.slabinfo
-rw-r--r-- 1 root root 0 Jan  9 05:56 memory.kmem.tcp.failcnt
-rw-r--r-- 1 root root 0 Jan  9 05:56 memory.kmem.tcp.limit_in_bytes
-rw-r--r-- 1 root root 0 Jan  9 05:56 memory.kmem.tcp.max_usage_in_bytes
-r--r--r-- 1 root root 0 Jan  9 05:56 memory.kmem.tcp.usage_in_bytes
-r--r--r-- 1 root root 0 Jan  9 05:56 memory.kmem.usage_in_bytes
-rw-r--r-- 1 root root 0 Jan  9 05:56 memory.limit_in_bytes
-rw-r--r-- 1 root root 0 Jan  9 05:56 memory.max_usage_in_bytes
-rw-r--r-- 1 root root 0 Jan  9 05:56 memory.move_charge_at_immigrate
-r--r--r-- 1 root root 0 Jan  9 05:56 memory.numa_stat
-rw-r--r-- 1 root root 0 Jan  9 05:56 memory.oom_control
---------- 1 root root 0 Jan  9 05:56 memory.pressure_level
-rw-r--r-- 1 root root 0 Jan  9 05:56 memory.soft_limit_in_bytes
-r--r--r-- 1 root root 0 Jan  9 05:56 memory.stat
-rw-r--r-- 1 root root 0 Jan  9 05:56 memory.swappiness
-r--r--r-- 1 root root 0 Jan  9 05:56 memory.usage_in_bytes
-rw-r--r-- 1 root root 0 Jan  9 05:56 memory.use_hierarchy
-rw-r--r-- 1 root root 0 Jan  9 05:56 notify_on_release
-rw-r--r-- 1 root root 0 Jan  9 05:56 tasks
```

（根据你的系统，目录的地点或结构可能会有所不同）

现在我们已经创建了 cgroup，让我们设置我们的限制。我们将设置 50 兆字节的限制，这意味着在这个上下文中运行的任何进程都不能超过这个限制。类似地，谈到进程组时，它们的总需求也不能超过这个限制。

要设置内存限制，请运行以下命令：

```py
sudo cgset -r memory.limit_in_bytes=50M memhog-limiter
```

这个命令为 cgroup `momhog-limiter` 设置了 50 兆字节的内存限制。如果你查看我们之前看到的目录结构中的相应文件，你会看到正是这个（以字节为单位）：

```py
cat /sys/fs/cgroup/memory/memhog-limiter/memory.limit_in_bytes 
52428800
```

我们准备在这个上下文中运行我们的应用程序和命名空间。

# 设定你的限制！

我们现在的状态如下：我们创建了一个每两秒分配 100 兆字节内存的服务。我们还创建了一个限制进程或一组进程可以消耗的内存为 50 兆字节的 cgroup。如果我们在这个上下文中尝试运行我们的服务，你期望会发生什么？

不再废话，要在这个上下文中执行服务，请运行下面的命令：

```py
sudo cgexec -g memory:memhog-limiter ./memhogtest.sh
```

结果正如我们预期的那样：每次它尝试运行时，Linux 内核都会终止服务：

```py
....../memhogtest.sh: line 2: 174662 Killed                  memhog 100M
....../memhogtest.sh: line 2: 174668 Killed                  memhog 100M
```

很好；这正是我们想看到的。但现在，我们如何在这个上下文中创建一个命名空间呢？这类似于容器的功能，因此如果我们实现了这一点，我们将朝着在没有 Docker 的情况下创建类似容器的环境的目标迈进，这也是本系列的终极目标。

在这个上下文中创建命名空间是相当简单的。以下命令可能对你来说很熟悉：

```py
sudo cgexec -g memory:memhog-limiter unshare -fp --mount-proc
```

当然，我们在之前使用了第一部分来在`memhog-limiter` cgroup 的上下文中运行服务（`sudo cgexec -g memory:memhog-limiter`）。此外，我们在文章中看到了命令的第二部分，我们讨论了命名空间（`unshare -fp — mount-proc`）：这是我们用来创建新的`PID`命名空间的命令。

因此，如果我们将所有内容结合起来，这个命令在我们的 cgroup 上下文中创建了一个新的`PID`命名空间。要验证你是否在新的命名空间中，请运行以下命令：

```py
# ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 08:00 pts/0    00:00:00 -bash
root          12       1  0 08:00 pts/0    00:00:00 ps -ef
```

正如你所看到的，在你的新命名空间中，只有`bash`作为`PID` 1 运行。因此，你现在启动的每个服务都将在我们的 cgroup 上下文中启动。让我们验证一下：

```py
# ./memhogtest.sh 
....../memhogtest.sh: line 2:    14 Killed                  memhog 100M
....../memhogtest.sh: line 2:    16 Killed                  memhog 100M
....../memhogtest.sh: line 2:    18 Killed                  memhog 100M
```

太好了！我们得到了和之前一样的输出。如果你想尝试一下，你可以减少`memhog`尝试分配的内存量，让它正常工作。无论如何，祝贺你！你离创建自己的 Linux 容器而无需 Docker 更进一步了！

# 结论

总之，Linux cgroups 是管理和分配 Linux 系统资源的强大工具。它们允许管理员限制进程或进程组可以使用的资源量，确保重要的系统服务始终能够获得其正常运行所需的资源。

在容器的上下文中，cgroups 提供了一种让容器控制自身资源的方式，使它们能够在共享的主机环境中更高效、更可靠地运行。

这个故事探讨了如何在实践中使用 cgroups，并让我们离最终目标更近了一步：在 Linux 中创建类似容器的环境而无需 Docker。下一站？覆盖文件系统！

# 关于作者

我的名字是[Dimitris Poulopoulos](https://www.dimpo.me/?utm_source=medium&utm_medium=article&utm_campaign=cgroups)，我是为[Arrikto](https://www.arrikto.com/)工作的机器学习工程师。我为欧洲委员会、Eurostat、IMF、欧洲中央银行、OECD 和宜家等主要客户设计并实施了人工智能和软件解决方案。

如果你对阅读更多关于机器学习、深度学习、数据科学和数据运维的帖子感兴趣，可以在[Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow)、[LinkedIn](https://www.linkedin.com/in/dpoulopoulos/)或在 Twitter 上关注[@james2pl](https://twitter.com/james2pl)。

表达的观点完全是我个人的，并不代表我雇主的观点或意见。
