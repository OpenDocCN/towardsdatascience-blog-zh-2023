# 什么是t分布

> 原文：[https://towardsdatascience.com/beyond-the-bell-curve-an-introduction-to-the-t-distribution-87a7cf5b3e40?source=collection_archive---------11-----------------------#2023-09-02](https://towardsdatascience.com/beyond-the-bell-curve-an-introduction-to-the-t-distribution-87a7cf5b3e40?source=collection_archive---------11-----------------------#2023-09-02)

## 探索著名t分布背后的起源、理论和应用

[](https://medium.com/@egorhowell?source=post_page-----87a7cf5b3e40--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----87a7cf5b3e40--------------------------------)[](https://towardsdatascience.com/?source=post_page-----87a7cf5b3e40--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----87a7cf5b3e40--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----87a7cf5b3e40--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-the-bell-curve-an-introduction-to-the-t-distribution-87a7cf5b3e40&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----87a7cf5b3e40---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----87a7cf5b3e40--------------------------------) ·6分钟阅读·2023年9月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F87a7cf5b3e40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-the-bell-curve-an-introduction-to-the-t-distribution-87a7cf5b3e40&user=Egor+Howell&userId=1cac491223b2&source=-----87a7cf5b3e40---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F87a7cf5b3e40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-the-bell-curve-an-introduction-to-the-t-distribution-87a7cf5b3e40&source=-----87a7cf5b3e40---------------------bookmark_footer-----------)![](../Images/e67164bc68ae94d764080514d881d30b.png)

照片由[Agence Olloweb](https://unsplash.com/@olloweb?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上拍摄

# 什么是**t分布**？

[**t分布**](https://en.wikipedia.org/wiki/Student%27s_t-distribution)**，**是一个连续的概率分布，非常类似于[**正态分布**](https://en.wikipedia.org/wiki/Normal_distribution)**，**但有以下关键区别：

+   **更重的尾部**：*它的概率质量更多地集中在极端值（更高的* [***峰度***](https://www.scribbr.com/statistics/kurtosis/#:~:text=Kurtosis%20is%20a%20measure%20of,(thin%20tails)%20are%20platykurtic.) *)。这意味着它更容易产生远离均值的值。*

+   **一个参数**：*t-分布只有一个参数，即* [***自由度***](https://en.wikipedia.org/wiki/Degrees_of_freedom_(statistics)) *，因为它在我们不知道总体方差时使用。*

关于 t-分布的一个有趣事实是，它有时被称为“学生 t-分布”。这是因为该分布的发明者，[**威廉·西利·戈塞特**](https://en.wikipedia.org/wiki/William_Sealy_Gosset)，是一位英国统计学家，他用化名“学生”发表了这个分布，以保持身份匿名，因此得名“学生 t-分布”。

# 理论与定义
