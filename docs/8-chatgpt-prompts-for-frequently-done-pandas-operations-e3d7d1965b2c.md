# 8 个 ChatGPT 提示，适用于常用的 Pandas 操作

> 原文：[https://towardsdatascience.com/8-chatgpt-prompts-for-frequently-done-pandas-operations-e3d7d1965b2c?source=collection_archive---------4-----------------------#2023-05-26](https://towardsdatascience.com/8-chatgpt-prompts-for-frequently-done-pandas-operations-e3d7d1965b2c?source=collection_archive---------4-----------------------#2023-05-26)

## 使用 Pandas 快速完成任务

[](https://sonery.medium.com/?source=post_page-----e3d7d1965b2c--------------------------------)[![Soner Yıldırım](../Images/c589572e9d1ee176cd4f5a0008173f1b.png)](https://sonery.medium.com/?source=post_page-----e3d7d1965b2c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e3d7d1965b2c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e3d7d1965b2c--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----e3d7d1965b2c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F8-chatgpt-prompts-for-frequently-done-pandas-operations-e3d7d1965b2c&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----e3d7d1965b2c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e3d7d1965b2c--------------------------------) ·7 分钟阅读·2023年5月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe3d7d1965b2c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F8-chatgpt-prompts-for-frequently-done-pandas-operations-e3d7d1965b2c&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=-----e3d7d1965b2c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe3d7d1965b2c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F8-chatgpt-prompts-for-frequently-done-pandas-operations-e3d7d1965b2c&source=-----e3d7d1965b2c---------------------bookmark_footer-----------)![](../Images/c2e8e682e64722afd27b4fea46bcaa24.png)

由 [Karsten Winegeart](https://unsplash.com/@karsten116?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供的照片，来源于 [Unsplash](https://unsplash.com/photos/YzxeHEzBZ6I?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我们都听说过 ChatGPT。这不仅是科技行业的关注点，也在更广泛的媒体中成为头条新闻。我经常有朋友联系我问我是否听说过它。是的，我当然听说过——我每天都在使用它。

尽管对其在简单任务上的性能和可靠性存在一些批评，ChatGPT和其他大型语言模型（LLMs）在各种任务中表现出色。对我来说，它们是一个显著的生产力提升工具。

最近，我决定利用ChatGPT来处理我在数据清洗和分析中常常进行的Pandas操作。在本文中，我将分享并指导你了解8个提示示例，其中我询问了ChatGPT如何使用Pandas完成任务。

*如果你想了解更多关于Pandas的信息，请访问我的课程* [*500个练习掌握Python Pandas*](https://www.udemy.com/course/500-exercises-to-master-python-pandas/learn/lecture/37842166#overview)*。*

**首个提示以定义其角色：**

*提示：你是一个Python导师，教我Pandas库。我将询问你如何用Pandas完成特定任务，并希望你能为我解释清楚。还请在解释时展示代码。*

在开始之前，我提供了DataFrame的结构，包括列名和数据类型……
