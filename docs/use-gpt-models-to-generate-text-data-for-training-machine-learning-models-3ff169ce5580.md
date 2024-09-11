# 使用 GPT 模型生成用于训练机器学习模型的文本数据

> 原文：[`towardsdatascience.com/use-gpt-models-to-generate-text-data-for-training-machine-learning-models-3ff169ce5580?source=collection_archive---------6-----------------------#2023-07-12`](https://towardsdatascience.com/use-gpt-models-to-generate-text-data-for-training-machine-learning-models-3ff169ce5580?source=collection_archive---------6-----------------------#2023-07-12)

## 一个逐步指南，使用 Python

![Jin Cui](https://jin-cui.medium.com/?source=post_page-----3ff169ce5580--------------------------------) ![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3ff169ce5580--------------------------------) [Jin Cui](https://jin-cui.medium.com/?source=post_page-----3ff169ce5580--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F333e9ae026bc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-gpt-models-to-generate-text-data-for-training-machine-learning-models-3ff169ce5580&user=Jin+Cui&userId=333e9ae026bc&source=post_page-333e9ae026bc----3ff169ce5580---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3ff169ce5580--------------------------------) · 9 分钟阅读 · 2023 年 7 月 12 日

--

![](img/8915005799ca4a06d7b79a06fea29dd9.png)

图片由[Claudio Schwarz](https://unsplash.com/@purzlbaum?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上

# 动机

数据是构建机器学习模型的基础，但用于训练机器学习模型的文本数据难以收集，原因如下：

+   开源文本数据集有限。隐私规则和商业机密通常会限制特权数据的分发。此外，公开的数据集可能没有商业使用许可，或者更关键的是可能与实际上下文不相关。例如，IMDB 的电影评论不太可能对分析客户对银行产品的情感有意义。

+   机器学习模型通常需要大量的训练数据才能发挥作用。一个公司，特别是初创公司，可能需要相当长的时间才能收集到可信的文本数据。此外，这些数据可能没有针对特定机器学习任务标记响应变量。例如，一个公司可能收集了客户投诉的逐字记录，但可能没有对这些投诉的主题或情感有细致的了解。

我们如何克服上述限制，以一种可扩展且具有成本效益的方式生成符合目的的文本数据？鉴于最近…
