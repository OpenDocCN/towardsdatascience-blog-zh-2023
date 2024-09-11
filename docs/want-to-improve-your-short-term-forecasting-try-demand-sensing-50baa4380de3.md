# 想要提高短期预测能力？试试需求感知

> 原文：[https://towardsdatascience.com/want-to-improve-your-short-term-forecasting-try-demand-sensing-50baa4380de3?source=collection_archive---------8-----------------------#2023-06-20](https://towardsdatascience.com/want-to-improve-your-short-term-forecasting-try-demand-sensing-50baa4380de3?source=collection_archive---------8-----------------------#2023-06-20)

## 当传统的预测方法在准确性上达到瓶颈时，我们如何推动进一步的改进？

[](https://medium.com/@rkumar5680?source=post_page-----50baa4380de3--------------------------------)[![Ramkumar K](../Images/37695331380b695bbc3b3a23eae6a267.png)](https://medium.com/@rkumar5680?source=post_page-----50baa4380de3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----50baa4380de3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----50baa4380de3--------------------------------) [Ramkumar K](https://medium.com/@rkumar5680?source=post_page-----50baa4380de3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe330097ea68c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwant-to-improve-your-short-term-forecasting-try-demand-sensing-50baa4380de3&user=Ramkumar+K&userId=e330097ea68c&source=post_page-e330097ea68c----50baa4380de3---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----50baa4380de3--------------------------------) ·12 分钟阅读·2023年6月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F50baa4380de3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwant-to-improve-your-short-term-forecasting-try-demand-sensing-50baa4380de3&user=Ramkumar+K&userId=e330097ea68c&source=-----50baa4380de3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F50baa4380de3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwant-to-improve-your-short-term-forecasting-try-demand-sensing-50baa4380de3&source=-----50baa4380de3---------------------bookmark_footer-----------)![](../Images/3483dc57ec31532423d2ffd7d4dc8e2b.png)

照片由 [JJ Ying](https://unsplash.com/@jjying?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

需求预测是一个估算组织未来某一时间段销售情况的过程。短期需求预测通常覆盖1到3个月，而中期预测可以覆盖6到18个月。长期预测则通常达到3到5年。预测帮助企业决定销售什么、何时销售以及销售多少，持有多少库存，并决定未来在产能上的投资，以满足动态的客户需求。公司通常依赖历史趋势，并结合客户输入，同时考虑促销或清仓销售，以创建需求预测。

需求预测非常重要，原因有几个。它位于销售与运营计划（S&OP）过程的顶部，在这一阶段生成的预测会影响到后续的供应规划、生产调度、物流规划和库存优化等其他阶段。需求预测的准确性至关重要，以避免因库存过多或过少而产生的成本。过度预测可能导致过多的营运资本被占用在库存上。另一方面…
