# Pandas 中 if-else 条件语句的 5 种应用方法

> 原文：[https://towardsdatascience.com/5-ways-to-apply-if-else-conditional-statements-in-pandas-b9627e5f475b?source=collection_archive---------2-----------------------#2023-01-06](https://towardsdatascience.com/5-ways-to-apply-if-else-conditional-statements-in-pandas-b9627e5f475b?source=collection_archive---------2-----------------------#2023-01-06)

## 重新审视 Pandas 基础知识并提升你的数据整理技能

[](https://medium.com/@insightsbees?source=post_page-----b9627e5f475b--------------------------------)[![我的数据谈话](../Images/8adf8046dbb807d7613a324bdab8bc02.png)](https://medium.com/@insightsbees?source=post_page-----b9627e5f475b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b9627e5f475b--------------------------------)[![数据科学之道](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b9627e5f475b--------------------------------) [我的数据谈话](https://medium.com/@insightsbees?source=post_page-----b9627e5f475b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff10c9348da09&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-ways-to-apply-if-else-conditional-statements-in-pandas-b9627e5f475b&user=My+Data+Talk&userId=f10c9348da09&source=post_page-f10c9348da09----b9627e5f475b---------------------post_header-----------) 发表在 [数据科学之道](https://towardsdatascience.com/?source=post_page-----b9627e5f475b--------------------------------) ·5 分钟阅读·2023年1月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb9627e5f475b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-ways-to-apply-if-else-conditional-statements-in-pandas-b9627e5f475b&user=My+Data+Talk&userId=f10c9348da09&source=-----b9627e5f475b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb9627e5f475b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-ways-to-apply-if-else-conditional-statements-in-pandas-b9627e5f475b&source=-----b9627e5f475b---------------------bookmark_footer-----------)![](../Images/653970acab6f79ac21f6f4b15288ac6d.png)

图片来源：[muxin alkayis](https://pixabay.com/users/muxin25-17301468/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=5364820) 来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=5364820)

在 Pandas 数据框中创建新列或修改现有列——基于一组 `if-else` 条件——可能是所有不同类型数据处理任务中最常见的问题之一。在这篇文章中，我想与您分享我的笔记本，总结了在 Pandas 数据框中应用 `if-else` 条件语句的 5 种流行方法，并附上实用的代码片段。为了简单起见，我创建了一个小样本数据集，并将在整个教程中使用它进行演示。

假设我们有一个如下面所示的 pandas 数据框。列‘visits_30days’显示了一个客户在过去 30 天内访问网站的次数。我们想要创建一个新列，将这些客户分为‘非访客’或‘访客’（一种二元分类），或者将他们分为多个桶，如‘0 次访问’，‘1–5 次访问’，‘6-10 次访问’等。我们将这个新列命名为‘visits_category’。

![](../Images/327ad85ebfcb6fd6fbd9c0021d719432.png)

作者提供的图片

## 方法 1：使用 **numpy.where()** 函数
