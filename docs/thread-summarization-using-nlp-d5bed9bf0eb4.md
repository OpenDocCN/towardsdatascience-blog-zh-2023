# 使用 NLP 进行线程摘要

> 原文：[https://towardsdatascience.com/thread-summarization-using-nlp-d5bed9bf0eb4?source=collection_archive---------4-----------------------#2023-01-07](https://towardsdatascience.com/thread-summarization-using-nlp-d5bed9bf0eb4?source=collection_archive---------4-----------------------#2023-01-07)

## 使用 POS 标记、NER 和情感分析进行提取式摘要

[](https://jagota-arun.medium.com/?source=post_page-----d5bed9bf0eb4--------------------------------)[![Arun Jagota](../Images/3c3eb142f671b5fb933c2826d8ed78d9.png)](https://jagota-arun.medium.com/?source=post_page-----d5bed9bf0eb4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d5bed9bf0eb4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d5bed9bf0eb4--------------------------------) [Arun Jagota](https://jagota-arun.medium.com/?source=post_page-----d5bed9bf0eb4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fef9ed921edad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthread-summarization-using-nlp-d5bed9bf0eb4&user=Arun+Jagota&userId=ef9ed921edad&source=post_page-ef9ed921edad----d5bed9bf0eb4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d5bed9bf0eb4--------------------------------) ·13 min read·2023年1月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd5bed9bf0eb4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthread-summarization-using-nlp-d5bed9bf0eb4&user=Arun+Jagota&userId=ef9ed921edad&source=-----d5bed9bf0eb4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd5bed9bf0eb4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthread-summarization-using-nlp-d5bed9bf0eb4&source=-----d5bed9bf0eb4---------------------bookmark_footer-----------)![](../Images/2df0e0ffea8820634c8b6f6285b32460.png)

图片由 [Holger](https://pixabay.com/users/hodihu-84128/) 提供，来源于 [Pixabay](https://pixabay.com/)

设想一个涉及多人文本（或电子邮件）消息的对话。以下是一个包含两个人一起计划旅行的示例。各条消息由空行分隔。

```py
Here is some lodging near Pismo beach: Cliffs Hotel, SeaCrest, 
Madonna Inn, Shell beach inn

Shell beach & Madonna - a bit pricey.

Check out Inn at the Pier.

Looks good.

What about food?

Coya Peruvian

Any seafood restaurants?

Ada's Fish House. Oyster Loft.

+1 on Oyster loft.

What about bars?

11 22, Boardroom

breweries?

Shell beach brewhouse

Check this lodge out: Wayfarer

Not close enough to where we are going.
```

来回的对话可以呈现大量信息，尽管这些信息分散在不同的文本消息中。当这个对话在线上聊天应用程序中展开时，为了跟上对话，用户需要不断地向上滚动查看新的消息，这可能会很有挑战性。

如果一个 NLP 机器人可以阅读对话中的所有消息，并将其总结为一条新消息，那该多好！然后可以将这条新消息作为一条新消息发布在同一个对话中。

这篇文章解决了这个问题。
