# 揭示文本生成 AI 的黑箱

> 原文：[https://towardsdatascience.com/illuminating-the-black-box-of-ai-ddea07e65c35?source=collection_archive---------3-----------------------#2023-12-17](https://towardsdatascience.com/illuminating-the-black-box-of-ai-ddea07e65c35?source=collection_archive---------3-----------------------#2023-12-17)

## 见解的需求

[](https://medium.com/@alcarazanthony1?source=post_page-----ddea07e65c35--------------------------------)[![Anthony Alcaraz](../Images/6a71a1752677bd07c384246fb0c7f7e8.png)](https://medium.com/@alcarazanthony1?source=post_page-----ddea07e65c35--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ddea07e65c35--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ddea07e65c35--------------------------------) [Anthony Alcaraz](https://medium.com/@alcarazanthony1?source=post_page-----ddea07e65c35--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F30bc9ffd2f4b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Filluminating-the-black-box-of-ai-ddea07e65c35&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=post_page-30bc9ffd2f4b----ddea07e65c35---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ddea07e65c35--------------------------------) ·8 分钟阅读·2023年12月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fddea07e65c35&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Filluminating-the-black-box-of-ai-ddea07e65c35&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=-----ddea07e65c35---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fddea07e65c35&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Filluminating-the-black-box-of-ai-ddea07e65c35&source=-----ddea07e65c35---------------------bookmark_footer-----------)

*人工智能软件被用来提升本文文本的语法、流畅性和可读性。*

像 ChatGPT、Claude 3、Gemini 和 Mistral 这样的语言模型以其表达能力和博学广受关注。然而，这些大型语言模型仍然是黑箱，隐藏了驱动其回应的复杂机制。它们生成类似人类的文本的能力超过了我们理解其机器思维如何运作的能力。

但随着人工智能被应用于信任和透明度至关重要的场景，如招聘和风险评估，解释性问题开始凸显。解释性不再是复杂系统的可选配件，它是安全推进高影响领域 AI 的必要前提。

为了揭开这些黑箱模型的面纱，充满活力的可解释 NLP 领域提供了不断增长的工具包——从揭示关注模式的注意力可视化，到探测输入的随机部分以量化影响。一些方法，如 LIME，创建了简化模型以局部模仿关键决策。其他方法，如 SHAP，借用合作博弈论的概念，根据模型的最终输出在模型输入的不同部分之间分配“信用”和“责备”。

无论技术如何，所有方法都追求同一个关键目标：阐明语言模型如何利用我们提供的大量文本来编写连贯的段落或…
