# 《Pandas 和 Python 技巧：数据科学与数据分析》第四部分

> 原文：[https://towardsdatascience.com/pandas-python-tricks-for-data-science-data-analysis-part-4-67f61f69d62c?source=collection_archive---------11-----------------------#2023-03-16](https://towardsdatascience.com/pandas-python-tricks-for-data-science-data-analysis-part-4-67f61f69d62c?source=collection_archive---------11-----------------------#2023-03-16)

## 这是我《Pandas 和 Python 技巧》第四部分。

[](https://zoumanakeita.medium.com/?source=post_page-----67f61f69d62c--------------------------------)[![Zoumana Keita](../Images/34a15c1d03687816dbdbc065f5719f80.png)](https://zoumanakeita.medium.com/?source=post_page-----67f61f69d62c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----67f61f69d62c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----67f61f69d62c--------------------------------) [Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----67f61f69d62c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe6ae785a30d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-python-tricks-for-data-science-data-analysis-part-4-67f61f69d62c&user=Zoumana+Keita&userId=e6ae785a30d&source=post_page-e6ae785a30d----67f61f69d62c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----67f61f69d62c--------------------------------) ·4分钟阅读·2023年3月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F67f61f69d62c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-python-tricks-for-data-science-data-analysis-part-4-67f61f69d62c&user=Zoumana+Keita&userId=e6ae785a30d&source=-----67f61f69d62c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F67f61f69d62c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-python-tricks-for-data-science-data-analysis-part-4-67f61f69d62c&source=-----67f61f69d62c---------------------bookmark_footer-----------)![](../Images/27ca0e033ce93d3ce87f795c76186721.png)

图片来源：[Andrew Neel](https://unsplash.com/@andrewtneel) 供图于 [Unsplash](https://unsplash.com/photos/cckf4TsHAuw)

# 介绍

几天前，我分享了 [一些 Python 和 Pandas 技巧](https://medium.com/towards-data-science/pandas-python-tricks-for-data-science-data-analysis-part-3-462d0e952925)，帮助数据分析师和数据科学家快速学习他们可能还不了解的宝贵新概念。这也是我每天在 [LinkedIn](https://www.linkedin.com/in/zoumana-keita/) 分享的技巧集合的一部分。

# Pandas

## 更改列的数据类型

错误的数据格式是处理现实世界 🌏 数据时常见的挑战。

例如，你可能有一个存储为字符串的数值，如“34”而不是 34。

✅ 使用 `astype` 函数，你可以轻松将数据从一种类型转换为另一种类型（例如：字符串转换为数值）。

下面是一个示例 💡

![](../Images/48b5339ef72d95868b1ac641d1bca53b.png)

## 检查两个 DataFrame 是否相等

两列同名但可能不包含相同的值，两行相同索引也可能不完全相同。
