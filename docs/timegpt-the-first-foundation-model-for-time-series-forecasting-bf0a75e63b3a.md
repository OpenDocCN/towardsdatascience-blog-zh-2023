# TimeGPT：时间序列预测的第一个基础模型

> 原文：[https://towardsdatascience.com/timegpt-the-first-foundation-model-for-time-series-forecasting-bf0a75e63b3a?source=collection_archive---------0-----------------------#2023-10-24](https://towardsdatascience.com/timegpt-the-first-foundation-model-for-time-series-forecasting-bf0a75e63b3a?source=collection_archive---------0-----------------------#2023-10-24)

## 探索第一个生成预训练的预测模型，并在项目中使用Python应用。

[](https://medium.com/@marcopeixeiro?source=post_page-----bf0a75e63b3a--------------------------------)[![Marco Peixeiro](../Images/7cf0a81d87281d35ff47f51e3026a3e9.png)](https://medium.com/@marcopeixeiro?source=post_page-----bf0a75e63b3a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bf0a75e63b3a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bf0a75e63b3a--------------------------------) [Marco Peixeiro](https://medium.com/@marcopeixeiro?source=post_page-----bf0a75e63b3a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F741c1c8fcfbd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftimegpt-the-first-foundation-model-for-time-series-forecasting-bf0a75e63b3a&user=Marco+Peixeiro&userId=741c1c8fcfbd&source=post_page-741c1c8fcfbd----bf0a75e63b3a---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----bf0a75e63b3a--------------------------------) ·12 分钟阅读·2023年10月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbf0a75e63b3a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftimegpt-the-first-foundation-model-for-time-series-forecasting-bf0a75e63b3a&user=Marco+Peixeiro&userId=741c1c8fcfbd&source=-----bf0a75e63b3a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbf0a75e63b3a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftimegpt-the-first-foundation-model-for-time-series-forecasting-bf0a75e63b3a&source=-----bf0a75e63b3a---------------------bookmark_footer-----------)![](../Images/69c006f4feeba3f9fe392aff3b2a35f0.png)

照片由[Boris Smokrovic](https://unsplash.com/@borisworkshop?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上拍摄。

时间序列预测领域正经历一个非常激动人心的时期。在过去的三年里，我们看到了许多重要的贡献，例如[N-BEATS](https://medium.com/towards-data-science/the-easiest-way-to-forecast-time-series-using-n-beats-d778fcc2ba60)、[N-HiTS](https://medium.com/towards-data-science/all-about-n-hits-the-latest-breakthrough-in-time-series-forecasting-a8ddcb27b0d5)、[PatchTST](https://medium.com/towards-data-science/patchtst-a-breakthrough-in-time-series-forecasting-e02d48869ccc)和[TimesNet](https://medium.com/towards-data-science/timesnet-the-latest-advance-in-time-series-forecasting-745b69068c9c)。

与此同时，[大型语言模型（LLMs）](https://medium.com/towards-data-science/catch-up-on-large-language-models-8daf784f46f8)最近也获得了广泛关注，如ChatGPT，它们能够适应各种任务而无需进一步训练。

这引出了一个问题：是否可以像自然语言处理领域一样，为时间序列领域存在基础模型？一个在大量时间序列数据上预训练的大型模型是否能够在未见过的数据上产生准确的预测？

在[TimeGPT-1](https://arxiv.org/pdf/2310.03589.pdf)中，Azul Garza 和 Max Mergenthaler-Canseco 提出了将LLMs背后的技术和架构应用于预测领域，成功构建了第一个能够进行零-shot推理的时间序列基础模型。

在这篇文章中，我们首先探讨了TimeGPT的架构以及模型的训练过程。然后，我们将在一个预测项目中应用它，以评估其相对于其他模型的性能……
