# 使用 Hydra 跟踪你的实验

> 原文：[`towardsdatascience.com/keep-track-of-your-experiments-with-hydra-b29937a99fc9?source=collection_archive---------14-----------------------#2023-08-01`](https://towardsdatascience.com/keep-track-of-your-experiments-with-hydra-b29937a99fc9?source=collection_archive---------14-----------------------#2023-08-01)

![](img/01cd12756484fac696d8c911c15cd612.png)

(作者提供的图片)

## 使用 YAML 文件配置超参数，提升你的研究效率！

[](https://medium.com/@marcellopoliti?source=post_page-----b29937a99fc9--------------------------------)![Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----b29937a99fc9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b29937a99fc9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b29937a99fc9--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----b29937a99fc9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7390355d40fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fkeep-track-of-your-experiments-with-hydra-b29937a99fc9&user=Marcello+Politi&userId=7390355d40fe&source=post_page-7390355d40fe----b29937a99fc9---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b29937a99fc9--------------------------------) · 5 min read · 2023 年 8 月 1 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb29937a99fc9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fkeep-track-of-your-experiments-with-hydra-b29937a99fc9&user=Marcello+Politi&userId=7390355d40fe&source=-----b29937a99fc9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb29937a99fc9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fkeep-track-of-your-experiments-with-hydra-b29937a99fc9&source=-----b29937a99fc9---------------------bookmark_footer-----------)

## 介绍

就像在第一次尝试时无法编写没有错误的代码一样，在第一次尝试时也不可能训练出合适的模型。

有一定机器学习和深度学习经验的人都知道，你经常需要**花费大量时间选择正确的模型超参数**。这些超参数例如学习率、批次大小和输出类别数量，但这些只是最常见的一些，一个项目可能会有数百个这样的参数。

通过改变超参数，我们可能会得到不同的结果（更好或更差），而在某些时候**跟踪所有的测试非常困难**。

我很长一段时间所做的是：我会手动将所有这些超参数写在 Excel 表格中，并在旁边写下每个实验的结果，例如损失值。后来我“进化”了，开始编写超参数的配置文件，在其中放入我想要测试的各种值。我会编写自定义的 Python 函数来读取这些值并将它们传递到训练函数中。YAML 文件基本上是…
