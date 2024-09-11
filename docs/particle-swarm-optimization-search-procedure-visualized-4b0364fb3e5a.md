# 粒子群优化：搜索过程，可视化

> 原文：[`towardsdatascience.com/particle-swarm-optimization-search-procedure-visualized-4b0364fb3e5a?source=collection_archive---------4-----------------------#2023-12-01`](https://towardsdatascience.com/particle-swarm-optimization-search-procedure-visualized-4b0364fb3e5a?source=collection_archive---------4-----------------------#2023-12-01)

## 直觉 + 数学 + 代码，面向从业者

[](https://medium.com/@byjameskoh?source=post_page-----4b0364fb3e5a--------------------------------)![James Koh, PhD](https://medium.com/@byjameskoh?source=post_page-----4b0364fb3e5a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4b0364fb3e5a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4b0364fb3e5a--------------------------------) [James Koh, PhD](https://medium.com/@byjameskoh?source=post_page-----4b0364fb3e5a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F780706b02d58&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fparticle-swarm-optimization-search-procedure-visualized-4b0364fb3e5a&user=James+Koh%2C+PhD&userId=780706b02d58&source=post_page-780706b02d58----4b0364fb3e5a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4b0364fb3e5a--------------------------------) ·9 分钟阅读·2023 年 12 月 1 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4b0364fb3e5a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fparticle-swarm-optimization-search-procedure-visualized-4b0364fb3e5a&source=-----4b0364fb3e5a---------------------bookmark_footer-----------)![](img/861b57cca91b8b669ecc1b8b6623e6df.png)

由 DALL·E 3 创建的图像，基于提示“在城市景观中描绘一幅科幻主题的图像，描绘一群无人机搜索目标”的创作

人类喜欢模仿自然界的许多事物。

我们模仿青蛙游泳。我们通过在飞机上安装翅膀来模仿鸟类提供升力。我们模仿鹤/蛇/螳螂进行武术。我们模仿白蚁用于建造具有高效温控的结构（见东门中心）。

这同样适用于数学算法，你可能听说过人工**蜜蜂**群、**蚂蚁**群优化、**布谷鸟**搜索和**萤火虫**算法。我之前还谈到过[进化算法](https://medium.com/towards-data-science/evolutionary-algorithm-selections-explained-2515fb8d4287)，它遵循自然选择的原则。

今天，我将讨论粒子群优化（PSO）。在本文的最后，你将拥有代码来实现解决方案，并生成一个 gif 来可视化搜索过程。

# 用例

在高维空间中寻找最优解是困难的。刚接触机器学习的学生可能会在第一周就听说过“维度诅咒”这个术语。

高维空间不仅仅是一个抽象的数学概念。考虑一个供应链问题。一家公司必须决定生产工厂、仓库、配送等的最佳位置……
