# 分类特征：标签编码的问题在哪里？

> 原文：[`towardsdatascience.com/categorical-features-whats-wrong-with-label-encoding-81184c3dfb69?source=collection_archive---------1-----------------------#2023-11-20`](https://towardsdatascience.com/categorical-features-whats-wrong-with-label-encoding-81184c3dfb69?source=collection_archive---------1-----------------------#2023-11-20)

## 为什么我们不能随意编码分类特征

[](https://harrisonfhoffman.medium.com/?source=post_page-----81184c3dfb69--------------------------------)![哈里森·霍夫曼](https://harrisonfhoffman.medium.com/?source=post_page-----81184c3dfb69--------------------------------)[](https://towardsdatascience.com/?source=post_page-----81184c3dfb69--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----81184c3dfb69--------------------------------) [哈里森·霍夫曼](https://harrisonfhoffman.medium.com/?source=post_page-----81184c3dfb69--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F38889d0801d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcategorical-features-whats-wrong-with-label-encoding-81184c3dfb69&user=Harrison+Hoffman&userId=38889d0801d0&source=post_page-38889d0801d0----81184c3dfb69---------------------post_header-----------) 发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----81184c3dfb69--------------------------------) ·10 分钟阅读·2023 年 11 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F81184c3dfb69&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcategorical-features-whats-wrong-with-label-encoding-81184c3dfb69&user=Harrison+Hoffman&userId=38889d0801d0&source=-----81184c3dfb69---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F81184c3dfb69&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcategorical-features-whats-wrong-with-label-encoding-81184c3dfb69&source=-----81184c3dfb69---------------------bookmark_footer-----------)![](img/49e87931489a1c96a6402b71184b64fa.png)

云朵。作者提供的图片。

众所周知，**许多**机器学习模型无法直接处理分类特征。虽然也有一些例外情况，但通常由实践者决定每个分类特征的数值表示。有[许多方法](https://contrib.scikit-learn.org/category_encoders/)可以实现这一点，但很少推荐的策略之一是[标签编码](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html)。

标签编码会用任意数值替换每个分类值。例如，如果我们有一个包含字母的特征，标签编码可能会将字母“A”赋值为 0，字母“B”赋值为 1，依此类推，直到“Z”被赋值为 25。在此过程之后，理论上任何算法都应该能处理已编码的特征。

但问题出在哪里？难道精密的机器学习模型不应该能处理这种类型的编码吗？为什么像[Catboost](https://catboost.ai/)和[其他编码策略](https://contrib.scikit-learn.org/category_encoders/)这样的库存在来处理高基数分类特征？

这篇文章将探讨两个示例，展示了**为什么**标签编码可能对机器学习模型有问题。这些示例将帮助我们理解为什么有这么多[替代方案](https://contrib.scikit-learn.org/category_encoders/)来替代标签编码，而且它将深化我们对…
