# BigQuery 的奇妙特性及其使用时机

> 原文：[https://towardsdatascience.com/fantastic-beasts-of-bigquery-and-when-to-use-them-13af9a17f3db?source=collection_archive---------4-----------------------#2023-12-31](https://towardsdatascience.com/fantastic-beasts-of-bigquery-and-when-to-use-them-13af9a17f3db?source=collection_archive---------4-----------------------#2023-12-31)

## 揭示 BigQuery Studio、DataFrames、生成性 AI/AI 功能和 DuetAI 的特征

[](https://medium.com/@martosi?source=post_page-----13af9a17f3db--------------------------------)[![Marina Tosic](../Images/cb2168826ed9ed608d61c6c90843c535.png)](https://medium.com/@martosi?source=post_page-----13af9a17f3db--------------------------------)[](https://towardsdatascience.com/?source=post_page-----13af9a17f3db--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----13af9a17f3db--------------------------------) [Marina Tosic](https://medium.com/@martosi?source=post_page-----13af9a17f3db--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe40b4f03cd3e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffantastic-beasts-of-bigquery-and-when-to-use-them-13af9a17f3db&user=Marina+Tosic&userId=e40b4f03cd3e&source=post_page-e40b4f03cd3e----13af9a17f3db---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----13af9a17f3db--------------------------------) · 8 分钟阅读 · 2023年12月31日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F13af9a17f3db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffantastic-beasts-of-bigquery-and-when-to-use-them-13af9a17f3db&user=Marina+Tosic&userId=e40b4f03cd3e&source=-----13af9a17f3db---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F13af9a17f3db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffantastic-beasts-of-bigquery-and-when-to-use-them-13af9a17f3db&source=-----13af9a17f3db---------------------bookmark_footer-----------)![](../Images/21d378b95769bcc96f6dcb557570b5a2.png)

“BigQuery 是一个集成了 DB-BI-ML-GenAI 功能的 Google 全能服务。” [图片由 [Korng Sok](https://unsplash.com/@korng_sok?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)]

# 发现更多 BigQuery 世界的内容

我最喜欢的书之一是 “[神奇动物在哪里](https://en.wikipedia.org/wiki/Fantastic_Beasts_and_Where_to_Find_Them)” 作者是 [J.K. 罗琳](https://en.wikipedia.org/wiki/J._K._Rowling)。这是一个关于魔法生物在非魔法世界中失控的故事。这个故事也展示了*Maj*（巫师）和*No-Maj*（非巫师）人们如何形成友谊，以保护魔法生物。在这个任务中，主要的*No-Maj*角色发现了一个魔法的世界，并爱上了它所带来的所有挑战，渴望自己也是一名巫师。

作为一名*No-Maj*， [我从机械工程转到数据领域的过程最初充满了挑战](https://medium.com/towards-data-science/ending-the-year-with-12-lessons-about-data-career-8786afc068f4)。每当我进入一个新的数据领域时，我都会想：*“要是我能成为巫师就好了”*。 ;)

当我第一次开始学习 ***数据库 (DB) 和商业智能 (BI)*** 时，我脑海里有这种想法。

当我深入到 ***机器学习 (ML)*** 话题时，这种想法又一次出现。

现在，我正在尝试在 ***生成式 AI (GenAI) 开发*** 中摸索——没错，这个想法再次伴随着我。
