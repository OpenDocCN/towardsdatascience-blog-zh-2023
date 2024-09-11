# 使用云工作流和云调度器轻松构建事件提醒

> 原文：[`towardsdatascience.com/easily-build-an-event-reminder-with-cloud-workflows-and-cloud-scheduler-efba59b3df9a?source=collection_archive---------17-----------------------#2023-04-26`](https://towardsdatascience.com/easily-build-an-event-reminder-with-cloud-workflows-and-cloud-scheduler-efba59b3df9a?source=collection_archive---------17-----------------------#2023-04-26)

## 使用案例：识别生日并发送通知邮件

[](https://marcgeremie.medium.com/?source=post_page-----efba59b3df9a--------------------------------)![Marc Djohossou](https://marcgeremie.medium.com/?source=post_page-----efba59b3df9a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----efba59b3df9a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----efba59b3df9a--------------------------------) [Marc Djohossou](https://marcgeremie.medium.com/?source=post_page-----efba59b3df9a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd99fd7fe99ef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feasily-build-an-event-reminder-with-cloud-workflows-and-cloud-scheduler-efba59b3df9a&user=Marc+Djohossou&userId=d99fd7fe99ef&source=post_page-d99fd7fe99ef----efba59b3df9a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----efba59b3df9a--------------------------------) ·7 min read·2023 年 4 月 26 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fefba59b3df9a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feasily-build-an-event-reminder-with-cloud-workflows-and-cloud-scheduler-efba59b3df9a&user=Marc+Djohossou&userId=d99fd7fe99ef&source=-----efba59b3df9a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fefba59b3df9a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feasily-build-an-event-reminder-with-cloud-workflows-and-cloud-scheduler-efba59b3df9a&source=-----efba59b3df9a---------------------bookmark_footer-----------)![](img/4defb51e3279ab731da36613dcca9606.png)

[Imants Kaziļuns](https://unsplash.com/@imants72?utm_source=medium&utm_medium=referral) 拍摄于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

构建事件提醒，例如 Facebook 的周年提醒，看起来可能比实际情况更复杂。在这篇文章中，我详细讲解了一种简单而高效的方法来构建一个生日提醒应用。请阅读本文，解锁包含完整工作代码的仓库。以下是主要内容：

> 云工作流与 Cloud Run 介绍
> 
> 构建生日识别服务
> 
> 构建电子邮件通知服务
> 
> 构建生日提醒服务
> 
> 总结

## 云工作流与 Cloud Run 介绍

[云工作流](https://cloud.google.com/workflows?hl=en) 是 Google Cloud 提供的一项服务，允许你编排一系列基于 HTTP 的服务。这些服务可以是内部的（当它们属于 Google Cloud 域时）或外部的。除了具有吸引力的成本外，云工作流还提供一些独特而有趣的功能，例如 [等待和回调](https://cloud.google.com/workflows/docs/creating-callback-endpoints)，可以让你最多等待 1 年以便某个事件发生。
