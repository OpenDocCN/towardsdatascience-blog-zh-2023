# 如何降低主数据管理费用

> 原文：[https://towardsdatascience.com/how-to-reduce-your-master-data-management-bill-a95bf45b2353?source=collection_archive---------9-----------------------#2023-04-28](https://towardsdatascience.com/how-to-reduce-your-master-data-management-bill-a95bf45b2353?source=collection_archive---------9-----------------------#2023-04-28)

## 利用开源抓住“低悬果”

[](https://medium.com/@paul.kinsvater?source=post_page-----a95bf45b2353--------------------------------)[![Paul Kinsvater](../Images/a117d2ba4ede758108ed76492e48a56a.png)](https://medium.com/@paul.kinsvater?source=post_page-----a95bf45b2353--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a95bf45b2353--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a95bf45b2353--------------------------------) [Paul Kinsvater](https://medium.com/@paul.kinsvater?source=post_page-----a95bf45b2353--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8ae6dbd4c80b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-reduce-your-master-data-management-bill-a95bf45b2353&user=Paul+Kinsvater&userId=8ae6dbd4c80b&source=post_page-8ae6dbd4c80b----a95bf45b2353---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a95bf45b2353--------------------------------) ·9分钟阅读·2023年4月28日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa95bf45b2353&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-reduce-your-master-data-management-bill-a95bf45b2353&user=Paul+Kinsvater&userId=8ae6dbd4c80b&source=-----a95bf45b2353---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa95bf45b2353&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-reduce-your-master-data-management-bill-a95bf45b2353&source=-----a95bf45b2353---------------------bookmark_footer-----------)

主数据管理，或称MDM，是商业供应商用来指代[实体解析](https://en.wikipedia.org/wiki/Record_linkage)框架的流行词。我与几家供应商进行了交谈，大多数提供SaaS服务，按从数据源中摄取的记录总数定价。这对于大型企业来说，每年总费用通常在六到七位数美元之间。

## 本文的目标读者

你是否计划尽快实施MDM？你是否向供应商询问了报价？或者你的公司是否已经投资于MDM SaaS？这确实是一笔不小的投资。

如果你能通过几天的工程工作显著降低年度订阅费用会怎样？一句话总结：

> 利用开源工具抓住眼前的机会，让MDM来完成繁重的工作。

这一举措可以轻松转化为两位数百分比或每年5到7位数的$节省。

## 实体解析为何重要

一般规模的企业使用多个数据源。用于其运营（ERP）、客户关系管理（CRM）、分析（数据湖、数据仓库）以及其他（文件系统、外部来源）。

![](../Images/00201203bf0751b6f94dda0ec1105b7f.png)

几乎每个系统中都存在冗余记录。一些交易形式上与记录AB InBev相关联，其他的则与AB INBEV NV相关联。如果重复记录未被发现，我们就会失去对该单一客户实体的完整了解。从[我写的一篇文章](https://medium.com/towards-data-science/deduplicate-and-clean-up-millions-of-location-records-abcffb308ebf)中借鉴的客户重复示例。图像由作者提供。
