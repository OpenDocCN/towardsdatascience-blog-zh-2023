# 《时间序列分析中的移动平均综合指南》

> 原文：[`towardsdatascience.com/the-comprehensive-guide-to-moving-averages-in-time-series-analysis-3fb2baa749a?source=collection_archive---------3-----------------------#2023-10-30`](https://towardsdatascience.com/the-comprehensive-guide-to-moving-averages-in-time-series-analysis-3fb2baa749a?source=collection_archive---------3-----------------------#2023-10-30)

![](img/5a59c54a66dd46223fbcca7e90e7958b.png)

图像由 Midjourney 创建

## 探索简单移动平均和指数加权移动平均的细微差别

[](https://eryk-lewinson.medium.com/?source=post_page-----3fb2baa749a--------------------------------)![Eryk Lewinson](https://eryk-lewinson.medium.com/?source=post_page-----3fb2baa749a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3fb2baa749a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3fb2baa749a--------------------------------) [Eryk Lewinson](https://eryk-lewinson.medium.com/?source=post_page-----3fb2baa749a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F44bc27317e6b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-comprehensive-guide-to-moving-averages-in-time-series-analysis-3fb2baa749a&user=Eryk+Lewinson&userId=44bc27317e6b&source=post_page-44bc27317e6b----3fb2baa749a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3fb2baa749a--------------------------------) ·9 分钟阅读·2023 年 10 月 30 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3fb2baa749a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-comprehensive-guide-to-moving-averages-in-time-series-analysis-3fb2baa749a&user=Eryk+Lewinson&userId=44bc27317e6b&source=-----3fb2baa749a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3fb2baa749a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-comprehensive-guide-to-moving-averages-in-time-series-analysis-3fb2baa749a&source=-----3fb2baa749a---------------------bookmark_footer-----------)

移动平均是时间序列分析中的一种基础工具，并且被广泛应用于各种目的。移动平均在时间序列分析中的一些最受欢迎的应用包括：

+   **噪声减少：** 移动平均能够有效过滤时间序列数据中的随机波动和噪声。使用移动平均进行平滑处理有助于分析师关注潜在的模式，而非短期波动，帮助他们识别有意义的趋势。

+   **季节性分解：** 将时间序列数据分解成其组成部分通常涉及使用移动平均。 季节性分解 技术利用移动平均从数据中提取季节性模式。此外，移动平均还可以帮助识别时间序列数据中的周期性成分，这些成分表示重复的模式，而不仅仅是季节性的。

+   **预测：** 移动平均是各种预测模型的基础组件。

+   **过滤异常值：** 移动平均可以用来识别和过滤时间序列数据中的异常值。那些显著偏离移动平均的异常数据点…
