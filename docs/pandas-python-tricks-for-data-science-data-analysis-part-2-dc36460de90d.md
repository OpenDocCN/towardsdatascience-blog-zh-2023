# Pandas 与 Python 技巧用于数据科学与数据分析 — 第二部分

> 原文：[https://towardsdatascience.com/pandas-python-tricks-for-data-science-data-analysis-part-2-dc36460de90d?source=collection_archive---------8-----------------------#2023-01-17](https://towardsdatascience.com/pandas-python-tricks-for-data-science-data-analysis-part-2-dc36460de90d?source=collection_archive---------8-----------------------#2023-01-17)

## 这是我《Pandas 与 Python 技巧》系列的第二部分

[](https://zoumanakeita.medium.com/?source=post_page-----dc36460de90d--------------------------------)[![Zoumana Keita](../Images/34a15c1d03687816dbdbc065f5719f80.png)](https://zoumanakeita.medium.com/?source=post_page-----dc36460de90d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dc36460de90d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----dc36460de90d--------------------------------) [Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----dc36460de90d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe6ae785a30d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-python-tricks-for-data-science-data-analysis-part-2-dc36460de90d&user=Zoumana+Keita&userId=e6ae785a30d&source=post_page-e6ae785a30d----dc36460de90d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dc36460de90d--------------------------------) ·5分钟阅读·2023年1月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdc36460de90d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-python-tricks-for-data-science-data-analysis-part-2-dc36460de90d&user=Zoumana+Keita&userId=e6ae785a30d&source=-----dc36460de90d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdc36460de90d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-python-tricks-for-data-science-data-analysis-part-2-dc36460de90d&source=-----dc36460de90d---------------------bookmark_footer-----------)![](../Images/11eeeaf9d71dbc5826179cfa3cd1c5e2.png)

[Andrew Neel](https://unsplash.com/@andrewtneel) 摄影，发布于 [Unsplash](https://unsplash.com/photos/cckf4TsHAuw)

# 介绍

几天前，我分享了 [一些 Python 和 Pandas 技巧](https://medium.com/towards-data-science/pandas-and-python-tips-and-tricks-for-data-science-and-data-analysis-1b1e05b7d93a)，帮助数据分析师和数据科学家快速学习他们可能尚未了解的新宝贵概念。这也是我每天在 [LinkedIn](https://www.linkedin.com/in/zoumana-keita/) 上分享技巧的一部分。

这些技巧的视频系列可以在我的YouTube频道上找到，链接如下。

# Python

## 从列表中移除重复项

当尝试从列表中移除重复项时，你可能会尝试使用𝗳𝗼𝗿循环的方法。

这样做有效但在处理非常大的数据时效率较低❌。

相反，使用𝘀𝗲𝘁() ✅，它本身不接受重复项。

下面是一个示例💡
