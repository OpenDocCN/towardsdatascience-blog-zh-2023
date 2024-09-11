# Julia 中的数据过滤：你需要知道的一切

> 原文：[`towardsdatascience.com/data-filtering-in-julia-everything-you-need-to-know-d0bf08a00328?source=collection_archive---------10-----------------------#2023-07-11`](https://towardsdatascience.com/data-filtering-in-julia-everything-you-need-to-know-d0bf08a00328?source=collection_archive---------10-----------------------#2023-07-11)

## 关于 Julia 中的数据过滤，你需要知道的一切

[](https://emmaccode.medium.com/?source=post_page-----d0bf08a00328--------------------------------)![艾玛·布德罗](https://emmaccode.medium.com/?source=post_page-----d0bf08a00328--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d0bf08a00328--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d0bf08a00328--------------------------------) [艾玛·布德罗](https://emmaccode.medium.com/?source=post_page-----d0bf08a00328--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fea170050148c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-filtering-in-julia-everything-you-need-to-know-d0bf08a00328&user=Emma+Boudreau&userId=ea170050148c&source=post_page-ea170050148c----d0bf08a00328---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d0bf08a00328--------------------------------) ·7 min read·2023 年 7 月 11 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd0bf08a00328&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-filtering-in-julia-everything-you-need-to-know-d0bf08a00328&user=Emma+Boudreau&userId=ea170050148c&source=-----d0bf08a00328---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd0bf08a00328&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-filtering-in-julia-everything-you-need-to-know-d0bf08a00328&source=-----d0bf08a00328---------------------bookmark_footer-----------)![](img/0d8be971b1ee16caf88f89d7affa2ca9.png)

图片由 [Najib Kalil](https://unsplash.com/@nkalil?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在数据科学中，进行假设检验、机器学习甚至分析时，获取结果的最重要组成部分是拥有良好的数据。数据通常需要满足多种不同的要求。一个在数据世界中极为常见且频繁使用的技术是数据过滤。数据过滤可能是移除不属于数据的部分，或者是抓取符合某一参数或多个参数的样本。

我们可以说，过滤的一个示例是当我们从数据中移除缺失值时。这是数据科学过程中的一个关键步骤，通常使用过滤技术完成。抓取符合某些设定参数的样本的示例是，当我们尝试测试身高与撞头之间的统计显著性时，我们会过滤掉所有身材矮小的数据，以便仅用高个子的人的数据进行测试。

这种技术有许多应用。过滤也可能是执行一些常见数据科学任务的关键，因此它是…
