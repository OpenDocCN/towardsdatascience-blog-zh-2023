# R 中的多项逻辑回归

> 原文：[`towardsdatascience.com/multinomial-logistic-regression-in-r-428d9bb7dc70?source=collection_archive---------2-----------------------#2023-01-29`](https://towardsdatascience.com/multinomial-logistic-regression-in-r-428d9bb7dc70?source=collection_archive---------2-----------------------#2023-01-29)

## R 中的统计系列

[](https://mdsohel-mahmood.medium.com/?source=post_page-----428d9bb7dc70--------------------------------)![Md Sohel Mahmood](https://mdsohel-mahmood.medium.com/?source=post_page-----428d9bb7dc70--------------------------------)[](https://towardsdatascience.com/?source=post_page-----428d9bb7dc70--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----428d9bb7dc70--------------------------------) [Md Sohel Mahmood](https://mdsohel-mahmood.medium.com/?source=post_page-----428d9bb7dc70--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd4a38d280273&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultinomial-logistic-regression-in-r-428d9bb7dc70&user=Md+Sohel+Mahmood&userId=d4a38d280273&source=post_page-d4a38d280273----428d9bb7dc70---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----428d9bb7dc70--------------------------------) ·6 min read·2023 年 1 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F428d9bb7dc70&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultinomial-logistic-regression-in-r-428d9bb7dc70&user=Md+Sohel+Mahmood&userId=d4a38d280273&source=-----428d9bb7dc70---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F428d9bb7dc70&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultinomial-logistic-regression-in-r-428d9bb7dc70&source=-----428d9bb7dc70---------------------bookmark_footer-----------)![](img/4b92b97f4d508c05364527387fd1e0bd.png)

图片由 [Edge2Edge Media](https://unsplash.com/@edge2edgemedia?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/uKlneQRwaxY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> **介绍**

如果你已经阅读了我之前关于“R 语言统计系列”的文章，你可能对逻辑回归在 R 中的实现有了良好的理解，并且对不同类型的逻辑回归模型有了基本的了解。在回归领域，我们已经走过了很长一段路，涵盖了二项回归、比例优势（PO）、广义回归以及部分比例优势（PPO）模型。在这篇文章中，我将讨论多项逻辑回归，它具有特定应用，并且在理解具有多个无序响应变量的逻辑回归模型中可能至关重要。例如，可能发生的回归类型包括人们对政党的归属，例如共和党、民主党、独立党等。

> **简要回顾**

首先，让我们回顾一下我们到目前为止所涵盖的内容。最开始，我们讨论了二项逻辑回归，它没有顺序响应。在这里，我们只有两个类别，例如健康或不健康。接着，我们深入探讨了有多个有序响应的有序逻辑回归模型。在这个模型中，我们创建了更多的健康状态有序类别，并将其输入到有序模型中。这个模型的一个基本假设是系数与……
