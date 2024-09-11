# 关于Dask数据框分区大小的几乎所有信息

> 原文：[https://towardsdatascience.com/almost-everything-you-want-to-know-about-partition-size-of-dask-dataframes-ac1b136d7674?source=collection_archive---------11-----------------------#2023-12-01](https://towardsdatascience.com/almost-everything-you-want-to-know-about-partition-size-of-dask-dataframes-ac1b136d7674?source=collection_archive---------11-----------------------#2023-12-01)

## **以及如何在XGBoost模型中有效利用它**

[](https://medium.com/@mik.sarafanov?source=post_page-----ac1b136d7674--------------------------------)[![Mikhail Sarafanov](../Images/88869a3c6f664785c90539dd7aab6d74.png)](https://medium.com/@mik.sarafanov?source=post_page-----ac1b136d7674--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ac1b136d7674--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ac1b136d7674--------------------------------) [Mikhail Sarafanov](https://medium.com/@mik.sarafanov?source=post_page-----ac1b136d7674--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F209c78c40898&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Falmost-everything-you-want-to-know-about-partition-size-of-dask-dataframes-ac1b136d7674&user=Mikhail+Sarafanov&userId=209c78c40898&source=post_page-209c78c40898----ac1b136d7674---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ac1b136d7674--------------------------------) ·7 min read·2023年12月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fac1b136d7674&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Falmost-everything-you-want-to-know-about-partition-size-of-dask-dataframes-ac1b136d7674&user=Mikhail+Sarafanov&userId=209c78c40898&source=-----ac1b136d7674---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fac1b136d7674&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Falmost-everything-you-want-to-know-about-partition-size-of-dask-dataframes-ac1b136d7674&source=-----ac1b136d7674---------------------bookmark_footer-----------)![](../Images/6162b6b47f392693f9bcc37cc103e84f.png)

预览图（作者提供）

最近，我和我的同事们一直在处理一个使用 [Xgboost](https://xgboost.readthedocs.io/en/stable/tutorials/dask.html) 机器学习模型和 Dask 作为分布式数据处理和预测生成工具的大型高负载服务。在这里，我想分享我们如何最大化利用 Dask 进行数据准备和机器学习模型拟合的发现。

## **什么是Dask？**

[Dask](https://www.dask.org/)是一个用于大规模数据分布式处理的库。其基本概念是将大型数组划分为小部分（分区）。

这就是Dask Dataframes如何存储和处理：表可以被拆分成小的数据帧（可以将其视作pandas DataFrames），这样无需将整个表存储在RAM中。整个源表可能太大而无法加载到内存中，但单个分区可以。此外，这种数据存储允许有效利用多个处理器核心来并行计算。

与此同时，这些分区（块）的大小由开发者确定。因此，相同的数据帧可以使用例如‘Split 1’或‘Split 2’（见图1）分成几个分区。

![](../Images/541332b83eae5329024d552cd75a1d85.png)

图1\. 将Dask数据帧拆分为分区（图片由作者提供）

选择最佳的分区大小至关重要，因为如果分区大小不佳，数据处理可能会变慢。最佳分区大小取决于整体数据集的大小以及服务器（或笔记本电脑）的资源——CPU数量和可用RAM。

**免责声明：** 为了方便起见，接下来我们将通过行数来衡量数据集的大小。所有表将由4列组成（3个特征+1个目标）。在系统中实现算法时，我们构建的所有依赖项不是基于表中的行数，而是基于元素的总数（行数 x 列数）。

## **问题**

Dask可以用于计算简单统计数据和聚合，但Dask也可以用于训练大型机器学习模型（使用大量数据）。例如，XGBoost。由于我们正在开发的服务可能需要我们在仅有8-16 GB RAM（在小型虚拟机情况下）下对2-10百万条记录进行模型训练，因此我们决定进行实验。

即使是在计算简单统计数据的情况下，分区的大小也非常重要，因为它在两种情况下可能显著减慢计算算法：

+   **分区过大**，导致在RAM中处理这些分区需要耗费过多的时间和资源

+   **分区过小**，以至于Dask需要过于频繁地将这些表加载到RAM中——更多的时间花在同步和上传/下载上，而不是在计算本身上

因此，使用相同的计算资源选择非最佳的分区大小会显著降低程序的性能（见图2）。图2显示了在不同分区大小的Dask数据帧上拟合XGBoost模型的时间。显示的是5次运行的平均执行时间。

![](../Images/e211905900d041620463667f5c46022c.png)

图2\. 分区大小对XGBoost模型拟合速度的影响。这些实验的原始数据帧包含500,000行和4列（图片由作者提供）

本文讨论了**Dask数据框分区的最佳大小算法**。本文中展示的所有表格都用于拟合Dask Xgboost机器学习模型。我们还将分享一些您可能会觉得有用的技巧。

## **文档提示**

在官方的Dask文档中，有关于如何正确处理Dask对象（数据框、数组等）的网页，例如[最佳Dask数据框实践](https://docs.dask.org/en/stable/dataframe-best-practices.html)。

在此页面上，您可以看到以下建议：

> 目标应该是每个分区的数据量大约为100MB。

然而，这些建议比较粗略，并未考虑服务器的计算规格、源数据集的大小以及问题解决的具体情况。

## **实验设置**

如上所述，假定最佳分区大小取决于以下三个条件：

+   全数据集的大小；

+   XGBoost和Dask可以使用的CPU资源（进程数）；

+   可用的随机存取内存（RAM）。

因此，在实验过程中，计算资源的数量以及源数据集的大小均有所变化。考虑的情况：

+   分区大小，千行：**[5, 10, 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 1000]**（13种情况）

+   全数据集的大小，千行：**[100, 200, 300, 400, 500, 600, 1000, 2000, 3000, 4000]**（10种情况）

+   CPU资源（工作者）：**[2, 3, 4]**（3种情况）

+   每个工作者可用的随机存取内存（RAM）：**[1GB, 2GB, 4GB]**（3种情况）

注：[Dask中的工作者](https://distributed.dask.org/en/latest/worker.html)是计算机（服务器）上的一个进程，它使用分配给它的计算资源，并在与其他工作者隔离并行运行。

因此，分析了1170（13 x 10 x 3 x 3）种情况。**为了获得更稳健的执行时间估计，每种情况被运行了5次。然后对指标（执行时间）进行了平均。**

在调查过程中，我们希望了解数据集大小的限制，以便虚拟机无法处理负载，从而更成功地扩展服务。

## **结果**

首先，考虑实验的一般可视化结果。我们进行了不同CPU核心数量和RAM容量的运行，同时改变了原始数据集的大小和分区的大小。完成实验后，我们准备了一张表格，仅显示最佳解决方案（分区大小）。最佳分区大小是指在给定条件（RAM、CPU和源数据集大小）下，执行时间最短的大小。收集到的指标的相关性矩阵见图3。

![](../Images/9b73c7691d69963f279eca714bac9648.png)

图3\. 实验结果的相关性矩阵（作者提供的图像）

从图表中可以看出，对执行时间影响最大的是源数据集的大小。工作线程的数量和内存量对拟合时间也有显著影响。块大小的影响相对较弱。然而，这可能是因为执行时间与分区大小之间的依赖关系是非线性的，这一点从图 2 的曲线中得到了确认。此外，图 4 证实了测量结果是正确的，因为结果与我们的预期一致。

让我们来看一下带有 3d 图表的动画（动画 1）。

![](../Images/8c3609781e62bf3d33f1cda67ad40768.png)

动画 1\. 不同测试案例的图表，其中每一帧表示源数据集的固定大小。每种情况的最佳表面显示在图中（由作者提供）

在动画中，最佳（对于每种进程数量和每个工作线程的内存组合）情况用红色圈出。即显示了在给定数据集大小、核心数量、内存和分区大小下算法执行时间最小的条件。图表还显示了灰色的分段常数最佳表面（注意：表面对于所有情况都是全局的）。

从动画中可以看出，在某些帧中没有实验数据（没有点）（图 4）。这意味着所提议的计算资源不足以运行模型。

![](../Images/b5e51a0290a96c89d73ba7bb83ab086a.png)

图 4\. 对一个包含 400 万行的数据集的建模结果。对于小于 4 的内存没有结果（图片由作者提供）

从图中可以观察到，对于这种数据集大小，**如果核心数量较少，则应形成较大的分区**。请注意，这种依赖关系并不适用于所有情况。

基于计算资源不足的启动结果，准备了以下可视化（图 5）。

![](../Images/2bca7d7b9d8d34fbcae10ea0f6643511.png)

图 5\. 数据量的限制，超过此限制则无法开始模型拟合。对象数量的计算方法是将表格中的行数乘以列数（图片由作者提供）

此外，根据失败运行收集的统计数据得出结论（提示）：如果内存有限，使用较小的分区大小更可靠。

## **讨论**

根据这项研究，形成了针对 Dask XGBoost 模型系统更有效配置的若干提示。请注意，本研究的目的是为了在相对较小的服务器上更高效地运行 Dask（这些服务器没有数百吉字节的内存和几十个 CPU）。

实验揭示了最佳超平面。它们通过[高斯过程](https://scikit-learn.org/stable/modules/generated/sklearn.gaussian_process.GaussianProcessRegressor.html#sklearn.gaussian_process.GaussianProcessRegressor)进行建模。基于这一算法，最佳分区大小会被自动选择（动画 2）。

![](../Images/18d2a6daae39d15d2cd674a3ae13c6f5.png)

动画 2\. 不同条件下的最佳面（CPU、RAM 和源数据集大小）。每一帧显示特定源数据集大小的条件（作者提供）

从动画中可以看出，**当源数据集中的行数增加时，最佳分区大小通常会减少**。

## **结论（和建议）**

我希望你对了解哪个大小的分区被证明是训练 XGBoost 模型的最佳大小感兴趣。

我意识到这篇文章变得非常“技术化”。因此，对于那些读到最后的人，我将提供一些建议：

+   如果你正在测量执行时间，始终运行多次计算并取结果的平均值，因为运行时间是随机的；

+   如果你对选择什么大小的分区感到困惑，最好犯一个较小的错误（否则算法不仅会运行很长时间，还可能因错误而崩溃）；

+   在 Dask 中本地初始化集群时，使用 *cluster = LocalCluster()* 和 *Client(cluster)* [命令](https://docs.dask.org/en/stable/deploying-python.html)。我们强烈建议仅在代码中使用单例模式初始化这样的集群。你可以在 [这里](https://stackoverflow.com/questions/6760685/creating-a-singleton-in-python) 查看如何在 Python 中实现它。否则，每次启动时你都会初始化一个新集群；

+   平均来说，当源数据集中的行数增加时，最佳分区大小会减少

关于 Dask 和机器学习的故事由 [Mikhail Sarafanov](https://github.com/Dreamlone) 和 [Wiredhut 团队](https://wiredhut.com/) 提供
