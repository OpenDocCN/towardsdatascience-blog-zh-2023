# 发掘 MLflow 的力量

> 原文：[https://towardsdatascience.com/unleashing-the-power-of-mlflow-36c17a693033?source=collection_archive---------12-----------------------#2023-05-30](https://towardsdatascience.com/unleashing-the-power-of-mlflow-36c17a693033?source=collection_archive---------12-----------------------#2023-05-30)

## 机器学习生命周期管理的旋风之旅

[](https://ms101196.medium.com/?source=post_page-----36c17a693033--------------------------------)[![Merlin Schäfer](../Images/70108782dda329ca5bbc4cfb5d650e5e.png)](https://ms101196.medium.com/?source=post_page-----36c17a693033--------------------------------)[](https://towardsdatascience.com/?source=post_page-----36c17a693033--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----36c17a693033--------------------------------) [Merlin Schäfer](https://ms101196.medium.com/?source=post_page-----36c17a693033--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5a2c8842187b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funleashing-the-power-of-mlflow-36c17a693033&user=Merlin+Sch%C3%A4fer&userId=5a2c8842187b&source=post_page-5a2c8842187b----36c17a693033---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----36c17a693033--------------------------------) ·6 min read·2023年5月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F36c17a693033&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funleashing-the-power-of-mlflow-36c17a693033&user=Merlin+Sch%C3%A4fer&userId=5a2c8842187b&source=-----36c17a693033---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F36c17a693033&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funleashing-the-power-of-mlflow-36c17a693033&source=-----36c17a693033---------------------bookmark_footer-----------)![](../Images/8bdfedb66bb9008afbc4103934712f13.png)

照片由 [Stephen Dawson](https://unsplash.com/pt-br/@dawson2406?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

你是否曾经在训练模型、调整超参数和选择特征几个小时后才意识到，你其实已经有了一组好的参数，但却忘记跟踪它们或保存模型？我知道我有过这种情况，也许比我愿意承认的要多。在你打开电子表格，开始记录模型使用的 alpha 值或邻居数之前，我想向你介绍 MLflow。

MLflow 是一个多功能的开源平台，旨在管理整个机器学习生命周期，由 Databricks 开发。它为机器学习从业者、数据科学家和开发人员提供了一系列好处，使得实验、重现性和 ML 模型的部署变得更加流畅。那么，让我们来探索一下它能为你做些什么吧！

## MLflow 的主要组件

在我们深入了解 MLflow 的使用细节之前，了解 MLflow 是什么以及它为何在当今机器学习领域中是一个关键工具是至关重要的。

MLflow 有助于管理机器学习生命周期，包括 **实验**、**重现性** 和 **部署**。它支持任何（Python）机器学习库。它为最常用的库提供了即用的接口，从而赋予了它高度的...
