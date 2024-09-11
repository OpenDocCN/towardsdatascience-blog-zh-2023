# 如何检测机器学习模型中的漂移

> 原文：[`towardsdatascience.com/how-to-detect-drift-in-machine-learning-models-8a0be4049eed?source=collection_archive---------5-----------------------#2023-02-06`](https://towardsdatascience.com/how-to-detect-drift-in-machine-learning-models-8a0be4049eed?source=collection_archive---------5-----------------------#2023-02-06)

## *这可能是为什么你的模型在生产中性能下降的原因*

[](https://medium.com/@edwin.tan?source=post_page-----8a0be4049eed--------------------------------)![Edwin Tan](https://medium.com/@edwin.tan?source=post_page-----8a0be4049eed--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8a0be4049eed--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8a0be4049eed--------------------------------) [Edwin Tan](https://medium.com/@edwin.tan?source=post_page-----8a0be4049eed--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F484e02c96aa7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-detect-drift-in-machine-learning-models-8a0be4049eed&user=Edwin+Tan&userId=484e02c96aa7&source=post_page-484e02c96aa7----8a0be4049eed---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8a0be4049eed--------------------------------) ·8 分钟阅读·2023 年 2 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8a0be4049eed&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-detect-drift-in-machine-learning-models-8a0be4049eed&user=Edwin+Tan&userId=484e02c96aa7&source=-----8a0be4049eed---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8a0be4049eed&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-detect-drift-in-machine-learning-models-8a0be4049eed&source=-----8a0be4049eed---------------------bookmark_footer-----------)

# 介绍

你是否曾在测试集上获得了很好的结果，但在一段时间后模型在生产中表现不佳？如果是这样，你可能正在经历模型衰退。模型衰退是机器学习模型性能随着时间逐渐下降的现象。在这篇文章中，我们将探讨数据漂移如何导致模型衰退以及如何设置早期检测以发现漂移。

![](img/848e5f894f69fdf863aa02d8252caf76.png)

图片由 [Samuel Wong](https://unsplash.com/@samuelwong?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**机器学习中的漂移是什么？**

在机器学习中，模型漂移指的是模型所训练的数据的基础分布发生变化，从而导致模型在新数据上的性能下降。这种情况可能发生在模型部署到实际环境中时，模型所遇到的数据分布随着时间的推移发生变化。例如，一个在新冠疫情前训练的模型可能在新冠疫情期间的数据上表现不佳，因为数据的基础分布发生了变化。

**为什么跟踪模型漂移很重要？**

跟踪漂移很重要，因为它有助于确保机器学习模型随着时间的推移继续做出准确的预测。随着模型的部署…
