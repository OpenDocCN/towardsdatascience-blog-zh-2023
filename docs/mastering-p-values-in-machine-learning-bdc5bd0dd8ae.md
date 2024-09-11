# **掌握机器学习中的P值**

> 原文：[https://towardsdatascience.com/mastering-p-values-in-machine-learning-bdc5bd0dd8ae?source=collection_archive---------3-----------------------#2023-01-06](https://towardsdatascience.com/mastering-p-values-in-machine-learning-bdc5bd0dd8ae?source=collection_archive---------3-----------------------#2023-01-06)

## 了解P值及其在机器学习中的应用案例

[](https://spierre91.medium.com/?source=post_page-----bdc5bd0dd8ae--------------------------------)[![Sadrach Pierre, Ph.D.](../Images/0e4aab43c2b981546d552beccf2259ab.png)](https://spierre91.medium.com/?source=post_page-----bdc5bd0dd8ae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bdc5bd0dd8ae--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bdc5bd0dd8ae--------------------------------) [Sadrach Pierre, Ph.D.](https://spierre91.medium.com/?source=post_page-----bdc5bd0dd8ae--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F120b86134681&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-p-values-in-machine-learning-bdc5bd0dd8ae&user=Sadrach+Pierre%2C+Ph.D.&userId=120b86134681&source=post_page-120b86134681----bdc5bd0dd8ae---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bdc5bd0dd8ae--------------------------------) · 7分钟阅读 · 2023年1月6日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbdc5bd0dd8ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-p-values-in-machine-learning-bdc5bd0dd8ae&user=Sadrach+Pierre%2C+Ph.D.&userId=120b86134681&source=-----bdc5bd0dd8ae---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbdc5bd0dd8ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-p-values-in-machine-learning-bdc5bd0dd8ae&source=-----bdc5bd0dd8ae---------------------bookmark_footer-----------)![](../Images/136a6ff1e3460e47037dfa4e55d94e19.png)

图片由 [Karolina Grabowska](https://www.pexels.com/@karolina-grabowska/) 提供，发布在 [Pexels](https://www.pexels.com/photo/heart-shaped-gummy-candy-assorted-in-rows-with-one-candy-aside-against-pink-background-4016522/)

p值是一个统计指标，帮助统计学家决定是否接受或拒绝原假设。p值衡量变量之间没有关系的概率。低p值为反对原假设提供证据。p值常常被误解。例如，这往往导致人们错误地声称低p值意味着变量之间*存在*关系。声称存在关系与声称拒绝或接受没有关系略有不同。p值仅提供支持或反对原假设的证据。具体而言，p值<0.05是好的。p值<0.05意味着变量之间没有关系的概率为5%。此外，小p值可以解释为观察到的效应是由于偶然性的概率很小。

例如，如果一家制药公司在临床试验中测试一种新药，它可能观察到该药物在治疗特定疾病的症状上有效。尽管有这些观察结果，但仍然有可能观察到的药物效应是由于偶然性，并且该药物实际上无效。为了得出观察结果不是偶然性而代表真实效应的结论，可以使用p值来测量…
