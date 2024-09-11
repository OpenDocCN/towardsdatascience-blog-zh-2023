# 使用 XGBoost 预测水泵的功能性

> 原文：[https://towardsdatascience.com/predicting-the-functionality-of-water-pumps-with-xgboost-8768b07ac7bb?source=collection_archive---------1-----------------------#2023-06-01](https://towardsdatascience.com/predicting-the-functionality-of-water-pumps-with-xgboost-8768b07ac7bb?source=collection_archive---------1-----------------------#2023-06-01)

## 一个受“数据挖掘水泵竞赛”启发的端到端机器学习项目

[](https://medium.com/@aashishnair?source=post_page-----8768b07ac7bb--------------------------------)[![Aashish Nair](../Images/23f4b3839e464419332b690a4098d824.png)](https://medium.com/@aashishnair?source=post_page-----8768b07ac7bb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8768b07ac7bb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8768b07ac7bb--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----8768b07ac7bb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3087ba81e065&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpredicting-the-functionality-of-water-pumps-with-xgboost-8768b07ac7bb&user=Aashish+Nair&userId=3087ba81e065&source=post_page-3087ba81e065----8768b07ac7bb---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8768b07ac7bb--------------------------------) · 10 分钟阅读 · 2023年6月1日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8768b07ac7bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpredicting-the-functionality-of-water-pumps-with-xgboost-8768b07ac7bb&user=Aashish+Nair&userId=3087ba81e065&source=-----8768b07ac7bb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8768b07ac7bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpredicting-the-functionality-of-water-pumps-with-xgboost-8768b07ac7bb&source=-----8768b07ac7bb---------------------bookmark_footer-----------)![](../Images/e8584924a9bc02000263fad2b32827e6.png)

由 Kelly 拍摄：[https://www.pexels.com/photo/close-up-of-a-child-s-hands-catching-water-from-the-spout-of-a-water-pump-3030281/](https://www.pexels.com/photo/close-up-of-a-child-s-hands-catching-water-from-the-spout-of-a-water-pump-3030281/)

## 目录

∘ [介绍](#b283)

∘ [目标](#2473)

∘ [工具/框架](#1b28)

∘ [探索性数据分析](#1adb)

∘ [特征工程](#6874)

∘ [创建训练集和测试集](#708c)

∘ [确定评估指标](#ef5c)

∘ [创建基准模型](#4d47)

∘ [数据建模方法](#4f41)

∘ [超参数调优方法](#206d)

∘ [XGBoost 模型](#dd8b)

∘ [CatBoost 模型](#e73a)

∘ [LightGBM 模型](#16bf)

∘ [选择最佳模型](#45e7)

∘ [模型解释](#31b6)

∘ [模型部署](#923e)

∘ [局限性](#c2da)

∘ [结论](#1679)

∘ [参考文献](#07f5)

## 介绍

> 注意：该项目的灵感来源于由 DrivenData 主办的 [Pump it Up: Data Mining the Water Table 竞赛](https://www.drivendata.org/competitions/7/pump-it-up-data-mining-the-water-table/page/23/)。
