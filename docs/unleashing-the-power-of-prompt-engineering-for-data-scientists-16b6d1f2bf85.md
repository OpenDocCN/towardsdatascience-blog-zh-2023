# 解放数据科学家的提示工程力量

> 原文：[https://towardsdatascience.com/unleashing-the-power-of-prompt-engineering-for-data-scientists-16b6d1f2bf85?source=collection_archive---------6-----------------------#2023-06-07](https://towardsdatascience.com/unleashing-the-power-of-prompt-engineering-for-data-scientists-16b6d1f2bf85?source=collection_archive---------6-----------------------#2023-06-07)

## 如果你从事数据相关工作，如何以及为何撰写有效的提示

[](https://federicotrotta.medium.com/?source=post_page-----16b6d1f2bf85--------------------------------)[![Federico Trotta](../Images/e997e3a96940c16ab5071629016d82fd.png)](https://federicotrotta.medium.com/?source=post_page-----16b6d1f2bf85--------------------------------)[](https://towardsdatascience.com/?source=post_page-----16b6d1f2bf85--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----16b6d1f2bf85--------------------------------) [Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----16b6d1f2bf85--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F654cd4bbe899&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funleashing-the-power-of-prompt-engineering-for-data-scientists-16b6d1f2bf85&user=Federico+Trotta&userId=654cd4bbe899&source=post_page-654cd4bbe899----16b6d1f2bf85---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----16b6d1f2bf85--------------------------------) ·18分钟阅读·2023年6月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F16b6d1f2bf85&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funleashing-the-power-of-prompt-engineering-for-data-scientists-16b6d1f2bf85&user=Federico+Trotta&userId=654cd4bbe899&source=-----16b6d1f2bf85---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F16b6d1f2bf85&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funleashing-the-power-of-prompt-engineering-for-data-scientists-16b6d1f2bf85&source=-----16b6d1f2bf85---------------------bookmark_footer-----------)![](../Images/861db2c8c6d2d4a0826e0d619b005bff.png)

图片由 [Gerd Altmann](https://pixabay.com/it/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4126485) 提供，来源于 [Pixabay](https://pixabay.com/it//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4126485)

由于 GPT 模型的出现，提示工程正在成为数据科学领域的一个兴趣点。最开始，我们看到全球各地的许多人测试 ChatGPT 以试图欺骗它。然后，虽然这种趋势（终于！）已经结束，但使用它来自动化无聊的任务或帮助完成一般任务的行为却在稳步增长。

开发人员和数据科学家是从使用像 ChatGPT 这样的提示系统中受益最大的群体。因此，在本文中，我们将概述提示工程，并介绍如何为数据科学家编写高效的提示。

我知道你从 ChatGPT 中受益匪浅，对吧？！但事实上，有时候作为数据科学家，我们无法完全获得所需的信息。因此，让我们看看如何通过一些简单的预防措施来提升我们的提示技能。

这是你在本文中将阅读到的内容：

```py
**Table of Contents:**

The importance of prompt engineering today
How prompt engineering can affect Data Scientists
Examples of effective prompts for Data Scientists
```

# 今天的提示工程的重要性
