# LLMs，新的大规模虚假信息武器？

> 原文：[`towardsdatascience.com/llms-weapons-of-mass-disinformation-4def0dc3dc7?source=collection_archive---------9-----------------------#2023-05-29`](https://towardsdatascience.com/llms-weapons-of-mass-disinformation-4def0dc3dc7?source=collection_archive---------9-----------------------#2023-05-29)

## 大型语言模型（LLMs）的双刃剑

## 你认为 2016 年的英国脱欧和美国总统竞选已经够糟糕了吗？再想想吧。

[](https://medium.com/@anthony.mensier?source=post_page-----4def0dc3dc7--------------------------------)![安东尼·门西埃](https://medium.com/@anthony.mensier?source=post_page-----4def0dc3dc7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4def0dc3dc7--------------------------------)![面向数据科学](https://towardsdatascience.com/?source=post_page-----4def0dc3dc7--------------------------------) [安东尼·门西埃](https://medium.com/@anthony.mensier?source=post_page-----4def0dc3dc7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff8ca1fd30f6b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllms-weapons-of-mass-disinformation-4def0dc3dc7&user=Anthony+Mensier&userId=f8ca1fd30f6b&source=post_page-f8ca1fd30f6b----4def0dc3dc7---------------------post_header-----------) 发表在 [面向数据科学](https://towardsdatascience.com/?source=post_page-----4def0dc3dc7--------------------------------) ·17 分钟阅读·2023 年 5 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4def0dc3dc7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllms-weapons-of-mass-disinformation-4def0dc3dc7&user=Anthony+Mensier&userId=f8ca1fd30f6b&source=-----4def0dc3dc7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4def0dc3dc7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllms-weapons-of-mass-disinformation-4def0dc3dc7&source=-----4def0dc3dc7---------------------bookmark_footer-----------)![](img/95df736816ab4bcb32741ce617df12ba.png)

图片由作者使用 Midjourney 5 生成

# 欢迎来到大型语言模型的双刃剑系列！

说实话，ChatGPT 的宏大揭幕是一个震撼全球的现象，揭示了自然语言处理（NLP）进步的全新世界。就像帷幕被拉开，公众、政府和国际机构见证了这项技术在他们面前取得的大胆进展。接下来是一场真正的创新烟花秀。例如，[ThinkGPT](https://github.com/jina-ai/thinkgpt)是一个巧妙的 Python 库，为 LLM 提供了人工思维链和长期记忆，几乎让它们‘思考’（无意中）。或者[AutoGPT](https://github.com/Significant-Gravitas/Auto-GPT)，另一个能够处理复杂请求并生成 AI 代理来执行它们的库。

这些只是建立在 LLM 的 API 之上的数百个应用中的两个例子。我对人们如何巧妙地利用这些新工具，创造性地重新使用这些乐高积木的方式感到印象深刻，企业也相当慷慨地分发了这些工具。
