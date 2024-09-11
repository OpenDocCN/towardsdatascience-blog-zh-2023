# 使用 Polars 的数据管道：逐步指南

> 原文：[https://towardsdatascience.com/data-pipelines-with-polars-step-by-step-guide-f5474accacc4?source=collection_archive---------3-----------------------#2023-07-24](https://towardsdatascience.com/data-pipelines-with-polars-step-by-step-guide-f5474accacc4?source=collection_archive---------3-----------------------#2023-07-24)

## 使用 Polars 构建可扩展和快速的数据管道

[](https://medium.com/@antonsruberts?source=post_page-----f5474accacc4--------------------------------)[![Antons Tocilins-Ruberts](../Images/363a4f32aa793cca7a67dea68e76e3cf.png)](https://medium.com/@antonsruberts?source=post_page-----f5474accacc4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f5474accacc4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f5474accacc4--------------------------------) [Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----f5474accacc4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9dee50b0374b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-pipelines-with-polars-step-by-step-guide-f5474accacc4&user=Antons+Tocilins-Ruberts&userId=9dee50b0374b&source=post_page-9dee50b0374b----f5474accacc4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f5474accacc4--------------------------------) ·14 分钟阅读·2023年7月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff5474accacc4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-pipelines-with-polars-step-by-step-guide-f5474accacc4&user=Antons+Tocilins-Ruberts&userId=9dee50b0374b&source=-----f5474accacc4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff5474accacc4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-pipelines-with-polars-step-by-step-guide-f5474accacc4&source=-----f5474accacc4---------------------bookmark_footer-----------)![](../Images/bfaae191cbbf33d6ba469a6bffdb83bf.png)

图片来源于 [Filippo Vicini](https://unsplash.com/@filippo_vicini?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

本文旨在解释并展示如何使用 Polars 构建数据管道。它整合并使用了你从本系列前两部分中获得的所有知识，所以如果你还没有阅读它们，我强烈建议你先从那里开始，然后再回来阅读这里的内容。

[](/eda-with-polars-step-by-step-guide-for-pandas-users-part-1-b2ec500a1008?source=post_page-----f5474accacc4--------------------------------) [## 使用Polars进行EDA：Pandas用户的逐步指南（第1部分）]

### 使用Polars提升你的数据分析能力

[towardsdatascience.com](/eda-with-polars-step-by-step-guide-for-pandas-users-part-1-b2ec500a1008?source=post_page-----f5474accacc4--------------------------------) [](/eda-with-polars-step-by-step-guide-to-aggregate-and-analytic-functions-part-2-a22d986315aa?source=post_page-----f5474accacc4--------------------------------) [## 使用Polars进行EDA：汇总和分析函数的逐步指南（第2部分）]

### 使用Polars进行高级汇总和滚动平均，速度极快

[towardsdatascience.com](/eda-with-polars-step-by-step-guide-to-aggregate-and-analytic-functions-part-2-a22d986315aa?source=post_page-----f5474accacc4--------------------------------)

# 设置

你可以在这个[代码库](https://github.com/aruberts/tutorials/tree/main/polars)中找到所有代码，所以不要忘记克隆/拉取并给它加星。特别是，我们将探索这个[文件](https://github.com/aruberts/tutorials/blob/main/polars/data_preparation_pipeline.py)，这意味着我们将最终从笔记本进入现实世界！

本项目使用的数据可以从[Kaggle](https://www.kaggle.com/datasets/datasnaek/youtube-new?resource=download&sort=published)下载（CC0：公共领域）。这与之前使用的YouTube趋势数据集相同…
