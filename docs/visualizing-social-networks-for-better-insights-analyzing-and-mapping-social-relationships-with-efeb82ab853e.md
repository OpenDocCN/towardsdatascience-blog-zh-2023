# 可视化社交网络以获得更深刻的见解：使用Python的NetworkX库分析和绘制社交关系

> 原文：[https://towardsdatascience.com/visualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-efeb82ab853e?source=collection_archive---------5-----------------------#2023-05-20](https://towardsdatascience.com/visualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-efeb82ab853e?source=collection_archive---------5-----------------------#2023-05-20)

## 初学者指南：使用Python的NetworkX库进行社交网络分析

[](https://christineegan42.medium.com/?source=post_page-----efeb82ab853e--------------------------------)[![Christine Egan](../Images/d0a11bde52ceaa53d7162f2dd77c8041.png)](https://christineegan42.medium.com/?source=post_page-----efeb82ab853e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----efeb82ab853e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----efeb82ab853e--------------------------------) [Christine Egan](https://christineegan42.medium.com/?source=post_page-----efeb82ab853e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e9b4d1cb38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-efeb82ab853e&user=Christine+Egan&userId=8e9b4d1cb38&source=post_page-8e9b4d1cb38----efeb82ab853e---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----efeb82ab853e--------------------------------) ·6 min read·2023年5月20日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fefeb82ab853e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-efeb82ab853e&source=-----efeb82ab853e---------------------bookmark_footer-----------)

在侦探剧中，[使用包含相关证据的软木板](https://tvtropes.org/pmwiki/pmwiki.php/Main/StringTheory)，通过红色线条连接这些证据，通常被描绘为调查员可视化和跟踪调查中个人与事件之间联系的一种方法。红线用于在板子或墙上物理连接不同的节点（通常用照片或笔记表示），显示它们之间的关系。

![](../Images/dec485cff44fa5de03d7ae2f2233fda4.png)

[Volodymyr Hryshchenko](https://unsplash.com/@lunarts?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/investigation?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上的照片

这种普遍存在的情节被一些人称为[“线理论”](https://tvtropes.org/pmwiki/pmwiki.php/Main/StringTheory)（不是那个[弦理论](https://en.wikipedia.org/wiki/String_theory)），也有一些人将这种突现的物体称为[“疯狂墙”](https://www.esquire.com/uk/culture/film/news/a7703/detective-show-crazy-walls/)或[“线板”](https://slate.com/news-and-politics/2022/02/fbi-crazy-stringboard-recruiting-campaign.html)。在网络理论领域，这种分析人与组织或文档之间链接和关系的技术被称为[链接分析](https://doc.arcgis.com/en/insights/latest/analyze/link-analysis.htm)，并且这种方法已经在执法、欺诈检测、社会学研究和其他各种背景中使用了一个多世纪。

## 链接分析与社会网络分析
