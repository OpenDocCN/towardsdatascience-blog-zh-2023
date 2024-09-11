# 如何正确地对时间序列进行交叉验证

> 原文：[`towardsdatascience.com/how-to-correctly-perform-cross-validation-for-time-series-b083b869e42c?source=collection_archive---------4-----------------------#2023-01-10`](https://towardsdatascience.com/how-to-correctly-perform-cross-validation-for-time-series-b083b869e42c?source=collection_archive---------4-----------------------#2023-01-10)

## 避免在对时间序列和预测模型应用交叉验证时常见的陷阱。

[](https://medium.com/@egorhowell?source=post_page-----b083b869e42c--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----b083b869e42c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b083b869e42c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b083b869e42c--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----b083b869e42c--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-correctly-perform-cross-validation-for-time-series-b083b869e42c&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----b083b869e42c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b083b869e42c--------------------------------) ·5 分钟阅读·2023 年 1 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb083b869e42c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-correctly-perform-cross-validation-for-time-series-b083b869e42c&user=Egor+Howell&userId=1cac491223b2&source=-----b083b869e42c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb083b869e42c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-correctly-perform-cross-validation-for-time-series-b083b869e42c&source=-----b083b869e42c---------------------bookmark_footer-----------)![](img/7f3a6d6bf2bca0dd5fc4e9b0dbb20ffc.png)

图片由 [aceofnet](https://unsplash.com/@aceofnet?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

[交叉验证](https://www.youtube.com/watch?v=1rZpbvSI26c&t=29s)是构建任何统计或机器学习模型时的一个基础过程，并且在数据科学中无处不在。然而，对于更专业的 [时间序列分析](https://en.wikipedia.org/wiki/Time_series) 和预测领域，*错误*地进行交叉验证非常容易。

在这篇文章中，我想展示将常规交叉验证应用于时间序列模型的问题以及缓解这些问题的常见方法。我们还将通过一个使用交叉验证进行*超参数调优*的示例来探讨在 Python 中的时间序列模型。

# 什么是交叉验证？

交叉验证是一种通过在数据的不同部分上训练和测试模型来确定最佳表现模型和参数的方法。最常见和基本的方法是经典的*训练-测试拆分*。这是我们将…
