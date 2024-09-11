# 如何在 Amazon ECS 上将 ML 模型正确部署为 Flask API

> 原文：[https://towardsdatascience.com/how-to-properly-deploy-ml-models-as-flask-apis-on-amazon-ecs-98428f9a0ecf?source=collection_archive---------10-----------------------#2023-03-16](https://towardsdatascience.com/how-to-properly-deploy-ml-models-as-flask-apis-on-amazon-ecs-98428f9a0ecf?source=collection_archive---------10-----------------------#2023-03-16)

## 在 Amazon ECS 上部署 XGBoost 模型以推荐完美的小狗

[](https://medium.com/@nikola.kuzmic945?source=post_page-----98428f9a0ecf--------------------------------)[![Nikola Kuzmic](../Images/b6be2a8e377bc450ced5260a79a1f4bb.png)](https://medium.com/@nikola.kuzmic945?source=post_page-----98428f9a0ecf--------------------------------)[](https://towardsdatascience.com/?source=post_page-----98428f9a0ecf--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----98428f9a0ecf--------------------------------) [Nikola Kuzmic](https://medium.com/@nikola.kuzmic945?source=post_page-----98428f9a0ecf--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb8729243da6c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-properly-deploy-ml-models-as-flask-apis-on-amazon-ecs-98428f9a0ecf&user=Nikola+Kuzmic&userId=b8729243da6c&source=post_page-b8729243da6c----98428f9a0ecf---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----98428f9a0ecf--------------------------------) ·8 分钟阅读·2023 年 3 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F98428f9a0ecf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-properly-deploy-ml-models-as-flask-apis-on-amazon-ecs-98428f9a0ecf&user=Nikola+Kuzmic&userId=b8729243da6c&source=-----98428f9a0ecf---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F98428f9a0ecf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-properly-deploy-ml-models-as-flask-apis-on-amazon-ecs-98428f9a0ecf&source=-----98428f9a0ecf---------------------bookmark_footer-----------)![](../Images/d488dd1778ca6099a3740b2e610a8a63.png)

图片来源：[Carissa Weiser](https://unsplash.com/@carissaweiser?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

随着ChatGPT的巨大成功，AI技术对我们生活的影响变得越来越明显。然而，除非这些令人惊叹的机器学习模型能够向每个人提供，并且正确部署以满足高用户需求，否则它们将无法对世界产生任何积极影响。因此，不仅要开发AI解决方案，还必须知道如何正确部署它们，这一点尤为重要。更不用说，这项技能将使你在市场上具有极大的价值，并为你打开从事高薪ML工程角色的职业机会。

在这篇文章中，我们将使用**Gunicorn**应用服务器在亚马逊弹性容器服务**上**部署一个**XGBoost**模型作为**Flask API**。该模型将根据某人家的大小推荐一只腊肠犬或德国牧羊犬幼犬。

![](../Images/dc2a92633d6bb52863eea43d0f470682.png)

图片来源：作者，来源：[1–2]

## 👉 游戏计划

1.  训练一个XGBoost模型

1.  构建一个简单的Gunicorn-Flask API来进行推荐

1.  为…构建一个Docker镜像
