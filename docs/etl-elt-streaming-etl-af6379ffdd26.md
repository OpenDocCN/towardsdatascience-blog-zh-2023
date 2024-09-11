# ETL 与 ELT 与 Streaming ETL

> 原文：[`towardsdatascience.com/etl-elt-streaming-etl-af6379ffdd26?source=collection_archive---------3-----------------------#2023-08-29`](https://towardsdatascience.com/etl-elt-streaming-etl-af6379ffdd26?source=collection_archive---------3-----------------------#2023-08-29)

## 探索数据处理的批处理和实时设计范式

[](https://gmyrianthous.medium.com/?source=post_page-----af6379ffdd26--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----af6379ffdd26--------------------------------)[](https://towardsdatascience.com/?source=post_page-----af6379ffdd26--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----af6379ffdd26--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----af6379ffdd26--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fetl-elt-streaming-etl-af6379ffdd26&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----af6379ffdd26---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----af6379ffdd26--------------------------------) · 8 分钟阅读 · 2023 年 8 月 29 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Faf6379ffdd26&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fetl-elt-streaming-etl-af6379ffdd26&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----af6379ffdd26---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faf6379ffdd26&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fetl-elt-streaming-etl-af6379ffdd26&source=-----af6379ffdd26---------------------bookmark_footer-----------)![](img/ae87ff8dd8a63419b49aed24af4ba9ae.png)

图片来源：[Compare Fibre](https://unsplash.com/@comparefibre?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/INNsF0Zz_kQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

提取、转换、加载（ETL）和提取、加载、转换（ELT）是数据处理中的两个基本概念，用于描述数据摄取和转换的设计范式。虽然这些术语常常互换使用，但它们指的是略有不同的概念，并适用于不同的使用场景，这些场景也会施加不同的设计要求。

在本文中，我们将深入探讨 ETL 和 ELT 的异同，并讨论云计算和数据工程的环境如何影响数据处理设计模式。此外，我们将概述这两者在现代数据团队中提供的主要优缺点。最后，我们将讨论 Streaming ETL，这是一种新兴的数据处理模式，旨在解决传统批处理方法的各种缺点。

[**订阅数据管道**](https://thedatapipeline.substack.com/welcome)**，这是一个专注于数据工程的新闻通讯**

# 三个关键步骤

从外部源将数据摄取并持久化到目标系统涉及三个不同的步骤。

**提取** “提取”步骤涉及从源系统提取数据所需的所有过程。这些源...
