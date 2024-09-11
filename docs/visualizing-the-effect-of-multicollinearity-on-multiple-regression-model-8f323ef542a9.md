# 可视化多重共线性对多重回归模型的影响

> 原文：[https://towardsdatascience.com/visualizing-the-effect-of-multicollinearity-on-multiple-regression-model-8f323ef542a9?source=collection_archive---------3-----------------------#2023-06-12](https://towardsdatascience.com/visualizing-the-effect-of-multicollinearity-on-multiple-regression-model-8f323ef542a9?source=collection_archive---------3-----------------------#2023-06-12)

## 使用 Python 进行数据可视化来解释多重共线性对多重回归的影响

[](https://medium.com/@borih.k?source=post_page-----8f323ef542a9--------------------------------)[![Boriharn K](../Images/1b23a79640f5272c1382918bfdba03b0.png)](https://medium.com/@borih.k?source=post_page-----8f323ef542a9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8f323ef542a9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8f323ef542a9--------------------------------) [Boriharn K](https://medium.com/@borih.k?source=post_page-----8f323ef542a9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe20a7f1ba78f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualizing-the-effect-of-multicollinearity-on-multiple-regression-model-8f323ef542a9&user=Boriharn+K&userId=e20a7f1ba78f&source=post_page-e20a7f1ba78f----8f323ef542a9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8f323ef542a9--------------------------------) · 11 分钟阅读 · 2023年6月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8f323ef542a9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualizing-the-effect-of-multicollinearity-on-multiple-regression-model-8f323ef542a9&user=Boriharn+K&userId=e20a7f1ba78f&source=-----8f323ef542a9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8f323ef542a9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualizing-the-effect-of-multicollinearity-on-multiple-regression-model-8f323ef542a9&source=-----8f323ef542a9---------------------bookmark_footer-----------)![](../Images/078e3866353a1df09115ce1da99845c9.png)

图片由 [Alex](https://unsplash.com/@brizmaker?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## **什么是多重共线性？**

在多重回归中，当一个预测变量（自变量）与模型中的一个或多个其他预测变量高度相关时，就会发生多重共线性。

## **为什么这很重要？**

```py
### Multiple regression equation:

Y = β₀ + β₁X₁ + β₂X₂ + ... + βᵢXᵢ + ε
```

理论上，正如我们在方程中看到的，多元回归使用多个预测变量来预测因变量的值。

顺便说一下，多元回归通过确定一个预测变量单位均值的变化对因变量的影响，同时保持其他预测变量不变来工作。

如果一个预测变量与其他变量高度相关，那么要改变这个变量而不改变其他变量将非常困难。

## **多重共线性的影响**

简而言之，多重共线性会影响模型系数。数据的微小变化可能会影响系数估计。因此，解释变得困难……
