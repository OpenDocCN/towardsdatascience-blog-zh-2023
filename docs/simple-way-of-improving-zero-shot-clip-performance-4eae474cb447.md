# 提高零-shot CLIP 性能的简单方法

> 原文：[https://towardsdatascience.com/simple-way-of-improving-zero-shot-clip-performance-4eae474cb447?source=collection_archive---------5-----------------------#2023-11-03](https://towardsdatascience.com/simple-way-of-improving-zero-shot-clip-performance-4eae474cb447?source=collection_archive---------5-----------------------#2023-11-03)

## 第1部分 — 通过语言模型定制提示（CuPL）

[](https://medium.com/@alexml0123?source=post_page-----4eae474cb447--------------------------------)[![Alexey Kravets](../Images/3b31f9b3c73c6c7ca709f845e6f70023.png)](https://medium.com/@alexml0123?source=post_page-----4eae474cb447--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4eae474cb447--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4eae474cb447--------------------------------) [Alexey Kravets](https://medium.com/@alexml0123?source=post_page-----4eae474cb447--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcf3e4a05b535&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimple-way-of-improving-zero-shot-clip-performance-4eae474cb447&user=Alexey+Kravets&userId=cf3e4a05b535&source=post_page-cf3e4a05b535----4eae474cb447---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4eae474cb447--------------------------------) · 12 min 阅读 · 2023年11月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4eae474cb447&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimple-way-of-improving-zero-shot-clip-performance-4eae474cb447&user=Alexey+Kravets&userId=cf3e4a05b535&source=-----4eae474cb447---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4eae474cb447&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimple-way-of-improving-zero-shot-clip-performance-4eae474cb447&source=-----4eae474cb447---------------------bookmark_footer-----------)

单模态模型旨在处理来自单一模态的数据，可能是文本或图像。这些模型专注于理解和生成特定于其选择模态的内容。例如，GPT 擅长生成类人文本，已被用于语言翻译、文本生成和回答问题等任务。卷积神经网络（CNNs）是图像模型的一个例子，在图像分类、物体检测和图像生成等任务中表现出色。目前，许多有趣的任务，如视觉问答（VQA）和图像-文本检索等，要求具备多模态能力。是否可以将文本和图像处理结合起来？可以！CLIP 是最初成功的图像-文本模型之一，展示了在图像识别和文本理解方面的高超能力。

我们将把这篇文章分为以下几个部分：

1.  介绍

1.  架构

1.  训练过程与对比损失

1.  零样本能力

1.  CuPL

1.  结论

# 介绍

CLIP 模型是一个令人印象深刻的零样本预测器，可以在没有明确训练过的任务上进行预测。正如我们在接下来的部分中将详细探讨的那样，通过…
