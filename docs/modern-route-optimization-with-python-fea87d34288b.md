# 使用 Python 的现代路线优化

> 原文：[https://towardsdatascience.com/modern-route-optimization-with-python-fea87d34288b?source=collection_archive---------2-----------------------#2023-06-07](https://towardsdatascience.com/modern-route-optimization-with-python-fea87d34288b?source=collection_archive---------2-----------------------#2023-06-07)

## 最短路径、旅行商问题、车辆路线问题、绘制地图和动画

[](https://maurodp.medium.com/?source=post_page-----fea87d34288b--------------------------------)[![Mauro Di Pietro](../Images/3586d9d3238d904a1e1fa39c77b59d3f.png)](https://maurodp.medium.com/?source=post_page-----fea87d34288b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fea87d34288b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fea87d34288b--------------------------------) [Mauro Di Pietro](https://maurodp.medium.com/?source=post_page-----fea87d34288b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F44a176cd070a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmodern-route-optimization-with-python-fea87d34288b&user=Mauro+Di+Pietro&userId=44a176cd070a&source=post_page-44a176cd070a----fea87d34288b---------------------post_header-----------) 在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fea87d34288b--------------------------------) 发表 · 13 分钟阅读 · 2023年6月7日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffea87d34288b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmodern-route-optimization-with-python-fea87d34288b&user=Mauro+Di+Pietro&userId=44a176cd070a&source=-----fea87d34288b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffea87d34288b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmodern-route-optimization-with-python-fea87d34288b&source=-----fea87d34288b---------------------bookmark_footer-----------)

在这篇文章中，我将使用 Python 解决以下问题：为了向一组客户提供服务，1辆或多辆车辆的最佳路线是什么？

![](../Images/9f53173e6217b1a3283583fa432e80f1.png)

图片由 [Robert Anasch](https://unsplash.com/@diesektion?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上提供

## 摘要

**路线优化** 是确定最具成本效益的路线的过程。这并不一定意味着找到两点之间的最短路径，因为它包括所有相关因素（即利润、地点数量、时间窗口）。

这个问题在1930年代首次以数学方式提出，用于解决校车路线问题。它被称为[**旅行推销员问题**](https://en.wikipedia.org/wiki/Travelling_salesman_problem)，其内容是找到一种最短路径，让司机访问所有地点，前提是已知各地点之间的距离。

旅行推销员问题可以被泛化为[**车辆路线问题**](https://en.wikipedia.org/wiki/Vehicle_routing_problem)**:** 即为车辆规划路线，同时最小化由运营成本和用户偏好组成的目标函数。这是物流运输中的主要问题。例如，如果晚上最短路径上交通繁忙（或收费高），它可能不是晚餐配送的最佳路线。
