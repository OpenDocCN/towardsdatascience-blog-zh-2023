# 我们是否应该虚拟化我们的数据科学系统——还是不虚拟化？

> 原文：[`towardsdatascience.com/should-we-be-virtualizing-our-data-science-systems-and-or-not-6cb69b4850f3`](https://towardsdatascience.com/should-we-be-virtualizing-our-data-science-systems-and-or-not-6cb69b4850f3)

![](img/5f30971c4f4c248f248ced63c885dacc.png)

作者当前的家庭实验室设置

## 导航虚拟化数据科学过程的优缺点可能很困难，但有些性能趋势无法忽视

[](https://medium.com/@willkeefe?source=post_page-----6cb69b4850f3--------------------------------)![Will Keefe](https://medium.com/@willkeefe?source=post_page-----6cb69b4850f3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6cb69b4850f3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6cb69b4850f3--------------------------------) [Will Keefe](https://medium.com/@willkeefe?source=post_page-----6cb69b4850f3--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6cb69b4850f3--------------------------------) ·阅读时间 12 分钟·2023 年 9 月 12 日

--

随着“巨量数据”的利用在各个行业解决问题变得越来越相关，家庭实验室和数据湖规模的数据存储库需要比以往更多的并行计算能力来提取、转换、加载和分析数据。在创建自己的家庭实验室时，是否在虚拟机上还是在硬件上原生创建并行化设置让我感到困惑，我难以找到性能比较。在本文中，我们将探讨每种设置的优缺点，并对每种方法的虚拟和原生性能及基准测试进行逐一对比。

# 介绍

许多并行计算集群包括多个节点，即指定处理集群中分配任务的计算机。管理这些节点可能是一个大麻烦，这也是为什么[数据工程如此有利可图](https://www.thinkful.com/blog/data-analyst-vs-data-engineer/#:~:text=Data%20Analysis%20or%20Data%20Engineering,hand%2C%20is%20%24112%2C288%20a%20year.)相比于它们的分析对手。通常，公司会管理整个集群的队列，这使得几乎不可能对单独的节点给予个别关注，因此，“高可用性”设置，如 Proxmox、Kubernetes 和 Docker Swarm，是现代企业的必备工具。你可能已经与这些集群互动过，甚至没有意识到这一点——我今天午餐吃的 Chick-fil-A 鸡肉三明治就是通过一个[边缘计算 Kubernetes 集群](https://medium.com/chick-fil-atech/enterprise-restaurant-compute-f5e2fd63d20f)与他们的销售点系统完成的。

在虚拟化机器上计算有许多好处，包括：

+   整个操作系统可以从企业服务器快速部署到现场，几乎是瞬时的

+   图像可以实时备份

+   部署可以容器化以限制范围并增加安全性

+   在硬件故障的情况下，系统可以在最小的停机时间内迁移

这些并不是新概念，但随着每个组织对数据分析需求的增加，访问并行化部署的方式可以并且应该有所不同，因为虚拟化的缺点通常是你离裸金属越远，你的系统性能受到的影响就越大。虽然一个开发者在处理一个 Excel 文件时可能不会受到影响，但在处理几 GB 甚至 TB 的数据时，需要仔细考虑如何以及何时使用虚拟工具，并建立考虑处理能力的设置。

# 设置我们的比较

为了验证这一点，我们可以比较使用 readily available 企业硬件的小型到中型组织的设置（我负担不起那些高级设备）。在我的家庭实验室中，我有一个由多个翻新的企业单元构建的计算集群。我在下面的一些文章中链接了如何构建此设置以及我的用途，但现在让我们比较虚拟系统和裸金属系统之间的性能，并特别测量虚拟化的影响。

## 关于启动自己的数据分析家庭实验室的完整指南

### 现在是启动你的数据科学家庭实验室以分析对你有用的数据的最佳时机，存储……

towardsdatascience.com [](https://betterprogramming.pub/building-python-distributed-machine-learning-models-for-derivatives-on-homelab-cluster-dd9cb1d97e23?source=post_page-----6cb69b4850f3--------------------------------) [## 在家庭实验室集群上使用 Python 构建分布式机器学习模型

### 使用我们自己设置的经济实惠的家庭实验室设备，设置并行和分布式机器非常简单……

[betterprogramming.pub](https://betterprogramming.pub/building-python-distributed-machine-learning-models-for-derivatives-on-homelab-cluster-dd9cb1d97e23?source=post_page-----6cb69b4850f3--------------------------------)

自从写了上述文章后，我稍微升级了我的设置，增加了六台配备 Intel Core i7–7700 处理器、32 GB DDR4–2400 RAM 和 256–512 GB SATA III SSD 的 HP EliteDesk 800 G3 Mini。我在一个拍卖网站上以约 80 美元一台的便宜价格购买了这些设备，并额外支付了大约 30–40 美元以将它们升级为新 RAM 和硬盘。处理器都是 65W 型号，配有 90W 电源。即使按照今天的标准，处理器也不容小觑，*超频达到 4Ghz* 和 4 核心，8 线程。

今天的比较中，我有两个节点并排放置。一个节点运行 Proxmox，这是一个优化用于虚拟化和部署的 Linux 操作系统，运行一个 Windows 10 Pro 虚拟机，另一个节点在裸机上运行 Windows 10 Pro。没有“正确”的操作系统可用，因为这严重依赖于个人喜欢使用的工具，但每个操作系统都有其优缺点。

## Proxmox

Proxmox 的一个优点是，它声称对基线处理器的影响很小。静止时，我们可以看到我在节点上部署的虚拟机的资源使用非常低。下面的截图捕捉了仅一个节点的性能摘要。我们可以看到在空闲时，CPU 的使用率仅为极小的百分比，这也与非常有限的功耗（和电费）相关。

![](img/871ce8cb966bab307db68ea7d631d1c8.png)

针对特定节点的仪表板 — 作者截图

一旦部署了来宾操作系统，情况就完全不同了。此时的资源利用几乎完全由虚拟机配置决定。

我从玩弄 Proxmox 中学到的一些笔记包括：

+   学习曲线相当陡峭。这是一个企业工具，虽然使用 Proxmox 的文档非常丰富，但你需要花费大量时间阅读文档、论坛，甚至 Reddit 线程。

+   从另一个积极的角度来看，解决问题时有大量的文档可供参考，而且围绕该平台建立的社区非常强大。

+   尽管 Proxmox 具有非常直观的 GUI，但解决问题仍需要一些“跳出框框”的思维。例如，一旦我创建了一个 Windows 虚拟机，并将其调整到我想要的标准，我不能像第一次启动镜像时那样轻松地将其“拖放”到另一个节点。我不得不通过将外部硬盘添加到我运行 Windows 的破旧笔记本电脑中来创建网络附加存储（NAS）（有些人可能还记得我在第一篇文章中提到的那台发光的笔记本电脑）。这个存储充当了中介和备份库，用于克隆和迁移我的虚拟机。

+   我不是一个很精通 Linux 的人。我知道，我知道，我确实应该深入研究一下，但多年来 Mac 和 Windows PC 的便利性使得我在尝试用 CLI 完成操作时会感到挣扎，而这些操作我通常可以通过点击来完成。

+   Proxmox 非常容易扩展。将第一个节点添加到集群或所谓的“数据中心”花了一些时间才弄明白，但添加其他节点则没有花费任何时间。一开始我能够通过完成分配静态 IP 地址等管理员任务，按照我的要求自定义每个节点。一旦掌握了技巧，部署虚拟机也只需几分钟。

+   这非常酷，我非常喜欢那个展示所有操作统计数据的仪表板，我在操作过程中会密切监控。下图中，我们可以看到仪表板不仅监控了一个节点的使用情况，还记录了其他节点的情况。能够在数据中心的节点之间灵活切换是巨大的，当在裸机上监控可能需要在实例之间远程操作时。

![](img/84d4b439d2ee079747066aedcf599366.png)

Proxmox 图形界面在家庭实验室 — 作者截屏

最终，回顾起来我想到的一个问题是，不管我花了多少时间来配置 Windows 虚拟机以使其完全按照我希望的方式运行（本周早些时候，我花了一整晚来配置嵌套虚拟化以使 Docker 能够运行），总是会有一个额外的障碍或瓶颈。

## 容器化

我甚至不会对 Docker 进行比较，因为我在虚拟机中尝试启动的容器（一个为每半年一次的大学朋友 Minecraft 夜晚准备的 Minecraft 服务器）甚至无法达到令人满意的性能水平（服务器无法跟上，且无法进行游戏）。虽然如果我打算使用嵌套虚拟化，我的周末计划略有受阻，但也有实际应用可能会受到影响。

我经常用于工作和娱乐的一个工具是 PyCaret，一个专注于集成机器学习模型的 Python 库。机器学习模型常常有处理器或架构特定的注意事项，比如 PyCaret 不适用于 M1 Macs，因为 ARM 架构的原因，Tensorflow 没有针对我使用的 Radeon 显卡进行优化，而 Autogluon 在我的 i7 Mac 上无法构建（我甚至不知道为什么）。因此，这些包通常被容器化成 Docker 应用程序以实现便携性。我还在研究本地化到 Docker 的 DynamoDB，以利用强大的 AWS NoSQL 架构，而无需支付云端相关的高额费用。时间和速度是这些工具的卖点，而嵌套虚拟化对 Docker 的影响是巨大的（至少在这些 PC 上）。实际上，性能下降在每一级虚拟化中都是递增的，[每一级的性能下降超过 10%](https://cloud.google.com/compute/docs/instances/nested-virtualization/overview)。

对于那些可能会指出这一点的人来说，一个额外的反思是，能够运行 Docker 的 LXCs（Linux 容器）是在主机操作系统上运行的，因此像 ML 模型这样的大型程序可能会导致内核崩溃，如内存交换失败，不仅会杀死容器，还会影响操作系统（而不仅仅是客操作系统）。因此，我甚至没有考虑在这里使用它们，尽管它们无疑是轻量级应用程序的有用工具。

即使没有测量，我们也可以看到尽可能接近裸金属的方式能提升工具的性能。然而，一些人能够在虚拟化环境中仍然实现出色的性能。例如，[AWS Nitro](https://aws.amazon.com/ec2/nitro/)就是该领域中的一个真正的差异化因素，它为亚马逊的大规模计算和数据仓储成功做出了贡献，尽管这需要巨大的成本，使得一些数据科学工具如 Sagemaker 的费用相当于我为每台桌面电脑在一个月内支付的费用来租用一台笔记本电脑。我们可以看到下面一个标准的 Sagemaker 工作室笔记本实例，每天使用八小时，一周五天，规格与我们的机器相似（甚至时钟速度有限），大约每月需 $75。总体来看，每个单元的成本可能在 $100–120 之间，升级后，功耗在待机状态下约为 10–15W，峰值为 65W。这大致相当于每月 $2–3 的电费。这与整年相比节省了近一个数量级。

![](img/e7d6cb4db1a37a0f4e28be339840d177.png)

Sagemaker 计算成本通过 AWS 的公共成本估算器 — 作者截图

话虽如此，我相信随着时间的推移以及更好、更快的硬件在消费者和二手市场上的出现，虚拟化性能与实际性能之间的差距会缩小。如果[英特尔](https://medium.com/u/fb610dd2569b?source=post_page-----6cb69b4850f3--------------------------------)想送我一台 i9–13900K 或[NVIDIA AI](https://medium.com/u/ab69c39a85e1?source=post_page-----6cb69b4850f3--------------------------------)送我一台 RTX 4090，我会很乐意进行测试并向大家汇报。与此同时，我将满足于我的 HP mini 电脑和 AWS 免费套餐来满足我的数据分析和虚拟化需求。

# 比较

为了实际比较虚拟性能与物理性能，我们将对 Windows 10 虚拟机和物理系统分别进行一般化的基准测试，然后进行 Python 性能测试。在这里我要说明的是，我为虚拟机和 PC 分配了相同数量的核心和 RAM（虽然这让我在虚拟机方面有点冒险，因为我曾经在分配“所有”核心时遇到过问题，因为这影响了宿主虚拟化程序的性能，导致系统故障）。

毫不拖延，以下是基于 userbenchmark.com 为虚拟机构建的 [基准测试](https://www.userbenchmark.com/) 快照，运行于本地。以下我们可以看到 VCPU 的性能远低于基线和平均水平，我们的 RAM 也仅稍微低于基线。这表明要么我们的 CPU 没有得到充分利用，要么在测试期间这些数学密集型操作中，托管虚拟机的 CPU 存在大量开销。具体的整数计算性能等指标见截图。

![](img/7a0deab40be12559b20236689bfd3650.png)

作者截图

整体表现不算特别好，尽管单位本身目前输出了一些 BTUs。

让我们通过运行一个简单的 Python 脚本作为基准来评估性能（请注意，由于 GIL，该脚本是单线程运行的）。下面的非科学 Python 脚本是我编写的，用于创建一个粗略的“速度计算”以比较相对性能。

在两个单元格中，我们首先进行：

1.  计算一个任意大的数字并将每个数字添加到列表中

1.  在循环中乘以越来越大的数字

每个测试都有时间限制，并且会重复执行，以建立相对性能的基线，用于比较处理速度和内存 IO。以下是代码片段，供你感兴趣时自行运行以进一步比较。

```py
def test1(n):
    l = []
    for i in range(n):
        l.append(i)
n = 100000
%timeit -r 5 -n 1000 test1(n)

def test2(n):
    for i in range(n):
        i * (i-1)
n = 100000
%timeit -r 5 -n 1000 test2(n)
```

![](img/a3fd2855e045d18be71e53f3225320dd.png)

在虚拟机的 Jupyter Notebook 实例中运行 Python — 作者截图

第一个脚本平均完成时间为 8.84ms，第二个脚本平均完成时间为 11.5ms。我们将很快在虚拟机和裸金属之间比较这些数据。

在运行我们的脚本后，我们可以看到相当一部分 RAM 在使用中，然而，CPU 的利用率几乎没有增加，不过如果尝试在多个线程中分配此任务，我会担心弹性带来的问题。12.5GB 的空闲内存是一个显著的开销，应通过进一步的研究进行优化。

![](img/6b73b18862810ddfb3df704818d14647.png)

作者的任务管理器截图

## 现在谈谈裸金属……

在实际硬件上原生运行 Windows 10 Pro，我们可以看到使用相同测试套件的性能基准显著提高，这与虚拟机上使用的测试套件相同。

![](img/64d03b0c4b44c9268be859b3ff1d0300.png)

作者截图

我们的处理器在 Windows 原生模式下的整数比较性能几乎是虚拟化模式的两倍，这使得虚拟机的表现大打折扣。我们现在更接近基准线，一般而言，处理过程的平均水平也有所提高。至于我们的 RAM，读写速度显著提升，当在单核上运行时，吞吐量几乎提高了 3 倍。直接在裸金属上运行对 IO 和处理速度确实产生了巨大的影响。

在运行我们的 Python 测试时，我们注意到性能有类似的跳跃。我们的测试脚本运行速度是虚拟化模式的两倍，测试 1 为 4.06 毫秒，测试 2 为 6.04 毫秒。这是虚拟机上原始测试速度的一半。

![](img/37490889ae6b482c59cd4fbf2caf59eb.png)

作者的裸金属 Jupyter Notebook 截图

在未虚拟化的情况下，我们还可以看到使用的 RAM 是虚拟机空闲时的一半。总的来说，这表明与运行虚拟机的相同硬件相比，裸金属运行可以显著改善处理和内存性能。

![](img/735a780de6f349549ee68641c1789404.png)

作者的任务管理器截图

每个团队独特的数据科学需求没有一刀切的解决方案。对于企业而言，花更多的钱用于虚拟分析工具可能更为合理，因为这些工具的安全性和性能可以得到严格监控。较小的公司也可以利用云工具，具体取决于它们的预算。然而，对于个人和小型研究团队来说，基于裸金属的构建可能是实现最佳性能的必要条件。

我用于构建项目和管道的策略不是专注于管理特定的主机和节点，而是保持一个新安装的 Windows（去除多余软件）的备份，其中预安装了我所需的所有内容——某些 Python 包、代码重新分发包、服务器连接等。项目的其余部分集中在一个代码库中，我可以在运行时将其复制并部署到节点进行处理。从 2010 年代中期开始，大多数计算机都具备的千兆连接速度足够快，可以传输数据科学库和包。因此，对高可用计算和操作系统正常运行时间的需求减少了，因为我大规模管理硬盘，不会在本地进行大规模更改。一些服务仍然需要主动管理，例如在 Docker 中运行的容器，但这些服务无论如何都需要相当活跃的管理，将其放在我的本地 Windows 10 专业版安装中，更符合我如何支配时间的方式，因为无论如何都会发生故障。

你怎么看？你在哪里运行你的代码？你倾向于使用哪些工具和平台来托管你的数据科学工作流？请在下面告诉我，或者随时在 [LinkedIn](https://www.linkedin.com/in/will-keefe-476016127/) 上与我建立联系！

对我使用的硬件感兴趣？查看我在 [www.willkeefe.com](http://www.willkeefe.com) 上的评论。
