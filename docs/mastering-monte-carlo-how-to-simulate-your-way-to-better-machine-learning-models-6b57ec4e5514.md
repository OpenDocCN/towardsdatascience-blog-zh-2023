# 精通蒙特卡洛：如何通过模拟提高机器学习模型的效果

> 原文：[https://towardsdatascience.com/mastering-monte-carlo-how-to-simulate-your-way-to-better-machine-learning-models-6b57ec4e5514?source=collection_archive---------1-----------------------#2023-08-02](https://towardsdatascience.com/mastering-monte-carlo-how-to-simulate-your-way-to-better-machine-learning-models-6b57ec4e5514?source=collection_archive---------1-----------------------#2023-08-02)

## 通过增强预测算法的模拟技术应用概率方法

[](https://medium.com/@sydneynye?source=post_page-----6b57ec4e5514--------------------------------)[![Sydney Nye](../Images/e85d31078f0844694b857143ded3e912.png)](https://medium.com/@sydneynye?source=post_page-----6b57ec4e5514--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6b57ec4e5514--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6b57ec4e5514--------------------------------) [Sydney Nye](https://medium.com/@sydneynye?source=post_page-----6b57ec4e5514--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8a83f11e92c5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-monte-carlo-how-to-simulate-your-way-to-better-machine-learning-models-6b57ec4e5514&user=Sydney+Nye&userId=8a83f11e92c5&source=post_page-8a83f11e92c5----6b57ec4e5514---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6b57ec4e5514--------------------------------) ·26分钟阅读·2023年8月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6b57ec4e5514&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-monte-carlo-how-to-simulate-your-way-to-better-machine-learning-models-6b57ec4e5514&user=Sydney+Nye&userId=8a83f11e92c5&source=-----6b57ec4e5514---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6b57ec4e5514&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-monte-carlo-how-to-simulate-your-way-to-better-machine-learning-models-6b57ec4e5514&source=-----6b57ec4e5514---------------------bookmark_footer-----------)![](../Images/efbc38b2902307f75ef51ff4a36e9eba.png)

**爱德华·蒙克**（1892年）的《蒙特卡洛的轮盘桌》

# 一位科学家玩牌如何永远改变了统计游戏

在1945年的动荡年间，当世界正被第二次世界大战的最后挣扎所困扰时，一场纸牌接龙游戏静静地引发了计算领域的进步。这不是一场普通的游戏，而是引领蒙特卡罗方法([1](https://library.lanl.gov/la-pubs/00326866.pdf))诞生的游戏。玩家？正是科学家**斯坦尼斯瓦夫·乌拉姆**，他也深深参与了曼哈顿计划([2](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2924739/))。乌拉姆在康复期间，沉迷于纸牌接龙游戏。游戏中的复杂概率引起了他的兴趣，他意识到重复模拟游戏可以提供这些概率的良好近似([3](https://www.sciencedirect.com/topics/economics-econometrics-and-finance/monte-carlo-simulation))。这是一个灵光一闪的时刻，类似于牛顿的苹果，不过这次是扑克牌代替了水果。乌拉姆随后与他的同事**约翰·冯·诺依曼**讨论了这些想法，他们共同将蒙特卡罗方法正式化，命名为蒙特卡罗方法，源自摩纳哥著名的蒙特卡罗赌场（如上所示由爱德华·穆彻的著名画作描绘），在那里，赌注很高，机遇…
