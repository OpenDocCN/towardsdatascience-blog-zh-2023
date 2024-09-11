# 如何在 Pandas 中使用正则表达式模式处理复杂字符串

> 原文：[https://towardsdatascience.com/regex-patterns-in-pandas-api-afe70178f9e9?source=collection_archive---------6-----------------------#2023-02-27](https://towardsdatascience.com/regex-patterns-in-pandas-api-afe70178f9e9?source=collection_archive---------6-----------------------#2023-02-27)

## 正则表达式简化了对大量文本进行模式匹配的任务——Pandas 使其更加优雅

[](https://thuwarakesh.medium.com/?source=post_page-----afe70178f9e9--------------------------------)[![Thuwarakesh Murallie](../Images/44f1a14a899426592bbd8c7f73ce169d.png)](https://thuwarakesh.medium.com/?source=post_page-----afe70178f9e9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----afe70178f9e9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----afe70178f9e9--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----afe70178f9e9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F93ce19993bef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fregex-patterns-in-pandas-api-afe70178f9e9&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=post_page-93ce19993bef----afe70178f9e9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----afe70178f9e9--------------------------------) ·7 min read·2023年2月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fafe70178f9e9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fregex-patterns-in-pandas-api-afe70178f9e9&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=-----afe70178f9e9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fafe70178f9e9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fregex-patterns-in-pandas-api-afe70178f9e9&source=-----afe70178f9e9---------------------bookmark_footer-----------)![](../Images/624b2b35b8a6ef2a7e48b83e5d3c1041.png)

照片由 [Chris Moore](https://unsplash.com/@chrismoore_?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

正则表达式是清理和提取数据的最强大技术。如果你曾经处理过大型文本数据集，你会知道这会是多么耗时和费力。

我经常使用正则表达式来清理电话号码和电子邮件，并规范化地址。但也有一些复杂的用例。

我们注意到最近从某个特定数据源的管道中出现了不一致的办公室列。我们只需要这个列中的办公室代码。它由两个或三个字母后跟一个冒号和一个两位数的数字组成。早些时候，我们使用了简单的替换操作将列映射到我们想要的值。但由于新的数据与我们的假设不一致，我们不得不改变策略。由于我们可以确保模式是一致的，我们使用了正则表达式来清理它们。这样，我们就不必担心更改列值的问题。

但是如果你的数据集非常庞大，并且你需要将提取的值存储在每行旁边的新列中，你可能会倾向于使用 Pandas 中的 map 或 apply 方法。但 Pandas 本身提供了优秀的字符串操作 API，几乎所有的 API 都支持正则表达式。
