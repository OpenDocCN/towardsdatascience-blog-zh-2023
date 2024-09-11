# 欧洲旅行优化：遗传算法和 Google Maps API 解决旅行推销员问题

> 原文：[https://towardsdatascience.com/euro-trip-optimization-genetic-algorithms-and-google-maps-python-api-solve-the-traveling-salesman-4ad8e1548207?source=collection_archive---------13-----------------------#2023-09-02](https://towardsdatascience.com/euro-trip-optimization-genetic-algorithms-and-google-maps-python-api-solve-the-traveling-salesman-4ad8e1548207?source=collection_archive---------13-----------------------#2023-09-02)

## 使用遗传算法和 Google Maps API 探索欧洲 50 个最受欢迎城市的魅力，解锁高效的旅行路线

[](https://medium.com/@riccardo.andreoni?source=post_page-----4ad8e1548207--------------------------------)[![Riccardo Andreoni](../Images/5e22581e419639b373019a809d6e65c1.png)](https://medium.com/@riccardo.andreoni?source=post_page-----4ad8e1548207--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4ad8e1548207--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4ad8e1548207--------------------------------) [Riccardo Andreoni](https://medium.com/@riccardo.andreoni?source=post_page-----4ad8e1548207--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76784541161c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feuro-trip-optimization-genetic-algorithms-and-google-maps-python-api-solve-the-traveling-salesman-4ad8e1548207&user=Riccardo+Andreoni&userId=76784541161c&source=post_page-76784541161c----4ad8e1548207---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4ad8e1548207--------------------------------) ·8 分钟阅读·2023年9月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4ad8e1548207&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feuro-trip-optimization-genetic-algorithms-and-google-maps-python-api-solve-the-traveling-salesman-4ad8e1548207&user=Riccardo+Andreoni&userId=76784541161c&source=-----4ad8e1548207---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4ad8e1548207&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feuro-trip-optimization-genetic-algorithms-and-google-maps-python-api-solve-the-traveling-salesman-4ad8e1548207&source=-----4ad8e1548207---------------------bookmark_footer-----------)![](../Images/524c212758ae8a7ad835ce4182185b99.png)

来源：[unsplash.com](https://unsplash.com/photos/A5rCN8626Ck)

还记得看过像[《欧洲旅行记》](https://www.imdb.com/title/tt0356150/)这样的电影后那种感觉吗？影片中的角色穿梭于风景如画的欧洲城市，展开一场终生难忘的冒险。这种情景令人着迷。然而，现实很快会提醒我们：安排一场横跨多个目的地的旅行绝非易事。但这里有一个令人兴奋的转折——凭借编程专业知识和对遗传算法的掌握，我开始开发一种解决方案。想象一下，能够精准优化跨越数十个地点的复杂路线。这正是数据科学的世界与冒险规划艺术交汇的地方。在这篇文章中，我揭示了一个优雅地解决复杂[旅行推销员问题](https://en.wikipedia.org/wiki/Travelling_salesman_problem)（TSP）的算法脚本，旨在帮助旅行规划并提升我们对数据科学中优化的理解。

阅读本文将使你清楚地理解Python、Google Maps API和遗传算法之间的协同作用如何为复杂任务提供数据驱动的解决方案。

# 理解……
