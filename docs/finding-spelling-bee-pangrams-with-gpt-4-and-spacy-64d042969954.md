# 使用 GPT-4 和 SpaCy 查找拼写蜂蜜句

> 原文：[https://towardsdatascience.com/finding-spelling-bee-pangrams-with-gpt-4-and-spacy-64d042969954?source=collection_archive---------12-----------------------#2023-03-21](https://towardsdatascience.com/finding-spelling-bee-pangrams-with-gpt-4-and-spacy-64d042969954?source=collection_archive---------12-----------------------#2023-03-21)

## 解决《纽约时报》难题的探索

[](https://reddotblues.medium.com/?source=post_page-----64d042969954--------------------------------)[![肖恩·翟](../Images/c10a04893c3baac3252dea0b9f140271.png)](https://reddotblues.medium.com/?source=post_page-----64d042969954--------------------------------)[](https://towardsdatascience.com/?source=post_page-----64d042969954--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----64d042969954--------------------------------) [肖恩·翟](https://reddotblues.medium.com/?source=post_page-----64d042969954--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F656b0a1ac071&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-spelling-bee-pangrams-with-gpt-4-and-spacy-64d042969954&user=Sean+Zhai&userId=656b0a1ac071&source=post_page-656b0a1ac071----64d042969954---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----64d042969954--------------------------------) · 7 min read · Mar 21, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F64d042969954&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-spelling-bee-pangrams-with-gpt-4-and-spacy-64d042969954&user=Sean+Zhai&userId=656b0a1ac071&source=-----64d042969954---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F64d042969954&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-spelling-bee-pangrams-with-gpt-4-and-spacy-64d042969954&source=-----64d042969954---------------------bookmark_footer-----------)![](../Images/d9f49cf95d3a6aee9a44964094d5d557.png)

图片由 [Nemichandra Hombannavar](https://unsplash.com/@nemichandra?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

解答《纽约时报》拼字游戏可以是一种令人满足的体验，它平衡了挑战和单词探索的乐趣。虽然这并不总是轻而易举，但从找到每个单词中获得的满足感是非常值得的。在拼字游戏的各种语言成就中，发现**全字母句**就像发现了一个隐藏的宝藏。这个特别的词使用了所有给定的字母，突显了玩家在驾驭英语词汇丰富复杂性的技能。

寻找全字母句对于许多人来说是一项令人兴奋的活动，它也为自然语言处理（NLP）练习提供了一个引人注目的案例。SpaCy（Honnibal & Montani，2017）是我最喜欢的工具。它是基于MIT许可证的开源软件。你可以手动为SpaCy编写程序，但我想向你展示如何使用GPT-4开发这样的解决方案。

# 背景

## 拼字游戏

《纽约时报》拼字游戏是《纽约时报》报纸和网站上的一种受欢迎的单词谜题游戏。在游戏中，玩家会得到一组七个字母，其中一个字母被指定为“中心”字母。游戏的目标是…
