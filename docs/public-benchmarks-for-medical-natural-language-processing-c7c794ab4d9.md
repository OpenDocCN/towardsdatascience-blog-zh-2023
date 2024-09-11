# 医疗自然语言处理的公共基准

> 原文：[https://towardsdatascience.com/public-benchmarks-for-medical-natural-language-processing-c7c794ab4d9?source=collection_archive---------15-----------------------#2023-03-29](https://towardsdatascience.com/public-benchmarks-for-medical-natural-language-processing-c7c794ab4d9?source=collection_archive---------15-----------------------#2023-03-29)

## 对医疗自然语言处理的任务列表及对应数据集的总体介绍

[](https://eileen-code4fun.medium.com/?source=post_page-----c7c794ab4d9--------------------------------)[![Eileen Pangu](../Images/cbdab572af709b6e6b52cb3a078f220d.png)](https://eileen-code4fun.medium.com/?source=post_page-----c7c794ab4d9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c7c794ab4d9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c7c794ab4d9--------------------------------) [Eileen Pangu](https://eileen-code4fun.medium.com/?source=post_page-----c7c794ab4d9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F893d6b8a519f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpublic-benchmarks-for-medical-natural-language-processing-c7c794ab4d9&user=Eileen+Pangu&userId=893d6b8a519f&source=post_page-893d6b8a519f----c7c794ab4d9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c7c794ab4d9--------------------------------) ·6分钟阅读·2023年3月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc7c794ab4d9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpublic-benchmarks-for-medical-natural-language-processing-c7c794ab4d9&user=Eileen+Pangu&userId=893d6b8a519f&source=-----c7c794ab4d9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc7c794ab4d9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpublic-benchmarks-for-medical-natural-language-processing-c7c794ab4d9&source=-----c7c794ab4d9---------------------bookmark_footer-----------)![](../Images/e7f7329d67dd349b27bbe3c0f57628f7.png)

来源: [https://unsplash.com/](https://unsplash.com/)

自然语言处理（NLP）领域近年来发展迅猛。像transformer、BERT、GPT这样的突破一个接一个地出现。各行各业的从业者正在探索如何在特定的业务领域和工作流程中利用NLP的激动人心的发展[1]。医疗行业就是一个将从NLP改进中大大受益的行业。大量的自由文本医学笔记包含了惊人的数据洞察，这些数据可以为更好的护理提供信息、优化成本和推动医疗创新。为了衡量将NLP应用于医疗领域的效果，我们需要好的基准。本文列出了医学自然语言处理常见任务的权威公共基准。其目标是为医疗机器学习从业者提供一个测量他们NLP努力的起点。

> **实体/关系识别**

实体/关系识别的任务是检测和分类自由文本中的医学概念及其关系。这是更好地理解的关键步骤…
