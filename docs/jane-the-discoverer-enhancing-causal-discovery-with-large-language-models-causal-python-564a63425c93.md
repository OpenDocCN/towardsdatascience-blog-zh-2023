# Jane the Discoverer: 利用大型语言模型（因果 Python）增强因果发现

> 原文：[https://towardsdatascience.com/jane-the-discoverer-enhancing-causal-discovery-with-large-language-models-causal-python-564a63425c93?source=collection_archive---------2-----------------------#2023-10-22](https://towardsdatascience.com/jane-the-discoverer-enhancing-causal-discovery-with-large-language-models-causal-python-564a63425c93?source=collection_archive---------2-----------------------#2023-10-22)

## 一份关于如何利用 LLM 增强因果发现的实用指南，旨在最小化幻觉风险（包含 Python 代码）

[](https://aleksander-molak.medium.com/?source=post_page-----564a63425c93--------------------------------)[![Aleksander Molak](../Images/7fca5018f6ce88297fae31cef1fe0e6c.png)](https://aleksander-molak.medium.com/?source=post_page-----564a63425c93--------------------------------)[](https://towardsdatascience.com/?source=post_page-----564a63425c93--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----564a63425c93--------------------------------) [Aleksander Molak](https://aleksander-molak.medium.com/?source=post_page-----564a63425c93--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff390f1bdd353&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fjane-the-discoverer-enhancing-causal-discovery-with-large-language-models-causal-python-564a63425c93&user=Aleksander+Molak&userId=f390f1bdd353&source=post_page-f390f1bdd353----564a63425c93---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----564a63425c93--------------------------------) ·18分钟阅读·2023年10月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F564a63425c93&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fjane-the-discoverer-enhancing-causal-discovery-with-large-language-models-causal-python-564a63425c93&user=Aleksander+Molak&userId=f390f1bdd353&source=-----564a63425c93---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F564a63425c93&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fjane-the-discoverer-enhancing-causal-discovery-with-large-language-models-causal-python-564a63425c93&source=-----564a63425c93---------------------bookmark_footer-----------)![](../Images/495b9dc6cb0d9d7c93e6b71f72139551.png)

图片由[Artem Podrez](https://www.pexels.com/@artempodrez/)提供，来源于[Pexels.com](http://pexels.com)

> “世界就是一切存在的事物。”
> 
> **路德维希·维特根斯坦 — 逻辑哲学论（1922）**

婴儿在出生时无法理解运动的本质。

我们——人类——以及许多其他非人类动物在来到这个世界时，都带有帮助我们了解环境的系统，但在出生的那一天，我们对环境本身并不了解¹。

我们需要学习。

在这方面，我们与机器学习系统有相似之处。

关于人类和其他非人类动物学习的早期心理学和*(原始)*神经科学发现，激发了建立能够从经验中学习的人工系统的灵感。

2010年代机器学习革命中最成功的学习范式之一是有监督学习。

通过有监督的方式训练的神经网络使我们提前跨越了几十年…
