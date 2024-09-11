# 14 种让你的 Pandas 工作流飞快的技巧

> 原文：[`towardsdatascience.com/14-techniques-to-make-your-pandas-workflow-lighting-fast-b00ff0ac9267?source=collection_archive---------8-----------------------#2023-02-06`](https://towardsdatascience.com/14-techniques-to-make-your-pandas-workflow-lighting-fast-b00ff0ac9267?source=collection_archive---------8-----------------------#2023-02-06)

## 扩展您的 Pandas 工作流的基本指南

[](https://satyam-kumar.medium.com/?source=post_page-----b00ff0ac9267--------------------------------)![Satyam Kumar](https://satyam-kumar.medium.com/?source=post_page-----b00ff0ac9267--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b00ff0ac9267--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b00ff0ac9267--------------------------------) [Satyam Kumar](https://satyam-kumar.medium.com/?source=post_page-----b00ff0ac9267--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3d8bf96a415f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F14-techniques-to-make-your-pandas-workflow-lighting-fast-b00ff0ac9267&user=Satyam+Kumar&userId=3d8bf96a415f&source=post_page-3d8bf96a415f----b00ff0ac9267---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b00ff0ac9267--------------------------------) ·6 分钟阅读·2023 年 2 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb00ff0ac9267&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F14-techniques-to-make-your-pandas-workflow-lighting-fast-b00ff0ac9267&user=Satyam+Kumar&userId=3d8bf96a415f&source=-----b00ff0ac9267---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb00ff0ac9267&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F14-techniques-to-make-your-pandas-workflow-lighting-fast-b00ff0ac9267&source=-----b00ff0ac9267---------------------bookmark_footer-----------)![](img/f879822756fe6ccdee61b4615cebfbc5.png)

图片由 [Gerd Altmann](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=1340649) 提供，来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=1340649)

Pandas 是最受欢迎的 Python 数据探索和可视化库之一。Pandas 提供了大量 API 用于数据整理，但在处理大型数据集时，它的性能可能会失败或变得很慢。

在这篇文章中，我们将讨论使用各种技术、窍门或包来加速 Pandas 工作流的**14 种技巧**。

# 输入/输出：

## 1) 采样：

Pandas 一次性将整个文件加载到内存中，这使得处理可能导致内存崩溃的大型数据变得困难。其理念是仅读取所需的实例和特征。

`read_csv()` API 提供了 `nrows` 和 `usecols` 参数来分别过滤记录和特征的数量。

## 2) 分块处理：

采样技术将过滤并读取数据样本，但如果你需要处理所有数据，你…
