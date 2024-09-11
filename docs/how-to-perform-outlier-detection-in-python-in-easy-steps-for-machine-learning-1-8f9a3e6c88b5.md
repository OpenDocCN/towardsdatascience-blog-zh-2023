# 如何在Python中进行异常值检测以进行机器学习：第1部分

> 原文：[https://towardsdatascience.com/how-to-perform-outlier-detection-in-python-in-easy-steps-for-machine-learning-1-8f9a3e6c88b5?source=collection_archive---------2-----------------------#2023-01-28](https://towardsdatascience.com/how-to-perform-outlier-detection-in-python-in-easy-steps-for-machine-learning-1-8f9a3e6c88b5?source=collection_archive---------2-----------------------#2023-01-28)

## 地球是一个异常值——这个理论

[](https://ibexorigin.medium.com/?source=post_page-----8f9a3e6c88b5--------------------------------)[![Bex T.](../Images/516496f32596e8ad56bf07f178a643c6.png)](https://ibexorigin.medium.com/?source=post_page-----8f9a3e6c88b5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8f9a3e6c88b5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8f9a3e6c88b5--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----8f9a3e6c88b5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39db050c2ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-perform-outlier-detection-in-python-in-easy-steps-for-machine-learning-1-8f9a3e6c88b5&user=Bex+T.&userId=39db050c2ac2&source=post_page-39db050c2ac2----8f9a3e6c88b5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8f9a3e6c88b5--------------------------------) ·6分钟阅读·2023年1月28日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8f9a3e6c88b5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-perform-outlier-detection-in-python-in-easy-steps-for-machine-learning-1-8f9a3e6c88b5&source=-----8f9a3e6c88b5---------------------bookmark_footer-----------)![](../Images/da07cff3f5f613c032464ed39dcf1906.png)

图片来自[0fjd125gk87](https://pixabay.com/users/0fjd125gk87-51581/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1784245) [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1784245)

## 什么是异常值？

我们生活在一个异常值上。地球是银河系中唯一有生命的岩石块。我们银河系中的其他行星则是所谓的恒星和行星数据库中的正常数据点。

离群点有很多定义。简单来说，我们将离群点定义为数据集中显著不同于大多数数据点的数据点。离群点是那些稀有、极端的样本，不符合或不与数据集中的内点对齐。

从统计学角度来看，离群点来自于与特征中其他样本不同的分布。它们表现出统计上显著的异常。

这些定义取决于我们认为的“正常”。例如，CEO赚取数百万美元是完全正常的，但如果我们将他们的薪资信息加入家庭收入数据集中，他们就会变得不正常。

离群点检测是统计学和机器学习的一个领域，利用各种技术和算法来检测这些极端样本。

点击这里查看系列的第二部分：
