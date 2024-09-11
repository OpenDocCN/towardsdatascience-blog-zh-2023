# 使用 Scikit-Learn 的集成学习：一个友好的介绍

> 原文：[`towardsdatascience.com/ensemble-learning-with-scikit-learn-a-friendly-introduction-5dd64650de6c?source=collection_archive---------1-----------------------#2023-09-09`](https://towardsdatascience.com/ensemble-learning-with-scikit-learn-a-friendly-introduction-5dd64650de6c?source=collection_archive---------1-----------------------#2023-09-09)

## 像 XGBoost 或随机森林这样的集成学习算法在 Kaggle 竞赛中是表现最优秀的模型之一。它们是如何工作的？

[](https://medium.com/@riccardo.andreoni?source=post_page-----5dd64650de6c--------------------------------)![Riccardo Andreoni](https://medium.com/@riccardo.andreoni?source=post_page-----5dd64650de6c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5dd64650de6c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5dd64650de6c--------------------------------) [Riccardo Andreoni](https://medium.com/@riccardo.andreoni?source=post_page-----5dd64650de6c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76784541161c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fensemble-learning-with-scikit-learn-a-friendly-introduction-5dd64650de6c&user=Riccardo+Andreoni&userId=76784541161c&source=post_page-76784541161c----5dd64650de6c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5dd64650de6c--------------------------------) · 7 min read · 2023 年 9 月 9 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5dd64650de6c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fensemble-learning-with-scikit-learn-a-friendly-introduction-5dd64650de6c&user=Riccardo+Andreoni&userId=76784541161c&source=-----5dd64650de6c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5dd64650de6c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fensemble-learning-with-scikit-learn-a-friendly-introduction-5dd64650de6c&source=-----5dd64650de6c---------------------bookmark_footer-----------)![](img/d2fbdc1ade153ba4bfe5318e61d89304.png)

来源：[unsplash.com](https://unsplash.com/photos/0u_vbeOkMpk)

基本的学习算法，如逻辑回归或线性回归，通常过于简单，无法在机器学习问题中取得足够的结果。虽然可以使用神经网络作为一种解决方案，但它们需要大量的训练数据，而这些数据很少有。集成学习技术可以在数据有限的情况下提升简单模型的性能。

想象一下让一个人猜测一个大罐子里有多少颗糖果。一个人的答案不太可能是正确数字的精确估计。相反，如果我们问一千个人相同的问题，平均答案可能接近实际数字。这种现象被称为[*集体智慧*](https://en.wikipedia.org/wiki/Wisdom_of_the_crowd)[1]。在处理复杂的估计任务时，集体的准确性往往会高于个体。

集成学习算法通过汇聚一组模型（如回归器或分类器）的预测，利用了这个简单的原则。对于分类器的汇聚，集成模型可以简单地选择最常见的类别……
