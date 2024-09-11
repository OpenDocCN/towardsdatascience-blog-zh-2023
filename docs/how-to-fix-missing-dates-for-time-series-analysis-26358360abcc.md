# 如何修复时间序列分析中的缺失日期

> 原文：[https://towardsdatascience.com/how-to-fix-missing-dates-for-time-series-analysis-26358360abcc?source=collection_archive---------9-----------------------#2023-07-20](https://towardsdatascience.com/how-to-fix-missing-dates-for-time-series-analysis-26358360abcc?source=collection_archive---------9-----------------------#2023-07-20)

## 学习如何在 BigQuery 中使用 TVF，轻松生成时间序列分析所需的日期范围。

[](https://medium.com/@thomas.ellyatt?source=post_page-----26358360abcc--------------------------------)[![Tom Ellyatt](../Images/8756acdd11fef8db9a868820251e7575.png)](https://medium.com/@thomas.ellyatt?source=post_page-----26358360abcc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----26358360abcc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----26358360abcc--------------------------------) [Tom Ellyatt](https://medium.com/@thomas.ellyatt?source=post_page-----26358360abcc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F79566ac4b1bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-fix-missing-dates-for-time-series-analysis-26358360abcc&user=Tom+Ellyatt&userId=79566ac4b1bb&source=post_page-79566ac4b1bb----26358360abcc---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----26358360abcc--------------------------------) ·7分钟阅读·2023年7月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F26358360abcc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-fix-missing-dates-for-time-series-analysis-26358360abcc&user=Tom+Ellyatt&userId=79566ac4b1bb&source=-----26358360abcc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F26358360abcc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-fix-missing-dates-for-time-series-analysis-26358360abcc&source=-----26358360abcc---------------------bookmark_footer-----------)![](../Images/50974d6ab642db33cc05beee117d3727.png)

我撰写这篇文章的目标是帮助你理解 TVF（表值函数）及其使用方法，通过一个例子来解决时间序列分析中常见的缺失日期问题。

在某些情况下，零数据的日期是重要的，必须显示/包含在你的数据集中。例如：

+   企业可以通过识别零销售的日期来受益。这些日期受假期或客户行为变化的影响。

+   查找数据中的缺失日期有助于通过揭示系统故障或数据捕获不完整导致的异常或离群点来提高数据质量。显示缺失日期是实现这一目标的有用工具。

这些缺失日期可能会对分析和可视化造成问题。因此，你需要一个解决方案，确保所有日期都出现在输出中，即使没有相应的数据。

到文章结束时，你将拥有自己的 TVF，它可以生成这些…
