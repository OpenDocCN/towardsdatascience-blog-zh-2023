# 基础概念入门：你需要了解的统计测试基础

> 原文：[`towardsdatascience.com/a-primer-on-foundational-concepts-you-need-to-start-running-statistical-tests-ae6b6c79e9a4?source=collection_archive---------4-----------------------#2023-09-09`](https://towardsdatascience.com/a-primer-on-foundational-concepts-you-need-to-start-running-statistical-tests-ae6b6c79e9a4?source=collection_archive---------4-----------------------#2023-09-09)

## 定量研究设计、显著性检验和不同类别的统计测试。

[](https://murtaza5152-ali.medium.com/?source=post_page-----ae6b6c79e9a4--------------------------------)![穆尔塔扎·阿里](https://murtaza5152-ali.medium.com/?source=post_page-----ae6b6c79e9a4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ae6b6c79e9a4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ae6b6c79e9a4--------------------------------) [穆尔塔扎·阿里](https://murtaza5152-ali.medium.com/?source=post_page-----ae6b6c79e9a4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F607fa603b7ce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-primer-on-foundational-concepts-you-need-to-start-running-statistical-tests-ae6b6c79e9a4&user=Murtaza+Ali&userId=607fa603b7ce&source=post_page-607fa603b7ce----ae6b6c79e9a4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ae6b6c79e9a4--------------------------------) ·8 分钟阅读·2023 年 9 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fae6b6c79e9a4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-primer-on-foundational-concepts-you-need-to-start-running-statistical-tests-ae6b6c79e9a4&user=Murtaza+Ali&userId=607fa603b7ce&source=-----ae6b6c79e9a4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fae6b6c79e9a4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-primer-on-foundational-concepts-you-need-to-start-running-statistical-tests-ae6b6c79e9a4&source=-----ae6b6c79e9a4---------------------bookmark_footer-----------)![](img/becf0595d70abed3a621eda2c727bc38.png)

图片来源：[Szabo Viktor](https://unsplash.com/@vmxhu?utm_source=medium&utm_medium=referral) 通过 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我写这篇文章是经过了一系列既可以预测又意外的事件。我最近完成了一门统计测试与报告的课程，并打算写一系列文章来解释我学到的最有用的统计测试的细节。我这样做既是为了巩固自己的知识，也帮助其他数据科学家学习我发现极为有用的主题。

这些文章中的第一篇将会是关于**t 检验**的，这是一种常见的统计测试，用于确定来自不同数据集的两个均值（平均值）是否在统计上有显著差异。我开始写这篇文章，但意识到我首先需要解释有两种不同的 t 检验。接着，我意识到要解释这一点，我需要解释一个单独但相关的基础概念。计划文章时这个循环持续进行。

此外，我意识到我需要在每一篇新文章中都这样做，因为每个统计测试都需要相同的基础知识。与其在每篇文章中重复这些信息，不如引用一个统一的来源…
