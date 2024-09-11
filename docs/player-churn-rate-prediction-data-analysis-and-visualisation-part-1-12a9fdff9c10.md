# 如何预测玩家流失，借助 ChatGPT 的帮助

> 原文：[`towardsdatascience.com/player-churn-rate-prediction-data-analysis-and-visualisation-part-1-12a9fdff9c10?source=collection_archive---------4-----------------------#2023-06-20`](https://towardsdatascience.com/player-churn-rate-prediction-data-analysis-and-visualisation-part-1-12a9fdff9c10?source=collection_archive---------4-----------------------#2023-06-20)

## 使用低代码机器学习平台的数据科学 | Actable AI

## 使用低代码平台进行玩家流失预测的数据分析和模型训练

[](https://medium.com/@chrisgalea?source=post_page-----12a9fdff9c10--------------------------------)![Christian Galea](https://medium.com/@chrisgalea?source=post_page-----12a9fdff9c10--------------------------------)[](https://towardsdatascience.com/?source=post_page-----12a9fdff9c10--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----12a9fdff9c10--------------------------------) [Christian Galea](https://medium.com/@chrisgalea?source=post_page-----12a9fdff9c10--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa9be78db0c9b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplayer-churn-rate-prediction-data-analysis-and-visualisation-part-1-12a9fdff9c10&user=Christian+Galea&userId=a9be78db0c9b&source=post_page-a9be78db0c9b----12a9fdff9c10---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----12a9fdff9c10--------------------------------) ·22 分钟阅读·2023 年 6 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F12a9fdff9c10&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplayer-churn-rate-prediction-data-analysis-and-visualisation-part-1-12a9fdff9c10&user=Christian+Galea&userId=a9be78db0c9b&source=-----12a9fdff9c10---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F12a9fdff9c10&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplayer-churn-rate-prediction-data-analysis-and-visualisation-part-1-12a9fdff9c10&source=-----12a9fdff9c10---------------------bookmark_footer-----------)![](img/b1907bde077d48abe4a7127f8a720a6c.png)

照片由 [Tima Miroshnichenko 在 Pexels](https://www.pexels.com/photo/man-in-white-dress-shirt-with-eyeglasses-feeling-exhausted-7567442/) 提供

# 介绍

在游戏世界中，公司不仅致力于吸引玩家，还要尽可能长时间地留住他们，尤其是在依赖游戏内微交易的免费游戏中。这些微交易通常涉及购买游戏内货币，使玩家能够获得用于进度推进或自定义的物品，并资助游戏的开发。监控*流失率*，即停止玩游戏的玩家数量，是至关重要的。因为高流失率意味着收入的显著损失，这反过来会导致开发者和管理者的压力增大。

本文探讨了基于从移动应用中获取的数据的真实世界数据集的使用，特别关注用户玩过的关卡。利用*机器学习*，它已成为技术领域的一个关键部分，并构成了人工智能（AI）的基础，企业可以从他们的数据中提取有价值的见解。
