# 从数据仓库和数据湖到数据网格：企业数据架构指南

> 原文：[https://towardsdatascience.com/from-data-warehouses-and-lakes-to-data-mesh-a-guide-to-enterprise-data-architecture-e2d93b2466b1?source=collection_archive---------2-----------------------#2023-05-12](https://towardsdatascience.com/from-data-warehouses-and-lakes-to-data-mesh-a-guide-to-enterprise-data-architecture-e2d93b2466b1?source=collection_archive---------2-----------------------#2023-05-12)

## 了解大型公司数据的运作方式

[](https://col-jung.medium.com/?source=post_page-----e2d93b2466b1--------------------------------)[![Col Jung](../Images/45ef9475b60f22a3c78c9c8e428812c3.png)](https://col-jung.medium.com/?source=post_page-----e2d93b2466b1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e2d93b2466b1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e2d93b2466b1--------------------------------) [Col Jung](https://col-jung.medium.com/?source=post_page-----e2d93b2466b1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8d4e2c520037&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-data-warehouses-and-lakes-to-data-mesh-a-guide-to-enterprise-data-architecture-e2d93b2466b1&user=Col+Jung&userId=8d4e2c520037&source=post_page-8d4e2c520037----e2d93b2466b1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e2d93b2466b1--------------------------------) · 19分钟阅读 · 2023年5月12日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe2d93b2466b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-data-warehouses-and-lakes-to-data-mesh-a-guide-to-enterprise-data-architecture-e2d93b2466b1&user=Col+Jung&userId=8d4e2c520037&source=-----e2d93b2466b1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe2d93b2466b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-data-warehouses-and-lakes-to-data-mesh-a-guide-to-enterprise-data-architecture-e2d93b2466b1&source=-----e2d93b2466b1---------------------bookmark_footer-----------)![](../Images/9b6681fe49ae44a44ba803426f10d2ab.png)

图片： [Headway](https://unsplash.com/photos/5QgIuuBxKwM) (Unsplash)

数据科学课程与现实中处理数据的*现实*之间存在脱节。

当我五年前在澳大利亚的“四大”银行之一找到我的第一份分析工作时，我面临了一个复杂的数据环境，其特点是…

+   在**寻找**、**访问**和**使用**数据方面的挑战；

+   **竞争**的商业优先级将人们拉向不同的方向；

+   **遗留**系统难以维护和升级；

+   一种对数据驱动见解有抵触的**文化**；

+   **孤立**的团队彼此之间没有沟通。

**更新**：我现在在[YouTube](https://www.youtube.com/@col_builds)上发布分析内容。

一段时间以来，我默默地接受了这样的事实，也许这就是企业数据领域的现状。我相信，尽管我们的技术栈在迅速演变，但用户体验最终会跟上…

我在数据科学方面进行了自我培训，但实际上*做*数据科学并不是那么简单。在线课程并不能为此做好准备。
