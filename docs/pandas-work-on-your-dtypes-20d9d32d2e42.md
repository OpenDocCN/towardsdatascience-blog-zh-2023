# Pandas：优化你的数据类型！

> 原文：[https://towardsdatascience.com/pandas-work-on-your-dtypes-20d9d32d2e42?source=collection_archive---------9-----------------------#2023-09-26](https://towardsdatascience.com/pandas-work-on-your-dtypes-20d9d32d2e42?source=collection_archive---------9-----------------------#2023-09-26)

## 在 pandas 中拥有正确的数据类型对于清晰的数据分析至关重要。这里解释了原因和方法。

[](https://mocquin.medium.com/?source=post_page-----20d9d32d2e42--------------------------------)[![Yoann Mocquin](../Images/b30a0f70c56972aabd2bc0a74baa90bb.png)](https://mocquin.medium.com/?source=post_page-----20d9d32d2e42--------------------------------)[](https://towardsdatascience.com/?source=post_page-----20d9d32d2e42--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----20d9d32d2e42--------------------------------) [Yoann Mocquin](https://mocquin.medium.com/?source=post_page-----20d9d32d2e42--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F173731d06320&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-work-on-your-dtypes-20d9d32d2e42&user=Yoann+Mocquin&userId=173731d06320&source=post_page-173731d06320----20d9d32d2e42---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----20d9d32d2e42--------------------------------) ·8 min read·2023年9月26日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F20d9d32d2e42&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-work-on-your-dtypes-20d9d32d2e42&user=Yoann+Mocquin&userId=173731d06320&source=-----20d9d32d2e42---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F20d9d32d2e42&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-work-on-your-dtypes-20d9d32d2e42&source=-----20d9d32d2e42---------------------bookmark_footer-----------)

**为你的 Series 和 DataFrame 设置合适的数据类型非常重要，原因有很多：**

+   **内存管理**：为特定的*系列*使用正确的数据类型可以显著减少其内存使用量，这也同样适用于*数据框*。

+   **解释**：其他人（无论是人还是计算机）会根据数据类型对你的数据做出假设：如果一列充满整数却以字符串形式存储，他们将其视为字符串，而非整数。

+   **这强制你保持数据的清洁**，例如处理缺失值或记录错误的值。这将大大简化后续的数据处理。

还有可能有更多原因，你能说出一些吗？如果可以，请在评论中写下。

**在我系列的第一篇文章中，我想回顾一下 pandas 数据类型——或者说 dtypes 的基础知识。**

![](../Images/30c0a25974ae9b6e906206459f18ef8f.png)

照片由[Chris Curry](https://unsplash.com/@chriscurry92?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我们将首先回顾`pandas`提供的可用数据类型，然后我将重点介绍4种满足你95%需求的有用数据类型，即**数值型数据类型、布尔型数据类型、字符串数据类型和类别数据类型**。

**这篇第一篇文章的最终目标是让你更熟悉各种数据**…
