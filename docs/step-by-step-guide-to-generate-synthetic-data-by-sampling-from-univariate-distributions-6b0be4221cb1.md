# 逐步指南：通过从单变量分布中采样生成合成数据

> 原文：[https://towardsdatascience.com/step-by-step-guide-to-generate-synthetic-data-by-sampling-from-univariate-distributions-6b0be4221cb1?source=collection_archive---------7-----------------------#2023-03-27](https://towardsdatascience.com/step-by-step-guide-to-generate-synthetic-data-by-sampling-from-univariate-distributions-6b0be4221cb1?source=collection_archive---------7-----------------------#2023-03-27)

## 了解如何创建合成数据，以防你的项目数据不足或用于模拟

[](https://erdogant.medium.com/?source=post_page-----6b0be4221cb1--------------------------------)[![Erdogan Taskesen](../Images/8e62cdae0502687710d8ae4bbcd8966e.png)](https://erdogant.medium.com/?source=post_page-----6b0be4221cb1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6b0be4221cb1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6b0be4221cb1--------------------------------) [Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----6b0be4221cb1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4e636e2ef813&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstep-by-step-guide-to-generate-synthetic-data-by-sampling-from-univariate-distributions-6b0be4221cb1&user=Erdogan+Taskesen&userId=4e636e2ef813&source=post_page-4e636e2ef813----6b0be4221cb1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6b0be4221cb1--------------------------------) ·10 分钟阅读·2023年3月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6b0be4221cb1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstep-by-step-guide-to-generate-synthetic-data-by-sampling-from-univariate-distributions-6b0be4221cb1&user=Erdogan+Taskesen&userId=4e636e2ef813&source=-----6b0be4221cb1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6b0be4221cb1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstep-by-step-guide-to-generate-synthetic-data-by-sampling-from-univariate-distributions-6b0be4221cb1&source=-----6b0be4221cb1---------------------bookmark_footer-----------)![](../Images/c6e8220af678c94f7c2091863414a236.png)

图片来源：[Debby Hudson](https://unsplash.com/@hudsoncrafted?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 于 [Unsplash](https://unsplash.com/s/photos/create?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

数据是数据科学项目中的燃料。但如果观察数据稀少、昂贵或难以测量怎么办？合成数据可能是解决方案。合成数据是人工生成的数据，模仿现实世界事件的统计特性。***我将展示如何通过从单变量分布中抽样来创建连续的合成数据。*** *首先，我将演示如何通过仿真评估系统和过程，其中我们需要选择概率分布并指定参数。其次，我将展示如何生成模拟现有数据集特性的样本，即按概率模型分布的随机变量。所有示例均使用* ***scipy*** *和* [***distfit***](/how-to-find-the-best-theoretical-distribution-for-your-data-a26e5673b4bd) *库创建。*

*如果你觉得这篇文章有帮助，请使用我的* [*推荐链接*](https://medium.com/@erdogant/membership) *继续无极限地学习，并注册 Medium 会员。此外，* [*关注我*](http://erdogant.medium.com/) *以便及时了解我最新的内容！*

# 合成数据 — 背景。
