# 一份全面的 ML 实验跟踪工具对比

> 原文：[https://towardsdatascience.com/a-comprehensive-comparison-of-ml-experiment-tracking-tools-9f0192543feb?source=collection_archive---------7-----------------------#2023-04-26](https://towardsdatascience.com/a-comprehensive-comparison-of-ml-experiment-tracking-tools-9f0192543feb?source=collection_archive---------7-----------------------#2023-04-26)

![](../Images/2899664dc480f7c5aeb24145ebb7f7e0.png)

照片由 [BoliviaInteligente](https://unsplash.com/@boliviainteligente?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/photos/pwfFRdS-A-I?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 7 种领先工具的优缺点

[](https://eryk-lewinson.medium.com/?source=post_page-----9f0192543feb--------------------------------)[![Eryk Lewinson](../Images/56e09e19c0bbfecc582da58761d15078.png)](https://eryk-lewinson.medium.com/?source=post_page-----9f0192543feb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9f0192543feb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9f0192543feb--------------------------------) [Eryk Lewinson](https://eryk-lewinson.medium.com/?source=post_page-----9f0192543feb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F44bc27317e6b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-comprehensive-comparison-of-ml-experiment-tracking-tools-9f0192543feb&user=Eryk+Lewinson&userId=44bc27317e6b&source=post_page-44bc27317e6b----9f0192543feb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9f0192543feb--------------------------------) · 12 分钟阅读 · 2023年4月26日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9f0192543feb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-comprehensive-comparison-of-ml-experiment-tracking-tools-9f0192543feb&user=Eryk+Lewinson&userId=44bc27317e6b&source=-----9f0192543feb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9f0192543feb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-comprehensive-comparison-of-ml-experiment-tracking-tools-9f0192543feb&source=-----9f0192543feb---------------------bookmark_footer-----------)

构建机器学习模型是一个高度迭代的过程。在为我们的项目构建一个简单的 MVP 之后，我们很可能会进行一系列实验，尝试不同的模型（及其超参数）、创建或添加各种特征，或利用数据预处理技术。所有这些都是为了实现更好的性能。

随着实验数量的增加，跟踪它们变得具有挑战性。这时，一张纸或一个 Excel 表格可能已经不够用了。此外，当我们需要在投入生产之前重新生成表现最佳的实验时，复杂性会进一步增加。这就是 ML 实验跟踪发挥作用的地方！

# 什么是 ML 实验跟踪？

ML 实验跟踪是记录、组织和分析机器学习实验结果的过程。它帮助数据科学家跟踪他们的实验、重现他们的结果，并有效地与他人协作。实验跟踪工具使我们能够记录实验元数据，如超参数、数据集/代码版本和模型性能指标。此外，我们可以轻松地可视化实验结果和…
