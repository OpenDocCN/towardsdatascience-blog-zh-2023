# 数据可视化中的故事讲述——哪个区域的社会经济评分最高，为什么

> 原文：[https://towardsdatascience.com/story-telling-with-visualization-which-area-has-the-highest-socio-economic-score-and-why-c1205b2450c7?source=collection_archive---------2-----------------------#2023-12-26](https://towardsdatascience.com/story-telling-with-visualization-which-area-has-the-highest-socio-economic-score-and-why-c1205b2450c7?source=collection_archive---------2-----------------------#2023-12-26)

## 使用真实地理数据进行演示

[](https://jin-cui.medium.com/?source=post_page-----c1205b2450c7--------------------------------)[![Jin Cui](../Images/e5ddcbaa6d7da38f960d2c5fea71b538.png)](https://jin-cui.medium.com/?source=post_page-----c1205b2450c7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c1205b2450c7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c1205b2450c7--------------------------------) [Jin Cui](https://jin-cui.medium.com/?source=post_page-----c1205b2450c7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F333e9ae026bc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstory-telling-with-visualization-which-area-has-the-highest-socio-economic-score-and-why-c1205b2450c7&user=Jin+Cui&userId=333e9ae026bc&source=post_page-333e9ae026bc----c1205b2450c7---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c1205b2450c7--------------------------------) · 8分钟阅读 · 2023年12月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc1205b2450c7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstory-telling-with-visualization-which-area-has-the-highest-socio-economic-score-and-why-c1205b2450c7&user=Jin+Cui&userId=333e9ae026bc&source=-----c1205b2450c7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc1205b2450c7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstory-telling-with-visualization-which-area-has-the-highest-socio-economic-score-and-why-c1205b2450c7&source=-----c1205b2450c7---------------------bookmark_footer-----------)![](../Images/74337bc2945f63c1e7d7b1f88b1d8ec3.png)

图片由[Joash Viriah](https://unsplash.com/@joashzn?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

偶尔，为了更有效地分配资源，政府可能会收集个人或家庭有关其人口特征的数据，如年龄、性别和出生国，以及其社会经济特征的数据，如收入、职业和消费。这些数据有时会按地理区域进行汇总，并向公众提供。

在我居住的澳大利亚，政府通过澳大利亚统计局（ABS）校准一个名为[**经济资源指数（IER）**](https://www.abs.gov.au/statistics/people/people-and-communities/socio-economic-indexes-areas-seifa-australia/latest-release#index-of-economic-resources-ier-)**的指标，该指标使用来自五年一度的普查数据收集的各种变量来评分地理区域的相对社会经济状况。

IER 可以通过各种数字边界进行汇总，这些边界将澳大利亚划分为不同大小的地理区域。例如，州界（图像 1 中的虚线）将澳大利亚划分为 8 个州和领地，而[统计区域 1](https://www.abs.gov.au/ausstats/abs@.nsf/Lookup/2901.0Chapter23002016#SA1)（SA1）边界（图像 2 中）将澳大利亚划分为更细致的区域，有时甚至只是几个街区的聚集体。
