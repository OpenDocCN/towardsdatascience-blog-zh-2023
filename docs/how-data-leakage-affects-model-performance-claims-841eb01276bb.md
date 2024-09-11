# 数据泄漏如何影响模型性能声明

> 原文：[https://towardsdatascience.com/how-data-leakage-affects-model-performance-claims-841eb01276bb?source=collection_archive---------12-----------------------#2023-01-02](https://towardsdatascience.com/how-data-leakage-affects-model-performance-claims-841eb01276bb?source=collection_archive---------12-----------------------#2023-01-02)

[](https://medium.com/@georgiadeaconu?source=post_page-----841eb01276bb--------------------------------)[![Georgia Deaconu](../Images/39ba1bea77aa46bb39b2975108c3adaa.png)](https://medium.com/@georgiadeaconu?source=post_page-----841eb01276bb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----841eb01276bb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----841eb01276bb--------------------------------) [Georgia Deaconu](https://medium.com/@georgiadeaconu?source=post_page-----841eb01276bb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc4a98f38b0e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-data-leakage-affects-model-performance-claims-841eb01276bb&user=Georgia+Deaconu&userId=c4a98f38b0e&source=post_page-c4a98f38b0e----841eb01276bb---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----841eb01276bb--------------------------------) ·4 min read·2023年1月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F841eb01276bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-data-leakage-affects-model-performance-claims-841eb01276bb&user=Georgia+Deaconu&userId=c4a98f38b0e&source=-----841eb01276bb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F841eb01276bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-data-leakage-affects-model-performance-claims-841eb01276bb&source=-----841eb01276bb---------------------bookmark_footer-----------)

今年已经出现了由**机器学习驱动的研究**所带来的[几项](https://www.science.org/doi/10.1126/science.abi6983) [重要](https://www.deepmind.com/blog/alphafold-reveals-the-structure-of-the-protein-universe) [科学进展](https://www.nature.com/articles/s41586-021-04086-x.pdf)。与此同时，也出现了一些关于[基于机器学习的科学中的可重复性问题](https://arxiv.org/pdf/2207.07048.pdf)的担忧。已经识别出一些方法论问题，其中数据泄漏似乎是最为普遍的。一般来说，数据泄漏可能会扭曲结果并导致过于乐观的结论。

数据泄露可以以几种不同的方式发生。本帖的目标是展示一些最常遇到的类型，并提供一些识别和减轻它们的技巧。

![](../Images/a384eb770d0aa46df6ec4405d4a75f92.png)

作者使用 dreamstudio.ai 生成的图像

*数据泄露*可以定义为目标变量与其预测变量之间的一个人为关系，这种关系是通过数据收集方法或预处理策略无意中引入的。

我将尝试举例说明数据泄露的主要来源有：

1.  训练集和测试集之间的不当分离

1.  使用不合法的特征（代理变量）

1.  测试集未从感兴趣的分布中抽取

# 1\. 训练集和测试集之间的不当分离
