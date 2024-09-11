# 机器学习项目的简单 CI/CD 设置

> 原文：[`towardsdatascience.com/a-simple-ci-cd-setup-for-ml-projects-604de7fd64cd?source=collection_archive---------1-----------------------#2023-12-20`](https://towardsdatascience.com/a-simple-ci-cd-setup-for-ml-projects-604de7fd64cd?source=collection_archive---------1-----------------------#2023-12-20)

![](img/e7789e958d448c41d90f7ac7c763a80e.png)

图片由 [vackground.com](https://unsplash.com/@vackground?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 应用最佳实践，学习如何使用 GitHub Actions 构建强健的代码

[](https://medium.com/@marcellopoliti?source=post_page-----604de7fd64cd--------------------------------)![Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----604de7fd64cd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----604de7fd64cd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----604de7fd64cd--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----604de7fd64cd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7390355d40fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-simple-ci-cd-setup-for-ml-projects-604de7fd64cd&user=Marcello+Politi&userId=7390355d40fe&source=post_page-7390355d40fe----604de7fd64cd---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----604de7fd64cd--------------------------------) ·7 min 阅读·2023 年 12 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F604de7fd64cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-simple-ci-cd-setup-for-ml-projects-604de7fd64cd&user=Marcello+Politi&userId=7390355d40fe&source=-----604de7fd64cd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F604de7fd64cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-simple-ci-cd-setup-for-ml-projects-604de7fd64cd&source=-----604de7fd64cd---------------------bookmark_footer-----------)

## 介绍

处理**集成、部署、可扩展性**以及所有那些使机器学习项目成为真正产品的主题是一项独立的工作。这就是为什么从数据科学家到 ML 工程师以及 MLOps 领域存在不同职位的原因。尽管如此，即使你不需要在这些领域成为专家，**拥有一些标准的、定义明确的实践来帮助你启动项目**仍然是很有用的。当然！在这篇文章中，我概述了我开发的最佳实践——在代码质量与实施时间之间的平衡。我在 [Deepnote](https://deepnote.com/) 上运行我的代码，这是一个基于云的笔记本，非常适合协作数据科学项目。

## 简单开始 — Readme

这看起来可能微不足道，但**尽量保持 Readme 文件或多或少地更新**。如果这花费的时间不多，而且你喜欢这样，也可以尝试制作一个看起来不错的 Readme。可以包含图标、头部图像或其他任何内容。这个文件必须**清晰易懂**。请记住，在一个实际项目中，你不仅会与其他开发者合作，还会与销售人员和项目经理合作，他们可能会时不时地查看 Readme 以了解项目内容……
