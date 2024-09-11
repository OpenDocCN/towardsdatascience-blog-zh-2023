# 贝叶斯AB测试

> 原文：[https://towardsdatascience.com/bayesian-ab-testing-ed45cc8c964d?source=collection_archive---------3-----------------------#2023-01-10](https://towardsdatascience.com/bayesian-ab-testing-ed45cc8c964d?source=collection_archive---------3-----------------------#2023-01-10)

## [因果数据科学](https://towardsdatascience.com/tagged/causal-data-science)

## *在随机实验中使用和选择先验。*

[](https://medium.com/@matteo.courthoud?source=post_page-----ed45cc8c964d--------------------------------)[![Matteo Courthoud](../Images/d873eab35a0cf9fc696658c0bee16b33.png)](https://medium.com/@matteo.courthoud?source=post_page-----ed45cc8c964d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ed45cc8c964d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ed45cc8c964d--------------------------------) [Matteo Courthoud](https://medium.com/@matteo.courthoud?source=post_page-----ed45cc8c964d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F666130fb420f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbayesian-ab-testing-ed45cc8c964d&user=Matteo+Courthoud&userId=666130fb420f&source=post_page-666130fb420f----ed45cc8c964d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ed45cc8c964d--------------------------------) ·11分钟阅读·2023年1月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fed45cc8c964d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbayesian-ab-testing-ed45cc8c964d&user=Matteo+Courthoud&userId=666130fb420f&source=-----ed45cc8c964d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fed45cc8c964d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbayesian-ab-testing-ed45cc8c964d&source=-----ed45cc8c964d---------------------bookmark_footer-----------)![](../Images/df5da2cdfb6605d49b3a8df1019335f2.png)

封面，图片由作者提供

随机化实验，也称为**AB测试**，是业界用于估算因果效应的标准方法。通过将处理（新产品、功能、用户界面等）随机分配给人群的一个子集（用户、患者、客户等），我们可以确保，平均而言，结果（收入、访问量、点击量等）的差异可以归因于该处理。像[Booking.com](https://partner.booking.com/en-gb/click-magazine/industry-perspectives/role-experimentation-bookingcom)这样的成熟公司报告称，他们不断同时运行数千个AB测试。而像[Duolingo](https://blog.duolingo.com/improving-duolingo-one-experiment-at-a-time/)这样的新兴成长公司则将其成功的大部分归因于其大规模实验文化。

面对如此众多的实验，一个自然的问题是：在一个具体的实验中，是否可以利用之前测试的信息？如何利用？在这篇文章中，我将通过介绍**贝叶斯AB测试方法**来尝试回答这些问题。贝叶斯框架非常适合这种任务，因为它自然允许使用新数据更新现有知识（先验）。然而，这种方法对函数形式假设特别敏感，显而易见的模型选择，例如先验分布的偏斜性，可能会导致非常不同的估计结果。

# 搜索与无限滚动
