# 揭秘依赖关系及其在因果推断和因果验证中的重要性

> 原文：[https://towardsdatascience.com/demystifying-dependence-and-why-it-is-important-in-causal-inference-and-causal-validation-4263b18d5f04?source=collection_archive---------11-----------------------#2023-11-11](https://towardsdatascience.com/demystifying-dependence-and-why-it-is-important-in-causal-inference-and-causal-validation-4263b18d5f04?source=collection_archive---------11-----------------------#2023-11-11)

## 理解依赖概念的逐步指南，以及如何使用Python验证有向无环图

[](https://grahamharrison-86487.medium.com/?source=post_page-----4263b18d5f04--------------------------------)[![Graham Harrison](../Images/c6bfe00c6e0cfcdf3bd042c7fdc03554.png)](https://grahamharrison-86487.medium.com/?source=post_page-----4263b18d5f04--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4263b18d5f04--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4263b18d5f04--------------------------------) [Graham Harrison](https://grahamharrison-86487.medium.com/?source=post_page-----4263b18d5f04--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbd1c93739f33&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdemystifying-dependence-and-why-it-is-important-in-causal-inference-and-causal-validation-4263b18d5f04&user=Graham+Harrison&userId=bd1c93739f33&source=post_page-bd1c93739f33----4263b18d5f04---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4263b18d5f04--------------------------------) · 16分钟阅读 · 2023年11月11日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4263b18d5f04&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdemystifying-dependence-and-why-it-is-important-in-causal-inference-and-causal-validation-4263b18d5f04&user=Graham+Harrison&userId=bd1c93739f33&source=-----4263b18d5f04---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4263b18d5f04&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdemystifying-dependence-and-why-it-is-important-in-causal-inference-and-causal-validation-4263b18d5f04&source=-----4263b18d5f04---------------------bookmark_footer-----------)![](../Images/2acee73c46470784e1400ed88bbbe08b.png)

照片由[Ana Municio](https://unsplash.com/@lamunix?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)提供，来自[Unsplash](https://unsplash.com/photos/gray-and-brown-stones-on-gray-ground-PbzntH58GLQ?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

# 介绍

因果推断是数据科学中的一个新兴分支，关注于确定事件和结果之间的因果关系，它有潜力显著提升机器学习为组织创造的价值。

例如，传统的机器学习算法可以预测哪些贷款客户可能会违约，从而实现对客户的主动干预。然而，尽管该算法对减少贷款违约很有帮助，但它无法理解违约发生的原因，而了解违约原因将有助于解决根本问题。在这种情况下，主动干预可能不再必要，因为导致违约的因素已经被永久解决。

这是因果推断的承诺，也是它为何有潜力为能够利用这种潜力的组织带来重大影响和结果的原因。
