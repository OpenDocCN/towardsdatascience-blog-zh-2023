# 如何使用线性规划解决优化问题

> 原文：[`towardsdatascience.com/how-to-solve-optimisation-problems-using-linear-programming-912cc951afbb?source=collection_archive---------7-----------------------#2023-07-24`](https://towardsdatascience.com/how-to-solve-optimisation-problems-using-linear-programming-912cc951afbb?source=collection_archive---------7-----------------------#2023-07-24)

## 介绍线性规划及其如何通过图形和单纯形算法解决

[](https://medium.com/@egorhowell?source=post_page-----912cc951afbb--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----912cc951afbb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----912cc951afbb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----912cc951afbb--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----912cc951afbb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-solve-optimisation-problems-using-linear-programming-912cc951afbb&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----912cc951afbb---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----912cc951afbb--------------------------------) · 7 min read · 2023 年 7 月 24 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F912cc951afbb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-solve-optimisation-problems-using-linear-programming-912cc951afbb&source=-----912cc951afbb---------------------bookmark_footer-----------)![](img/8f8bc85a775d8dd0a1009a32df6eeacd.png)

图片由 [Isaac Smith](https://unsplash.com/es/@isaacmsmith?utm_source=medium&utm_medium=referral) 提供，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

[**线性规划 (LP)**](https://en.wikipedia.org/wiki/Linear_programming)是一种优化技术，用于从指定的*目标函数*中找到最佳解决方案，受限于一些*约束条件*。它被应用于从金融到电子商务的各种行业，因此如果你是数据科学家，了解它是非常值得的。LP 的关键特征是[**线性**](https://en.wikipedia.org/wiki/Linear_equation)部分，这意味着约束条件和目标函数都被形式化为线性关系。在这篇文章中，我们将通过一个 LP 问题的例子来探讨如何使用[**单纯形算法**](https://en.wikipedia.org/wiki/Simplex_algorithm)和[**图解法**](https://www.toppr.com/guides/maths/linear-programming/graphical-method-of-solving-a-linear-programming-problem/)来解决。

# 图解法

## 基本问题

我们将从图解法开始，因为它是最简单理解的方法，并且能提供关于 LP 的真实直觉。

假设我们经营一家小型企业，出售**£3**的冰沙和**£2**的咖啡，这些是我们的两个*决策变量*。由于我们的原料约束，我们每天只能生产**75**杯冰沙和**100**杯咖啡。此外，我们每天只有**140**个杯子来供应我们的冰沙和咖啡。现在，让我们制定…
