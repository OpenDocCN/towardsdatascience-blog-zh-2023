# 在Apple Silicon上解决所有Python依赖问题

> 原文：[https://towardsdatascience.com/solving-all-python-dependency-issues-on-apple-silicon-29a2171d1d7c?source=collection_archive---------6-----------------------#2023-02-21](https://towardsdatascience.com/solving-all-python-dependency-issues-on-apple-silicon-29a2171d1d7c?source=collection_archive---------6-----------------------#2023-02-21)

## 使用pyenv在Mac M1上管理多个Python架构的指南

[](https://david-farrugia.medium.com/?source=post_page-----29a2171d1d7c--------------------------------)[![David Farrugia](../Images/082ed61e24c7c26a4ae1c77343a87824.png)](https://david-farrugia.medium.com/?source=post_page-----29a2171d1d7c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----29a2171d1d7c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----29a2171d1d7c--------------------------------) [David Farrugia](https://david-farrugia.medium.com/?source=post_page-----29a2171d1d7c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3916826092a6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolving-all-python-dependency-issues-on-apple-silicon-29a2171d1d7c&user=David+Farrugia&userId=3916826092a6&source=post_page-3916826092a6----29a2171d1d7c---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----29a2171d1d7c--------------------------------) ·4 min read·2023年2月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F29a2171d1d7c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolving-all-python-dependency-issues-on-apple-silicon-29a2171d1d7c&user=David+Farrugia&userId=3916826092a6&source=-----29a2171d1d7c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F29a2171d1d7c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolving-all-python-dependency-issues-on-apple-silicon-29a2171d1d7c&source=-----29a2171d1d7c---------------------bookmark_footer-----------)![](../Images/35b90b1468e13b350e7dfa0cc53604f6.png)

照片由[Jonathan Francisca](https://unsplash.com/@jonathan_francisca?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我热爱MacOS。

也许是它的认证Unix特性，简洁与优雅，或是无缝的集成。但我热爱MacOS。

从我的经验来看，一切运作正常。

在我的职业生涯开始时，我主要使用Windows（并在一旁玩弄Linux发行版）。一旦我转向Apple，尽管这可能听起来很陈词滥调，我从未回头过。

不再需要按照逐步教程来面对奇怪的错误了。

不再需要为让某些Python包在Windows系统上正常工作而寻找模板代码了。

这种情况经常发生。作为数据科学家，我不断尝试和实验新的Python包。

自2019年发布以来，我一直在我的2019款Macbook Pro上处理数据。不过最近，我有机会在2020款Macbook Pro M1上工作了一段时间。

我很兴奋。我听说M1芯片相比于Intel芯片更强大，令人惊叹。
