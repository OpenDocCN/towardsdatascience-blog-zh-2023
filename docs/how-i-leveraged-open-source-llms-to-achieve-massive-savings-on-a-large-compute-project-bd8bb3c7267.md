# 我如何利用开源 LLMs 在大型计算项目中实现大规模节省

> 原文：[`towardsdatascience.com/how-i-leveraged-open-source-llms-to-achieve-massive-savings-on-a-large-compute-project-bd8bb3c7267?source=collection_archive---------4-----------------------#2023-08-30`](https://towardsdatascience.com/how-i-leveraged-open-source-llms-to-achieve-massive-savings-on-a-large-compute-project-bd8bb3c7267?source=collection_archive---------4-----------------------#2023-08-30)

## 使用开源 LLMs 和 GPU 租赁来解锁大型计算项目的成本效益。

[](https://medium.com/@ryanshrott?source=post_page-----bd8bb3c7267--------------------------------)![Ryan Shrott](https://medium.com/@ryanshrott?source=post_page-----bd8bb3c7267--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bd8bb3c7267--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd8bb3c7267--------------------------------) [Ryan Shrott](https://medium.com/@ryanshrott?source=post_page-----bd8bb3c7267--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Faba7ffb1d8f5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-leveraged-open-source-llms-to-achieve-massive-savings-on-a-large-compute-project-bd8bb3c7267&user=Ryan+Shrott&userId=aba7ffb1d8f5&source=post_page-aba7ffb1d8f5----bd8bb3c7267---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd8bb3c7267--------------------------------) ·6 分钟阅读·2023 年 8 月 30 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbd8bb3c7267&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-leveraged-open-source-llms-to-achieve-massive-savings-on-a-large-compute-project-bd8bb3c7267&user=Ryan+Shrott&userId=aba7ffb1d8f5&source=-----bd8bb3c7267---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbd8bb3c7267&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-leveraged-open-source-llms-to-achieve-massive-savings-on-a-large-compute-project-bd8bb3c7267&source=-----bd8bb3c7267---------------------bookmark_footer-----------)![](img/83382f63b21df0de2b0d47d33ad7212d.png)

图片由 [Alexander Grey](https://unsplash.com/@sharonmccutcheon?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 介绍

在大型语言模型（LLMs）的世界中，计算成本可能成为一个重大障碍，尤其是对于大规模项目。我最近开始了一个需要运行 4,000,000 个提示的项目，平均输入长度为 1000 个标记，平均输出长度为 200 个标记。这接近 50 亿个标记！传统的按标记付费方法，如 GPT-3.5 和 GPT-4 等模型所采用的，可能会导致一笔不小的账单。然而，我发现通过利用开源 LLMs，我可以将定价模型转变为按计算时间小时收费，从而节省了大量费用。本文将详细介绍我采取的方法，并对每种方法进行比较和对比。请注意，虽然我分享了我的定价经验，但这些费用可能会发生变化，并可能因地区和具体情况而异。这里的关键点是，利用开源 LLMs 和按小时租用 GPU 的潜在成本节省，而不是具体的报价价格。

## ChatGPT API
