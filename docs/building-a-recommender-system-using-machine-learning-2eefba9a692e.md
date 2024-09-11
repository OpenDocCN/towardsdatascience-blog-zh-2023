# 使用机器学习构建推荐系统

> 原文：[https://towardsdatascience.com/building-a-recommender-system-using-machine-learning-2eefba9a692e?source=collection_archive---------1-----------------------#2023-03-01](https://towardsdatascience.com/building-a-recommender-system-using-machine-learning-2eefba9a692e?source=collection_archive---------1-----------------------#2023-03-01)

## [Kaggle蓝图](/the-kaggle-blueprints-unlocking-winning-approaches-to-data-science-competitions-24d7416ef5fd)

## 使用共访矩阵和GBDT排序模型的“候选重排序”方法，Python实现

[](https://medium.com/@iamleonie?source=post_page-----2eefba9a692e--------------------------------)[![Leonie Monigatti](../Images/4044b1685ada53a30160b03dc78f9626.png)](https://medium.com/@iamleonie?source=post_page-----2eefba9a692e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2eefba9a692e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2eefba9a692e--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----2eefba9a692e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-recommender-system-using-machine-learning-2eefba9a692e&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----2eefba9a692e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2eefba9a692e--------------------------------) ·6分钟阅读·2023年3月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2eefba9a692e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-recommender-system-using-machine-learning-2eefba9a692e&user=Leonie+Monigatti&userId=3a38da70d8dc&source=-----2eefba9a692e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2eefba9a692e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-recommender-system-using-machine-learning-2eefba9a692e&source=-----2eefba9a692e---------------------bookmark_footer-----------)![](../Images/812a9ad1d2c667c59f32b0b97872a32d.png)

“一个很棒的选择，女士！我们的汉堡与配菜和饮品完美搭配。请允许我为您推荐一些选项？”（图片由作者提供）

欢迎阅读新文章系列的第一篇 “[Kaggle蓝图](/the-kaggle-blueprints-unlocking-winning-approaches-to-data-science-competitions-24d7416ef5fd)” ，在这里我们将分析 [Kaggle](https://www.kaggle.com/) 竞赛的顶级解决方案，以提炼可以应用于我们自己数据科学项目的经验。

这一版将回顾[“OTTO — 多目标推荐系统”](https://www.kaggle.com/competitions/otto-recommender-system/)竞赛中的技术和方法，该竞赛于2023年1月底结束。

# 问题陈述：多目标推荐系统

[“OTTO — 多目标推荐系统”](https://www.kaggle.com/competitions/otto-recommender-system/)竞赛的目标是构建**基于大规模隐式用户数据的数据集的多目标推荐系统（RecSys）**。

具体来说，在电子商务使用案例中，竞争者处理了以下细节：

+   多目标：点击量、购物车添加、订单

+   大规模数据集：超过2亿个事件，涉及大约180万个项目

+   隐式用户数据：用户会话中的先前事件
