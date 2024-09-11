# 如何训练 BERT 以进行掩码语言建模任务

> 原文：[https://towardsdatascience.com/how-to-train-bert-for-masked-language-modeling-tasks-3ccce07c6fdc?source=collection_archive---------0-----------------------#2023-10-17](https://towardsdatascience.com/how-to-train-bert-for-masked-language-modeling-tasks-3ccce07c6fdc?source=collection_archive---------0-----------------------#2023-10-17)

## 从头开始使用 Python 和 Transformers 库构建语言模型以进行 MLM 任务的实践指南

[](https://ransakaravihara.medium.com/?source=post_page-----3ccce07c6fdc--------------------------------)[![Ransaka Ravihara](../Images/ac09746938c10ad8f157d46ea0de27ca.png)](https://ransakaravihara.medium.com/?source=post_page-----3ccce07c6fdc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3ccce07c6fdc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3ccce07c6fdc--------------------------------) [Ransaka Ravihara](https://ransakaravihara.medium.com/?source=post_page-----3ccce07c6fdc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F61b4d96de932&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-train-bert-for-masked-language-modeling-tasks-3ccce07c6fdc&user=Ransaka+Ravihara&userId=61b4d96de932&source=post_page-61b4d96de932----3ccce07c6fdc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3ccce07c6fdc--------------------------------) · 7 分钟阅读 · 2023年10月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3ccce07c6fdc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-train-bert-for-masked-language-modeling-tasks-3ccce07c6fdc&user=Ransaka+Ravihara&userId=61b4d96de932&source=-----3ccce07c6fdc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3ccce07c6fdc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-train-bert-for-masked-language-modeling-tasks-3ccce07c6fdc&source=-----3ccce07c6fdc---------------------bookmark_footer-----------)![](../Images/f2df84bc5e99738d8d9c3264785f2390.png)

# 介绍

近年来，大型语言模型（LLMs）吸引了机器学习社区的所有注意。在 LLMs 出现之前，我们在各种语言建模技术上经历了一个关键的研究阶段，包括掩码语言建模、因果语言建模和序列到序列语言建模。

从上述列表来看，诸如 BERT 这样的掩码语言模型在分类和聚类等下游 NLP 任务中变得更加实用。得益于像 Hugging Face Transformers 这样的库，将这些模型应用于下游任务变得更加可及和可管理。同时，感谢开源社区，我们可以选择许多语言模型，覆盖了广泛使用的语言和领域。

本教程的 GitHub 仓库

[](https://github.com/Ransaka/Train-BERT-for-Masked-Language-Modeling-Tasks?source=post_page-----3ccce07c6fdc--------------------------------) [## GitHub - Ransaka/Train-BERT-for-Masked-Language-Modeling-Tasks: GitHub 仓库用于 TDS 文章 "如何…

### GitHub 仓库用于 TDS 文章 "如何训练 BERT 以处理掩码语言建模任务" - GitHub …

[github.com](https://github.com/Ransaka/Train-BERT-for-Masked-Language-Modeling-Tasks?source=post_page-----3ccce07c6fdc--------------------------------)

# 微调还是从头构建？
