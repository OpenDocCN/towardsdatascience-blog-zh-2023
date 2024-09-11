# 预测星巴克奖励计划的成功

> 原文：[https://towardsdatascience.com/predicting-success-of-a-reward-program-at-starbucks-b32b77dcf9b8?source=collection_archive---------15-----------------------#2023-06-20](https://towardsdatascience.com/predicting-success-of-a-reward-program-at-starbucks-b32b77dcf9b8?source=collection_archive---------15-----------------------#2023-06-20)

## 初学者友好——对从头到尾的完整项目进行逐步解释

[](https://erdemisbilen.medium.com/?source=post_page-----b32b77dcf9b8--------------------------------)[![Erdem Isbilen](../Images/6b6ca6806024725245132894b2958e49.png)](https://erdemisbilen.medium.com/?source=post_page-----b32b77dcf9b8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b32b77dcf9b8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b32b77dcf9b8--------------------------------) [Erdem Isbilen](https://erdemisbilen.medium.com/?source=post_page-----b32b77dcf9b8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd13ef140191e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpredicting-success-of-a-reward-program-at-starbucks-b32b77dcf9b8&user=Erdem+Isbilen&userId=d13ef140191e&source=post_page-d13ef140191e----b32b77dcf9b8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b32b77dcf9b8--------------------------------) · 9 分钟阅读 · 2023年6月20日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb32b77dcf9b8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpredicting-success-of-a-reward-program-at-starbucks-b32b77dcf9b8&user=Erdem+Isbilen&userId=d13ef140191e&source=-----b32b77dcf9b8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb32b77dcf9b8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpredicting-success-of-a-reward-program-at-starbucks-b32b77dcf9b8&source=-----b32b77dcf9b8---------------------bookmark_footer-----------)![](../Images/257d86a537c79400e22c628914ba71c3.png)

图片由 [Robert Linder](https://unsplash.com/@rwlinder?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 项目概述

这个项目专注于识别能够有效吸引星巴克现有客户并吸引新客户的奖励计划优惠。

星巴克是一家数据驱动的公司，通过利用包含客户信息、特别优惠和交易数据的数据集，投资于全面了解其客户。

为了开发一个能够确定奖励计划成功与否的模型，我将项目分为**三个阶段**：

1.  **检查和清理**由Udacity提供的数据。

1.  创建一个**结合所有相关信息**的数据集。

1.  构建和评估**三个分类模型**的性能，以**预测特定人群奖励计划的成功或失败**。

# 问题陈述

在市场营销活动中进行重大投资是一个复杂的决策，需要得到各方利益相关者的批准、财务资源和时间。因此，拥有一个可以预测是否值得启动的模型…
