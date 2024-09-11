# 理解工具变量

> 原文：[https://towardsdatascience.com/understanding-instrumental-variables-0ce5d3d6ba20?source=collection_archive---------2-----------------------#2023-11-13](https://towardsdatascience.com/understanding-instrumental-variables-0ce5d3d6ba20?source=collection_archive---------2-----------------------#2023-11-13)

## [因果数据科学](https://towardsdatascience.com/tagged/causal-data-science)

## *当你无法随机化处理时如何估计因果效应*

[](https://medium.com/@matteo.courthoud?source=post_page-----0ce5d3d6ba20--------------------------------)[![Matteo Courthoud](../Images/d873eab35a0cf9fc696658c0bee16b33.png)](https://medium.com/@matteo.courthoud?source=post_page-----0ce5d3d6ba20--------------------------------)[](https://towardsdatascience.com/?source=post_page-----0ce5d3d6ba20--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----0ce5d3d6ba20--------------------------------) [Matteo Courthoud](https://medium.com/@matteo.courthoud?source=post_page-----0ce5d3d6ba20--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F666130fb420f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-instrumental-variables-0ce5d3d6ba20&user=Matteo+Courthoud&userId=666130fb420f&source=post_page-666130fb420f----0ce5d3d6ba20---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----0ce5d3d6ba20--------------------------------) ·12 分钟阅读·2023年11月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F0ce5d3d6ba20&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-instrumental-variables-0ce5d3d6ba20&user=Matteo+Courthoud&userId=666130fb420f&source=-----0ce5d3d6ba20---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F0ce5d3d6ba20&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-instrumental-variables-0ce5d3d6ba20&source=-----0ce5d3d6ba20---------------------bookmark_footer-----------)![](../Images/9caba9cc613fed76bb306ba2e72466d9.png)

封面，图片由作者提供

A/B 测试是因果推断的黄金标准，因为它们允许我们在最小假设下做出有效的因果声明，这要归功于**随机化**。实际上，通过随机分配**处理**（药物、广告、产品等），我们能够比较**结果**（疾病、公司收入、客户满意度等）在**主体**（患者、用户、客户等）之间的差异，并将结果的平均差异归因于处理的因果效应。

然而，在许多情况下，**不可能随机分配**治疗，无论是出于伦理、法律还是实际原因。一个常见的在线设置是按需功能，如订阅或高级会员资格。其他设置包括我们无法区分客户的特性，如保险合同，或者特性被深度编码到可能不值得进行实验的程度。在这些情况下，我们仍然能够进行有效的因果推断吗？

答案是肯定的，这要归功于**工具变量**和相应的实验设计称为**鼓励设计**。在上述提到的许多情况下，我们无法随机*分配*治疗，但我们可以*鼓励*客户接受它。对于...
