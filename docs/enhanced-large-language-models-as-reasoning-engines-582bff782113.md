# 增强的大型语言模型作为推理引擎

> 原文：[`towardsdatascience.com/enhanced-large-language-models-as-reasoning-engines-582bff782113?source=collection_archive---------3-----------------------#2023-12-23`](https://towardsdatascience.com/enhanced-large-language-models-as-reasoning-engines-582bff782113?source=collection_archive---------3-----------------------#2023-12-23)

[](https://medium.com/@alcarazanthony1?source=post_page-----582bff782113--------------------------------)![Anthony Alcaraz](https://medium.com/@alcarazanthony1?source=post_page-----582bff782113--------------------------------)[](https://towardsdatascience.com/?source=post_page-----582bff782113--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----582bff782113--------------------------------) [Anthony Alcaraz](https://medium.com/@alcarazanthony1?source=post_page-----582bff782113--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F30bc9ffd2f4b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhanced-large-language-models-as-reasoning-engines-582bff782113&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=post_page-30bc9ffd2f4b----582bff782113---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----582bff782113--------------------------------) ·12 min read·2023 年 12 月 23 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F582bff782113&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhanced-large-language-models-as-reasoning-engines-582bff782113&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=-----582bff782113---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F582bff782113&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhanced-large-language-models-as-reasoning-engines-582bff782113&source=-----582bff782113---------------------bookmark_footer-----------)

*人工智能软件被用来增强本文的语法、流畅性和可读性。*

最近，大型语言模型（LLMs）在自然语言处理能力上的指数级进展引发了对其实现人类水平智能潜力的巨大兴奋。它们在接触到大量数据集后，能够生成非常连贯的文本并进行对话，这似乎指向了灵活的、通用的推理能力。

然而，越来越多的声音呼吁在盲目乐观的同时保持谨慎，指出神经方法的根本盲点。大型语言模型（LLMs）仍然经常犯一些基本的逻辑和数学错误，这揭示了其回应背后缺乏系统性。它们的知识依然本质上是统计性的，没有更深层次的语义结构。

更复杂的推理任务进一步暴露了这些局限性。大型语言模型在因果推理、反事实推理和组合推理等挑战面前挣扎，这些挑战需要超越表面模式识别。与学习抽象模式以灵活重组模块化概念的人类不同，神经网络记忆的是共现术语之间的关联。这导致了在狭窄训练分布之外的脆弱泛化。

这种差距强调了人类认知如何利用结构化的符号表示来实现系统性的组合性和因果模型，以便对动态进行概念化。我们通过操控…
