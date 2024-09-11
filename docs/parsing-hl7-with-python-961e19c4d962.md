# 使用 Python 解析 HL7

> 原文：[`towardsdatascience.com/parsing-hl7-with-python-961e19c4d962?source=collection_archive---------14-----------------------#2023-03-06`](https://towardsdatascience.com/parsing-hl7-with-python-961e19c4d962?source=collection_archive---------14-----------------------#2023-03-06)

## 使用 python-hl7 提取健康等级 7 数据指南

[](https://medium.com/@terrah27?source=post_page-----961e19c4d962--------------------------------)![Tara (Boyle) Hunter](https://medium.com/@terrah27?source=post_page-----961e19c4d962--------------------------------)[](https://towardsdatascience.com/?source=post_page-----961e19c4d962--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----961e19c4d962--------------------------------) [Tara (Boyle) Hunter](https://medium.com/@terrah27?source=post_page-----961e19c4d962--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2a7b274f3032&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fparsing-hl7-with-python-961e19c4d962&user=Tara+%28Boyle%29+Hunter&userId=2a7b274f3032&source=post_page-2a7b274f3032----961e19c4d962---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----961e19c4d962--------------------------------) · 4 分钟阅读 · 2023 年 3 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F961e19c4d962&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fparsing-hl7-with-python-961e19c4d962&user=Tara+%28Boyle%29+Hunter&userId=2a7b274f3032&source=-----961e19c4d962---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F961e19c4d962&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fparsing-hl7-with-python-961e19c4d962&source=-----961e19c4d962---------------------bookmark_footer-----------)![](img/0d69d501b5083c1b37d8376348218dd6.png)

照片由[Christina Victoria Craft](https://unsplash.com/@victoriabcphotographer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，发布在[Unsplash](https://unsplash.com/s/photos/medical?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上

解析 HL7 消息是许多医疗数据专业人员的重要任务。HL7 在医院和其他医疗机构中广泛用于在不同系统、应用程序和提供者之间交换患者数据。

根据[维基百科](https://en.wikipedia.org/wiki/Health_Level_7)，HL7 或健康级别 7 “指的是一组用于在不同医疗服务提供者使用的软件应用之间交换电子健康信息的国际标准”。这些标准使医疗服务提供者能够以一致且易于解读的方式共享临床信息。

在这里，我们将学习如何使用 Python 解析 HL7 消息。

## 介绍

在我们开始解析之前，需要对 HL7 消息结构有一个基本的了解。虽然 HL7 有不同的版本，但在这里我们将重点关注最常用的版本，即 2.x 系列。

HL7 消息通常是在响应患者事件（如接收、出院或转院）时创建并发送的。

一个 HL7 消息由几个部分组成。每一行称为一个段落。段落由三个字符的标签标识，并由回车符（\r）分隔。段落包含字段，这些字段之间用分隔符分开……
