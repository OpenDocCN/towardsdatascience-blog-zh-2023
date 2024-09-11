# Beam Search：序列模型中最常用的算法

> 原文：[https://towardsdatascience.com/beam-search-the-most-used-algorithm-in-sequence-models-107d56b23556?source=collection_archive---------9-----------------------#2023-12-13](https://towardsdatascience.com/beam-search-the-most-used-algorithm-in-sequence-models-107d56b23556?source=collection_archive---------9-----------------------#2023-12-13)

## 了解文本翻译和语音识别中最著名算法的工作原理。

[](https://medium.com/@riccardo.andreoni?source=post_page-----107d56b23556--------------------------------)[![Riccardo Andreoni](../Images/5e22581e419639b373019a809d6e65c1.png)](https://medium.com/@riccardo.andreoni?source=post_page-----107d56b23556--------------------------------)[](https://towardsdatascience.com/?source=post_page-----107d56b23556--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----107d56b23556--------------------------------) [Riccardo Andreoni](https://medium.com/@riccardo.andreoni?source=post_page-----107d56b23556--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76784541161c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeam-search-the-most-used-algorithm-in-sequence-models-107d56b23556&user=Riccardo+Andreoni&userId=76784541161c&source=post_page-76784541161c----107d56b23556---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----107d56b23556--------------------------------) ·6分钟阅读·2023年12月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F107d56b23556&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeam-search-the-most-used-algorithm-in-sequence-models-107d56b23556&user=Riccardo+Andreoni&userId=76784541161c&source=-----107d56b23556---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F107d56b23556&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeam-search-the-most-used-algorithm-in-sequence-models-107d56b23556&source=-----107d56b23556---------------------bookmark_footer-----------)![](../Images/c11105c573f0bc16e297b233f7f830a9.png)

Beam Search 允许考虑多个候选流。图片来源：[unsplash.com](https://unsplash.com/photos/multi-colored-light-streaks-on-white-background-ylMP3TetKoQ)。

想象一下你是一个[AI](https://en.wikipedia.org/wiki/Artificial_intelligence)语言模型，比如[ChatGPT](https://chat.openai.com/)，完成一个句子。你如何选择下一个词，使其不仅**语法正确**而且**上下文相关**？这就是[**Beam Search**](https://en.wikipedia.org/wiki/Beam_search#:~:text=In%20computer%20science%2C%20beam%20search,that%20reduces%20its%20memory%20requirements.) 发挥作用的地方。

通过高效地**并行探索多个可能性**并在每一步保持最佳候选，Beam Search 在预测**后续元素**的任务中发挥了关键作用。作为一种有效且强大的算法，它确保输出符合语法约束和上下文。

要了解 Beam Search 的影响，可以考虑所有需要精确序列生成的应用，如**语言翻译**、**文本完成**和**聊天机器人**。在所有这些应用中，Beam Search 扮演了至关重要的角色。

在这篇文章中，我将介绍理论并指导你通过 Beam Search 算法的实际步骤示例。我还会介绍几种 Beam Search 的变体，并详细说明这个基础算法的所有优缺点。

# 理解 Beam Search
