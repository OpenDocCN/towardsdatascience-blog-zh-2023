# 使用 ggplot2 提高对气候变化的认识

> 原文：[`towardsdatascience.com/raise-awareness-about-climate-change-with-ggplot2-f31f0cae3c70?source=collection_archive---------18-----------------------#2023-04-17`](https://towardsdatascience.com/raise-awareness-about-climate-change-with-ggplot2-f31f0cae3c70?source=collection_archive---------18-----------------------#2023-04-17)

## 学会有效绘制历史天气数据

[](https://medium.com/@bruno.ponne?source=post_page-----f31f0cae3c70--------------------------------)![Bruno Ponne](https://medium.com/@bruno.ponne?source=post_page-----f31f0cae3c70--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f31f0cae3c70--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f31f0cae3c70--------------------------------) [Bruno Ponne](https://medium.com/@bruno.ponne?source=post_page-----f31f0cae3c70--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2819bc6617ce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fraise-awareness-about-climate-change-with-ggplot2-f31f0cae3c70&user=Bruno+Ponne&userId=2819bc6617ce&source=post_page-2819bc6617ce----f31f0cae3c70---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f31f0cae3c70--------------------------------) · 8 分钟阅读 · 2023 年 4 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff31f0cae3c70&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fraise-awareness-about-climate-change-with-ggplot2-f31f0cae3c70&user=Bruno+Ponne&userId=2819bc6617ce&source=-----f31f0cae3c70---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff31f0cae3c70&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fraise-awareness-about-climate-change-with-ggplot2-f31f0cae3c70&source=-----f31f0cae3c70---------------------bookmark_footer-----------)![](img/d86afa77c46a5339549a91a1b29e3063.png)

图片由 [Ganapathy Kumar](https://unsplash.com/@gkumar2175?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 全球变暖不是一种预测，它正在发生。
> 
> *詹姆斯·汉森*

有确凿的证据表明地球上的气温正在上升。随着气候变化威胁到人类的生存，了解、研究和提高对这一关键问题的认识比以往任何时候都更加重要。

无论你是学生、政府工作人员、非政府组织成员还是私人公司员工，向你的同事展示你对相关全球问题的关注是至关重要的。

在本教程中，你将学习在哪里找到可靠的、整理过的历史温度数据，并使用 `ggplot2` 进行可视化。完成这篇文章后，你将：

+   知道在哪里找到经过整理的历史天气数据集；

+   感到舒适地使用 `ggplot2` 绘制历史天气数据；

+   能够自定义你的 `ggplot2` 图表以讲述你的故事。

## 第一步：找到并加载数据

本教程的数据可在 [**国家环境信息中心 (NCEI)**](https://www.ncei.noaa.gov/access/search/data-search/global-summary-of-the-year)*** 上获取。** NCEI 是美国环境数据的权威机构，并提供……
