# 新数据表明 2023 年是有史以来最热的夏天

> 原文：[https://towardsdatascience.com/new-data-demonstrates-that-2023-was-the-hottest-summer-ever-d92d500a8f01?source=collection_archive---------8-----------------------#2023-09-28](https://towardsdatascience.com/new-data-demonstrates-that-2023-was-the-hottest-summer-ever-d92d500a8f01?source=collection_archive---------8-----------------------#2023-09-28)

## 气候变化|数据可视化

## **我们在 Python 和 Plotly 中开发了可视化工具，以分析 2023 年 6 月至 8 月期间记录到的最高温度**

[](https://medium.com/@alan-jones?source=post_page-----d92d500a8f01--------------------------------)[![Alan Jones](../Images/359379fab1d6685ff08080b98173e67c.png)](https://medium.com/@alan-jones?source=post_page-----d92d500a8f01--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d92d500a8f01--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d92d500a8f01--------------------------------) [Alan Jones](https://medium.com/@alan-jones?source=post_page-----d92d500a8f01--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7d3f5fb94faa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnew-data-demonstrates-that-2023-was-the-hottest-summer-ever-d92d500a8f01&user=Alan+Jones&userId=7d3f5fb94faa&source=post_page-7d3f5fb94faa----d92d500a8f01---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d92d500a8f01--------------------------------) · 11 分钟阅读 · 2023 年 9 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd92d500a8f01&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnew-data-demonstrates-that-2023-was-the-hottest-summer-ever-d92d500a8f01&user=Alan+Jones&userId=7d3f5fb94faa&source=-----d92d500a8f01---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd92d500a8f01&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnew-data-demonstrates-that-2023-was-the-hottest-summer-ever-d92d500a8f01&source=-----d92d500a8f01---------------------bookmark_footer-----------)![](../Images/4e35b9b0291558c78c0d56da57bc8b6d.png)

图片由 [Luis Graterol](https://unsplash.com/@iguanaphoto?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

今年夏天的气温比自 1880 年以来的任何时候都要高！

数据科学家如何帮助展示我们的气候如何迅速变化，并在传达情况严重性的过程中发挥作用？我们将探讨如何通过分析和可视化有效地呈现数据，并对数据的表现进行微调。

但首先，让我们简要地了解一些全球变暖的后果。然后，我们将考虑如何使用 Plotly 和 Python 有效地可视化数据，并看看如何展示这些数据与二氧化碳排放的关系。

![](../Images/dd6383683b0a5360c360ef4c9ccd569e.png)

这张地图展示了2023年气象夏季（6月、7月和8月）全球温度异常。它显示了与1951年至1980年基准平均值相比，地球不同地区的温度是多么地偏暖或偏冷。***来源：NASA Earth Observatory/Lauren Dauphin***，[*已获许可使用*](https://www.nasa.gov/multimedia/guidelines/index.html)

这张来自NASA的地图展示了今年夏季全球温度异常，与1951年至1980年平均值进行比较，我不惊讶于看到我居住的欧洲部分地区是其中之一…
