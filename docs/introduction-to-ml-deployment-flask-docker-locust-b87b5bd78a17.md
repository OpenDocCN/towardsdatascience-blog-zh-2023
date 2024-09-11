# 机器学习部署简介：Flask、Docker 与 Locust

> 原文：[`towardsdatascience.com/introduction-to-ml-deployment-flask-docker-locust-b87b5bd78a17?source=collection_archive---------4-----------------------#2023-02-27`](https://towardsdatascience.com/introduction-to-ml-deployment-flask-docker-locust-b87b5bd78a17?source=collection_archive---------4-----------------------#2023-02-27)

## 学习如何在 Python 中部署你的模型，并使用 Locust 来测量性能

[](https://medium.com/@antonsruberts?source=post_page-----b87b5bd78a17--------------------------------)![Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----b87b5bd78a17--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b87b5bd78a17--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b87b5bd78a17--------------------------------) [Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----b87b5bd78a17--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9dee50b0374b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-ml-deployment-flask-docker-locust-b87b5bd78a17&user=Antons+Tocilins-Ruberts&userId=9dee50b0374b&source=post_page-9dee50b0374b----b87b5bd78a17---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----b87b5bd78a17--------------------------------) ·9 分钟阅读·2023 年 2 月 27 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb87b5bd78a17&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-ml-deployment-flask-docker-locust-b87b5bd78a17&user=Antons+Tocilins-Ruberts&userId=9dee50b0374b&source=-----b87b5bd78a17---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb87b5bd78a17&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-ml-deployment-flask-docker-locust-b87b5bd78a17&source=-----b87b5bd78a17---------------------bookmark_footer-----------)![](img/b57994b78fc248ef65b80754cfd3deaf.png)

照片由[İsmail Enes Ayhan](https://unsplash.com/@ismailenesayhan?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

你已经花了很多时间在 EDA 上，精心制作了特征，调优了模型几天，最终在测试集上表现良好。现在怎么办？现在，朋友，我们需要部署模型。毕竟，任何停留在笔记本中的模型，其价值为零，无论它多么优秀。

学习数据科学工作流中的这一部分可能会让人感到不知所措，特别是如果你没有很多软件工程经验的话。不过，别担心，本文的主要目的是通过介绍 Python 中最受欢迎的部署框架之一——Flask，来帮助你入门。此外，你还将学习如何将部署容器化以及如何衡量其性能，这两个步骤常常被忽视。

# 那么，“部署”到底是什么呢？

首先，让我们澄清一下我在本文中所说的部署是什么意思。机器学习部署是将训练好的模型集成到生产系统（下图中的服务器）中的过程，使其可以被终端用户或其他系统使用。
