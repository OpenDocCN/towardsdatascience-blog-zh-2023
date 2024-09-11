# 医疗数据本质上是有偏的

> 原文：[`towardsdatascience.com/healthcare-is-inherently-biased-b60bf00d4af7?source=collection_archive---------10-----------------------#2023-01-19`](https://towardsdatascience.com/healthcare-is-inherently-biased-b60bf00d4af7?source=collection_archive---------10-----------------------#2023-01-19)

## 这是如何避免被欺骗的方法

[](https://stefanygoradia.medium.com/?source=post_page-----b60bf00d4af7--------------------------------)![Stefany Goradia](https://stefanygoradia.medium.com/?source=post_page-----b60bf00d4af7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b60bf00d4af7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b60bf00d4af7--------------------------------) [Stefany Goradia](https://stefanygoradia.medium.com/?source=post_page-----b60bf00d4af7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F36e97dd8343d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhealthcare-is-inherently-biased-b60bf00d4af7&user=Stefany+Goradia&userId=36e97dd8343d&source=post_page-36e97dd8343d----b60bf00d4af7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b60bf00d4af7--------------------------------) ·10 min read·2023 年 1 月 19 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb60bf00d4af7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhealthcare-is-inherently-biased-b60bf00d4af7&user=Stefany+Goradia&userId=36e97dd8343d&source=-----b60bf00d4af7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb60bf00d4af7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhealthcare-is-inherently-biased-b60bf00d4af7&source=-----b60bf00d4af7---------------------bookmark_footer-----------)

通常，当你想到偏见时，你可能会想到一个人的信念，这些信念不经意间塑造了他们的假设和方法。这无疑是一个定义。

但偏见也指我们用于洞察的数据本身可能会在不知不觉中出现偏差和不完整，从而扭曲我们看待一切的视角。

## **许多——如果不是大多数——**我们在医疗保健中使用的数据集本质上都是有偏的，如果我们不小心，很容易使我们偏离正轨。

我将特别关注三个主要概念，这些概念影响医疗数据，甚至可能在低调地使您的整个分析失效。

![](img/86ebe2d55d0a27995076b8973336255d.png)

图片来源于 [Kaleb Nimz](https://unsplash.com/@kalebnimz?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在之前的[文章](https://medium.com/towards-data-science/is-healthcare-analytics-right-for-you-320897b34409)中，我写到了在医疗分析职业生涯中遇到的各种偏见类型，以及这些偏见如何对数据从业者构成挑战。对我来说，最大的主题包括：**个人偏见、数据偏见和确认偏见**。

在我发布那篇文章的四天后，一位导师给我发送了[这篇关于医疗索赔数据中抽样偏见以及社会健康决定因素（SDOH）在某些地区如何加剧这一偏见的 JAMA 文章](https://jamanetwork.com/journals/jamanetworkopen/article-abstract/2800118)。
