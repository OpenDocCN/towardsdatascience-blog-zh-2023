# 全球巧克力贸易概述

> 原文：[https://towardsdatascience.com/overviewing-the-global-chocolate-trade-6478adeb8ead?source=collection_archive---------5-----------------------#2023-11-24](https://towardsdatascience.com/overviewing-the-global-chocolate-trade-6478adeb8ead?source=collection_archive---------5-----------------------#2023-11-24)

![](../Images/93d4cc6863b78621420ccfcf677e7a6a.png)

全球巧克力贸易网络。

## 利用网络分析探索联合国商品贸易统计数据

[](https://medium.com/@janosovm?source=post_page-----6478adeb8ead--------------------------------)[![米兰·贾诺索夫](../Images/77b62460041f66ec4585a81baef81a03.png)](https://medium.com/@janosovm?source=post_page-----6478adeb8ead--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6478adeb8ead--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6478adeb8ead--------------------------------) [米兰·贾诺索夫](https://medium.com/@janosovm?source=post_page-----6478adeb8ead--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F838408aa2ad4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foverviewing-the-global-chocolate-trade-6478adeb8ead&user=Milan+Janosov&userId=838408aa2ad4&source=post_page-838408aa2ad4----6478adeb8ead---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6478adeb8ead--------------------------------) ·9分钟阅读·2023年11月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6478adeb8ead&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foverviewing-the-global-chocolate-trade-6478adeb8ead&user=Milan+Janosov&userId=838408aa2ad4&source=-----6478adeb8ead---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6478adeb8ead&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foverviewing-the-global-chocolate-trade-6478adeb8ead&source=-----6478adeb8ead---------------------bookmark_footer-----------)

在这篇文章中，我通过关注“含可可的巧克力及其他食品制品”贸易类别，探讨了联合国Comtrade国际贸易数据库。虽然这个特定的焦点为我的文章提供了一个明确的方向，针对一个字面上的小众市场，但分析步骤和方法层面是通用的。因此，基于这些步骤，可以快速分析任何类型的国际贸易关系，从能源到武器。当将贸易的时空维度纳入分析，并通过例如及时的政治事件、国际冲突等信息进行增强时，可以轻松将这些事件与其宏观经济影响联系起来。虽然国际贸易数据分析的影响深远，但让我们现在利用网络和探索性数据科学来深入了解主要出口国及其在巧克力贸易中的关系。

*在这篇文章中，所有图片——如果说明中未另行说明——均由作者创作。*

# 1\. 数据收集

一旦我登录到[Comtrade](https://comtradeplus.un.org/TradeFlow)网站，我进入了其TradeFlow界面——这是一个很棒的在线平台，在这里我可以轻松构建查询以获取任何国际贸易数据…
