# 图表讲故事

> 原文：[`towardsdatascience.com/storytelling-with-charts-c59c52c49871?source=collection_archive---------17-----------------------#2023-05-08`](https://towardsdatascience.com/storytelling-with-charts-c59c52c49871?source=collection_archive---------17-----------------------#2023-05-08)

## 第三部分：你想比较项目吗？

[](https://medium.com/@dar.wtz?source=post_page-----c59c52c49871--------------------------------)![Darío Weitz](https://medium.com/@dar.wtz?source=post_page-----c59c52c49871--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c59c52c49871--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c59c52c49871--------------------------------) [Darío Weitz](https://medium.com/@dar.wtz?source=post_page-----c59c52c49871--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7fb26b001728&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstorytelling-with-charts-c59c52c49871&user=Dar%C3%ADo+Weitz&userId=7fb26b001728&source=post_page-7fb26b001728----c59c52c49871---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c59c52c49871--------------------------------) ·7 分钟阅读·2023 年 5 月 8 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc59c52c49871&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstorytelling-with-charts-c59c52c49871&user=Dar%C3%ADo+Weitz&userId=7fb26b001728&source=-----c59c52c49871---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc59c52c49871&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstorytelling-with-charts-c59c52c49871&source=-----c59c52c49871---------------------bookmark_footer-----------)![](img/560885b13d332454947845149501e80e.png)

照片由 [charlesdeluvio](https://unsplash.com/@charlesdeluvio?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这是一个系列中的第三篇文章，旨在帮助从事数据可视化工作的人员选择最适合传达他们信息的图表类型。

在系列的[第一篇文章](https://medium.com/towards-data-science/storytelling-with-charts-23dd41096721)中指出了三种基于 Python 的图表，这些图表能够显示**单一定量变量的分布**。

当信息由**展示一组数字的大小**组成时，最合适的图表已在系列的[第二篇文章](https://medium.com/towards-data-science/storytelling-with-charts-dae59034f60)中指出。

现在信息是**比较项目**。以下是**常用的**用于呈现这种图形表示的图表：

+   · 标准条形图

+   · 聚类条形图

+   · 重叠条形图

+   · 棒棒糖图

+   · 哑铃图

+   · 发散条形图

# **标准条形图**

标准条形图（SBCs）**仅比较一个数值变量** **每项或每类别**。它们试图回答这个问题：“每个类别有多少个”？请记住，类别或项目指的是如国家、城市、姓氏、公司、品牌、年份等定性元素。
