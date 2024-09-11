# 解锁数据访问：在缺少 API 端点的情况下利用触发器

> 原文：[`towardsdatascience.com/unlocking-data-access-harnessing-triggers-in-the-absence-of-api-endpoints-4b5c8b775058?source=collection_archive---------10-----------------------#2023-06-09`](https://towardsdatascience.com/unlocking-data-access-harnessing-triggers-in-the-absence-of-api-endpoints-4b5c8b775058?source=collection_archive---------10-----------------------#2023-06-09)

## 使用触发器填补数据拼图中的缺失部分

[](https://mg-subha.medium.com/?source=post_page-----4b5c8b775058--------------------------------)![Subha Ganapathi](https://mg-subha.medium.com/?source=post_page-----4b5c8b775058--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4b5c8b775058--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4b5c8b775058--------------------------------) [Subha Ganapathi](https://mg-subha.medium.com/?source=post_page-----4b5c8b775058--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe911b9969577&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlocking-data-access-harnessing-triggers-in-the-absence-of-api-endpoints-4b5c8b775058&user=Subha+Ganapathi&userId=e911b9969577&source=post_page-e911b9969577----4b5c8b775058---------------------post_header-----------) 发表在[ Towards Data Science](https://towardsdatascience.com/?source=post_page-----4b5c8b775058--------------------------------) ·10 分钟阅读·2023 年 6 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4b5c8b775058&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlocking-data-access-harnessing-triggers-in-the-absence-of-api-endpoints-4b5c8b775058&user=Subha+Ganapathi&userId=e911b9969577&source=-----4b5c8b775058---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4b5c8b775058&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlocking-data-access-harnessing-triggers-in-the-absence-of-api-endpoints-4b5c8b775058&source=-----4b5c8b775058---------------------bookmark_footer-----------)![](img/8ab9ec3ecea0b81357cdfa9d26f5cf96.png)

图片来源于[Pixabay](https://www.pexels.com/photo/abstract-accuracy-accurate-aim-262438/)自[ Pexels](https://www.pexels.com/)

# 概述

**您是否曾经遇到过这样的场景：尝试通过其 API 从事务系统（例如电子商务系统）中提取关键信息，却发现所需的信息无法通过提供的端点访问？如果是这样，请继续阅读，以了解如何有效地使用触发器解决这一挑战。**

在没有端点的情况下，我们可能会认为直接从事务表中查询数据是一个选项**。** 直接查询事务表绝对不是一个好主意，因为这会对事务系统的性能和稳定性产生重大影响，特别是在涉及电子商务系统时。当你尝试从一个实时的电子商务系统中查询数据时，可能会对用户体验产生不利影响（想象一下在亚马逊购物时，等了 5 到 10 分钟才检索到你的购物车！）。

除此之外，在事务系统的表上运行作业可能会干扰正在进行的事务。如果你考虑在数据仓库表中进行‘Truncate-Load’操作，这个问题会变得更加关键…
