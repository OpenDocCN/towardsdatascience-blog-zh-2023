# 逻辑分类的意义，来自物理学

> 原文：[`towardsdatascience.com/the-meaning-behind-logistic-classification-from-physics-291774fda579?source=collection_archive---------2-----------------------#2023-01-24`](https://towardsdatascience.com/the-meaning-behind-logistic-classification-from-physics-291774fda579?source=collection_archive---------2-----------------------#2023-01-24)

## 在分类问题中，我们为何使用逻辑函数和 softmax 函数？热力学可能提供答案。

[](https://tim-lou.medium.com/?source=post_page-----291774fda579--------------------------------)![Tim Lou, PhD](https://tim-lou.medium.com/?source=post_page-----291774fda579--------------------------------)[](https://towardsdatascience.com/?source=post_page-----291774fda579--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----291774fda579--------------------------------) [Tim Lou, PhD](https://tim-lou.medium.com/?source=post_page-----291774fda579--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8d41b438feef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-meaning-behind-logistic-classification-from-physics-291774fda579&user=Tim+Lou%2C+PhD&userId=8d41b438feef&source=post_page-8d41b438feef----291774fda579---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----291774fda579--------------------------------) ·9 min read·2023 年 1 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F291774fda579&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-meaning-behind-logistic-classification-from-physics-291774fda579&user=Tim+Lou%2C+PhD&userId=8d41b438feef&source=-----291774fda579---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F291774fda579&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-meaning-behind-logistic-classification-from-physics-291774fda579&source=-----291774fda579---------------------bookmark_footer-----------)![](img/506d63dd0efa1ff62f42cfd27cce22b1.png)

逻辑回归无处不在，但它为何有效的直观理解是什么？（图片由作者提供）

逻辑回归可能是最受欢迎和最知名的机器学习模型。它解决了二分类问题——用于预测数据点是否属于某一类别。

关键成分是*逻辑*函数，它将输入转换为概率，写成以下形式：

但为何指数函数会出现？它的直观理解是什么，除了将实数转换为概率之外？

结果显示，热物理学有答案。但在深入物理学的见解之前，我们先来理解数学部分。

# 数学困境

在探讨逻辑函数背后的“为什么”之前，我们先来了解它的性质。

![](img/f4ca81396932f382d9d5bf1369094b35.png)

该是处理数学问题的时候了！（照片由 [Michal Matlon](https://unsplash.com/@michalmatlon?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)）

理解逻辑函数的更一般形式——适用于多个类别的形式——是很有帮助的，以便于…
