# 使用冻结的大型语言模型进行视觉问答

> 原文：[https://towardsdatascience.com/visual-question-answering-with-frozen-large-language-models-353d42791054?source=collection_archive---------4-----------------------#2023-10-09](https://towardsdatascience.com/visual-question-answering-with-frozen-large-language-models-353d42791054?source=collection_archive---------4-----------------------#2023-10-09)

## 与 LLMs 讨论图像，而不对 LLMs 进行图像训练。

[](https://medium.com/@danielwarfield1?source=post_page-----353d42791054--------------------------------)[![Daniel Warfield](../Images/c1c8b4dd514f6813e08e401401324bca.png)](https://medium.com/@danielwarfield1?source=post_page-----353d42791054--------------------------------)[](https://towardsdatascience.com/?source=post_page-----353d42791054--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----353d42791054--------------------------------) [Daniel Warfield](https://medium.com/@danielwarfield1?source=post_page-----353d42791054--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbdc4072cbfdc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisual-question-answering-with-frozen-large-language-models-353d42791054&user=Daniel+Warfield&userId=bdc4072cbfdc&source=post_page-bdc4072cbfdc----353d42791054---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----353d42791054--------------------------------) ·18分钟阅读·2023年10月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F353d42791054&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisual-question-answering-with-frozen-large-language-models-353d42791054&user=Daniel+Warfield&userId=bdc4072cbfdc&source=-----353d42791054---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F353d42791054&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisual-question-answering-with-frozen-large-language-models-353d42791054&source=-----353d42791054---------------------bookmark_footer-----------)![](../Images/20a13db13c9eda0a5658fa9c857f7ff0.png)

“桥接模态”，由 MidJourney 制作。所有图片均由作者提供，除非另有说明。

在本文中，我们将使用 Q-Former，这是一种桥接计算机视觉和自然语言模型的技术，以创建一个视觉问答系统。我们将深入讨论所需的理论，参考 [BLIP-2 论文](https://arxiv.org/abs/2301.12597)，然后实现一个可以用来与大型语言模型讨论图像的系统。

![](../Images/d999ca849fd33e7ae32b9d4a3a667559.png)

我们将构建的内容

**这对谁有用？** 对计算机视觉、自然语言处理和多模态建模感兴趣的数据科学家。

**这篇文章的难度如何？** 中级。如果你在计算机视觉和自然语言处理方面没有一些经验，你可能会感到吃力。

**前提条件：** 对变换器、嵌入和编码解码器有较高的熟悉度。所有这些主题都在以下文章中涵盖：
