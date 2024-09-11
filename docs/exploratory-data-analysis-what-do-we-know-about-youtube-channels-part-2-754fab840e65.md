# 探索性数据分析：我们对YouTube频道了解了什么（第二部分）

> 原文：[https://towardsdatascience.com/exploratory-data-analysis-what-do-we-know-about-youtube-channels-part-2-754fab840e65?source=collection_archive---------6-----------------------#2023-11-24](https://towardsdatascience.com/exploratory-data-analysis-what-do-we-know-about-youtube-channels-part-2-754fab840e65?source=collection_archive---------6-----------------------#2023-11-24)

## 使用Pandas和YouTube数据API获取统计见解

[](https://dmitryelj.medium.com/?source=post_page-----754fab840e65--------------------------------)[![Dmitrii Eliuseev](../Images/7c48f0c016930ead59ddb785eaf3e0e6.png)](https://dmitryelj.medium.com/?source=post_page-----754fab840e65--------------------------------)[](https://towardsdatascience.com/?source=post_page-----754fab840e65--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----754fab840e65--------------------------------) [Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----754fab840e65--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F65c1f6ba75db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploratory-data-analysis-what-do-we-know-about-youtube-channels-part-2-754fab840e65&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=post_page-65c1f6ba75db----754fab840e65---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----754fab840e65--------------------------------) ·14分钟阅读·2023年11月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F754fab840e65&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploratory-data-analysis-what-do-we-know-about-youtube-channels-part-2-754fab840e65&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=-----754fab840e65---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F754fab840e65&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploratory-data-analysis-what-do-we-know-about-youtube-channels-part-2-754fab840e65&source=-----754fab840e65---------------------bookmark_footer-----------)![](../Images/e78513f53e8dba730d5b6d2892ff1e4e.png)

图片来自Souvik Banerjee，[Unsplash](https://unsplash.com/photos/black-and-white-laptop-computer-8dOk8JVESxY)

在[第一部分](/exploratory-data-analysis-what-do-we-know-about-youtube-channels-3688c5cbc438)中，我收集了约3000个YouTube频道的统计数据，并获得了一些有趣的见解。在这一部分，我将更深入地探讨，从通用的“频道”到个别的“视频”级别。我将展示如何收集YouTube视频的数据，以及我们可以获得哪些见解。

## 方法论

为了收集有关 YouTube 视频的数据，我们需要执行几个步骤：

+   获取 YouTube Data API 的凭据。它是免费的，每天 10,000 次请求的 API 限额足够满足我们的任务。

+   查找我们想要分析的几个 YouTube 频道。

+   编写一些 Python 代码来获取选定频道的最新视频及其统计数据。YouTube 分析仅对频道拥有者开放，我们只能获得*当前时刻*的数据。但我们可以运行代码一段时间。在我的情况下，我使用 Apache Airflow 和 Raspberry Pi 收集了三周的数据。

+   执行数据分析。我将使用 Pandas、Matplotlib 和 Seaborn 来完成这一任务。
