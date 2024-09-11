# 解码 Auto-GPT

> 原文：[https://towardsdatascience.com/decoding-auto-gpt-fae16ff1ee75?source=collection_archive---------3-----------------------#2023-08-08](https://towardsdatascience.com/decoding-auto-gpt-fae16ff1ee75?source=collection_archive---------3-----------------------#2023-08-08)

## **自主** GPT-4 的机制

[](https://medium.com/@maartengrootendorst?source=post_page-----fae16ff1ee75--------------------------------)[![Maarten Grootendorst](../Images/58e24b9cf7e10ff1cd5ffd75a32d1a26.png)](https://medium.com/@maartengrootendorst?source=post_page-----fae16ff1ee75--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fae16ff1ee75--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fae16ff1ee75--------------------------------) [Maarten Grootendorst](https://medium.com/@maartengrootendorst?source=post_page-----fae16ff1ee75--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F22405c3b2875&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecoding-auto-gpt-fae16ff1ee75&user=Maarten+Grootendorst&userId=22405c3b2875&source=post_page-22405c3b2875----fae16ff1ee75---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fae16ff1ee75--------------------------------) ·8分钟阅读·2023年8月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffae16ff1ee75&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecoding-auto-gpt-fae16ff1ee75&user=Maarten+Grootendorst&userId=22405c3b2875&source=-----fae16ff1ee75---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffae16ff1ee75&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecoding-auto-gpt-fae16ff1ee75&source=-----fae16ff1ee75---------------------bookmark_footer-----------)![](../Images/ccce7f423325fb2990f06b6a85bc947e.png)

自 ChatGPT 发布以来，已经出现了许多有趣、复杂且创新的解决方案。社区探索了无数提高其能力的可能性。

其中之一是广为人知的[Auto-GPT](https://github.com/Significant-Gravitas/Auto-GPT)包。它拥有超过**14万**个星标，是Github上排名最高的仓库之一！

![](../Images/47bcc2b61fc850759406607d46b2e448.png)

Auto-GPT 在 Github 上的星标数量。来自 star-history.com

Auto-GPT 旨在使 GPT-4 完全**自主**。

> Auto-GPT 赋予 GPT-4 自主决策的能力

听起来不可思议，实际上确实如此！但它是如何工作的呢？

在这篇文章中，我们将深入探讨 Auto-GPT 的架构，并探索它如何实现自主行为。

# 架构

Auto-GPT 有一个整体架构，或者说是一个主要循环，它用来建模自主行为。

首先，我们来描述一下整体情况，然后我们将逐步深入每个步骤：
