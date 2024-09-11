# 使用 Python Pandas 清理混乱的汽车数据集

> 原文：[`towardsdatascience.com/cleaning-a-messy-car-dataset-with-python-pandas-700fe10a7180?source=collection_archive---------4-----------------------#2023-10-17`](https://towardsdatascience.com/cleaning-a-messy-car-dataset-with-python-pandas-700fe10a7180?source=collection_archive---------4-----------------------#2023-10-17)

## 无论你是在进行探索性数据分析还是构建复杂的机器学习系统，你都需要确保数据已被清理

[](https://sonery.medium.com/?source=post_page-----700fe10a7180--------------------------------)![Soner Yıldırım](https://sonery.medium.com/?source=post_page-----700fe10a7180--------------------------------)[](https://towardsdatascience.com/?source=post_page-----700fe10a7180--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----700fe10a7180--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----700fe10a7180--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcleaning-a-messy-car-dataset-with-python-pandas-700fe10a7180&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----700fe10a7180---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----700fe10a7180--------------------------------) ·7 分钟阅读·2023 年 10 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F700fe10a7180&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcleaning-a-messy-car-dataset-with-python-pandas-700fe10a7180&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=-----700fe10a7180---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F700fe10a7180&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcleaning-a-messy-car-dataset-with-python-pandas-700fe10a7180&source=-----700fe10a7180---------------------bookmark_footer-----------)![](img/4d84b2673b18c91810618f7530f6b299.png)

（图像由作者使用 Midjourney 创建）

网络是一个极其宝贵的数据来源。例如，用于创建大型语言模型的大量训练数据来自网络。

然而，数据通常不是最适合的格式。网络数据主要是非结构化的（即以自由文本的形式存在）。即使它有预定义的结构，网络数据在用于分析之前也需要大量的清理和预处理。

在本文中，我们将使用包含汽车价格及其他一些属性的混乱数据集，并利用 pandas 库对其进行清理。

如果你想跟着一起操作并执行代码，可以从我的 [datasets](https://github.com/SonerYldrm/datasets) 仓库中下载数据集。它叫做“mock_car_dataset”。我们将在这个混乱的数据集上执行的一些操作如下：

+   字符串操作

+   处理数据类型

+   基于字符串进行过滤

+   替换值

+   使用其他列更新列值

+   格式化数值数据
