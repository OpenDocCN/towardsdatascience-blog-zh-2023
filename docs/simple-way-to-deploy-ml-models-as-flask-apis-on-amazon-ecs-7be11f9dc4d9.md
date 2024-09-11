# 在 Amazon ECS 上以简单方式部署 ML 模型作为 Flask APIs

> 原文：[`towardsdatascience.com/simple-way-to-deploy-ml-models-as-flask-apis-on-amazon-ecs-7be11f9dc4d9?source=collection_archive---------8-----------------------#2023-03-10`](https://towardsdatascience.com/simple-way-to-deploy-ml-models-as-flask-apis-on-amazon-ecs-7be11f9dc4d9?source=collection_archive---------8-----------------------#2023-03-10)

## 在 4 分钟内将 Flask APIs 部署到 Amazon ECS

[](https://medium.com/@nikola.kuzmic945?source=post_page-----7be11f9dc4d9--------------------------------)![Nikola Kuzmic](https://medium.com/@nikola.kuzmic945?source=post_page-----7be11f9dc4d9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7be11f9dc4d9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7be11f9dc4d9--------------------------------) [Nikola Kuzmic](https://medium.com/@nikola.kuzmic945?source=post_page-----7be11f9dc4d9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb8729243da6c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimple-way-to-deploy-ml-models-as-flask-apis-on-amazon-ecs-7be11f9dc4d9&user=Nikola+Kuzmic&userId=b8729243da6c&source=post_page-b8729243da6c----7be11f9dc4d9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7be11f9dc4d9--------------------------------) ·6 分钟阅读·2023 年 3 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7be11f9dc4d9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimple-way-to-deploy-ml-models-as-flask-apis-on-amazon-ecs-7be11f9dc4d9&user=Nikola+Kuzmic&userId=b8729243da6c&source=-----7be11f9dc4d9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7be11f9dc4d9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimple-way-to-deploy-ml-models-as-flask-apis-on-amazon-ecs-7be11f9dc4d9&source=-----7be11f9dc4d9---------------------bookmark_footer-----------)![](img/e65f1c65818c31933d75bdf5d19ede50.png)

图片由 [Arjan van den Berg](https://unsplash.com/@arjan71?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇文章中，我们将介绍如何部署一个线性回归的 XGBoost 模型，该模型根据开发者的经验年限预测其薪资。

## 👉 游戏计划

1.  训练一个 XGBoost 模型

1.  构建一个简单的 Flask API 来提供模型预测

1.  为 Flask API 构建一个 Docker 镜像

1.  在 Amazon ECS 上部署 Docker 容器

完整源代码 Github 仓库：[链接](https://github.com/kuzmicni/flask-on-ecs/tree/simple)🧑‍💻

```py
flask-on-ecs - repo structure
.
├── Dockerfile
├── README.md
├── myapp.py
├── requirements.txt
└── train_xgb.ipynb
```

## 为什么我们需要 API 来部署机器学习模型

如果你正在阅读这篇文章，说明你已经到了数据科学项目中的一个阶段，即希望将你出色的机器学习模型提供给互联网上的所有人。人们将这个步骤称为将模型部署到生产环境中。

在这里，我们不会把事情弄得太复杂，也不会探讨一个合适的生产级部署应该是什么样的，而是简单地利用默认的 Flask 开发服务器来展示从头到尾的过程，展示完成所需的步骤…
