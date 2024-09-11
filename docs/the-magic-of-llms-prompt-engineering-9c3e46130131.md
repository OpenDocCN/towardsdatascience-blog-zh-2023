# LLMs的魔力——提示工程

> 原文：[https://towardsdatascience.com/the-magic-of-llms-prompt-engineering-9c3e46130131?source=collection_archive---------1-----------------------#2023-04-08](https://towardsdatascience.com/the-magic-of-llms-prompt-engineering-9c3e46130131?source=collection_archive---------1-----------------------#2023-04-08)

## 垃圾进，垃圾出，这句话从未如此真实。

[](https://franklyai.medium.com/?source=post_page-----9c3e46130131--------------------------------)[![Frank Neugebauer](../Images/0da70d082d0f9c7ad8ccf574ed215df2.png)](https://franklyai.medium.com/?source=post_page-----9c3e46130131--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9c3e46130131--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9c3e46130131--------------------------------) [Frank Neugebauer](https://franklyai.medium.com/?source=post_page-----9c3e46130131--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F29e3b5503cd1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-magic-of-llms-prompt-engineering-9c3e46130131&user=Frank+Neugebauer&userId=29e3b5503cd1&source=post_page-29e3b5503cd1----9c3e46130131---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9c3e46130131--------------------------------) ·6分钟阅读·2023年4月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9c3e46130131&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-magic-of-llms-prompt-engineering-9c3e46130131&user=Frank+Neugebauer&userId=29e3b5503cd1&source=-----9c3e46130131---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9c3e46130131&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-magic-of-llms-prompt-engineering-9c3e46130131&source=-----9c3e46130131---------------------bookmark_footer-----------)![](../Images/8c28b5ab704518e11aef83823349fb09.png)

[Gary Chan](https://unsplash.com/es/@gary_at_unsplash?utm_source=medium&utm_medium=referral) 摄影作品，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 机器的崛起

大型语言模型（LLMs）能够以会话方式提供前所未有的机器学习生成信息的水平，当*被提问得当*时。例如，LLMs可以生成文章（不，这段文本不是由LLM生成的）、诗歌、歌词，与他人就某个主题进行对话、解读某些类型的文件/表格等等。然而，关于LLMs的一个很少被提及的问题是，知道**正确的问题**并不总是容易，而没有这一关键部分，LLMs可能过于准确（即信息过多）、不准确，或者对于许多商业应用来说变异性太大。

提问正确问题的艺术，或称为**提示工程**，是我们克服这一限制（至少在短期内）的方法，它是LLMs的*魔法背后的魔法*。

# 转变问题

在不提供LLMs概览（由LLM撰写）的情况下，我想直接跳入LLMs的一个重要特性——它们是非确定性的; 你并不总能每次对同一个问题得到相同的答案。例如，如果我连续多次问谷歌Bard一个简单的问题“1 + 1等于多少”，结果是未编辑的如下：
