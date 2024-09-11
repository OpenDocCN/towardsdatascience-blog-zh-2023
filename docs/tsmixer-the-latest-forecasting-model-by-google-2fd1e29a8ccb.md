# TSMixer：谷歌最新的预测模型

> 原文：[`towardsdatascience.com/tsmixer-the-latest-forecasting-model-by-google-2fd1e29a8ccb?source=collection_archive---------3-----------------------#2023-11-14`](https://towardsdatascience.com/tsmixer-the-latest-forecasting-model-by-google-2fd1e29a8ccb?source=collection_archive---------3-----------------------#2023-11-14)

## 探索 TSMixer 的架构，并在 Python 中实现长期多变量预测任务

[](https://medium.com/@marcopeixeiro?source=post_page-----2fd1e29a8ccb--------------------------------)![Marco Peixeiro](https://medium.com/@marcopeixeiro?source=post_page-----2fd1e29a8ccb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2fd1e29a8ccb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2fd1e29a8ccb--------------------------------) [Marco Peixeiro](https://medium.com/@marcopeixeiro?source=post_page-----2fd1e29a8ccb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F741c1c8fcfbd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftsmixer-the-latest-forecasting-model-by-google-2fd1e29a8ccb&user=Marco+Peixeiro&userId=741c1c8fcfbd&source=post_page-741c1c8fcfbd----2fd1e29a8ccb---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2fd1e29a8ccb--------------------------------) ·12 min read·Nov 14, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2fd1e29a8ccb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftsmixer-the-latest-forecasting-model-by-google-2fd1e29a8ccb&user=Marco+Peixeiro&userId=741c1c8fcfbd&source=-----2fd1e29a8ccb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2fd1e29a8ccb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftsmixer-the-latest-forecasting-model-by-google-2fd1e29a8ccb&source=-----2fd1e29a8ccb---------------------bookmark_footer-----------)![](img/e73873566af0876cf840a6284a7f2f21.png)

照片由[Zdeněk Macháček](https://unsplash.com/@zmachacek?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上拍摄

时间序列预测领域依然充满活力，近期有许多重要的贡献，如 N-HiTS、PatchTST、TimesNet，当然还有 TimeGPT。

与此同时，Transformer 架构在自然语言处理（NLP）领域取得了前所未有的性能，但这并不适用于时间序列预测。

实际上，许多基于 Transformer 的模型如 Autoformer、Informer、FEDformer 等也被提出。这些模型通常训练时间较长，而简单的线性模型在许多基准数据集上表现优于它们（见[郑等，2022](https://arxiv.org/pdf/2205.13504.pdf)）。

针对这一点，2023 年 9 月，Google Cloud AI Research 的研究人员提出了**TSMixer**，一种基于多层感知器（MLP）的模型，专注于混合时间和特征维度，以做出更好的预测。

在他们的论文[TSMixer: An All-MLP Architecture for Time Series Forecasting](https://arxiv.org/pdf/2303.06053.pdf)中，作者展示了该模型在许多基准数据集上达到了最先进的性能，同时实现起来也很简单。
