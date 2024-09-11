# 点积在人工智能中的力量

> 原文：[`towardsdatascience.com/the-power-of-the-dot-product-in-artificial-intelligence-c002331e1829?source=collection_archive---------7-----------------------#2023-05-15`](https://towardsdatascience.com/the-power-of-the-dot-product-in-artificial-intelligence-c002331e1829?source=collection_archive---------7-----------------------#2023-05-15)

## 一个简单工具如何引发惊人的复杂性

[](https://manuel-brenner.medium.com/?source=post_page-----c002331e1829--------------------------------)![Manuel Brenner](https://manuel-brenner.medium.com/?source=post_page-----c002331e1829--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c002331e1829--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c002331e1829--------------------------------) [Manuel Brenner](https://manuel-brenner.medium.com/?source=post_page-----c002331e1829--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fde95441432&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-power-of-the-dot-product-in-artificial-intelligence-c002331e1829&user=Manuel+Brenner&userId=1fde95441432&source=post_page-1fde95441432----c002331e1829---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c002331e1829--------------------------------) ·9 min read·2023 年 5 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc002331e1829&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-power-of-the-dot-product-in-artificial-intelligence-c002331e1829&user=Manuel+Brenner&userId=1fde95441432&source=-----c002331e1829---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc002331e1829&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-power-of-the-dot-product-in-artificial-intelligence-c002331e1829&source=-----c002331e1829---------------------bookmark_footer-----------)

将简单的事物扩展以实现更复杂的目标是一个强大的理念，它奠定了[生活](https://manuel-brenner.medium.com/the-importance-of-noise-327fcab7c4fb)、计算机（基于[Turing 机器](https://medium.com/discourse/a-non-technical-guide-to-turing-machines-f8c6da9596e5)的简单性）以及深度学习技术（基于神经网络中的神经元理念）的基础。

[更多即常常不同](https://manuel-brenner.medium.com/more-is-different-a49e833260b3)，这一点我们在前沿架构不断扩展（如大型语言模型 [GPT](https://arxiv.org/abs/2005.14165) 正在改变我们所处的世界）时依然可以观察到，从而带来更好的泛化能力和令人印象深刻的结果。

研究人员声称，大型语言模型展示了[涌现能力](https://openreview.net/pdf?id=yzkSU5zdwD)，这些能力的实现并没有太多的架构创新，而主要来自于计算资源的增加和大规模重复简单操作（有关警示，请参见[这篇文章](https://arxiv.org/abs/2304.15004)）。

尽管训练深度学习模型需要社区多年积累的独创性和技术，但大多数深度学习方法的基本构建块仍然相当简单，仅由少数数学操作组成。

也许最重要的是点积。在本文的其余部分，我想深入探讨点积的作用，它为何如此重要，以及为何将其扩展至今已达到令人惊叹的人工智能水平。
