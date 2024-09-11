# 如何利用预训练的变换器模型进行自定义文本分类？

> 原文：[https://towardsdatascience.com/how-to-leverage-pre-trained-transformer-models-for-custom-text-categorisation-3757c517bd65?source=collection_archive---------5-----------------------#2023-03-31](https://towardsdatascience.com/how-to-leverage-pre-trained-transformer-models-for-custom-text-categorisation-3757c517bd65?source=collection_archive---------5-----------------------#2023-03-31)

## 所以，你有一些自定义文本数据集需要分类，不知道怎么做？好吧，让我来展示一下如何使用预训练的最先进语言模型来实现。

[](https://saedhussain.medium.com/?source=post_page-----3757c517bd65--------------------------------)[![Saed Hussain](../Images/cb8d313c1c1a15fd1632979dc3c080a7.png)](https://saedhussain.medium.com/?source=post_page-----3757c517bd65--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3757c517bd65--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3757c517bd65--------------------------------) [Saed Hussain](https://saedhussain.medium.com/?source=post_page-----3757c517bd65--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa61c823447e0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-leverage-pre-trained-transformer-models-for-custom-text-categorisation-3757c517bd65&user=Saed+Hussain&userId=a61c823447e0&source=post_page-a61c823447e0----3757c517bd65---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3757c517bd65--------------------------------) · 11 分钟阅读 · 2023年3月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3757c517bd65&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-leverage-pre-trained-transformer-models-for-custom-text-categorisation-3757c517bd65&user=Saed+Hussain&userId=a61c823447e0&source=-----3757c517bd65---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3757c517bd65&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-leverage-pre-trained-transformer-models-for-custom-text-categorisation-3757c517bd65&source=-----3757c517bd65---------------------bookmark_footer-----------)![](../Images/948131b577b7ca3d0260d3ee6b5e9654.png)

照片由 [Meagan Carsience](https://unsplash.com/@mcarsience_photography?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

好的，我们直接进入正题！你有一些自定义数据，现在你想将它们分类到你的自定义类别中。在本文中，我将展示如何使用两种方法来实现这一目标。它们都利用了基于最新技术的预训练模型。

请注意，本文的目的是与您分享方法及其使用方式。这并不是一个包含最佳实践的完整数据科学教程。不幸的是，这超出了本文的范围。

本文中的所有代码可以在这个 [GitHub 仓库](https://github.com/saedhussain/medium/tree/main/text_category/notebooks) 中找到。

# 1: 零样本分类

## 概述

零样本分类是一种技术，它允许您在不为该任务训练特定模型的情况下对文本进行分类。相反，它使用已经在大量数据上训练好的预训练模型来执行此分类。这些模型通常在…
