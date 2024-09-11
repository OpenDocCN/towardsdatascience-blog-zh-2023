# 提高零-shot CLIP 的性能和解释性

> 原文：[`towardsdatascience.com/improving-performance-and-explainability-of-zero-shot-clip-33e579d3f4bb?source=collection_archive---------9-----------------------#2023-11-25`](https://towardsdatascience.com/improving-performance-and-explainability-of-zero-shot-clip-33e579d3f4bb?source=collection_archive---------9-----------------------#2023-11-25)

## 第二部分 — 通过 LLMs 的描述进行视觉分类

[](https://medium.com/@alexml0123?source=post_page-----33e579d3f4bb--------------------------------)![Alexey Kravets](https://medium.com/@alexml0123?source=post_page-----33e579d3f4bb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----33e579d3f4bb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----33e579d3f4bb--------------------------------) [Alexey Kravets](https://medium.com/@alexml0123?source=post_page-----33e579d3f4bb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcf3e4a05b535&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimproving-performance-and-explainability-of-zero-shot-clip-33e579d3f4bb&user=Alexey+Kravets&userId=cf3e4a05b535&source=post_page-cf3e4a05b535----33e579d3f4bb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----33e579d3f4bb--------------------------------) ·6 分钟阅读·2023 年 11 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F33e579d3f4bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimproving-performance-and-explainability-of-zero-shot-clip-33e579d3f4bb&user=Alexey+Kravets&userId=cf3e4a05b535&source=-----33e579d3f4bb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F33e579d3f4bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimproving-performance-and-explainability-of-zero-shot-clip-33e579d3f4bb&source=-----33e579d3f4bb---------------------bookmark_footer-----------)

这是关于提升零-shot CLIP 性能系列文章的第二部分。在第一部分中，我详细解释了 CLIP 模型的操作原理，并描述了一种简单的方法来提高其性能。这涉及到通过大型语言模型（LLM）生成的自定义提示，扩展标准提示，如*“{class}的图片”*。如果你还没有阅读第一部分，可以在[这里](https://medium.com/towards-data-science/simple-way-of-improving-zero-shot-clip-performance-4eae474cb447)找到。在本文中，我们将介绍一种相对类似的方法来提高零-shot CLIP 性能，并且这种方法具有高度的可解释性。

# 介绍

CLIP 模型是一种令人印象深刻的零-shot 预测器，能够对其没有明确训练过的任务进行预测。尽管它具有内在的能力，但仍然存在若干策略可以显著提高其性能。在第一篇文章中，我们已经看到其中一种策略。然而，虽然提升性能是有价值的，但在某些情况下，我们可能愿意做出权衡，以优先考虑更好的可解释性。在系列的第二篇文章中，我们将探讨一种方法，这种方法不仅提升了零-shot CLIP 模型的性能，还确保其预测结果易于理解和解释。

# 深度神经网络的可解释性
