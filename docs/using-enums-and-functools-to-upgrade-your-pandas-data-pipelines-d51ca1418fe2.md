# 使用枚举和`functools`升级你的Pandas数据管道

> 原文：[https://towardsdatascience.com/using-enums-and-functools-to-upgrade-your-pandas-data-pipelines-d51ca1418fe2?source=collection_archive---------4-----------------------#2023-06-09](https://towardsdatascience.com/using-enums-and-functools-to-upgrade-your-pandas-data-pipelines-d51ca1418fe2?source=collection_archive---------4-----------------------#2023-06-09)

## 编程

## 查看两个逐步示例来提升数据处理的编程效率

[](https://byrondolon.medium.com/?source=post_page-----d51ca1418fe2--------------------------------)[![Byron Dolon](../Images/9ff32138c7b1913be24cc7ab971752b0.png)](https://byrondolon.medium.com/?source=post_page-----d51ca1418fe2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d51ca1418fe2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d51ca1418fe2--------------------------------) [Byron Dolon](https://byrondolon.medium.com/?source=post_page-----d51ca1418fe2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6b5d063df5dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-enums-and-functools-to-upgrade-your-pandas-data-pipelines-d51ca1418fe2&user=Byron+Dolon&userId=6b5d063df5dd&source=post_page-6b5d063df5dd----d51ca1418fe2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d51ca1418fe2--------------------------------) ·12分钟阅读·2023年6月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd51ca1418fe2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-enums-and-functools-to-upgrade-your-pandas-data-pipelines-d51ca1418fe2&user=Byron+Dolon&userId=6b5d063df5dd&source=-----d51ca1418fe2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd51ca1418fe2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-enums-and-functools-to-upgrade-your-pandas-data-pipelines-d51ca1418fe2&source=-----d51ca1418fe2---------------------bookmark_footer-----------)![](../Images/4628e4600a99a70c1aab75fbd4986ddd.png)

图片使用得到了我的才华横溢的姐妹 [ohmintyartz](https://www.instagram.com/ohmintyartz/)

当你创建一个处理原始数据的管道时，你可能已经使用过Pandas。编写代码来筛选、分组和对数据进行计算只是构建数据管道和ETL过程的第一步。

在大规模处理数据时，除了这些，我们还应该编写**功能性**和**易于阅读和维护**的代码。

有很多不同的方法可以改善你现有的数据管道，例如[添加高效的日志记录](/why-you-need-to-write-dry-code-with-decorators-in-python-3930ea23f569)、[包括数据验证](/how-to-do-data-validation-on-your-data-on-pandas-with-pytest-d5dda51ad0e4)，甚至使用除了 Pandas 之外的新库，比如 PySpark 和 Polars。

此外，你还可以以不同的方式结构化你实际用于数据处理的代码。这意味着不是为了提高管道的性能，而是关注编写易于修改和迭代的代码。

在这篇文章中，让我们看看如何通过使用一些原生 Python 的两个简单示例来做到这一点，特别是通过使用**枚举**和**functools**。
