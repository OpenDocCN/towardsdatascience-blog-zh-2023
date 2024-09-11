# 使用 AWS 和 Power BI 检视美国航班

> 原文：[https://towardsdatascience.com/examining-flights-in-the-u-s-with-aws-and-power-bi-297a29fb2c13?source=collection_archive---------7-----------------------#2023-07-06](https://towardsdatascience.com/examining-flights-in-the-u-s-with-aws-and-power-bi-297a29fb2c13?source=collection_archive---------7-----------------------#2023-07-06)

## 我们可以通过 ETL 和 BI 获得什么洞察？

[](https://medium.com/@aashishnair?source=post_page-----297a29fb2c13--------------------------------)[![Aashish Nair](../Images/23f4b3839e464419332b690a4098d824.png)](https://medium.com/@aashishnair?source=post_page-----297a29fb2c13--------------------------------)[](https://towardsdatascience.com/?source=post_page-----297a29fb2c13--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----297a29fb2c13--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----297a29fb2c13--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3087ba81e065&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexamining-flights-in-the-u-s-with-aws-and-power-bi-297a29fb2c13&user=Aashish+Nair&userId=3087ba81e065&source=post_page-3087ba81e065----297a29fb2c13---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----297a29fb2c13--------------------------------) ·9 min read·2023年7月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F297a29fb2c13&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexamining-flights-in-the-u-s-with-aws-and-power-bi-297a29fb2c13&user=Aashish+Nair&userId=3087ba81e065&source=-----297a29fb2c13---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F297a29fb2c13&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexamining-flights-in-the-u-s-with-aws-and-power-bi-297a29fb2c13&source=-----297a29fb2c13---------------------bookmark_footer-----------)![](../Images/7bf31178eb840ce3f7f2851143eee040.png)

[John McArthur](https://unsplash.com/@snowjam?utm_source=medium&utm_medium=referral) 通过 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供的照片

## 目录

∘ [引言](#1403)

∘ [问题陈述](#f26a)

∘ [数据](#2077)

∘ [AWS 架构](#cac6)

∘ [使用 AWS S3 存储数据](#b9c0)

∘ [设计模式](#b132)

∘ [使用 AWS Glue 进行 ETL](#0a30)

∘ [使用 AWS Redshift 进行数据仓储](#85b9)

∘ [使用 AWS Redshift 提取洞察](#935e)

∘ [使用 Power BI 可视化数据](#c25c)

∘ [未来步骤](#cbd7)

∘ [结论](#1a36)

∘ [参考文献](#eb18)

## 引言

航空旅行已成为我们生活中不可或缺的一部分。它是企业建立联系和进行商务的手段，也是家庭探访亲人或旅行的方式。

尽管其影响深远，但航空业常常面临 turbulence。由于经济萧条和繁荣、气候变化、Covid-19 大流行以及推动更多依赖可再生能源等外部因素的影响，航空业不断变化。

为了了解这些变化及其对航空旅行的影响，值得跟踪这些航班……
