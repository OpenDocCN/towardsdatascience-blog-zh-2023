# 使用不平衡数据进行模型选择：仅凭AUC可能无法拯救你

> 原文：[https://towardsdatascience.com/model-selection-with-imbalance-data-only-auc-may-not-save-you-5aed73c5efed?source=collection_archive---------7-----------------------#2023-02-22](https://towardsdatascience.com/model-selection-with-imbalance-data-only-auc-may-not-save-you-5aed73c5efed?source=collection_archive---------7-----------------------#2023-02-22)

## 你是否有效地搜索参数？

[](https://medium.com/@cerlymarco?source=post_page-----5aed73c5efed--------------------------------)[![Marco Cerliani](../Images/48a07a024349bac3c8e397bf5a0372e2.png)](https://medium.com/@cerlymarco?source=post_page-----5aed73c5efed--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5aed73c5efed--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5aed73c5efed--------------------------------) [Marco Cerliani](https://medium.com/@cerlymarco?source=post_page-----5aed73c5efed--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc843902314c7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmodel-selection-with-imbalance-data-only-auc-may-not-save-you-5aed73c5efed&user=Marco+Cerliani&userId=c843902314c7&source=post_page-c843902314c7----5aed73c5efed---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5aed73c5efed--------------------------------) ·6分钟阅读·2023年2月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5aed73c5efed&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmodel-selection-with-imbalance-data-only-auc-may-not-save-you-5aed73c5efed&user=Marco+Cerliani&userId=c843902314c7&source=-----5aed73c5efed---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5aed73c5efed&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmodel-selection-with-imbalance-data-only-auc-may-not-save-you-5aed73c5efed&source=-----5aed73c5efed---------------------bookmark_footer-----------)![](../Images/0fd9065c148b0219514a0be4d198178b.png)

图片由 [Mpho Mojapelo](https://unsplash.com/@mpho_mojapelo?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

大多数数据科学家在会议上向业务利益相关者展示机器学习结果时，通常会回答这样的问题：

> AUC？这是什么？能详细说明一下吗？

**数据科学日常工作中标准的术语和概念可能对大多数人来说都很陌生**。当人工智能产品被开发用于解决实际问题时，这种情况经常发生。在这种情况下，数据科学家与领域专家一起工作，合作以理解领域动态，并相应地将其融入自动化解决方案中。

**对人工智能在解决业务问题中提供的附加值保持批判性视角是至关重要的**。在许多情况下，采用机器学习可能没有意义，因为任务可以通过简单的自动化规则解决，或者现有数据中没有证据支持使用人工智能技术。也就是说，**选择最合适的指标来评估提出的解决方案的有效性是一个非常重要的步骤**。

**选择合适的指标是领域依赖的，并且根据需求而变化**。选择AUC作为指标，以展示给…
