# 演化算法 — 选择机制解释

> 原文：[`towardsdatascience.com/evolutionary-algorithm-selections-explained-2515fb8d4287?source=collection_archive---------12-----------------------#2023-11-04`](https://towardsdatascience.com/evolutionary-algorithm-selections-explained-2515fb8d4287?source=collection_archive---------12-----------------------#2023-11-04)

## 了解发生了什么，通过可视化和代码

![James Koh, PhD](https://medium.com/@byjameskoh?source=post_page-----2515fb8d4287--------------------------------) [James Koh, PhD](https://medium.com/@byjameskoh?source=post_page-----2515fb8d4287--------------------------------) ![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2515fb8d4287--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F780706b02d58&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fevolutionary-algorithm-selections-explained-2515fb8d4287&user=James+Koh%2C+PhD&userId=780706b02d58&source=post_page-780706b02d58----2515fb8d4287---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2515fb8d4287--------------------------------) ·8 分钟阅读·2023 年 11 月 4 日

--

![](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2515fb8d4287&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fevolutionary-algorithm-selections-explained-2515fb8d4287&source=-----2515fb8d4287---------------------bookmark_footer-----------)![](img/a196f197fbda7a28f3ddab8d2b5563e8.png)

图片由 DALL·E 3 根据“绘制一幅描绘演化算法中的父代选择的科幻主题图像”生成

旅行商问题（TSP）是一个著名的 np-hard 问题，在现实生活中有明显的实际应用。例如，如果一个人计划在欧洲旅行，并估算各个地点之间的旅行时间，这个问题就非常有用。

仅仅 30 个城市，就有超过 10³⁰种可能的排列组合。用暴力破解法解决 30 个城市的 TSP 是不现实的（地球上的沙粒数量‘仅有’10¹⁹）。

阅读完这篇文章后，你将能够轻松地使用进化算法（EA）解决此类问题。你将能够自己得到以下结果。

更重要的是，你将深入了解幕后究竟发生了什么，以及每个组件（特别是各种类型的变异）如何对解决方案做出贡献。

# 1\. 价值主张

许多指南错误地使用了“遗传算法（GA）”和“进化算法（EA）”这两个词……
