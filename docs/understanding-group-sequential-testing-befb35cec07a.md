# 理解组间顺序测试

> 原文：[https://towardsdatascience.com/understanding-group-sequential-testing-befb35cec07a?source=collection_archive---------1-----------------------#2023-12-26](https://towardsdatascience.com/understanding-group-sequential-testing-befb35cec07a?source=collection_archive---------1-----------------------#2023-12-26)

## [因果数据科学](https://towardsdatascience.com/tagged/causal-data-science)

## *如何进行有效的实验，包括观察和早期停止。*

[](https://medium.com/@matteo.courthoud?source=post_page-----befb35cec07a--------------------------------)[![Matteo Courthoud](../Images/d873eab35a0cf9fc696658c0bee16b33.png)](https://medium.com/@matteo.courthoud?source=post_page-----befb35cec07a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----befb35cec07a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----befb35cec07a--------------------------------) [Matteo Courthoud](https://medium.com/@matteo.courthoud?source=post_page-----befb35cec07a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F666130fb420f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-group-sequential-testing-befb35cec07a&user=Matteo+Courthoud&userId=666130fb420f&source=post_page-666130fb420f----befb35cec07a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----befb35cec07a--------------------------------) ·15分钟阅读·2023年12月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbefb35cec07a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-group-sequential-testing-befb35cec07a&user=Matteo+Courthoud&userId=666130fb420f&source=-----befb35cec07a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbefb35cec07a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-group-sequential-testing-befb35cec07a&source=-----befb35cec07a---------------------bookmark_footer-----------)![](../Images/36e109332a0572fb79582557617c167a.png)

封面，图片由作者提供

A/B 测试被认为是因果推断的黄金标准，因为它们允许我们在最小假设下做出有效的因果陈述，这要归功于**随机化**。实际上，通过随机分配**处理**（药物、广告、产品等），我们可以比较**结果**（疾病、公司收入、客户满意度等）在**受试者**（患者、用户、客户等）之间的差异，并将结果的平均差异归因于处理的因果效应。

实施 A/B 测试通常不是瞬时完成的，尤其是在在线环境中。用户常常会被**实时**或分**批次**处理。在这些环境中，人们可以在数据收集完成之前查看数据，一次或多次。这种现象被称为**窥探**。虽然窥探本身并没有问题，但在窥探时使用标准测试程序可能会导致**误导性结论**。

**窥探**的解决方法是相应地调整测试程序。最著名且传统的方法是所谓的**序列概率比率检验 (SPRT)**，这种方法可以追溯到第二次世界大战。如果你想了解更多关于这个测试及其迷人历史的信息，我写了一篇博客文章。
