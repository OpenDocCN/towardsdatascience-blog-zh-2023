# 时间序列预测：深度学习与统计学——谁更胜一筹？

> 原文：[https://towardsdatascience.com/time-series-forecasting-deep-learning-vs-statistics-who-wins-c568389d02df?source=collection_archive---------0-----------------------#2023-04-05](https://towardsdatascience.com/time-series-forecasting-deep-learning-vs-statistics-who-wins-c568389d02df?source=collection_archive---------0-----------------------#2023-04-05)

## 关于**终极困境**的全面指南

[](https://medium.com/@nikoskafritsas?source=post_page-----c568389d02df--------------------------------)[![Nikos Kafritsas](../Images/de965cfcd8fbd8e1baf849017d365cbb.png)](https://medium.com/@nikoskafritsas?source=post_page-----c568389d02df--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c568389d02df--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c568389d02df--------------------------------) [Nikos Kafritsas](https://medium.com/@nikoskafritsas?source=post_page-----c568389d02df--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbec849d9e1d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-forecasting-deep-learning-vs-statistics-who-wins-c568389d02df&user=Nikos+Kafritsas&userId=bec849d9e1d2&source=post_page-bec849d9e1d2----c568389d02df---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c568389d02df--------------------------------) · 14 分钟阅读 · 2023年4月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc568389d02df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-forecasting-deep-learning-vs-statistics-who-wins-c568389d02df&user=Nikos+Kafritsas&userId=bec849d9e1d2&source=-----c568389d02df---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc568389d02df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-forecasting-deep-learning-vs-statistics-who-wins-c568389d02df&source=-----c568389d02df---------------------bookmark_footer-----------)![](../Images/91b4bda3cb99b5a14f28d669d8fa4454.png)

由 Stable Diffusion 创建 [1]

**近年来，深度学习在自然语言处理领域取得了显著进展。**

时间序列，作为一种顺序特性，引发了一个问题：*如果我们将预训练变换器的全部力量带入时间序列预测，会发生什么*？

然而，一些论文，如 [2] 和 [3]，对深度学习模型进行了审查。这些论文并未呈现完整的图景。即使在自然语言处理案例中，有些人也将GPT模型的突破归因于“*更多的数据和计算能力*”，而不是“*更好的机器学习研究*”。

本文旨在澄清混乱并提供客观视角，使用来自学术界和工业界的可靠数据和来源。具体来说，我们将讨论：

+   **深度学习**和**统计模型**的利弊。

+   何时使用统计模型，何时使用深度学习。

+   如何处理预测案例。

+   如何通过选择最适合你的案例和数据集的模型来节省时间和金钱。

让我们深入探讨吧。

> 我已经推出了[**AI Horizon Forecast**](https://aihorizonforecast.substack.com)**，**一份专注于时间序列和创新AI研究的新闻通讯。订阅[这里](https://aihorizonforecast.substack.com)以拓宽你的……
