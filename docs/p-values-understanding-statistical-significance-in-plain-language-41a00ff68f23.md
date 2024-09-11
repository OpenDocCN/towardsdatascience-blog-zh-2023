# P 值：用通俗语言理解统计显著性

> 原文：[`towardsdatascience.com/p-values-understanding-statistical-significance-in-plain-language-41a00ff68f23?source=collection_archive---------3-----------------------#2023-08-21`](https://towardsdatascience.com/p-values-understanding-statistical-significance-in-plain-language-41a00ff68f23?source=collection_archive---------3-----------------------#2023-08-21)

## 选择通向显著结果的路径

[](https://medium.com/@MahamsMultiverse?source=post_page-----41a00ff68f23--------------------------------)![Maham Haroon](https://medium.com/@MahamsMultiverse?source=post_page-----41a00ff68f23--------------------------------)[](https://towardsdatascience.com/?source=post_page-----41a00ff68f23--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----41a00ff68f23--------------------------------) [Maham Haroon](https://medium.com/@MahamsMultiverse?source=post_page-----41a00ff68f23--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F398c9514a58b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fp-values-understanding-statistical-significance-in-plain-language-41a00ff68f23&user=Maham+Haroon&userId=398c9514a58b&source=post_page-398c9514a58b----41a00ff68f23---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----41a00ff68f23--------------------------------) ·8 分钟阅读·2023 年 8 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F41a00ff68f23&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fp-values-understanding-statistical-significance-in-plain-language-41a00ff68f23&user=Maham+Haroon&userId=398c9514a58b&source=-----41a00ff68f23---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F41a00ff68f23&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fp-values-understanding-statistical-significance-in-plain-language-41a00ff68f23&source=-----41a00ff68f23---------------------bookmark_footer-----------)![](img/7e4971f4271b52b0b806314cb1c098d5.png)

摄影：Jens Lelie，来源于 Unsplash ([`unsplash.com/@madebyjens`](https://unsplash.com/@madebyjens))

你好！

今天，我们将对统计学进行有趣的探索，讨论一个既熟悉又常被误解的概念——难以捉摸但始终存在的 p 值。如果你曾对它感到困惑，不用担心；我会以一种有趣且清晰的方式来解析它。

# p 值的意义

在深入探讨之前，让我们从一个相关的场景开始：

想象一下，作为一名刚毕业的数据科学家，你正在寻找第一份工作，你已经尽职尽责，投入了无数小时攻克像 leet code 这样的编码挑战，并掌握了复杂的机器学习算法概念，你为第一次面试做好了充分准备且充满信心。面试官热情友好，氛围轻松愉快，问题也在你的知识范围之内，然后他们问你：“p 值究竟是什么？”

尽管你以前遇到过这个术语，你当时的回答可能是：“它表示我们假设的显著性。”然而，当面试官进一步深入提问时，你意识到你可能正在深入探索比这更深奥的领域……
