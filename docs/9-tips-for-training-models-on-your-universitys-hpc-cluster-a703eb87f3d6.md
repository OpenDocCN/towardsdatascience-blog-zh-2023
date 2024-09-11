# 关于如何在大学的 HPC 集群上训练模型的 9 个技巧

> 原文：[`towardsdatascience.com/9-tips-for-training-models-on-your-universitys-hpc-cluster-a703eb87f3d6?source=collection_archive---------9-----------------------#2023-03-28`](https://towardsdatascience.com/9-tips-for-training-models-on-your-universitys-hpc-cluster-a703eb87f3d6?source=collection_archive---------9-----------------------#2023-03-28)

## 如何在资源受限的环境中有效运行和调试代码

[](https://conorosullyds.medium.com/?source=post_page-----a703eb87f3d6--------------------------------)![Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----a703eb87f3d6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a703eb87f3d6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a703eb87f3d6--------------------------------) [Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----a703eb87f3d6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ae48256fb37&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F9-tips-for-training-models-on-your-universitys-hpc-cluster-a703eb87f3d6&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=post_page-4ae48256fb37----a703eb87f3d6---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a703eb87f3d6--------------------------------) ·6 分钟阅读·2023 年 3 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa703eb87f3d6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F9-tips-for-training-models-on-your-universitys-hpc-cluster-a703eb87f3d6&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=-----a703eb87f3d6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa703eb87f3d6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F9-tips-for-training-models-on-your-universitys-hpc-cluster-a703eb87f3d6&source=-----a703eb87f3d6---------------------bookmark_footer-----------)![](img/c5fc4bc25739d8d8186821bd8e9d602a.png)

图片由 [Martijn Baudoin](https://unsplash.com/ja/@martijnbaudoin?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

队列作业，等待 24 小时，**cuda runtime error:** 内存不足

队列作业，等待 24 小时，**FileNotFoundError:** 没有这样的文件或目录

队列作业，等待 24 小时，**RuntimeError:** 堆栈期望每个张量…

啊啊啊啊啊啊!!!

在高性能计算（HPC）集群上调试代码可能非常令人沮丧。更糟糕的是，在大学里你需要和其他学生共享资源。作业会被加入到队列中。你可能要等上几个小时才知道你的代码是否有错误。

我最近在大学的 HPC 上训练模型。我已经学到了一些东西（都是通过艰难的方式）。我想分享这些技巧，希望能让你的体验顺利一些。我会保持通用，这样你可以将它们应用于任何系统。

## 尽可能在个人计算机上进行开发

一个 HPC 集群有一个目标——处理数据。没有华丽的 IDE，没有调试器，也没有副驾驶。你真的想用 vim 来编写整个项目吗？
