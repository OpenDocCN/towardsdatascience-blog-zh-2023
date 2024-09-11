# 使用 React 构建互动界面以处理令人兴奋的数据集

> 原文：[`towardsdatascience.com/using-react-to-build-interactive-interfaces-to-exciting-dataset-c01691a5fc38?source=collection_archive---------4-----------------------#2023-09-19`](https://towardsdatascience.com/using-react-to-build-interactive-interfaces-to-exciting-dataset-c01691a5fc38?source=collection_archive---------4-----------------------#2023-09-19)

## 数据教程

## 使用网页开发创建更动态的数据可视化体验

[](https://medium.com/@oscarleo?source=post_page-----c01691a5fc38--------------------------------)![Oscar Leo](https://medium.com/@oscarleo?source=post_page-----c01691a5fc38--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c01691a5fc38--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c01691a5fc38--------------------------------) [Oscar Leo](https://medium.com/@oscarleo?source=post_page-----c01691a5fc38--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd7e5c1ca65b7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-react-to-build-interactive-interfaces-to-exciting-dataset-c01691a5fc38&user=Oscar+Leo&userId=d7e5c1ca65b7&source=post_page-d7e5c1ca65b7----c01691a5fc38---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c01691a5fc38--------------------------------) ·5 分钟阅读·2023 年 9 月 19 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc01691a5fc38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-react-to-build-interactive-interfaces-to-exciting-dataset-c01691a5fc38&user=Oscar+Leo&userId=d7e5c1ca65b7&source=-----c01691a5fc38---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc01691a5fc38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-react-to-build-interactive-interfaces-to-exciting-dataset-c01691a5fc38&source=-----c01691a5fc38---------------------bookmark_footer-----------)

在我全职担任小型机器学习公司的首席执行官的同时，我的爱好是创建美丽的数据可视化。

我通常使用 Matplotlib，但这次我想创建一个更互动的体验。

由于我喜欢网页开发和设计，我决定为世界银行的[人口估计与预测](https://datacatalog.worldbank.org/search/dataset/0037655/Population-Estimates-and-Projections)数据集创建一个 React 应用程序。

这是一个引人入胜的数据集，你可以查看 1960 年到 2022 年间所有国家和地区的人口金字塔，包括对 2050 年的预测。它采用 Creative Commons Attribution 4.0 许可证。

这个数据集也非常适合用于一个交互式界面，人们可以快速更改年份和地区。

在这个故事中，我将分享我工作中的见解和所学到的知识。

如果你想测试这个解决方案，你可以在这里找到它: [`datawonder.io/population-pyramids`](https://datawonder.io/population-pyramids)
