# 如何有效比较机器学习解决方案

> 原文：[`towardsdatascience.com/how-to-compare-ml-solutions-effectively-28384e2cbca1?source=collection_archive---------15-----------------------#2023-07-06`](https://towardsdatascience.com/how-to-compare-ml-solutions-effectively-28384e2cbca1?source=collection_archive---------15-----------------------#2023-07-06)

![](img/f230bb52240a5717e53947dd913ed392.png)

图片由作者使用 [Midjourney](https://www.midjourney.com/app/) 创建。

## 提高将模型投入生产的机会

[](https://hennie-de-harder.medium.com/?source=post_page-----28384e2cbca1--------------------------------)![Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----28384e2cbca1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----28384e2cbca1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----28384e2cbca1--------------------------------) [Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----28384e2cbca1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffb96be98b7b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-compare-ml-solutions-effectively-28384e2cbca1&user=Hennie+de+Harder&userId=fb96be98b7b9&source=post_page-fb96be98b7b9----28384e2cbca1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----28384e2cbca1--------------------------------) ·6 min read·2023 年 7 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F28384e2cbca1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-compare-ml-solutions-effectively-28384e2cbca1&user=Hennie+de+Harder&userId=fb96be98b7b9&source=-----28384e2cbca1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F28384e2cbca1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-compare-ml-solutions-effectively-28384e2cbca1&source=-----28384e2cbca1---------------------bookmark_footer-----------)

**在评估和比较机器学习解决方案时，你的首选评估指标可能是预测能力。用一个单一指标来比较不同模型是很简单的，这在 Kaggle 比赛中完全没问题。然而，在现实生活中情况却不同。假设有两个模型：一个模型使用了 100 个特征和复杂的架构，另一个模型使用了 10 个特征和 XGBoost。复杂模型的得分仅比 XGBoost 模型高一点。在这种情况下，你会选择性能最好的模型还是选择更简单的模型？**

这篇文章将为你概述在比较不同的机器学习解决方案时可以考虑的各种因素。通过一个例子，我将展示如何以比仅仅依靠预测能力更好的方式来比较模型。让我们开始吧！

除了预测结果，还有几个重要因素需要考虑，当比较机器学习原型时，这些因素提供了有关模型在现实场景中总体适用性和有效性的宝贵见解。通过不仅仅关注预测能力，你将增加将机器学习解决方案投入生产的机会。
