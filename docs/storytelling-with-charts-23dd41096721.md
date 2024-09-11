# 使用图表讲故事

> 原文：[https://towardsdatascience.com/storytelling-with-charts-23dd41096721?source=collection_archive---------14-----------------------#2023-02-10](https://towardsdatascience.com/storytelling-with-charts-23dd41096721?source=collection_archive---------14-----------------------#2023-02-10)

## 第1部分：展示单一的定量变量

[](https://medium.com/@dar.wtz?source=post_page-----23dd41096721--------------------------------)[![Darío Weitz](../Images/28efa942b4c5bd2763d58c44584cf583.png)](https://medium.com/@dar.wtz?source=post_page-----23dd41096721--------------------------------)[](https://towardsdatascience.com/?source=post_page-----23dd41096721--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----23dd41096721--------------------------------) [Darío Weitz](https://medium.com/@dar.wtz?source=post_page-----23dd41096721--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7fb26b001728&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstorytelling-with-charts-23dd41096721&user=Dar%C3%ADo+Weitz&userId=7fb26b001728&source=post_page-7fb26b001728----23dd41096721---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----23dd41096721--------------------------------) ·8分钟阅读·2023年2月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F23dd41096721&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstorytelling-with-charts-23dd41096721&user=Dar%C3%ADo+Weitz&userId=7fb26b001728&source=-----23dd41096721---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F23dd41096721&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstorytelling-with-charts-23dd41096721&source=-----23dd41096721---------------------bookmark_footer-----------)![](../Images/87afa223cff95ec78d84158a9c6912e6.png)

由 [Derek Story](https://unsplash.com/@derekstory?utm_source=medium&utm_medium=referral) 拍摄的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

每个数据集都包含大量细节。此外，许多数据集仅仅是充满了未分类的数字列表。

例如：

· 过去20年中，阿根廷中央地区1月份的平均降雨量。

· 信息系统工程学生的智商测试结果。

· 根据2022年人口普查，阿根廷24个省份的人口数据。

· 根据星期几和一天中的小时数，阿根廷的车祸死亡人数。

上述是具有相对较少数量的**单一定量变量**的数据集的典型示例。请记住，定量数据表示数量。同时，还要记住，根据值是来自测量还是计数，定量变量可以是连续的或离散的。

为什么数据分析师应该对这样一个数字列表感兴趣？首先，尽管许多科学、商业或管理问题涉及数值变量之间的比较、关系、组成或趋势，但**对数据集中每个变量的可视化**作为基础的探索性数据分析，对于理解模式是重要的。
