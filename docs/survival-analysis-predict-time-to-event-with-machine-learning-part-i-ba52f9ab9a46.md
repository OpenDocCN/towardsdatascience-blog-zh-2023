# 生存分析：利用机器学习预测事件发生时间（第一部分）

> 原文：[`towardsdatascience.com/survival-analysis-predict-time-to-event-with-machine-learning-part-i-ba52f9ab9a46?source=collection_archive---------1-----------------------#2023-02-09`](https://towardsdatascience.com/survival-analysis-predict-time-to-event-with-machine-learning-part-i-ba52f9ab9a46?source=collection_archive---------1-----------------------#2023-02-09)

![](img/4df3d3f659d7620733d12c7a48b6a04b.png)

插图由作者提供

## 客户流失预测的实际应用

[](https://linafaik.medium.com/?source=post_page-----ba52f9ab9a46--------------------------------)![Lina Faik](https://linafaik.medium.com/?source=post_page-----ba52f9ab9a46--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ba52f9ab9a46--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ba52f9ab9a46--------------------------------) [Lina Faik](https://linafaik.medium.com/?source=post_page-----ba52f9ab9a46--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb6c0e8e98c84&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsurvival-analysis-predict-time-to-event-with-machine-learning-part-i-ba52f9ab9a46&user=Lina+Faik&userId=b6c0e8e98c84&source=post_page-b6c0e8e98c84----ba52f9ab9a46---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ba52f9ab9a46--------------------------------) · 11 分钟阅读 · 2023 年 2 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fba52f9ab9a46&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsurvival-analysis-predict-time-to-event-with-machine-learning-part-i-ba52f9ab9a46&user=Lina+Faik&userId=b6c0e8e98c84&source=-----ba52f9ab9a46---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fba52f9ab9a46&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsurvival-analysis-predict-time-to-event-with-machine-learning-part-i-ba52f9ab9a46&source=-----ba52f9ab9a46---------------------bookmark_footer-----------)

预测事件发生的概率很好，但预测事件发生前剩余的时间更好！

以客户流失为例。如果你不仅能够预测客户在接下来的几个月内离开公司的概率，还能在未来几个月的多个时间点上预测这一概率呢？这种方法的好处显而易见。它将使你能够更有效地提前规划和优先安排你的营销行动，并最终降低流失率。

这正好属于**生存分析**的范畴，也称为时间-事件分析。它指的是一种学习框架和一组技术，可以用来根据观察结果估计感兴趣的事件发生所需的时间。

生存分析的名称来源于它最初应用的典型用例：预测临床研究中的死亡时间。然而，人们不应该被它的名称误导：它并不局限于医学领域，而是可以应用于多个行业的用例。随着数据科学的最新进展，生存分析已经重新出现，离开了经典统计学的世界…
