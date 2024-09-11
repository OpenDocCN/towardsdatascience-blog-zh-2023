# 使用 Mage 在数据管道中实现行为驱动开发

> 原文：[https://towardsdatascience.com/implement-behaviour-driven-development-in-data-pipelines-using-mage-19496fea7890?source=collection_archive---------14-----------------------#2023-07-06](https://towardsdatascience.com/implement-behaviour-driven-development-in-data-pipelines-using-mage-19496fea7890?source=collection_archive---------14-----------------------#2023-07-06)

## 最大化数据管道的质量和生产力

[](https://medium.com/@xiaoxugao?source=post_page-----19496fea7890--------------------------------)[![Xiaoxu Gao](../Images/8712a7e5f3bad0d2abd7e04792fad66f.png)](https://medium.com/@xiaoxugao?source=post_page-----19496fea7890--------------------------------)[](https://towardsdatascience.com/?source=post_page-----19496fea7890--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----19496fea7890--------------------------------) [Xiaoxu Gao](https://medium.com/@xiaoxugao?source=post_page-----19496fea7890--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2adc5a07e772&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplement-behaviour-driven-development-in-data-pipelines-using-mage-19496fea7890&user=Xiaoxu+Gao&userId=2adc5a07e772&source=post_page-2adc5a07e772----19496fea7890---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----19496fea7890--------------------------------) ·7 分钟阅读·2023年7月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F19496fea7890&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplement-behaviour-driven-development-in-data-pipelines-using-mage-19496fea7890&user=Xiaoxu+Gao&userId=2adc5a07e772&source=-----19496fea7890---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F19496fea7890&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplement-behaviour-driven-development-in-data-pipelines-using-mage-19496fea7890&source=-----19496fea7890---------------------bookmark_footer-----------)![](../Images/491eca353ee8a49b28a714e7d2305c7f.png)

图片由 [Nick Fewings](https://unsplash.com/@jannerboy62) 提供，来源于 [Unsplash](https://unsplash.com/)

在我的[上一篇文章](https://medium.com/towards-data-science/how-to-create-valuable-data-tests-850e778718e1)中，我详细讨论了数据管道测试的重要性，以及如何分别创建数据测试和单元测试。尽管测试在开发周期中扮演着重要角色，但它可能并不是最令人兴奋的部分。因此，许多现代数据栈引入了框架或插件，以加快数据测试的实施。此外，Python 中的单元测试框架，如 Pytest 和 unittest，已经存在很长时间，帮助工程师高效地为数据管道和任何 Python 应用程序创建单元测试。

在这篇文章中，我想介绍一个结合了两种现代技术的设置：以业务为导向的测试框架**行为驱动开发（BDD）**和现代数据管道工具[Mage](https://github.com/mage-ai/mage-ai)。通过将这两种技术结合在一起，目标是为数据管道创建高质量的单元测试，同时提供无缝的开发者体验。

## 什么是**行为驱动开发（BDD）**？

在为业务构建数据管道时，我们很可能会遇到复杂而棘手的业务逻辑。例如，定义客户细分的过程…
