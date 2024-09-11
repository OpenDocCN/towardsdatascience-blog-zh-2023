# Deep GPVAR：升级 DeepAR 以实现多维度预测

> 原文：[https://towardsdatascience.com/deep-gpvar-upgrading-deepar-for-multi-dimensional-forecasting-e39204d90af3?source=collection_archive---------2-----------------------#2023-03-24](https://towardsdatascience.com/deep-gpvar-upgrading-deepar-for-multi-dimensional-forecasting-e39204d90af3?source=collection_archive---------2-----------------------#2023-03-24)

## 亚马逊的新时间序列预测模型

[](https://medium.com/@nikoskafritsas?source=post_page-----e39204d90af3--------------------------------)[![Nikos Kafritsas](../Images/de965cfcd8fbd8e1baf849017d365cbb.png)](https://medium.com/@nikoskafritsas?source=post_page-----e39204d90af3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e39204d90af3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e39204d90af3--------------------------------) [Nikos Kafritsas](https://medium.com/@nikoskafritsas?source=post_page-----e39204d90af3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbec849d9e1d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-gpvar-upgrading-deepar-for-multi-dimensional-forecasting-e39204d90af3&user=Nikos+Kafritsas&userId=bec849d9e1d2&source=post_page-bec849d9e1d2----e39204d90af3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e39204d90af3--------------------------------) · 19分钟阅读 · 2023年3月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe39204d90af3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-gpvar-upgrading-deepar-for-multi-dimensional-forecasting-e39204d90af3&user=Nikos+Kafritsas&userId=bec849d9e1d2&source=-----e39204d90af3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe39204d90af3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-gpvar-upgrading-deepar-for-multi-dimensional-forecasting-e39204d90af3&source=-----e39204d90af3---------------------bookmark_footer-----------)![](../Images/c5191da2e60488dbedbc2c1d45651487.png)

使用 DALLE [1] 创建

> **警告**：关于此模型的信息，包括下面的教程，可能已经过时。请查看更新版本 [这里](https://aihorizonforecast.substack.com/p/deep-gpvar-upgrading-deepar-for-multi)

阅读一篇新论文时，最让你享受的是什么？对我来说，以下内容就是：

想象一下，一个流行的模型突然得到升级——只需几个优雅的调整。

三年后，[*DeepAR*](https://medium.com/towards-data-science/deepar-mastering-time-series-forecasting-with-deep-learning-bc717771ce85)[1]，亚马逊工程师发布了其改进版，称为***Deep GPVAR* [2] *(D****eep* ***G****aussian-****P****rocess* ***V****ector* ***A****uto-****r****egressive)*。

这是原始版本的显著改进版。此外，它是开源的。本文讨论了：

+   **深入了解*Deep GPVAR*的工作原理。**

+   **DeepAR和*Deep GPVAR*的不同之处。**

+   **Deep GPVAR解决了哪些问题，为什么它比DeepAR更好？**

+   **关于能源需求预测的实用教程。**

> 我已启动了[**AI Horizon Forecast**](https://aihorizonforecast.substack.com/)**，** 这是一个专注于时间序列和创新AI研究的新闻通讯。订阅[这里](https://aihorizonforecast.substack.com/)以拓宽你的视野！

# *什么是Deep GPVAR？*

> Deep GPVAR是一个自回归深度学习模型，利用...
