# Pandas 和 Python 数据科学与数据分析技巧 — 第 3 部分

> 原文：[https://towardsdatascience.com/pandas-python-tricks-for-data-science-data-analysis-part-3-462d0e952925?source=collection_archive---------6-----------------------#2023-02-16](https://towardsdatascience.com/pandas-python-tricks-for-data-science-data-analysis-part-3-462d0e952925?source=collection_archive---------6-----------------------#2023-02-16)

## 这是我分享的 Pandas 和 Python 技巧的第三部分

[](https://zoumanakeita.medium.com/?source=post_page-----462d0e952925--------------------------------)[![Zoumana Keita](../Images/34a15c1d03687816dbdbc065f5719f80.png)](https://zoumanakeita.medium.com/?source=post_page-----462d0e952925--------------------------------)[](https://towardsdatascience.com/?source=post_page-----462d0e952925--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----462d0e952925--------------------------------) [Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----462d0e952925--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe6ae785a30d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-python-tricks-for-data-science-data-analysis-part-3-462d0e952925&user=Zoumana+Keita&userId=e6ae785a30d&source=post_page-e6ae785a30d----462d0e952925---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----462d0e952925--------------------------------) · 5 分钟阅读 · 2023 年 2 月 16 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F462d0e952925&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-python-tricks-for-data-science-data-analysis-part-3-462d0e952925&user=Zoumana+Keita&userId=e6ae785a30d&source=-----462d0e952925---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F462d0e952925&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-python-tricks-for-data-science-data-analysis-part-3-462d0e952925&source=-----462d0e952925---------------------bookmark_footer-----------)![](../Images/d611a2a92c7483193eb35b857f113a1e.png)

图片由 [Andrew Neel](https://unsplash.com/@andrewtneel) 提供，来源于 [Unsplash](https://unsplash.com/photos/cckf4TsHAuw)

# 介绍

几天前，我分享了一些 [Python 和 Pandas 技巧](https://medium.com/towards-data-science/pandas-and-python-tips-and-tricks-for-data-science-and-data-analysis-1b1e05b7d93a)，帮助数据分析师和数据科学家快速学习他们可能不了解的新概念。这也是我每天在 [LinkedIn](https://www.linkedin.com/in/zoumana-keita/) 上分享的技巧集的一部分。

# Pandas

## 基于条件替换数据框中的值

如果你想根据条件替换数据框中的值

✅ 你可以使用 Pandas 中内置的 𝗺𝗮𝘀𝗸() 函数。

下面是一个示例 💡

![](../Images/b7db149aeeffcf76acaec343ca8080cc.png)

## 给你的 Pandas 数据框应用颜色

你是否曾经想通过查看数据框快速找到一些信息❓

比如：

✨ 每列中哪些值是负数？

✨ 每列的最大值或最小值是多少？
