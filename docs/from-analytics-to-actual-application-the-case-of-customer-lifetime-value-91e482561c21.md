# 从分析到实际应用：顾客终身价值的案例

> 原文：[`towardsdatascience.com/from-analytics-to-actual-application-the-case-of-customer-lifetime-value-91e482561c21?source=collection_archive---------0-----------------------#2023-07-02`](https://towardsdatascience.com/from-analytics-to-actual-application-the-case-of-customer-lifetime-value-91e482561c21?source=collection_archive---------0-----------------------#2023-07-02)

## 关于 CLV 技术和实际应用案例的全面实用指南第一部分

[](https://katherineamunro.medium.com/?source=post_page-----91e482561c21--------------------------------)![Katherine Munro](https://katherineamunro.medium.com/?source=post_page-----91e482561c21--------------------------------)[](https://towardsdatascience.com/?source=post_page-----91e482561c21--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----91e482561c21--------------------------------) [Katherine Munro](https://katherineamunro.medium.com/?source=post_page-----91e482561c21--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb84716d39740&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-analytics-to-actual-application-the-case-of-customer-lifetime-value-91e482561c21&user=Katherine+Munro&userId=b84716d39740&source=post_page-b84716d39740----91e482561c21---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----91e482561c21--------------------------------) ·9 分钟阅读·2023 年 7 月 2 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F91e482561c21&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-analytics-to-actual-application-the-case-of-customer-lifetime-value-91e482561c21&user=Katherine+Munro&userId=b84716d39740&source=-----91e482561c21---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F91e482561c21&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-analytics-to-actual-application-the-case-of-customer-lifetime-value-91e482561c21&source=-----91e482561c21---------------------bookmark_footer-----------)![](img/0a1af0787c3fb48fdec1dea568e09ffe.png)

*警告：这可能是你遇到过的最全面的 CLV 指南，所有内容都基于我与之工作的经验。所以，准备好迎接挑战吧。（或者如果你不介意错过一些精彩内容，可以观看我的* [*60 秒总结*](https://www.youtube.com/watch?v=gCvxpMhDJ_s&t=17s)*）。来源：Smarter Ecommerce。*

无论你是数据科学家、市场营销人员还是数据领导者，如果你曾经搜索过“客户终生价值”，你很可能感到失望。我也有过这种感觉，当时我在电子商务领域的数据科学团队中帮助领导一个新的 CLV 研究项目。我们寻找最先进的方法，但谷歌返回的只有基本的教程，数据集处理得过于理想化，还有市场营销的‘空洞’帖子，描述了 CLV 模糊而缺乏创意的用途。关于应用于现实世界数据和真实客户的可用方法的利弊没有任何内容。我们是自己学会这些的，现在我想分享这些经验。

## 介绍：CLV 教程遗漏的所有内容。

在这篇文章中，我将涵盖：

+   什么是 CLV？（我会简要说明，因为你可能已经知道这部分）

+   你真的需要 CLV 预测吗？还是可以从历史 CLV 计算开始？

+   你的公司*已经*能从历史 CLV 信息中获得什么，尤其是当你将其与其他业务数据结合时？
