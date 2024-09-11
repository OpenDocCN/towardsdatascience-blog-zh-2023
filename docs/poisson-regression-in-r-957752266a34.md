# R中的泊松回归

> 原文：[https://towardsdatascience.com/poisson-regression-in-r-957752266a34?source=collection_archive---------7-----------------------#2023-02-10](https://towardsdatascience.com/poisson-regression-in-r-957752266a34?source=collection_archive---------7-----------------------#2023-02-10)

## R语言中的统计学系列

[](https://mdsohel-mahmood.medium.com/?source=post_page-----957752266a34--------------------------------)[![Md Sohel Mahmood](../Images/7819fed767baf023a725a47050c08f7b.png)](https://mdsohel-mahmood.medium.com/?source=post_page-----957752266a34--------------------------------)[](https://towardsdatascience.com/?source=post_page-----957752266a34--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----957752266a34--------------------------------) [Md Sohel Mahmood](https://mdsohel-mahmood.medium.com/?source=post_page-----957752266a34--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd4a38d280273&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpoisson-regression-in-r-957752266a34&user=Md+Sohel+Mahmood&userId=d4a38d280273&source=post_page-d4a38d280273----957752266a34---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----957752266a34--------------------------------) · 6分钟阅读 · 2023年2月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F957752266a34&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpoisson-regression-in-r-957752266a34&user=Md+Sohel+Mahmood&userId=d4a38d280273&source=-----957752266a34---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F957752266a34&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpoisson-regression-in-r-957752266a34&source=-----957752266a34---------------------bookmark_footer-----------)![](../Images/a5a246e4298f189f0d3c16a11253c2d4.png)

照片由[Michael Dziedzic](https://unsplash.com/@lazycreekimages?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，发布于[Unsplash](https://unsplash.com/photos/ir5gC4hlqT0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> **介绍**

回归分析是一个广阔的领域。我们可以根据数据类型进行几种不同类型的回归分析。我们在之前的文章中详细介绍了逻辑回归。在本文中，我将深入探讨泊松回归，并通过R中的一个示例来实现它。

> **简要背景**

线性回归可以用于数值数据，而逻辑回归则用于分类数据。我们可以对二元变量执行简单的二元逻辑回归，也可以进行多元逻辑回归。根据需要，我们可以选择部分比例赔率模型或广义回归模型。但很多时候，我们需要处理计数数据。例如，博物馆的访问次数可以通过调查收集，为了对这个计数响应变量进行建模，我们需要使用泊松回归。其他例子还包括医院的访问次数或学生在特定时间内修读的数学课程数量。

> **泊松分布**

计数响应变量的泊松分布表示为：

在这里，x = 计数变量，λ = 事件的平均数量。在泊松分布中，事件的平均数量等于该变量的方差……
