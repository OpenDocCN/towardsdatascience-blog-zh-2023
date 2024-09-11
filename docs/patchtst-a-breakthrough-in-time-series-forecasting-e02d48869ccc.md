# PatchTST：时间序列预测的突破性进展

> 原文：[https://towardsdatascience.com/patchtst-a-breakthrough-in-time-series-forecasting-e02d48869ccc?source=collection_archive---------0-----------------------#2023-06-20](https://towardsdatascience.com/patchtst-a-breakthrough-in-time-series-forecasting-e02d48869ccc?source=collection_archive---------0-----------------------#2023-06-20)

## 从理论到实践，了解PatchTST算法并在Python中与N-BEATS和N-HiTS一起应用

[](https://medium.com/@marcopeixeiro?source=post_page-----e02d48869ccc--------------------------------)[![Marco Peixeiro](../Images/7cf0a81d87281d35ff47f51e3026a3e9.png)](https://medium.com/@marcopeixeiro?source=post_page-----e02d48869ccc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e02d48869ccc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e02d48869ccc--------------------------------) [Marco Peixeiro](https://medium.com/@marcopeixeiro?source=post_page-----e02d48869ccc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F741c1c8fcfbd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpatchtst-a-breakthrough-in-time-series-forecasting-e02d48869ccc&user=Marco+Peixeiro&userId=741c1c8fcfbd&source=post_page-741c1c8fcfbd----e02d48869ccc---------------------post_header-----------) 发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----e02d48869ccc--------------------------------) ·10分钟阅读·2023年6月20日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe02d48869ccc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpatchtst-a-breakthrough-in-time-series-forecasting-e02d48869ccc&source=-----e02d48869ccc---------------------bookmark_footer-----------)![](../Images/3a14de30ec6761d39da8521d597c09d3.png)

照片由[Ray Hennessy](https://unsplash.com/@rayhennessy?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上

基于Transformer的模型已经成功应用于许多领域，如自然语言处理（例如BERT或GPT模型）和计算机视觉等。

然而，在时间序列方面，最先进的结果大多由 MLP 模型（多层感知器）如 [N-BEATS](https://medium.com/towards-data-science/the-easiest-way-to-forecast-time-series-using-n-beats-d778fcc2ba60) 和 [N-HiTS](/all-about-n-hits-the-latest-breakthrough-in-time-series-forecasting-a8ddcb27b0d5) 实现。最近的一篇论文甚至表明，在许多基准数据集上，简单的线性模型优于复杂的基于变换器的预测模型（参见 [Zheng et al., 2022](https://arxiv.org/pdf/2205.13504.pdf)）。

尽管如此，已经提出了一种新的基于变换器的模型，它在长期预测任务中取得了**最先进的**结果：**PatchTST**。

PatchTST 代表补丁时间序列变换器，它由 Nie、Nguyen 等人在他们的论文中首次提出：[时间序列值64词：使用变换器进行长期预测](https://arxiv.org/pdf/2211.14730.pdf)。与其他基于变换器的模型相比，他们提出的方法取得了最先进的结果。

在本文中，我们首先直观地探索 PatchTST 的内部工作原理，而不使用方程。然后，我们将该模型应用于预测项目并进行比较……
