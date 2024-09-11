# 各种逻辑回归模型中的预测（第二部分）

> 原文：[https://towardsdatascience.com/prediction-in-various-logistic-regression-models-part-2-f8994e306a4c?source=collection_archive---------11-----------------------#2023-04-27](https://towardsdatascience.com/prediction-in-various-logistic-regression-models-part-2-f8994e306a4c?source=collection_archive---------11-----------------------#2023-04-27)

## R 统计系列

[](https://mdsohel-mahmood.medium.com/?source=post_page-----f8994e306a4c--------------------------------)[![Md Sohel Mahmood](../Images/7819fed767baf023a725a47050c08f7b.png)](https://mdsohel-mahmood.medium.com/?source=post_page-----f8994e306a4c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f8994e306a4c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f8994e306a4c--------------------------------) [Md Sohel Mahmood](https://mdsohel-mahmood.medium.com/?source=post_page-----f8994e306a4c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd4a38d280273&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprediction-in-various-logistic-regression-models-part-2-f8994e306a4c&user=Md+Sohel+Mahmood&userId=d4a38d280273&source=post_page-d4a38d280273----f8994e306a4c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f8994e306a4c--------------------------------) · 8 分钟阅读 · 2023年4月27日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff8994e306a4c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprediction-in-various-logistic-regression-models-part-2-f8994e306a4c&source=-----f8994e306a4c---------------------bookmark_footer-----------)![](../Images/5c26d66472baa3d5092e9d3b71163bc3.png)

图片来源：[Vladimir Fedotov](https://unsplash.com/@fedotov_vs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 于 [Unsplash](https://unsplash.com/backgrounds/colors/light?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> **介绍**

我们已经涵盖了二元和序数数据的逻辑回归模型，并且展示了如何在R中实现该模型。此外，使用R库的预测分析也在之前的文章中进行了讨论。我们已经看到单个和多个预测变量对响应变量的影响，并进行了量化。采用了二元和序数响应变量来展示如何处理不同类型的数据。在本文中，我们将介绍四种逻辑回归模型的预测分析，包括广义序数回归模型，[部分比例奇数模型](https://medium.com/towards-data-science/partial-proportional-odd-model-in-r-dc5face36e40)，[多项式逻辑回归](https://medium.com/towards-data-science/multinomial-logistic-regression-in-r-428d9bb7dc70)模型和[泊松回归](https://medium.com/towards-data-science/poisson-regression-in-r-957752266a34)模型。

> **数据集**

我们的研究将使用相同的[UCI机器学习库的成人数据集](https://archive.ics.uci.edu/ml/datasets/adult)作为案例研究。该数据集收集了超过30000个个体的人口统计数据。数据包括每个个体的种族、教育、工作、性别、薪资、工作经历、每周工作小时数和收入。为了复习，考虑的变量如下所示。

+   教育：数值型和连续型。个人的健康状况可以受到教育的显著影响。

+   婚姻状况：二元（0表示...
