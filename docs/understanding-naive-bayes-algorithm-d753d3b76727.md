# 理解 Naive Bayes 算法

> 原文：[`towardsdatascience.com/understanding-naive-bayes-algorithm-d753d3b76727?source=collection_archive---------5-----------------------#2023-12-29`](https://towardsdatascience.com/understanding-naive-bayes-algorithm-d753d3b76727?source=collection_archive---------5-----------------------#2023-12-29)

## 它是什么以及如何将其应用于实际场景

[](https://meaganburkhart.medium.com/?source=post_page-----d753d3b76727--------------------------------)![Meagan Burkhart](https://meaganburkhart.medium.com/?source=post_page-----d753d3b76727--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d753d3b76727--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d753d3b76727--------------------------------) [梅根·伯克哈特](https://meaganburkhart.medium.com/?source=post_page-----d753d3b76727--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F37a56d8d1d6a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-naive-bayes-algorithm-d753d3b76727&user=Meagan+Burkhart&userId=37a56d8d1d6a&source=post_page-37a56d8d1d6a----d753d3b76727---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d753d3b76727--------------------------------) · 6 分钟阅读 · 2023 年 12 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd753d3b76727&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-naive-bayes-algorithm-d753d3b76727&user=Meagan+Burkhart&userId=37a56d8d1d6a&source=-----d753d3b76727---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd753d3b76727&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-naive-bayes-algorithm-d753d3b76727&source=-----d753d3b76727---------------------bookmark_footer-----------)![](img/e991a2baa8392c3db178a9a83844d8bd.png)

图片来源：[Google DeepMind](https://unsplash.com/@googledeepmind?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

今年，我的决心是回归数据科学的基础。我每天都在处理数据，但如果你在做重复性的任务，很容易忘记一些核心算法的运作原理。我打算在每周的《Towards Data Science》上深入研究一个数据算法。本周，我将讲解 Naive Bayes。

# 如何发音 Naive Bayes

只需访问[这里](https://pronouncebee.com/naive-bayes/)即可学习如何发音 Naive Bayes。

现在我们知道如何发音了，让我们看看它的含义…

# 什么是朴素贝叶斯分类器？

这个概率分类器基于[贝叶斯定理](https://www.geeksforgeeks.org/bayes-theorem/)，其总结如下：

当第二个事件已经发生时，第一个事件的条件概率是“事件 B，在 A 给定的情况下的概率，除以事件 B 的概率”的乘积。

***P(A|B) = P(B|A)P(A) / P(B)***

*一个常见的误解是贝叶斯定理和条件概率是同义的。*

然而，有一个区别——贝叶斯定理使用条件概率的定义来找出…
