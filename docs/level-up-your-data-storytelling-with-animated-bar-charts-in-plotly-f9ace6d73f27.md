# 使用 Plotly 的动画条形图提升你的数据讲故事能力

> 原文：[https://towardsdatascience.com/level-up-your-data-storytelling-with-animated-bar-charts-in-plotly-f9ace6d73f27?source=collection_archive---------5-----------------------#2023-12-02](https://towardsdatascience.com/level-up-your-data-storytelling-with-animated-bar-charts-in-plotly-f9ace6d73f27?source=collection_archive---------5-----------------------#2023-12-02)

## 将静态图表转变为引人入胜的叙事

[](https://brian-mattis.medium.com/?source=post_page-----f9ace6d73f27--------------------------------)[![Brian Mattis](../Images/40296183ee475462f7699b65f3a30eba.png)](https://brian-mattis.medium.com/?source=post_page-----f9ace6d73f27--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f9ace6d73f27--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f9ace6d73f27--------------------------------) [Brian Mattis](https://brian-mattis.medium.com/?source=post_page-----f9ace6d73f27--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F47e773cabf10&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flevel-up-your-data-storytelling-with-animated-bar-charts-in-plotly-f9ace6d73f27&user=Brian+Mattis&userId=47e773cabf10&source=post_page-47e773cabf10----f9ace6d73f27---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f9ace6d73f27--------------------------------) ·6 min read·2023年12月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff9ace6d73f27&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flevel-up-your-data-storytelling-with-animated-bar-charts-in-plotly-f9ace6d73f27&user=Brian+Mattis&userId=47e773cabf10&source=-----f9ace6d73f27---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff9ace6d73f27&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flevel-up-your-data-storytelling-with-animated-bar-charts-in-plotly-f9ace6d73f27&source=-----f9ace6d73f27---------------------bookmark_footer-----------)![](../Images/4e12d61700b107cf6c44cc6091ebec87.png)

图片由 [Teemu Paananen](https://unsplash.com/@xteemu?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Plotly 支持动画图表的优良基础。我强烈推荐他们的基础教程 [在这里](https://plotly.com/python/animations/)。然而，Plotly 动画主要是为了为可视化添加另一个维度——通常是时间。这对于为图表增加更多含义非常有帮助。

然而，*动画并不一定要用于增加图表的复杂性*。使用动画**强调**的能力是非常强大的。当我们有一个关键的图表给观众时，我们希望在不明确喊出“这是你应该特别注意的图表！”的情况下吸引他们的注意。我们的眼睛天生被移动的东西所吸引，动画图表可以在构建期待感方面发挥作用。你的观众会参与其中，并在实时中尝试预测下一个条形或线条。想象一下，如果关键的商业决策依赖于这个图表——你的观众在结果展现在他们眼前时会屏息以待！

换个角度来看——假设你已经听了同事的演讲20分钟——哪个图表能抓住你的注意力？这个简单的（由pandas生成的）图表：
