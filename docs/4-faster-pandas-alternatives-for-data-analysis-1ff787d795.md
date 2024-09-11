# 4 个更快的 Pandas 数据分析替代工具

> 原文：[https://towardsdatascience.com/4-faster-pandas-alternatives-for-data-analysis-1ff787d795?source=collection_archive---------8-----------------------#2023-02-09](https://towardsdatascience.com/4-faster-pandas-alternatives-for-data-analysis-1ff787d795?source=collection_archive---------8-----------------------#2023-02-09)

## 流行数据分析工具的性能基准测试

[](https://chengzhizhao.medium.com/?source=post_page-----1ff787d795--------------------------------)[![Chengzhi Zhao](../Images/186bba91822dbcc0f926426e56faf543.png)](https://chengzhizhao.medium.com/?source=post_page-----1ff787d795--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1ff787d795--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1ff787d795--------------------------------) [Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----1ff787d795--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff956c63a9571&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-faster-pandas-alternatives-for-data-analysis-1ff787d795&user=Chengzhi+Zhao&userId=f956c63a9571&source=post_page-f956c63a9571----1ff787d795---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1ff787d795--------------------------------) ·9分钟阅读·2023年2月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1ff787d795&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-faster-pandas-alternatives-for-data-analysis-1ff787d795&user=Chengzhi+Zhao&userId=f956c63a9571&source=-----1ff787d795---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1ff787d795&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-faster-pandas-alternatives-for-data-analysis-1ff787d795&source=-----1ff787d795---------------------bookmark_footer-----------)![](../Images/5bcd0c2d4c70afe2ff9a9f29d02c97e8.png)

摄影： [Mateusz Butkiewicz](https://unsplash.com/@puszkins?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Pandas 无疑是 Python 中最受欢迎的库之一。其 DataFrame 直观且拥有丰富的数据操作 API。许多 Python 库与 Pandas DataFrame 集成，以提高它们的采纳率。

然而，Pandas 在处理大数据集时并不出色。它主要用于单台机器上的数据分析，而非集群。在这篇文章中，我将尝试测量**Polars、DuckDB、Vaex 和 Modin 作为替代方案与 Pandas 的性能进行比较。**

[类似数据库操作的基准测试](https://h2oai.github.io/db-benchmark/)由 [h2oai](https://h2oai.github.io/) 发布，启发了本文的思路。基准测试实验于2021年5月进行。本文旨在在两年后回顾这一领域的许多特性和改进。

# **为什么 Pandas 在处理大型数据集时很慢？**

主要原因是 Pandas 并未设计为支持多核处理。Pandas **一次只使用一个 CPU 核心来执行数据操作任务**，而且在现代多核 PC 上并未利用并行处理的优势。

当数据量很大（但仍能适应一台机器）而 Pandas 执行时间较长时，如何缓解这一问题？一种解决方案是……
