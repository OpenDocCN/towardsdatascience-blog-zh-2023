# Taipy：一种构建用户友好型生产就绪数据科学应用程序的工具

> 原文：[https://towardsdatascience.com/taipy-a-tool-for-building-user-friendly-production-ready-data-scientists-applications-80de97aaf7dd?source=collection_archive---------1-----------------------#2023-07-06](https://towardsdatascience.com/taipy-a-tool-for-building-user-friendly-production-ready-data-scientists-applications-80de97aaf7dd?source=collection_archive---------1-----------------------#2023-07-06)

## 一种简单、快速且高效的方式来构建全栈数据应用程序

[](https://zoumanakeita.medium.com/?source=post_page-----80de97aaf7dd--------------------------------)[![Zoumana Keita](../Images/34a15c1d03687816dbdbc065f5719f80.png)](https://zoumanakeita.medium.com/?source=post_page-----80de97aaf7dd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----80de97aaf7dd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----80de97aaf7dd--------------------------------) [Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----80de97aaf7dd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe6ae785a30d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftaipy-a-tool-for-building-user-friendly-production-ready-data-scientists-applications-80de97aaf7dd&user=Zoumana+Keita&userId=e6ae785a30d&source=post_page-e6ae785a30d----80de97aaf7dd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----80de97aaf7dd--------------------------------) ·14 min read·2023年7月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F80de97aaf7dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftaipy-a-tool-for-building-user-friendly-production-ready-data-scientists-applications-80de97aaf7dd&user=Zoumana+Keita&userId=e6ae785a30d&source=-----80de97aaf7dd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F80de97aaf7dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftaipy-a-tool-for-building-user-friendly-production-ready-data-scientists-applications-80de97aaf7dd&source=-----80de97aaf7dd---------------------bookmark_footer-----------)![](../Images/dd3f4a403960bdb49c1abb76cda2d813.png)

图片由 [Campaign Creators](https://unsplash.com/@campaign_creators) 提供，来源于 [Unsplash](https://unsplash.com/photos/pypeCEaJeZY)

# 介绍

作为数据科学家，你可能会想要创建数据可视化仪表盘、可视化数据，甚至实施业务应用程序，以帮助利益相关者做出可操作的决策。

多种工具和技术可以用来完成这些任务，无论是开源还是专有软件。然而，这些可能由于以下原因而不理想：

+   一些开源技术需要较高的学习曲线，并且需要雇佣具有相关专业知识的人员。因此，组织可能面临新员工的入职时间增加、培训成本上升以及找到合格候选人的潜在挑战。

+   其他开源解决方案非常适合原型，但无法扩展到生产就绪的应用程序。

+   类似地，专有工具也有其自身的挑战，包括较高的许可费用、有限的定制化选项，以及企业在切换到其他解决方案时的困难。

> *如果有一个工具不是这样就好了*…
