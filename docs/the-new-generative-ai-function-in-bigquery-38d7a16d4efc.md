# BigQuery 中的新生成式 AI 函数

> 原文：[`towardsdatascience.com/the-new-generative-ai-function-in-bigquery-38d7a16d4efc?source=collection_archive---------5-----------------------#2023-12-01`](https://towardsdatascience.com/the-new-generative-ai-function-in-bigquery-38d7a16d4efc?source=collection_archive---------5-----------------------#2023-12-01)

## 如何使用 BigQuery GENERATE_TEXT 远程函数

[](https://medium.com/@martosi?source=post_page-----38d7a16d4efc--------------------------------)![Marina Tosic](https://medium.com/@martosi?source=post_page-----38d7a16d4efc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----38d7a16d4efc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----38d7a16d4efc--------------------------------) [Marina Tosic](https://medium.com/@martosi?source=post_page-----38d7a16d4efc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe40b4f03cd3e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-new-generative-ai-function-in-bigquery-38d7a16d4efc&user=Marina+Tosic&userId=e40b4f03cd3e&source=post_page-e40b4f03cd3e----38d7a16d4efc---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----38d7a16d4efc--------------------------------) ·10 min read·Dec 1, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F38d7a16d4efc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-new-generative-ai-function-in-bigquery-38d7a16d4efc&user=Marina+Tosic&userId=e40b4f03cd3e&source=-----38d7a16d4efc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F38d7a16d4efc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-new-generative-ai-function-in-bigquery-38d7a16d4efc&source=-----38d7a16d4efc---------------------bookmark_footer-----------)![](img/6bc5b9d4bfe58fdc6f69a6a33a2454d3.png)

“每个人都可以在 BigQuery 中利用 SQL 知识和良好的提示结构进行编码和进行 NLP 分析” [照片来自 [Adi Goldstein](https://unsplash.com/@adigold1?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)]

# 介绍

自从我开始使用 Google 平台以来，Google 就一直用其 [BigQuery](https://cloud.google.com/bigquery) (BQ) 的功能和发展不断给我带来惊喜。

四年前，我经历了一个真正的“哇”时刻。

我记得仿佛就在昨天，我坐在 [Big Data London](https://bigdataldn.com/) 2019 会议的前排。那时我对仅使用 BQ 函数创建机器学习模型的可能性了解甚少，或者更确切地说，对 BQ 机器学习（[BQML](https://cloud.google.com/bigquery/docs/bqml-introduction)）知之甚少。

至少直到会议上，谷歌的同事展示了如何仅使用谷歌的 SQL 创建 [分类](https://cloud.google.com/bigquery/docs/logistic-regression-prediction)、[聚类](https://cloud.google.com/bigquery/docs/kmeans-tutorial) 和 [时间序列预测](https://cloud.google.com/bigquery/docs/arima-single-time-series-forecasting-tutorial) 模型。

当时我脑海中的第一个想法是“你一定是在开玩笑”。

我脑海中的第二个想法是，“这是否意味着所有只知道 SQL 的人都能够创建机器学习模型？”

如你所料，如果你正在使用 BigQuery 作为数据仓库，那么答案是“***是的***”。

现在，在使用了 [BQML](https://cloud.google.com/bigquery/docs/bqml-introduction) 函数一段时间后，对上述问题的正确答案是“***也许***”。
