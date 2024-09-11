# 超越温暖的怀抱：深入探讨 Hugging Face

> 原文：[https://towardsdatascience.com/beyond-the-warm-embrace-a-deeper-look-at-hugging-face-96b3497788e7?source=collection_archive---------8-----------------------#2023-11-03](https://towardsdatascience.com/beyond-the-warm-embrace-a-deeper-look-at-hugging-face-96b3497788e7?source=collection_archive---------8-----------------------#2023-11-03)

## 对语言模型进行微调以实现命名实体识别

[](https://medium.com/@raicik.zach?source=post_page-----96b3497788e7--------------------------------)[![扎卡里·雷西克](../Images/860760b53fcc75013007067190e8ca65.png)](https://medium.com/@raicik.zach?source=post_page-----96b3497788e7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----96b3497788e7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----96b3497788e7--------------------------------) [扎卡里·雷西克](https://medium.com/@raicik.zach?source=post_page-----96b3497788e7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28b350f36c59&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-the-warm-embrace-a-deeper-look-at-hugging-face-96b3497788e7&user=Zachary+Raicik&userId=28b350f36c59&source=post_page-28b350f36c59----96b3497788e7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----96b3497788e7--------------------------------) ·9分钟阅读·2023年11月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F96b3497788e7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-the-warm-embrace-a-deeper-look-at-hugging-face-96b3497788e7&user=Zachary+Raicik&userId=28b350f36c59&source=-----96b3497788e7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F96b3497788e7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-the-warm-embrace-a-deeper-look-at-hugging-face-96b3497788e7&source=-----96b3497788e7---------------------bookmark_footer-----------)![](../Images/ea850ddee92eada7c048b2f96261e036.png)

图片由 [Choong Deng Xiang](https://unsplash.com/@dengxiangs?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Hugging Face 是一个提供各种自然语言处理（NLP）和自然语言理解（NLU）任务工具和预训练模型的平台。在我们之前的文章中，[温暖的拥抱：探索 Hugging Face](https://medium.com/@raicik.zach/a-warm-embrace-exploring-hugging-face-210c7f4a9078)，我们深入探讨了该平台的基础知识及其开源库，其中包含了许多最先进的变换器架构的实现。本文通过为新兴的数据科学家提供对各种 Hugging Face 工具的单一、连贯视图，从而增强了 Hugging Face 的文档。特别是，本文解释了如何将多个 Hugging Face 功能组合起来，以微调现有的语言模型用于命名实体识别（“NER”）。

# 相关背景

在本节中，我们简要回顾了构建我们模型所需的两个基础概念。提醒一下，我们在[温暖的拥抱：探索 Hugging Face](https://medium.com/@raicik.zach/a-warm-embrace-exploring-hugging-face-210c7f4a9078)中涵盖了 Hugging Face 的基础知识。

+   命名实体识别

+   模型微调

在接下来的章节中，假设你对模型开发及相关概念有一定的了解——然而，如果有任何不清楚的地方，随时可以联系我！
