# 什么是组合优化？

> 原文：[https://towardsdatascience.com/breaking-down-combinatorial-optimization-why-some-problems-are-inherently-difficult-65230e25d4da?source=collection_archive---------11-----------------------#2023-04-10](https://towardsdatascience.com/breaking-down-combinatorial-optimization-why-some-problems-are-inherently-difficult-65230e25d4da?source=collection_archive---------11-----------------------#2023-04-10)

## 展示组合爆炸的真正威力以及它们如何影响简单问题。

[](https://medium.com/@egorhowell?source=post_page-----65230e25d4da--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----65230e25d4da--------------------------------)[](https://towardsdatascience.com/?source=post_page-----65230e25d4da--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----65230e25d4da--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----65230e25d4da--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbreaking-down-combinatorial-optimization-why-some-problems-are-inherently-difficult-65230e25d4da&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----65230e25d4da---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----65230e25d4da--------------------------------) ·5 min read·2023年4月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F65230e25d4da&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbreaking-down-combinatorial-optimization-why-some-problems-are-inherently-difficult-65230e25d4da&user=Egor+Howell&userId=1cac491223b2&source=-----65230e25d4da---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F65230e25d4da&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbreaking-down-combinatorial-optimization-why-some-problems-are-inherently-difficult-65230e25d4da&source=-----65230e25d4da---------------------bookmark_footer-----------)![](../Images/e25a11d4888e8f7a8808ec5eff99b3b2.png)

图片由 [Shubham Dhage](https://unsplash.com/@theshubhamdhage?utm_source=medium&utm_medium=referral) 提供，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 什么是组合优化？

想象你是一名数据科学家，为一家航空公司工作，你的任务是寻找每周的最佳航班安排，以最大化航班数量，同时考虑燃料和空域等限制条件。

***你会怎么做？***

好吧，你可以尝试每一种可能的解决方案，这称为[***暴力搜索***](https://en.wikipedia.org/wiki/Brute-force_search)，不过如果我告诉你我们要进行***500***次飞行呢？你需要尝试的不同[***组合***](https://medium.com/towards-data-science/a-primer-on-combinations-and-permutations-d654aacea292)将达到***~500!***，大约等于***1.22 x 10¹¹³⁴***。这个数字极其庞大，使得暴力搜索[***不可行***](https://www.umsl.edu/~siegelj/information_theory/classassignments/Lombardo/04_intractableproblems.html)。

***那么，你怎么解决这个问题？***

> 组合优化！

[***组合优化***](https://en.wikipedia.org/wiki/Combinatorial_optimization)涉及从有限对象中找到最佳解决方案，而这个问题通常是不可解的。这听起来非常像我们上面的问题！

事实上，组合优化被应用于各种领域：

+   *物流和供应链*

+   *制造*
