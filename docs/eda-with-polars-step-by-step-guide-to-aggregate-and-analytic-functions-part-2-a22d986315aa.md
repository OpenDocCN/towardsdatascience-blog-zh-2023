# 使用 Polars 进行 EDA：聚合和分析函数的逐步指南（第二部分）

> 原文：[`towardsdatascience.com/eda-with-polars-step-by-step-guide-to-aggregate-and-analytic-functions-part-2-a22d986315aa?source=collection_archive---------7-----------------------#2023-07-10`](https://towardsdatascience.com/eda-with-polars-step-by-step-guide-to-aggregate-and-analytic-functions-part-2-a22d986315aa?source=collection_archive---------7-----------------------#2023-07-10)

## 使用 Polars 进行快速的高级聚合和滚动平均

[](https://medium.com/@antonsruberts?source=post_page-----a22d986315aa--------------------------------)![Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----a22d986315aa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a22d986315aa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a22d986315aa--------------------------------) [Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----a22d986315aa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9dee50b0374b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feda-with-polars-step-by-step-guide-to-aggregate-and-analytic-functions-part-2-a22d986315aa&user=Antons+Tocilins-Ruberts&userId=9dee50b0374b&source=post_page-9dee50b0374b----a22d986315aa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a22d986315aa--------------------------------) ·9 分钟阅读·2023 年 7 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa22d986315aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feda-with-polars-step-by-step-guide-to-aggregate-and-analytic-functions-part-2-a22d986315aa&user=Antons+Tocilins-Ruberts&userId=9dee50b0374b&source=-----a22d986315aa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa22d986315aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feda-with-polars-step-by-step-guide-to-aggregate-and-analytic-functions-part-2-a22d986315aa&source=-----a22d986315aa---------------------bookmark_footer-----------)![](img/e244e107d7b8e9c628cb4f7312d675a8.png)

照片由 [Spencer Davis](https://unsplash.com/@spencerdavis?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

在本系列的第一部分中，我们介绍了 Polars 的基础知识，并将其功能和语法与 Pandas 进行了比较。这一部分将使我们的查询复杂度提高一个档次，我们将看到如何执行一些相当复杂的聚合、滚动统计等。如果你不熟悉 Polars 或者需要复习一下，确保查看之前的部分。否则，让我们继续探索 Polars 吧！

# 设置

如同上一部分，请确保克隆/拉取这个[GitHub 仓库](https://github.com/aruberts/tutorials/tree/main/polars)，因为它包含了本篇文章所需的所有代码。特别是，我们将涵盖这个[笔记本](https://github.com/aruberts/tutorials/blob/main/polars/time_analysis.ipynb)，所以如果你想跟随学习，请确保获取它。

本项目使用的数据可以从[Kaggle](https://www.kaggle.com/datasets/datasnaek/youtube-new?resource=download&sort=published)（CC0: 公开领域）下载。我假设你已经安装了 Polars，因此只需确保使用 `pip install -U polars` 将其更新到最新版本即可。

# 数据处理

## 数据读取

与上一篇文章类似，让我们读取英国趋势数据集和…的映射。
