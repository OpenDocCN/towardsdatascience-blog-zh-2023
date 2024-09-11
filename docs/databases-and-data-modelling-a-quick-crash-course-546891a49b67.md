# 数据库和数据建模——快速入门课程

> 原文：[https://towardsdatascience.com/databases-and-data-modelling-a-quick-crash-course-546891a49b67?source=collection_archive---------4-----------------------#2023-05-12](https://towardsdatascience.com/databases-and-data-modelling-a-quick-crash-course-546891a49b67?source=collection_archive---------4-----------------------#2023-05-12)

## 数据仓库101：初学者的实用指南

[](https://col-jung.medium.com/?source=post_page-----546891a49b67--------------------------------)[![Col Jung](../Images/45ef9475b60f22a3c78c9c8e428812c3.png)](https://col-jung.medium.com/?source=post_page-----546891a49b67--------------------------------)[](https://towardsdatascience.com/?source=post_page-----546891a49b67--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----546891a49b67--------------------------------) [Col Jung](https://col-jung.medium.com/?source=post_page-----546891a49b67--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8d4e2c520037&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdatabases-and-data-modelling-a-quick-crash-course-546891a49b67&user=Col+Jung&userId=8d4e2c520037&source=post_page-8d4e2c520037----546891a49b67---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----546891a49b67--------------------------------) · 12分钟阅读 · 2023年5月12日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F546891a49b67&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdatabases-and-data-modelling-a-quick-crash-course-546891a49b67&source=-----546891a49b67---------------------bookmark_footer-----------)![](../Images/9cf91f1f43637c4805c94af41ab2eb8f.png)

图片由作者提供

在我 [五年的企业分析工作经验](https://from-data-warehouses-and-lakes-to-data-mesh-a-guide-to-enterprise-data-architecture-e2d93b2466b1) 中，我观察到许多数据科学家在入职时对数据仓库和数据建模的知识了解有限。

这不应该让人感到意外。

数据科学家来自数学、统计学、心理学和编程等不同背景。许多人在大学期间可能不会深入研究数据库系统的复杂性。

这包括我自己，一个从数学家转行成为数据科学家的。

我通过在线课程自学数据科学——这是获得*数据科学*工作的必要前提——但只是通过工作才掌握了数据库基础。

而且，既然数据湖现在如此流行，谁还需要仓库呢，对吧？（那只是个笑话！）

我写了这篇文章作为对那些没有太多数据仓库和数据建模知识的分析工作新手的快速入门指南。

我们将涵盖三个主题：

+   一个**企业数据仓库工作流程**是什么样的**；**

+   数据库**规范化**能实现什么；
