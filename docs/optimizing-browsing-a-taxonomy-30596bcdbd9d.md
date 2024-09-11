# 优化浏览分类

> 原文：[`towardsdatascience.com/optimizing-browsing-a-taxonomy-30596bcdbd9d?source=collection_archive---------25-----------------------#2023-01-04`](https://towardsdatascience.com/optimizing-browsing-a-taxonomy-30596bcdbd9d?source=collection_archive---------25-----------------------#2023-01-04)

## **问题、模型和实施问题**

[](https://jagota-arun.medium.com/?source=post_page-----30596bcdbd9d--------------------------------)![Arun Jagota](https://jagota-arun.medium.com/?source=post_page-----30596bcdbd9d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----30596bcdbd9d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----30596bcdbd9d--------------------------------) [Arun Jagota](https://jagota-arun.medium.com/?source=post_page-----30596bcdbd9d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fef9ed921edad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimizing-browsing-a-taxonomy-30596bcdbd9d&user=Arun+Jagota&userId=ef9ed921edad&source=post_page-ef9ed921edad----30596bcdbd9d---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----30596bcdbd9d--------------------------------) ·20 分钟阅读·2023 年 1 月 4 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F30596bcdbd9d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimizing-browsing-a-taxonomy-30596bcdbd9d&user=Arun+Jagota&userId=ef9ed921edad&source=-----30596bcdbd9d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F30596bcdbd9d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimizing-browsing-a-taxonomy-30596bcdbd9d&source=-----30596bcdbd9d---------------------bookmark_footer-----------)![](img/701afca47c30a4c877941cba92f3156f.png)

图片来自[mcmurryjulie](https://pixabay.com/users/mcmurryjulie-2375405/)的[pixabay](https://pixabay.com/)

这方面的主要用例是[`jagota-arun.medium.com/interactive-and-adaptive-menu-ordering-agent-bb447c58b3af`](https://jagota-arun.medium.com/interactive-and-adaptive-menu-ordering-agent-bb447c58b3af)（不是必读内容）。

在这里我们专注于建模。它是什么？它如何与推荐系统相关？然后是实现方面。就像面向对象设计一样，包括类等。

面向对象的阐述风格将排序、模块化、组合性和扩展性的好处扩展到写作中。在这里非常适用。

本文中最重要的概念是分类法。一切都发生在分类法上。用户浏览、用户在叶子上的操作、建模以及评分，都是为了动态调整分类法，以优化用户的浏览体验。

那么我们先来定义一下。所谓分类法，是指一个选择树，其叶子上有具体的兴趣项。我们最终假设用户关心的是到达感兴趣的叶子。内部节点主要用于导航。我们通过用户是否在叶子上采取行动来进行学习。

一个例子是餐馆菜单。菜单项位于叶子上。内部节点是菜单上的各种类别：饮料、开胃菜、主菜等。用户的操作是订购特定的菜单项。如果用户…
