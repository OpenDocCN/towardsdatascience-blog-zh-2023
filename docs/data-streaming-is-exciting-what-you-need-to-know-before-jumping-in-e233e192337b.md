# 数据流处理令人兴奋：在跳入之前你需要知道什么

> 原文：[`towardsdatascience.com/data-streaming-is-exciting-what-you-need-to-know-before-jumping-in-e233e192337b?source=collection_archive---------13-----------------------#2023-02-27`](https://towardsdatascience.com/data-streaming-is-exciting-what-you-need-to-know-before-jumping-in-e233e192337b?source=collection_archive---------13-----------------------#2023-02-27)

## 数据流处理适合你的业务吗？需要考虑的关键事实

[](https://chengzhizhao.medium.com/?source=post_page-----e233e192337b--------------------------------)![Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----e233e192337b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e233e192337b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e233e192337b--------------------------------) [Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----e233e192337b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff956c63a9571&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-streaming-is-exciting-what-you-need-to-know-before-jumping-in-e233e192337b&user=Chengzhi+Zhao&userId=f956c63a9571&source=post_page-f956c63a9571----e233e192337b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e233e192337b--------------------------------) · 5 分钟阅读 · 2023 年 2 月 27 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe233e192337b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-streaming-is-exciting-what-you-need-to-know-before-jumping-in-e233e192337b&user=Chengzhi+Zhao&userId=f956c63a9571&source=-----e233e192337b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe233e192337b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-streaming-is-exciting-what-you-need-to-know-before-jumping-in-e233e192337b&source=-----e233e192337b---------------------bookmark_footer-----------)![](img/1d33cd261fc165c0bb572ab116c342c5.png)

图片由 [Stephen Leonardi](https://unsplash.com/@stephenleo1982?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/DpvBWb4JW1M?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

流数据是数据领域一个令人兴奋的领域，近年来获得了巨大的关注。由于兴奋，开源项目的领域变得拥挤。许多技术使流数据处理比以往任何时候都更简单：Kafka、Flink、Spark、Storm、Beam 等技术已经存在多年，并建立了坚实的用户基础。

> “让我们进行流处理。”

对于数据专业人士来说，这是一个不可避免的话题。然而，在有人告诉你关于流数据的事之前，我们应该退一步，用一个简单的问题来再次确认自己：**我是否需要为这个用例使用流数据？** 在深入了解之前，让我们面对流数据的现实。

# 流数据

在我们查看流数据的事实之前，首先了解什么是流数据。Hadoop 为处理大数据集奠定了基础，并赋予数据专业人士设计更复杂的数据处理工具的能力。

[Tyler Akidau](https://www.oreilly.com/people/tyler-akidau/) 在 2013 年发表的关于[MillWheel：互联网规模的容错流处理](https://research.google/pubs/pub41378/)的论文奠定了…
