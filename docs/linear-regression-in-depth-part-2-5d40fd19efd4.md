# 线性回归深入探讨（第2部分）

> 原文：[https://towardsdatascience.com/linear-regression-in-depth-part-2-5d40fd19efd4?source=collection_archive---------14-----------------------#2023-04-25](https://towardsdatascience.com/linear-regression-in-depth-part-2-5d40fd19efd4?source=collection_archive---------14-----------------------#2023-04-25)

## 深入探讨多重线性回归及其在 Python 中的示例

[](https://medium.com/@roiyeho?source=post_page-----5d40fd19efd4--------------------------------)[![Dr. Roi Yehoshua](../Images/905a512ffc8879069403a87dbcbeb4db.png)](https://medium.com/@roiyeho?source=post_page-----5d40fd19efd4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5d40fd19efd4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5d40fd19efd4--------------------------------) [Dr. Roi Yehoshua](https://medium.com/@roiyeho?source=post_page-----5d40fd19efd4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3886620c5cf9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-regression-in-depth-part-2-5d40fd19efd4&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=post_page-3886620c5cf9----5d40fd19efd4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5d40fd19efd4--------------------------------) ·14 min read·2023年4月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5d40fd19efd4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-regression-in-depth-part-2-5d40fd19efd4&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=-----5d40fd19efd4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5d40fd19efd4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-regression-in-depth-part-2-5d40fd19efd4&source=-----5d40fd19efd4---------------------bookmark_footer-----------)![](../Images/89c03a1c4491d8f8e9ce98926d1fbb9e.png)

图片由[ThisisEngineering RAEng](https://unsplash.com/@thisisengineering?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，发布在[Unsplash](https://unsplash.com/photos/GzDrm7SYQ0g?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在这篇文章的[第一部分](https://medium.com/towards-data-science/linear-regression-in-depth-part-1-485f997fd611)中，我们正式定义了线性回归问题，并展示了如何解决简单线性回归问题，其中数据集仅包含一个特征。在文章的第二部分，我们将讨论多重线性回归问题，其中数据集可以包含任意数量的特征。

我们将首先将我们为简单线性回归找到的封闭形式解推广到任意数量的特征。然后，我们将建议一种基于梯度下降的替代方法来解决线性回归问题，并讨论这种方法与使用封闭形式解的优缺点。此外，我们还将探索 Scikit-Learn 中实现这两种方法的类，并演示如何在真实数据集上使用它们。

# 多重线性回归定义

回顾一下，在回归问题中，我们给定了一组*n*标记示例：*D* = {(**x**₁, *y*₁), (**x**₂, *y*₂), … , (**x**ₙ, *y*ₙ*)}，其中**x**ᵢ 是一个*m*维向量，包含了示例*i*的**特征**，而 *yᵢ* 是一个实值，表示该示例的**标签**。
