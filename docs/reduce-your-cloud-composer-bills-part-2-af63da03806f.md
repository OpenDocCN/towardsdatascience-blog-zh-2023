# 减少你的 Cloud Composer 账单（第二部分）

> 原文：[https://towardsdatascience.com/reduce-your-cloud-composer-bills-part-2-af63da03806f?source=collection_archive---------11-----------------------#2023-03-31](https://towardsdatascience.com/reduce-your-cloud-composer-bills-part-2-af63da03806f?source=collection_archive---------11-----------------------#2023-03-31)

## 使用计划 CICD 管道关闭环境并恢复到先前状态

[](https://marcgeremie.medium.com/?source=post_page-----af63da03806f--------------------------------)[![Marc Djohossou](../Images/096f7877d14b7671e48500175931dbea.png)](https://marcgeremie.medium.com/?source=post_page-----af63da03806f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----af63da03806f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----af63da03806f--------------------------------) [Marc Djohossou](https://marcgeremie.medium.com/?source=post_page-----af63da03806f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd99fd7fe99ef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freduce-your-cloud-composer-bills-part-2-af63da03806f&user=Marc+Djohossou&userId=d99fd7fe99ef&source=post_page-d99fd7fe99ef----af63da03806f---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----af63da03806f--------------------------------) · 5 分钟阅读 · 2023年3月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Faf63da03806f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freduce-your-cloud-composer-bills-part-2-af63da03806f&user=Marc+Djohossou&userId=d99fd7fe99ef&source=-----af63da03806f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faf63da03806f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freduce-your-cloud-composer-bills-part-2-af63da03806f&source=-----af63da03806f---------------------bookmark_footer-----------)![](../Images/5916b0798af7eae9eb8a1b97188a2840.png)

图片来自 [Sasun Bughdaryan](https://unsplash.com/@sasun1990?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这是两部分系列的第二部分，旨在介绍一种高效的节省 Cloud Composer 作业编排费用的方法。因此，我强烈建议查看[第一部分](https://medium.com/towards-data-science/reduce-your-cloud-composer-bills-f03e112df689)（如果尚未完成）。

以下是将涵盖的主要主题：

> 理解 Cloud Composer 2 定价（第一部分）
> 
> 快照作为关闭 Composer 但仍保留其状态的一种方法（第 1 部分）
> 
> 使用快照创建 Composer 环境（第 1 部分）
> 
> 总结（第 1 部分）
> 
> 销毁 Composer 环境以节省开支（第 2 部分）
> 
> 更新 Composer 环境（第 2 部分）
> 
> 自动化 Composer 环境的创建与销毁（第 2 部分）
> 
> 总结（第 2 部分）

## 销毁 Composer 环境以节省开支

以下是如何降低 Cloud Composer 费用的方法：**在未使用时关闭环境**。
