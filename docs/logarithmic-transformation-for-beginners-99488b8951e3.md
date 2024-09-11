# 初学者的对数变换

> 原文：[`towardsdatascience.com/logarithmic-transformation-for-beginners-99488b8951e3?source=collection_archive---------7-----------------------#2023-05-26`](https://towardsdatascience.com/logarithmic-transformation-for-beginners-99488b8951e3?source=collection_archive---------7-----------------------#2023-05-26)

## 无单位的关联解释和其他好处

[](https://medium.com/@jaekim8080?source=post_page-----99488b8951e3--------------------------------)![Jae Kim](https://medium.com/@jaekim8080?source=post_page-----99488b8951e3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----99488b8951e3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----99488b8951e3--------------------------------) [Jae Kim](https://medium.com/@jaekim8080?source=post_page-----99488b8951e3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a7641c3f8c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flogarithmic-transformation-for-beginners-99488b8951e3&user=Jae+Kim&userId=3a7641c3f8c1&source=post_page-3a7641c3f8c1----99488b8951e3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----99488b8951e3--------------------------------) ·7 分钟阅读·2023 年 5 月 26 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F99488b8951e3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flogarithmic-transformation-for-beginners-99488b8951e3&user=Jae+Kim&userId=3a7641c3f8c1&source=-----99488b8951e3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F99488b8951e3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flogarithmic-transformation-for-beginners-99488b8951e3&source=-----99488b8951e3---------------------bookmark_footer-----------)![](img/af1190126698b57beaa04749e17fcdd0.png)

由 [Joshua Sortino](https://unsplash.com/@sortino?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供的照片

在统计和机器学习模型中，变量通常会被转换为自然对数。这样做有许多好处，包括…

+   无单位的关系解释；

+   方差稳定性；

+   缓解异常值的影响；

+   线性化；以及

+   与正态分布的接近程度。

在这篇文章中，我将通过一个应用详细解释上述观点。数据和 R 代码可以从[这里](https://github.com/jh8080/Chicago)获取。

# 1\. 函数的变化和斜率

为简单起见，考虑 Y = 1 + 2X，其中 Y 是响应变量，X 是输入变量。我们通常关心的是 Y 在 X 变化时的变化量。让 Δ 表示变化操作符。也就是说，

ΔY = Y1 — Y0: Y 从 Y0 变化到 Y1 的变化量；并且

ΔX = X1 — X0: X 从 X0 变化到 X1 的变化量。

假设，在我们的例子中（Y = 1 + 2 X），X 从 1 变化到 3。对此，Y 从 3 变化到 7。也就是说，ΔY = 4 和 ΔX = 2。
