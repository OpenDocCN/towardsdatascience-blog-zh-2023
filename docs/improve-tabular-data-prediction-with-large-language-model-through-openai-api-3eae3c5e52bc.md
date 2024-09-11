# 通过 OpenAI API 提高表格数据预测的准确性

> 原文：[`towardsdatascience.com/improve-tabular-data-prediction-with-large-language-model-through-openai-api-3eae3c5e52bc?source=collection_archive---------5-----------------------#2023-07-13`](https://towardsdatascience.com/improve-tabular-data-prediction-with-large-language-model-through-openai-api-3eae3c5e52bc?source=collection_archive---------5-----------------------#2023-07-13)

## 使用 Python 实现机器学习分类、提示工程、文本嵌入的特征工程以及 OpenAI API 的模型可解释性

[](https://medium.com/@tonyzhangsky?source=post_page-----3eae3c5e52bc--------------------------------)![Tony Zhang](https://medium.com/@tonyzhangsky?source=post_page-----3eae3c5e52bc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3eae3c5e52bc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3eae3c5e52bc--------------------------------) [Tony Zhang](https://medium.com/@tonyzhangsky?source=post_page-----3eae3c5e52bc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F92789d320912&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimprove-tabular-data-prediction-with-large-language-model-through-openai-api-3eae3c5e52bc&user=Tony+Zhang&userId=92789d320912&source=post_page-92789d320912----3eae3c5e52bc---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----3eae3c5e52bc--------------------------------) ·11 分钟阅读·2023 年 7 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3eae3c5e52bc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimprove-tabular-data-prediction-with-large-language-model-through-openai-api-3eae3c5e52bc&user=Tony+Zhang&userId=92789d320912&source=-----3eae3c5e52bc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3eae3c5e52bc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimprove-tabular-data-prediction-with-large-language-model-through-openai-api-3eae3c5e52bc&source=-----3eae3c5e52bc---------------------bookmark_footer-----------)![](img/46d4892886bd0e239397790eb79e98a9.png)

照片由[Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)拍摄，发布于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如今，大型语言模型及其应用或工具频繁出现在新闻和社交媒体上。GitHub 趋势页面展示了大量广泛使用大型语言模型的代码库。我们已经见证了大型语言模型在营销写作、文档总结、音乐创作和软件开发代码生成方面的惊人能力。

企业内部和在线积累了大量的表格数据（这是最古老且最普遍的数据格式之一，可以以行和列的形式表示在表格中）。我们能否在传统的机器学习生命周期中将大型语言模型应用于表格数据，以提高模型性能并增加业务价值？

在这篇文章中，我们将探讨以下主题，并提供完整的 Python 实现代码：

+   在 Kaggle [心脏病分析与预测数据集](https://www.kaggle.com/datasets/rashikrahmanpritom/heart-attack-analysis-prediction-dataset) 公共数据集上构建广义线性模型和基于树的模型。
