# 使用 Streamlit 部署您的时间序列预测模型

> 原文：[`towardsdatascience.com/deploy-your-time-series-forecasting-model-with-streamlit-c3ce5a7edf19?source=collection_archive---------10-----------------------#2023-04-25`](https://towardsdatascience.com/deploy-your-time-series-forecasting-model-with-streamlit-c3ce5a7edf19?source=collection_archive---------10-----------------------#2023-04-25)

## 一个关于使用 Python 构建网页应用程序以部署您的预测模型的实践指南

[](https://medium.com/@marcopeixeiro?source=post_page-----c3ce5a7edf19--------------------------------)![Marco Peixeiro](https://medium.com/@marcopeixeiro?source=post_page-----c3ce5a7edf19--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c3ce5a7edf19--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c3ce5a7edf19--------------------------------) [Marco Peixeiro](https://medium.com/@marcopeixeiro?source=post_page-----c3ce5a7edf19--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F741c1c8fcfbd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploy-your-time-series-forecasting-model-with-streamlit-c3ce5a7edf19&user=Marco+Peixeiro&userId=741c1c8fcfbd&source=post_page-741c1c8fcfbd----c3ce5a7edf19---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c3ce5a7edf19--------------------------------) ·13 min read·Apr 25, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc3ce5a7edf19&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploy-your-time-series-forecasting-model-with-streamlit-c3ce5a7edf19&user=Marco+Peixeiro&userId=741c1c8fcfbd&source=-----c3ce5a7edf19---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc3ce5a7edf19&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploy-your-time-series-forecasting-model-with-streamlit-c3ce5a7edf19&source=-----c3ce5a7edf19---------------------bookmark_footer-----------)![](img/597cc6da68b8a31e827f4f2607f94c3d.png)

图片由 [SpaceX](https://unsplash.com/@spacex?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

作为数据科学家，我们经常处于实验阶段。我们在笔记本中工作，编写脚本来评估模型，然后就到此为止。

然而，直到模型部署之前，我们的工作才算真正完成。这个关键步骤带来了新的挑战，因为我们需要考虑错误处理和构建与模型交互的界面。

这就是 [Streamlit](https://streamlit.io/) 发挥作用的地方：它是一个 Python 库，允许我们快速构建数据应用，因为我们仅需使用 Python 来处理建模部分和构建用户界面。

虽然 Streamlit 是一个很棒的原型工具，可以快速部署模型，但它可能无法支持高流量的完整数据应用。你应该把它看作是为你的模型增加互动性并与他人分享数据科学工作的一个方式。

在本文中，我们将介绍使用 Streamlit 部署时间序列模型的部分内容。完整的项目是一个多页面应用，我们可以在其中探索和预测数据，但在本文中，我们仅关注预测功能。
