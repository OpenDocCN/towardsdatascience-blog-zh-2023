# 使用图表讲故事

> 原文：[https://towardsdatascience.com/storytelling-with-charts-dae59034f60?source=collection_archive---------16-----------------------#2023-04-03](https://towardsdatascience.com/storytelling-with-charts-dae59034f60?source=collection_archive---------16-----------------------#2023-04-03)

## 第二部分：你是否想展示数量？

[](https://medium.com/@dar.wtz?source=post_page-----dae59034f60--------------------------------)[![Darío Weitz](../Images/28efa942b4c5bd2763d58c44584cf583.png)](https://medium.com/@dar.wtz?source=post_page-----dae59034f60--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dae59034f60--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----dae59034f60--------------------------------) [Darío Weitz](https://medium.com/@dar.wtz?source=post_page-----dae59034f60--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7fb26b001728&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstorytelling-with-charts-dae59034f60&user=Dar%C3%ADo+Weitz&userId=7fb26b001728&source=post_page-7fb26b001728----dae59034f60---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dae59034f60--------------------------------) ·8 min read·2023年4月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdae59034f60&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstorytelling-with-charts-dae59034f60&user=Dar%C3%ADo+Weitz&userId=7fb26b001728&source=-----dae59034f60---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdae59034f60&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstorytelling-with-charts-dae59034f60&source=-----dae59034f60---------------------bookmark_footer-----------)![](../Images/59abc501c8b14f358548a31674a610fb.png)

图片来源：[Claudio Schwarz](https://unsplash.com/@purzlbaum?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数据可视化的第一条规则是：“数据可视化是沟通的工具”。它是讲述商业、商业、科学、学术和创业环境中的故事的最佳工具。

但请记住，你总是有观众。也许他们包括你的老板、同事、客户、员工、公务员和/或你自己。

在任何情况下，关键问题是：我是否清晰地传达了信息？

下一个问题是：我选择了正确的图表来讲述我的故事吗？

在[本系列的第一篇文章](https://medium.com/towards-data-science/storytelling-with-charts-23dd41096721)中，我们介绍了三种基于Python的探索性图表，这些图表可以用来可视化单一定量变量的分布。在本文（以及接下来的几篇文章中），我们将建议根据要传达给观众的信息的性质，选择最合适的图表。

# **信息1：展示数量**

该信息包括展示某一组数字的大小：销售金额、销售数量、缴纳的税款、生产的物品、生产的项目、完成的程度、人口数据、绩效数据、商业智能仪表板中的关键绩效指标（KPIs）等。
