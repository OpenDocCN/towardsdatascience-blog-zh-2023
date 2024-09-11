# 在 Python 中处理日期时间数据的三大强大技巧

> 原文：[`towardsdatascience.com/3-powerful-tricks-to-work-with-date-time-data-in-python-67c2d3834338?source=collection_archive---------4-----------------------#2023-07-14`](https://towardsdatascience.com/3-powerful-tricks-to-work-with-date-time-data-in-python-67c2d3834338?source=collection_archive---------4-----------------------#2023-07-14)

## 使用 Python 进行数据科学

## 学习如何使用 datetime 模块处理 Python 中的日期和时间数据

[](https://medium.com/@17.rsuraj?source=post_page-----67c2d3834338--------------------------------)![Suraj Gurav](https://medium.com/@17.rsuraj?source=post_page-----67c2d3834338--------------------------------)[](https://towardsdatascience.com/?source=post_page-----67c2d3834338--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----67c2d3834338--------------------------------) [Suraj Gurav](https://medium.com/@17.rsuraj?source=post_page-----67c2d3834338--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fdda183cca2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-powerful-tricks-to-work-with-date-time-data-in-python-67c2d3834338&user=Suraj+Gurav&userId=1fdda183cca2&source=post_page-1fdda183cca2----67c2d3834338---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----67c2d3834338--------------------------------) ·9 分钟阅读·2023 年 7 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F67c2d3834338&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-powerful-tricks-to-work-with-date-time-data-in-python-67c2d3834338&user=Suraj+Gurav&userId=1fdda183cca2&source=-----67c2d3834338---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F67c2d3834338&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-powerful-tricks-to-work-with-date-time-data-in-python-67c2d3834338&source=-----67c2d3834338---------------------bookmark_footer-----------)![](img/63596805f41357087220097558b71bbd.png)

图片由 [Nathan Dumlao](https://unsplash.com/@nate_dumlao?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/photos/Y3AqmbmtLQI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

日期时间数据是你将处理的最常见且耗时的数据类型之一。

无论是进行时间序列分析、为机器学习模型准备数据，还是进行探索性数据分析，你都需要处理数据中存在的日期时间值。

此外，当你将来自多个来源的数据结合在一起时，日期时间数据的格式可能不同。为了保持数据的一致性，你需要将所有日期时间值转换为统一/标准格式，因此你必须掌握这些处理技巧。

你一定花了至少五分钟寻找如何处理这类日期时间数据。而且每次处理这种数据时都是如此。我总是花时间在这上面！！

因此，在这篇文章中，我将分享**处理日期时间数据的 3 个快捷技巧**，通过这些技巧你可以完成大部分日期时间数据的处理。我建议你将这篇文章保存以便未来快速参考。
