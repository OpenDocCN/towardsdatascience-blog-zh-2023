# 少量样本学习如何自动化文档标注

> 原文：[https://towardsdatascience.com/how-few-shot-learning-is-automating-document-labeling-43f9868c0f74?source=collection_archive---------2-----------------------#2023-04-07](https://towardsdatascience.com/how-few-shot-learning-is-automating-document-labeling-43f9868c0f74?source=collection_archive---------2-----------------------#2023-04-07)

## 利用GPT模型

[](https://walidamamou.medium.com/?source=post_page-----43f9868c0f74--------------------------------)[![Walid Amamou](../Images/c5ae089c59a5ff070f0f90ad63ee3817.png)](https://walidamamou.medium.com/?source=post_page-----43f9868c0f74--------------------------------)[](https://towardsdatascience.com/?source=post_page-----43f9868c0f74--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----43f9868c0f74--------------------------------) [Walid Amamou](https://walidamamou.medium.com/?source=post_page-----43f9868c0f74--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F706f7e2641d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-few-shot-learning-is-automating-document-labeling-43f9868c0f74&user=Walid+Amamou&userId=706f7e2641d7&source=post_page-706f7e2641d7----43f9868c0f74---------------------post_header-----------) 发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----43f9868c0f74--------------------------------) ·5分钟阅读·2023年4月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F43f9868c0f74&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-few-shot-learning-is-automating-document-labeling-43f9868c0f74&user=Walid+Amamou&userId=706f7e2641d7&source=-----43f9868c0f74---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F43f9868c0f74&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-few-shot-learning-is-automating-document-labeling-43f9868c0f74&source=-----43f9868c0f74---------------------bookmark_footer-----------)![](../Images/6ec1bdbc7b5261ca7850c4adbf1af6e4.png)

图片由[DeepMind](https://unsplash.com/@deepmind?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，拍摄于[Unsplash](https://unsplash.com/photos/Vqm8hzQIzic?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

手动文档标注是一个耗时且枯燥的过程，通常需要大量资源，并且容易出错。然而，最近机器学习的进展，特别是所谓的少量样本学习技术，使得自动化标注过程变得更容易。特别是大型语言模型（LLMs），由于其在上下文学习中的新兴能力，是优秀的少量样本学习者。

在本文中，我们将更深入地探讨少样本学习如何改变文档标注，特别是对于命名实体识别（Named Entity Recognition），这是文档处理中的最重要任务。我们将展示[UBIAI](https://ubiai.tools)的平台如何通过少样本标注技术使自动化这一关键任务变得比以往任何时候都更加容易。

# 什么是少样本学习？

少样本学习是一种机器学习技术，使模型能够仅凭少量标注样本学习特定任务。在不修改其权重的情况下，模型可以通过在输入中包括这些任务的串联训练样本并要求模型预测目标文本的输出，来调整以执行特定任务。以下是使用3个示例进行命名实体识别（NER）任务的少样本学习示例：
