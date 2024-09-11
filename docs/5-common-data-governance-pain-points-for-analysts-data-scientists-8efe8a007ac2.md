# 5 个分析师和数据科学家的常见数据治理痛点

> 原文：[https://towardsdatascience.com/5-common-data-governance-pain-points-for-analysts-data-scientists-8efe8a007ac2?source=collection_archive---------5-----------------------#2023-08-24](https://towardsdatascience.com/5-common-data-governance-pain-points-for-analysts-data-scientists-8efe8a007ac2?source=collection_archive---------5-----------------------#2023-08-24)

## 了解支持创新的保护措施

[](https://col-jung.medium.com/?source=post_page-----8efe8a007ac2--------------------------------)[![Col Jung](../Images/45ef9475b60f22a3c78c9c8e428812c3.png)](https://col-jung.medium.com/?source=post_page-----8efe8a007ac2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8efe8a007ac2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8efe8a007ac2--------------------------------) [Col Jung](https://col-jung.medium.com/?source=post_page-----8efe8a007ac2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8d4e2c520037&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-common-data-governance-pain-points-for-analysts-data-scientists-8efe8a007ac2&user=Col+Jung&userId=8d4e2c520037&source=post_page-8d4e2c520037----8efe8a007ac2---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8efe8a007ac2--------------------------------) ·14 分钟阅读·2023年8月24日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8efe8a007ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-common-data-governance-pain-points-for-analysts-data-scientists-8efe8a007ac2&source=-----8efe8a007ac2---------------------bookmark_footer-----------)![](../Images/c78eba6a2cdcd390971321a0512f7688.png)

图片：[Headway](https://unsplash.com/photos/5QgIuuBxKwM) (Unsplash)

你是大公司里的分析师或数据科学家吗？

如果你曾遇到过这些令人困惑的问题，请举手：

+   **寻找数据** 感觉就像是去进行一次 *福尔摩斯* 探险。

+   **理解数据血统** 令人感到异常挫败。

+   **获取数据** 成了一场与繁文缛节怪兽的对决。

这是我常听到的一个常见调侃，无论是来自公民分析师还是专业分析师：

> “那些数据治理的人真会让生活变得有趣……”

是时候放松一下了。

借鉴我在澳大利亚某大银行担任工程师和数据科学家的经历[半个十年](/from-data-warehouses-and-lakes-to-data-mesh-a-guide-to-enterprise-data-architecture-e2d93b2466b1)，我有幸跨越这道激烈的界限：既是数据的渴求者，同时又是他人的数据守门员。

**更新**：我现在在[YouTube](https://www.youtube.com/@col_builds)上讨论分析和数据。

在这篇文章中，我将分三部分深入探讨…

1.  *基础知识*：数据如何在组织中**流动**。*这很混乱！*

1.  *理解* **常见痛点**…
