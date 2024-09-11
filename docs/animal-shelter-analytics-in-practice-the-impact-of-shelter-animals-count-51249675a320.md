# 动物收容所分析的实践：收容动物数量的影响

> 原文：[https://towardsdatascience.com/animal-shelter-analytics-in-practice-the-impact-of-shelter-animals-count-51249675a320?source=collection_archive---------7-----------------------#2023-12-27](https://towardsdatascience.com/animal-shelter-analytics-in-practice-the-impact-of-shelter-animals-count-51249675a320?source=collection_archive---------7-----------------------#2023-12-27)

## 对 SAC 作为数据驱动社会企业开创性作用的探讨

[](https://yadramshankar.medium.com/?source=post_page-----51249675a320--------------------------------)[![Ramshankar Yadhunath](../Images/122a014cca92e8936117ce9f69dafbd1.png)](https://yadramshankar.medium.com/?source=post_page-----51249675a320--------------------------------)[](https://towardsdatascience.com/?source=post_page-----51249675a320--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----51249675a320--------------------------------) [Ramshankar Yadhunath](https://yadramshankar.medium.com/?source=post_page-----51249675a320--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7664499c5cdb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanimal-shelter-analytics-in-practice-the-impact-of-shelter-animals-count-51249675a320&user=Ramshankar+Yadhunath&userId=7664499c5cdb&source=post_page-7664499c5cdb----51249675a320---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----51249675a320--------------------------------) ·8分钟阅读·2023年12月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F51249675a320&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanimal-shelter-analytics-in-practice-the-impact-of-shelter-animals-count-51249675a320&user=Ramshankar+Yadhunath&userId=7664499c5cdb&source=-----51249675a320---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F51249675a320&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanimal-shelter-analytics-in-practice-the-impact-of-shelter-animals-count-51249675a320&source=-----51249675a320---------------------bookmark_footer-----------)![](../Images/74ef5b2f1f0fae7c257ee35de3563b3d.png)

图像由作者使用 DALL-E 生成

在社会企业中，数据不仅仅是电子表格上的数字。每一个数据点都代表了一条生命、一个故事或一次积极变化的机会。数据驱动的社会企业中最显著的例子之一就是动物福利组织。每个收容所都尝试收集动物、志愿者以及日常运营的数据。

小型社会企业中固有的数据挑战使得这些数据的谨慎使用成为避难所的一项艰巨任务。这些挑战包括数据整合、标准化和民主化消费方面的问题。这些挑战的原因通常是缺乏专门的数据资源和组织中缺乏数据驱动的文化。

然而，在动物福利领域，有一些组织正在有意识地推动进展。其中一个在重新定义美国动物收容所数据管理方式的组织是[Shelter Animals Count](https://www.shelteranimalscount.org/)（SAC）。对于在小型组织中从事数据工作的人来说，你会发现SAC面临的挑战与自己的挑战如出一辙。
