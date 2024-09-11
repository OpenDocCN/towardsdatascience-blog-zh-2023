# 解锁因果推断与前门调整的力量：数据科学家的深入指南

> 原文：[https://towardsdatascience.com/unlock-the-power-of-causal-inference-front-door-adjustment-an-in-depth-guide-for-data-scientists-8e7b8ba33421?source=collection_archive---------6-----------------------#2023-02-14](https://towardsdatascience.com/unlock-the-power-of-causal-inference-front-door-adjustment-an-in-depth-guide-for-data-scientists-8e7b8ba33421?source=collection_archive---------6-----------------------#2023-02-14)

## 包含所有 Python 源代码的因果推断前门调整的完整解释及示例

[](https://grahamharrison-86487.medium.com/?source=post_page-----8e7b8ba33421--------------------------------)[![Graham Harrison](../Images/c6bfe00c6e0cfcdf3bd042c7fdc03554.png)](https://grahamharrison-86487.medium.com/?source=post_page-----8e7b8ba33421--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8e7b8ba33421--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8e7b8ba33421--------------------------------) [Graham Harrison](https://grahamharrison-86487.medium.com/?source=post_page-----8e7b8ba33421--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbd1c93739f33&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlock-the-power-of-causal-inference-front-door-adjustment-an-in-depth-guide-for-data-scientists-8e7b8ba33421&user=Graham+Harrison&userId=bd1c93739f33&source=post_page-bd1c93739f33----8e7b8ba33421---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8e7b8ba33421--------------------------------) · 11 分钟阅读 · 2023 年 2 月 14 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8e7b8ba33421&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlock-the-power-of-causal-inference-front-door-adjustment-an-in-depth-guide-for-data-scientists-8e7b8ba33421&user=Graham+Harrison&userId=bd1c93739f33&source=-----8e7b8ba33421---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8e7b8ba33421&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlock-the-power-of-causal-inference-front-door-adjustment-an-in-depth-guide-for-data-scientists-8e7b8ba33421&source=-----8e7b8ba33421---------------------bookmark_footer-----------)![](../Images/5e39a34966dc35b7649fbd31372905e3.png)

图片由 [Evelyn Paris](https://unsplash.com/@evelynparis?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/front-door?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 目标

> 到了本文的结尾，你将理解因果推断前门调整的魔力，它可以计算一个事件对结果的影响，即使存在其他影响这两个因素的未测量或甚至未知的因素，同时你将完全获得所有的 Python 代码。

我在互联网上和许多书籍中搜寻了很多，试图找到一个在 Python 中完全有效的前门公式示例，但始终一无所获，因此，除非有我遗漏的资料，否则你即将阅读的内容确实是独一无二的……

# 介绍

在最近的一篇文章中，我探讨了后门调整公式的力量，以计算一个事件对结果的真实影响，即使存在可观察的“混杂”因素。
