# 如何将多个 CSV 文件加载到 Pandas DataFrame 中

> 原文：[https://towardsdatascience.com/load-multiple-csv-pandas-9c0c88c5adff?source=collection_archive---------9-----------------------#2023-01-31](https://towardsdatascience.com/load-multiple-csv-pandas-9c0c88c5adff?source=collection_archive---------9-----------------------#2023-01-31)

## 导入并合并多个 CSV 文件到一个 pandas DataFrame

[](https://gmyrianthous.medium.com/?source=post_page-----9c0c88c5adff--------------------------------)[![Giorgos Myrianthous](../Images/ff4b116e4fb9a095ce45eb064fde5af3.png)](https://gmyrianthous.medium.com/?source=post_page-----9c0c88c5adff--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9c0c88c5adff--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9c0c88c5adff--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----9c0c88c5adff--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fload-multiple-csv-pandas-9c0c88c5adff&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----9c0c88c5adff---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9c0c88c5adff--------------------------------) ·5 min 阅读·2023年1月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9c0c88c5adff&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fload-multiple-csv-pandas-9c0c88c5adff&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----9c0c88c5adff---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9c0c88c5adff&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fload-multiple-csv-pandas-9c0c88c5adff&source=-----9c0c88c5adff---------------------bookmark_footer-----------)![](../Images/649a65c5a7ba242fecd95611a28a9576.png)

图片由 [Daniel K Cheung](https://unsplash.com/@danielkcheung?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/photos/cPF2nlWcMY4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

CSV（逗号分隔值）是一种流行的文件格式，用于存储和交换数据。实际上，这种数据源常用于相对小量的数据。

`pandas` 是一个常用的 Python 包，它允许开发者处理和转换数据，作为分析和数据科学任务的一部分。然而，在执行任何任务之前，pandas 需要将所有数据加载到内存中。这意味着该包只能用于处理相对较小的数据量——虽然这取决于主机机器的能力，但对于普通机器，我们只能在内存中加载几 GB 的数据。

因此，将 CSV 文件加载到内存中，然后使用 pandas 处理数据，是一项非常常见的任务，因为这类文件通常包含的数据量可以被加载到内存中。

[**订阅数据管道**](https://thedatapipeline.substack.com/welcome)**，这是一个专注于数据工程的新闻通讯**

在这篇文章中，我们将展示如何将多个 CSV 文件加载到一个 pandas 数据框中。此外，我们还会展示如何在每条记录中识别源文件，以便我们可以判断哪些数据点属于特定的数据文件。
