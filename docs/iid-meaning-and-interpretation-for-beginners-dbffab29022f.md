# IID: 初学者的含义与解释

> 原文：[`towardsdatascience.com/iid-meaning-and-interpretation-for-beginners-dbffab29022f?source=collection_archive---------7-----------------------#2023-08-19`](https://towardsdatascience.com/iid-meaning-and-interpretation-for-beginners-dbffab29022f?source=collection_archive---------7-----------------------#2023-08-19)

## 独立同分布

[](https://medium.com/@jaekim8080?source=post_page-----dbffab29022f--------------------------------)![Jae Kim](https://medium.com/@jaekim8080?source=post_page-----dbffab29022f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dbffab29022f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dbffab29022f--------------------------------) [Jae Kim](https://medium.com/@jaekim8080?source=post_page-----dbffab29022f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a7641c3f8c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fiid-meaning-and-interpretation-for-beginners-dbffab29022f&user=Jae+Kim&userId=3a7641c3f8c1&source=post_page-3a7641c3f8c1----dbffab29022f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dbffab29022f--------------------------------) ·9 min read·2023 年 8 月 19 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdbffab29022f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fiid-meaning-and-interpretation-for-beginners-dbffab29022f&user=Jae+Kim&userId=3a7641c3f8c1&source=-----dbffab29022f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdbffab29022f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fiid-meaning-and-interpretation-for-beginners-dbffab29022f&source=-----dbffab29022f---------------------bookmark_footer-----------)![](img/7fb20a7ddc2a72850a13b99b256989e0.png)

摄影：[Yu Kato](https://unsplash.com/@yukato?utm_source=medium&utm_medium=referral) 摄于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在统计学、数据分析和机器学习领域中，IID 的概念常作为基本假设或条件出现。它代表“*独立同分布*”。IID 随机变量或序列是统计模型或机器模型中的一个重要组成部分，同时也在时间序列分析中发挥作用。

在这篇文章中，我以直观的方式解释了 IID 在三种不同背景下的概念：采样、建模和可预测性。结合 R 代码的应用在时间序列分析和可预测性方面进行了展示。

# 采样中的 IID

符号 X ~ IID(μ,σ²) 表示以*纯随机*的方式从均值 μ 和方差 σ² 的总体中采样（X1, …, Xn）。也就是说，

+   每一次连续的 X 实现都是*独立的*，与之前或之后的实现没有关联；并且

+   每一次连续的 X 实现都是从具有*相同*均值和方差的相同分布中获得的。

## 示例

假设从某国个人年收入的分布中收集了一个样本（X1, …, Xn）。

1.  一位研究人员发现……
