# TimesNet：时间序列预测的最新进展

> 原文：[`towardsdatascience.com/timesnet-the-latest-advance-in-time-series-forecasting-745b69068c9c?source=collection_archive---------0-----------------------#2023-10-10`](https://towardsdatascience.com/timesnet-the-latest-advance-in-time-series-forecasting-745b69068c9c?source=collection_archive---------0-----------------------#2023-10-10)

## 了解 TimesNet 架构并应用于 Python 预测任务

[](https://medium.com/@marcopeixeiro?source=post_page-----745b69068c9c--------------------------------)![Marco Peixeiro](https://medium.com/@marcopeixeiro?source=post_page-----745b69068c9c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----745b69068c9c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----745b69068c9c--------------------------------) [Marco Peixeiro](https://medium.com/@marcopeixeiro?source=post_page-----745b69068c9c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F741c1c8fcfbd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftimesnet-the-latest-advance-in-time-series-forecasting-745b69068c9c&user=Marco+Peixeiro&userId=741c1c8fcfbd&source=post_page-741c1c8fcfbd----745b69068c9c---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----745b69068c9c--------------------------------) ·10 分钟阅读·2023 年 10 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F745b69068c9c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftimesnet-the-latest-advance-in-time-series-forecasting-745b69068c9c&user=Marco+Peixeiro&userId=741c1c8fcfbd&source=-----745b69068c9c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F745b69068c9c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftimesnet-the-latest-advance-in-time-series-forecasting-745b69068c9c&source=-----745b69068c9c---------------------bookmark_footer-----------)![](img/9a499aed5a781591ea06eef5f3cb4222.png)

图片由 [Rachel Hisko](https://unsplash.com/@rachelhisko?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在之前的文章中，我们探讨了最新的前沿预测技术，始于 2020 年发布的[N-BEATS](https://medium.com/towards-data-science/the-easiest-way-to-forecast-time-series-using-n-beats-d778fcc2ba60)、2022 年的[N-HiTS](https://medium.com/towards-data-science/all-about-n-hits-the-latest-breakthrough-in-time-series-forecasting-a8ddcb27b0d5)和 2023 年 3 月的[PatchTST](https://medium.com/towards-data-science/patchtst-a-breakthrough-in-time-series-forecasting-e02d48869ccc)。回顾一下，N-BEATS 和 N-HiTS 依赖于多层感知器架构，而 PatchTST 则利用了 Transformer 架构。

截至 2023 年 4 月，文献中发布了一个新模型，它在时间序列分析的多个任务中，如预测、补全、分类和异常检测中，取得了前沿的结果：**TimesNet**。

TimesNet 由 Wu、Hu、Liu 等人在他们的论文中提出：[TimesNet: Temporal 2D-Variation Modeling For General Time Series Analysis](https://browse.arxiv.org/pdf/2210.02186.pdf)。

与之前的模型不同，它使用基于 CNN 的架构，在不同任务中实现了前沿的结果，使其成为时间序列分析基础模型的理想选择。

在本文中，我们将深入探讨 TimesNet 的架构和内部机制。然后，我们将模型应用于预测任务，并与 N-BEATS 和 N-HiTS 一起完成我们的小实验。
