# 为AI代理启用市场：发现和匹配

> 原文：[https://towardsdatascience.com/constraints-composition-for-autogpt-240a3fa00ab4?source=collection_archive---------13-----------------------#2023-05-02](https://towardsdatascience.com/constraints-composition-for-autogpt-240a3fa00ab4?source=collection_archive---------13-----------------------#2023-05-02)

## 发现能够执行给定用户任务的AI代理

[](https://debmalyabiswas.medium.com/?source=post_page-----240a3fa00ab4--------------------------------)[![Debmalya Biswas](../Images/4b985e2a5b362a4b962d3ad29665dfea.png)](https://debmalyabiswas.medium.com/?source=post_page-----240a3fa00ab4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----240a3fa00ab4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----240a3fa00ab4--------------------------------) [Debmalya Biswas](https://debmalyabiswas.medium.com/?source=post_page-----240a3fa00ab4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fad84805121fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconstraints-composition-for-autogpt-240a3fa00ab4&user=Debmalya+Biswas&userId=ad84805121fe&source=post_page-ad84805121fe----240a3fa00ab4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----240a3fa00ab4--------------------------------) · 8 min read · 2023年5月2日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F240a3fa00ab4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconstraints-composition-for-autogpt-240a3fa00ab4&user=Debmalya+Biswas&userId=ad84805121fe&source=-----240a3fa00ab4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F240a3fa00ab4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconstraints-composition-for-autogpt-240a3fa00ab4&source=-----240a3fa00ab4---------------------bookmark_footer-----------)![](../Images/31436955cfbacc8502f3d448e3a5df24.png)

图：由Soma Biswas探索未知 ([Flickr](https://www.flickr.com/photos/somabiswas/52006785817/in/dateposted/)，经许可转载)

# 1. 介绍

关于ChatGPT的讨论现在已经发展到AutoGPT。虽然ChatGPT [1] 主要是一个可以生成文本响应的聊天机器人，但AutoGPT是一个更强大的AI代理，可以执行复杂的任务，例如：完成销售、计划旅行、预订航班、聘请承包商进行家庭工作、订购披萨。

AutoGPT继承了围绕自主代理的长期研究，尤其是目标导向代理 [2, 3]。

> 给定一个用户任务，AutoGPT 旨在识别（组成）能够执行该任务的代理（代理组）。

解决此类复杂任务的高层次方法包括：（a）将给定的复杂任务分解为（一个层级或工作流的）简单任务，接着（b）组成能够执行简单任务的代理[4]。

这可以通过**动态**或**静态**的方式实现。在动态方法中，给定一个复杂的用户任务，系统会根据运行时可用代理的能力提出一个满足请求的计划。在静态方法中，给定一组代理，复合代理是在设计时手动定义的…
