# 量化及其相关技术：将 LLMs 的推理时间减少 80%

> 原文：[`towardsdatascience.com/quantisation-and-co-reducing-inference-times-on-llms-by-80-671db9349bdb?source=collection_archive---------2-----------------------#2023-10-27`](https://towardsdatascience.com/quantisation-and-co-reducing-inference-times-on-llms-by-80-671db9349bdb?source=collection_archive---------2-----------------------#2023-10-27)

[](https://medium.com/@christopher_karg?source=post_page-----671db9349bdb--------------------------------)![Christopher Karg](https://medium.com/@christopher_karg?source=post_page-----671db9349bdb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----671db9349bdb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----671db9349bdb--------------------------------) [Christopher Karg](https://medium.com/@christopher_karg?source=post_page-----671db9349bdb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5fbda6d16c39&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fquantisation-and-co-reducing-inference-times-on-llms-by-80-671db9349bdb&user=Christopher+Karg&userId=5fbda6d16c39&source=post_page-5fbda6d16c39----671db9349bdb---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----671db9349bdb--------------------------------) ·12 分钟阅读·2023 年 10 月 27 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F671db9349bdb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fquantisation-and-co-reducing-inference-times-on-llms-by-80-671db9349bdb&user=Christopher+Karg&userId=5fbda6d16c39&source=-----671db9349bdb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F671db9349bdb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fquantisation-and-co-reducing-inference-times-on-llms-by-80-671db9349bdb&source=-----671db9349bdb---------------------bookmark_footer-----------)![](img/c44c0f469d07bf09ab4e359dd48265fe.png)

来源: [`www.pexels.com/photo/cropland-in-autumn-18684338/`](https://www.pexels.com/photo/cropland-in-autumn-18684338/)

量化是一种用于各种不同算法的技术，但随着大型语言模型（LLMs）的最近普及，它变得越来越重要。在这篇文章中，我旨在提供有关 LLM 量化的信息以及该技术对本地运行这些模型的影响。我将介绍量化以外的不同策略，以进一步减少运行这些模型的计算需求。我将解释这些技术为何可能对你感兴趣，并展示一些基准和代码示例，以说明这些技术的有效性。我还简要介绍了硬件要求/建议和现代工具，帮助你在自己的机器上实现 LLM 目标。在后续文章中，我计划提供逐步的说明和代码，用于微调你自己的 LLM，请密切关注。

TL;DR — 通过对我们的 LLM 进行量化并改变张量*数据类型*，我们能够在具有 2 倍参数的 LLM 上运行推理，同时将*墙时间*减少 80%。

如往常一样，如果你想讨论我在这里提到的任何内容，请[联系我](http://www.linkedin.com/in/-christopherkarg)。

本文中的所有观点均为我个人观点。本文未获得赞助。

# 什么是 LLM 的量化？
