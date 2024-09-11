# 一种预测概率分布的新方法

> 原文：[https://towardsdatascience.com/a-new-way-to-predict-probability-distributions-e7258349f464?source=collection_archive---------0-----------------------#2023-02-14](https://towardsdatascience.com/a-new-way-to-predict-probability-distributions-e7258349f464?source=collection_archive---------0-----------------------#2023-02-14)

## 探索使用 Catboost 的多分位回归

[](https://harrisonfhoffman.medium.com/?source=post_page-----e7258349f464--------------------------------)[![Harrison Hoffman](../Images/5eaa3e2bd0507297eb6c4a7efcf06324.png)](https://harrisonfhoffman.medium.com/?source=post_page-----e7258349f464--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e7258349f464--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e7258349f464--------------------------------) [Harrison Hoffman](https://harrisonfhoffman.medium.com/?source=post_page-----e7258349f464--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F38889d0801d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-new-way-to-predict-probability-distributions-e7258349f464&user=Harrison+Hoffman&userId=38889d0801d0&source=post_page-38889d0801d0----e7258349f464---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e7258349f464--------------------------------) ·10分钟阅读·2023年2月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe7258349f464&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-new-way-to-predict-probability-distributions-e7258349f464&user=Harrison+Hoffman&userId=38889d0801d0&source=-----e7258349f464---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe7258349f464&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-new-way-to-predict-probability-distributions-e7258349f464&source=-----e7258349f464---------------------bookmark_footer-----------)![](../Images/908e3e8a3e927aa30c512a2bb15cd827.png)

骰子。作者提供的图像。

我们对机器学习模型预测的信心有多大？这个问题在过去十年中一直是研究的重点，并且在金融和医疗等高风险机器学习应用中具有重要意义。虽然许多分类模型，尤其是[校准的](/why-calibrators-part-1-of-the-series-on-probability-calibration-9110831c6bde)模型，通过对目标类别预测概率分布来进行不确定性量化，但在回归任务中量化不确定性则要复杂得多。

在众多提出的方法中，分位数回归因其不对目标分布做任何假设而成为最受欢迎的方法之一。直到最近，分位数回归的主要缺点是需要为每个预测分位数训练一个模型。例如，为了预测目标分布的第10、第50和第90百分位数，需要训练三个独立的模型。Catboost 已经通过 [多分位数损失函数](https://catboost.ai/en/docs/concepts/loss-functions-regression#MultiQuantile:~:text=MultiQuantile,-%5Cdisplaystyle%5Cfrac%7B%5Csum) 解决了这个问题——一种使单个模型能够预测任意数量分位数的损失函数。

本文将探讨使用多分位数损失函数在合成数据上的两个示例。虽然这些示例未必反映现实世界的数据集，但它们将帮助我们……
