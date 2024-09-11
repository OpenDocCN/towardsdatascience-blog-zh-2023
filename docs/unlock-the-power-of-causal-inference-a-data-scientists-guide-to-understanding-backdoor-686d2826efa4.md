# 解锁因果推断的力量：数据科学家的后门调整公式理解指南

> 原文：[https://towardsdatascience.com/unlock-the-power-of-causal-inference-a-data-scientists-guide-to-understanding-backdoor-686d2826efa4?source=collection_archive---------9-----------------------#2023-01-19](https://towardsdatascience.com/unlock-the-power-of-causal-inference-a-data-scientists-guide-to-understanding-backdoor-686d2826efa4?source=collection_archive---------9-----------------------#2023-01-19)

## 使用 Python 和 pgmpy 库的后门调整公式的完整示例

[](https://grahamharrison-86487.medium.com/?source=post_page-----686d2826efa4--------------------------------)[![Graham Harrison](../Images/c6bfe00c6e0cfcdf3bd042c7fdc03554.png)](https://grahamharrison-86487.medium.com/?source=post_page-----686d2826efa4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----686d2826efa4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----686d2826efa4--------------------------------) [Graham Harrison](https://grahamharrison-86487.medium.com/?source=post_page-----686d2826efa4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbd1c93739f33&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlock-the-power-of-causal-inference-a-data-scientists-guide-to-understanding-backdoor-686d2826efa4&user=Graham+Harrison&userId=bd1c93739f33&source=post_page-bd1c93739f33----686d2826efa4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----686d2826efa4--------------------------------) ·9 分钟阅读·2023年1月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F686d2826efa4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlock-the-power-of-causal-inference-a-data-scientists-guide-to-understanding-backdoor-686d2826efa4&user=Graham+Harrison&userId=bd1c93739f33&source=-----686d2826efa4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F686d2826efa4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlock-the-power-of-causal-inference-a-data-scientists-guide-to-understanding-backdoor-686d2826efa4&source=-----686d2826efa4---------------------bookmark_footer-----------)![](../Images/0959161efc60244eb04bbac98fcace83.png)

照片由 [Roberto Huczek](https://unsplash.com/@tamoio?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄，来源于 [Unsplash](https://unsplash.com/s/photos/door?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

在概率论中，根据知道其他变量的情况来查看数据集并计算事件的概率是非常直接的。

例如：

即，销售的概率等于在产品已被搜索的情况下，点击链接的概率。

然而，当数据中存在因果效应时，这种方法就会失效，这时因果推断就派上用场了。根据因果模式的不同，有多种方法，这篇文章将重点探讨解锁后门调整公式的力量。

“后门准则”存在于当X对Y的因果影响被一个影响X和Y的第三因素“混淆”时……

在这种情况下，由于Z的混淆效应，公式𝑝(𝑌|𝑋)不起作用，需要从Pearlean “do” 微积分应用后门调整公式才能获得正确结果。
