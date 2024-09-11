# 处理转换指标？考虑贝塔-二项模型

> 原文：[https://towardsdatascience.com/dealing-with-conversion-metrics-consider-beta-binomial-model-29733906ff38?source=collection_archive---------8-----------------------#2023-07-26](https://towardsdatascience.com/dealing-with-conversion-metrics-consider-beta-binomial-model-29733906ff38?source=collection_archive---------8-----------------------#2023-07-26)

![](../Images/52c7b004ff37e2cbacac7625eaf7ab8d.png)

图片由 [Karim MANJRA](https://unsplash.com/@karim_manjra?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 学习一种特征工程技术，使基于转换的指标如CTR/CVR更加具代表性和稳定

[](https://medium.com/@pararawendy19?source=post_page-----29733906ff38--------------------------------)[![Pararawendy Indarjo](../Images/afba0cb7f3af9554a187bbc7a3c00e60.png)](https://medium.com/@pararawendy19?source=post_page-----29733906ff38--------------------------------)[](https://towardsdatascience.com/?source=post_page-----29733906ff38--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----29733906ff38--------------------------------) [Pararawendy Indarjo](https://medium.com/@pararawendy19?source=post_page-----29733906ff38--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcf29bfb08abb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdealing-with-conversion-metrics-consider-beta-binomial-model-29733906ff38&user=Pararawendy+Indarjo&userId=cf29bfb08abb&source=post_page-cf29bfb08abb----29733906ff38---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----29733906ff38--------------------------------) ·10分钟阅读·2023年7月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F29733906ff38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdealing-with-conversion-metrics-consider-beta-binomial-model-29733906ff38&user=Pararawendy+Indarjo&userId=cf29bfb08abb&source=-----29733906ff38---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F29733906ff38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdealing-with-conversion-metrics-consider-beta-binomial-model-29733906ff38&source=-----29733906ff38---------------------bookmark_footer-----------)

行业内转换指标丰富。而且，我们常常希望将它们作为特征用于我们的机器学习模型。比如，搜索页面上显示的产品的展示点击转化率（CTR）可能与模型中是否会购买该产品相关，进而被用作一个特征。

在这篇博客中，我们将学习一种用于此类转换指标的特征工程技术。为了实现这一目标，博客的其余部分将按以下结构进行。

1.  解释为什么我们需要谨慎处理转换特征（即，我们不应使用这些特征的原始形式）。

1.  解决方案：Beta-二项式模型将原始转换值转化为更稳定/具有代表性的版本

1.  Beta-二项式模型的理论基础

1.  调整模型Beta-先验分布参数的指南

1.  进行Beta-二项式转换的Python代码（提示：非常简单！）
