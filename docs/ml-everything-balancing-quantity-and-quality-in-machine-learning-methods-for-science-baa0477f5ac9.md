# “ML-一切”？在科学中的机器学习方法中平衡数量和质量

> 原文：[https://towardsdatascience.com/ml-everything-balancing-quantity-and-quality-in-machine-learning-methods-for-science-baa0477f5ac9?source=collection_archive---------13-----------------------#2023-03-14](https://towardsdatascience.com/ml-everything-balancing-quantity-and-quality-in-machine-learning-methods-for-science-baa0477f5ac9?source=collection_archive---------13-----------------------#2023-03-14)

## 需要适当的验证和优质的数据集，这些数据集既客观又平衡，并且预测在现实场景中是有用的。

[](https://lucianosphere.medium.com/?source=post_page-----baa0477f5ac9--------------------------------)[![LucianoSphere (Luciano Abriata, PhD)](../Images/a8ae3085d094749bbdd1169cca672b86.png)](https://lucianosphere.medium.com/?source=post_page-----baa0477f5ac9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----baa0477f5ac9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----baa0477f5ac9--------------------------------) [LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----baa0477f5ac9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd28939b5ab78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fml-everything-balancing-quantity-and-quality-in-machine-learning-methods-for-science-baa0477f5ac9&user=LucianoSphere+%28Luciano+Abriata%2C+PhD%29&userId=d28939b5ab78&source=post_page-d28939b5ab78----baa0477f5ac9---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----baa0477f5ac9--------------------------------) ·12分钟阅读·2023年3月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbaa0477f5ac9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fml-everything-balancing-quantity-and-quality-in-machine-learning-methods-for-science-baa0477f5ac9&user=LucianoSphere+%28Luciano+Abriata%2C+PhD%29&userId=d28939b5ab78&source=-----baa0477f5ac9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbaa0477f5ac9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fml-everything-balancing-quantity-and-quality-in-machine-learning-methods-for-science-baa0477f5ac9&source=-----baa0477f5ac9---------------------bookmark_footer-----------)![](../Images/61425b6c60082fe85bbe42e083edbd99.png)

图片由 [Tingey Injury Law Firm](https://unsplash.com/@tingeyinjurylawfirm?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

近期的机器学习（ML）研究在多个领域取得了显著进展，包括科学应用。然而，仍有一些限制需要解决，以确保新模型的有效性、测试和验证程序的质量，以及所开发模型对实际问题的适用性。这些限制包括不公平、主观和不平衡的评估，这些评估可能不是有意为之，但确实存在；使用无法正确反映现实世界使用案例的数据集（例如“过于简单”的数据集）；将数据集错误地划分为训练、测试和验证子集等。在本文中，我将讨论所有这些问题，并使用生物学领域的例子，该领域正受到ML方法的革命性影响。

在此过程中，我还将简要讨论ML模型的可解释性，目前这一点非常有限但却极为重要，因为它可以帮助澄清文章第一部分讨论的许多方面。
