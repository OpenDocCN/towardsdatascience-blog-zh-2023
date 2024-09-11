# 减少 Pandas DataFrame 内存使用的 2 个简单步骤

> 原文：[https://towardsdatascience.com/2-simple-steps-to-reduce-the-memory-usage-of-your-pandas-dataframe-b654565d654?source=collection_archive---------4-----------------------#2023-03-21](https://towardsdatascience.com/2-simple-steps-to-reduce-the-memory-usage-of-your-pandas-dataframe-b654565d654?source=collection_archive---------4-----------------------#2023-03-21)

## 如何将大型数据集加载到 Python 的 RAM 中

[](https://medium.com/@iamleonie?source=post_page-----b654565d654--------------------------------)[![Leonie Monigatti](../Images/4044b1685ada53a30160b03dc78f9626.png)](https://medium.com/@iamleonie?source=post_page-----b654565d654--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b654565d654--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b654565d654--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----b654565d654--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F2-simple-steps-to-reduce-the-memory-usage-of-your-pandas-dataframe-b654565d654&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----b654565d654---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b654565d654--------------------------------) ·6 分钟阅读·2023年3月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb654565d654&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F2-simple-steps-to-reduce-the-memory-usage-of-your-pandas-dataframe-b654565d654&user=Leonie+Monigatti&userId=3a38da70d8dc&source=-----b654565d654---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb654565d654&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F2-simple-steps-to-reduce-the-memory-usage-of-your-pandas-dataframe-b654565d654&source=-----b654565d654---------------------bookmark_footer-----------)![](../Images/5f2b021fb91fe1b599baedad75f9c28c.png)

“如果我合适，我就坐下” — 将大型数据集加载到 RAM 中（图像由作者手绘）

数据分析师和科学家每天都在处理大型数据集。如果数据集消耗了所有可用内存，或者你需要在内存中操作数据，数据集的大小很快就会成为问题。

> 如果数据集消耗了所有可用内存，数据集的大小很快就会成为问题

在这种情况下，我们将探讨在将大型数据集从 CSV 文件加载到 Pandas DataFrame 时，可以应用的两个简单技巧，以便适应可用的 RAM。

1.  [探索哪些列对你是相关的](#5efd)

1.  [减少数据类型（降级）](#63fe)

**免责声明：** 这些步骤可以帮助减少数据框所需的内存量，但不能保证在应用这些步骤后，你的数据集会足够小，以适应你的内存。

# 步骤 1：探索哪些列对你是相关的

你不需要在内存中保留数据集的所有列。这就是为什么第一步是审查哪些列对你是相关的。
