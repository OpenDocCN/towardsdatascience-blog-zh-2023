# 用 Tableau 仪表板体验大数据：挑战与收获

> 原文：[`towardsdatascience.com/tableau-dashboards-and-big-data-learnings-e0a29cb7377c?source=collection_archive---------20-----------------------#2023-01-05`](https://towardsdatascience.com/tableau-dashboards-and-big-data-learnings-e0a29cb7377c?source=collection_archive---------20-----------------------#2023-01-05)

## 用 Tableau 专业地分析大数据

[](https://alle-sravani.medium.com/?source=post_page-----e0a29cb7377c--------------------------------)![Alle Sravani](https://alle-sravani.medium.com/?source=post_page-----e0a29cb7377c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e0a29cb7377c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e0a29cb7377c--------------------------------) [Alle Sravani](https://alle-sravani.medium.com/?source=post_page-----e0a29cb7377c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F79c59886365f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftableau-dashboards-and-big-data-learnings-e0a29cb7377c&user=Alle+Sravani&userId=79c59886365f&source=post_page-79c59886365f----e0a29cb7377c---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e0a29cb7377c--------------------------------) ·阅读 7 分钟·2023 年 1 月 5 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe0a29cb7377c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftableau-dashboards-and-big-data-learnings-e0a29cb7377c&user=Alle+Sravani&userId=79c59886365f&source=-----e0a29cb7377c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe0a29cb7377c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftableau-dashboards-and-big-data-learnings-e0a29cb7377c&source=-----e0a29cb7377c---------------------bookmark_footer-----------)![](img/9e55124e1464def465f8fdc6e9a68f7e.png)

[Myriam Jessier](https://unsplash.com/@mjessier?utm_source=medium&utm_medium=referral) 拍摄的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我的简单仪表盘创建之旅始于 Excel。从那时起，我使用了多个工具，如 Qlik、Sisense、PowerBI 和 Tableau。Tableau 仍然是我的最爱，因为它永远不会显得枯燥。它易于使用和学习，但也可以迅速变得复杂。完成一项任务的满足感是无价的。我有机会与许多 Tableau 专家合作。我逐渐掌握了许多制作强大视觉效果的技巧和窍门。尽管我已经掌握了这个工具的使用，但我认为我只是刚刚触及表面。

我最近面临的最大挑战之一是使用大数据构建仪表盘。我需要设计一个仪表盘来跟踪发送到移动应用用户的推送通知的有效性。推送通知发送的原因有很多，包括状态更新、新优惠、提醒、广告活动等。根据用户数量和发送的通知数量，这些数据可能变得极其庞大。当我开始这个项目时，我拥有三个月的数据，源表中记录超过 5000 万条！
