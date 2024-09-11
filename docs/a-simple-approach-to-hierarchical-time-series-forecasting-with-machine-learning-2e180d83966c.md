# 机器学习层次时间序列预测的简单方法

> 原文：[`towardsdatascience.com/a-simple-approach-to-hierarchical-time-series-forecasting-with-machine-learning-2e180d83966c?source=collection_archive---------1-----------------------#2023-03-14`](https://towardsdatascience.com/a-simple-approach-to-hierarchical-time-series-forecasting-with-machine-learning-2e180d83966c?source=collection_archive---------1-----------------------#2023-03-14)

## Kaggle 蓝图

## 如何使用 LightGBM 和 Python“提升”你的周期性销售数据预测

[](https://medium.com/@iamleonie?source=post_page-----2e180d83966c--------------------------------)![Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----2e180d83966c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2e180d83966c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2e180d83966c--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----2e180d83966c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-simple-approach-to-hierarchical-time-series-forecasting-with-machine-learning-2e180d83966c&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----2e180d83966c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2e180d83966c--------------------------------) ·8 min read·2023 年 3 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2e180d83966c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-simple-approach-to-hierarchical-time-series-forecasting-with-machine-learning-2e180d83966c&user=Leonie+Monigatti&userId=3a38da70d8dc&source=-----2e180d83966c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2e180d83966c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-simple-approach-to-hierarchical-time-series-forecasting-with-machine-learning-2e180d83966c&source=-----2e180d83966c---------------------bookmark_footer-----------)![](img/a12cc1074687ea692350259d6b5e4ec7.png)

层次时间序列预测（图片由作者绘制）

欢迎来到另一期“Kaggle 蓝图”，在这里我们将分析[Kaggle](https://www.kaggle.com/)竞赛的获胜解决方案，以获取可以应用于我们自己数据科学项目的经验。

本期将回顾[M5 预测 — 准确性](https://www.kaggle.com/competitions/m5-forecasting-accuracy/)竞赛中的技术和方法，该竞赛于 2020 年 6 月底结束。

# 问题描述：层次时间序列预测

“[M5 预测 — 准确性](https://www.kaggle.com/competitions/m5-forecasting-accuracy/)”竞赛的目标是预测 42,840 个销售数据的层次时间序列在接下来的 28 天的表现。

[](https://www.kaggle.com/competitions/m5-forecasting-accuracy/?source=post_page-----2e180d83966c--------------------------------) [## M5 预测 — 准确性

### 估计沃尔玛零售商品的单位销售量

[www.kaggle.com](https://www.kaggle.com/competitions/m5-forecasting-accuracy/?source=post_page-----2e180d83966c--------------------------------)

**层次时间序列** — 与常见的多变量时间序列问题不同，层次时间序列可以在不同层次上聚合：例如，商品层次、商店层次和州层次。在这个竞赛中，竞争者…
