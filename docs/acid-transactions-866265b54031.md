# 什么是 ACID 事务？

> 原文：[https://towardsdatascience.com/acid-transactions-866265b54031?source=collection_archive---------13-----------------------#2023-07-05](https://towardsdatascience.com/acid-transactions-866265b54031?source=collection_archive---------13-----------------------#2023-07-05)

## 在数据库事务的背景下理解 ACID 属性

[](https://gmyrianthous.medium.com/?source=post_page-----866265b54031--------------------------------)[![Giorgos Myrianthous](../Images/ff4b116e4fb9a095ce45eb064fde5af3.png)](https://gmyrianthous.medium.com/?source=post_page-----866265b54031--------------------------------)[](https://towardsdatascience.com/?source=post_page-----866265b54031--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----866265b54031--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----866265b54031--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Facid-transactions-866265b54031&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----866265b54031---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----866265b54031--------------------------------) ·3 min read·2023年7月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F866265b54031&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Facid-transactions-866265b54031&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----866265b54031---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F866265b54031&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Facid-transactions-866265b54031&source=-----866265b54031---------------------bookmark_footer-----------)![](../Images/3d32ef5f17f84fc39dd7972aa55efea2.png)

图片由 [Michal Matlon](https://unsplash.com/@michalmatlon?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/_4VlY7cW86M?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在数据库操作的背景下，事务指的是被视为一个单一逻辑工作单元的操作，旨在使底层系统保持一致的状态。

一致性是通过确保所有操作在成功完成后执行，或者在任何操作因某种原因失败时不执行任何操作来维持的。

## ACID 属性

数据库事务应符合所谓的 ACID 属性。这样的系统称为事务型系统，确保每个操作，包括读、写或更新，符合 ACID 属性。ACID 的首字母缩写代表 **A**tomicity（原子性）、**C**onsistency（一致性）、**I**solation（隔离性）和 **D**urability（持久性）。

**原子性**：这一属性指的是事务被视为一个整体工作单元。如果事务中的任何操作失败，那么没有任何操作会被提交，之前执行的操作（如果有的话）会被回滚，系统将恢复到之前的状态。原子性可以防止数据丢失或数据损坏，因为只有在每个语句都成功执行的情况下，事务才会被提交。
