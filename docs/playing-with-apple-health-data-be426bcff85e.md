# 玩转苹果健康数据

> 原文：[`towardsdatascience.com/playing-with-apple-health-data-be426bcff85e?source=collection_archive---------9-----------------------#2023-02-27`](https://towardsdatascience.com/playing-with-apple-health-data-be426bcff85e?source=collection_archive---------9-----------------------#2023-02-27)

## 数据科学项目

## 血糖数据的分类与时间序列预测

[](https://medium.com/@ednalyn.dedios?source=post_page-----be426bcff85e--------------------------------)![Ednalyn C. De Dios](https://medium.com/@ednalyn.dedios?source=post_page-----be426bcff85e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----be426bcff85e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----be426bcff85e--------------------------------) [Ednalyn C. De Dios](https://medium.com/@ednalyn.dedios?source=post_page-----be426bcff85e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F92e30e2c69cb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplaying-with-apple-health-data-be426bcff85e&user=Ednalyn+C.+De+Dios&userId=92e30e2c69cb&source=post_page-92e30e2c69cb----be426bcff85e---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----be426bcff85e--------------------------------) · 12 分钟阅读·2023 年 2 月 27 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbe426bcff85e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplaying-with-apple-health-data-be426bcff85e&user=Ednalyn+C.+De+Dios&userId=92e30e2c69cb&source=-----be426bcff85e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbe426bcff85e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplaying-with-apple-health-data-be426bcff85e&source=-----be426bcff85e---------------------bookmark_footer-----------)![](img/a211c1f0d5bf63634e544c80fad268be.png)

图片由[Laesa D](https://unsplash.com/@proudestpuff21?utm_source=medium&utm_medium=referral)提供，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

今天，我们将进入苹果的世界。

我是一个糖尿病患者，佩戴着一个连续血糖监测器，简称 CGM。它每隔几分钟记录一次我的血糖，并将结果传送到我的手机上。方便的是，我可以与我的医生共享这些数据，我们可以一起讨论具体的趋势和模式，并做出真正基于数据的决策。

自然，我对应用程序收集的数据感兴趣。由于 Dexcom G6（我的 CGM 品牌）没有 csv 导出功能，我通过将数据同步到 Apple Health 来绕过这个限制。问题是，Apple Health 不允许用户筛选导出的数据。因此，生成的导出文件是一个庞大的单体。如果你已经用了手机一年以上，导出文件大到几乎无法打开。几乎！

我接受了挑战：找到一种读取文件并进行操作的方法。

Python，来救援！

# 计划

我们将像进行一个经典的数据科学项目那样来应对这个挑战。首先，我们将使用 PAPEM-DM 框架来生成有洞察力的图形和干净的 csv…
