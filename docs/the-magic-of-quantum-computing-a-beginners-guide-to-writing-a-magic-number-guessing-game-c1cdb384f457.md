# 量子计算的魔力：初学者编写魔术数字猜测游戏指南

> 原文：[https://towardsdatascience.com/the-magic-of-quantum-computing-a-beginners-guide-to-writing-a-magic-number-guessing-game-c1cdb384f457?source=collection_archive---------11-----------------------#2023-02-10](https://towardsdatascience.com/the-magic-of-quantum-computing-a-beginners-guide-to-writing-a-magic-number-guessing-game-c1cdb384f457?source=collection_archive---------11-----------------------#2023-02-10)

## 教程

## 编程量子计算机是有趣的！

[](https://medium.com/@KoryBecker?source=post_page-----c1cdb384f457--------------------------------)[![Kory Becker](../Images/53a2493fe53f215d3e715d456b36c553.png)](https://medium.com/@KoryBecker?source=post_page-----c1cdb384f457--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c1cdb384f457--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c1cdb384f457--------------------------------) [Kory Becker](https://medium.com/@KoryBecker?source=post_page-----c1cdb384f457--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9f206469e308&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-magic-of-quantum-computing-a-beginners-guide-to-writing-a-magic-number-guessing-game-c1cdb384f457&user=Kory+Becker&userId=9f206469e308&source=post_page-9f206469e308----c1cdb384f457---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c1cdb384f457--------------------------------) · 9分钟阅读 · 2023年2月10日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc1cdb384f457&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-magic-of-quantum-computing-a-beginners-guide-to-writing-a-magic-number-guessing-game-c1cdb384f457&user=Kory+Becker&userId=9f206469e308&source=-----c1cdb384f457---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc1cdb384f457&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-magic-of-quantum-computing-a-beginners-guide-to-writing-a-magic-number-guessing-game-c1cdb384f457&source=-----c1cdb384f457---------------------bookmark_footer-----------)![](../Images/6931c4891a0299efe800b4c0f900ef11.png)

来源：[稳定扩散](https://stablediffusionweb.com/)。

# 从经典到量子

我不太确定量子计算到底是什么让人感到如此惊奇和好奇，但它确实感觉与编程经典计算机有很大的不同。量子计算领域仍然年轻且充满潜力，为像*你*这样的人打开了许多可能性的大门，可以产生影响。

我发现学习一项技术的最佳方式之一是通过游戏。还有什么比自己编写一个游戏更好的方式来学习量子计算呢？

# 一个简单游戏的设计

我们将使用 Python 和一个名为 Qiskit 的开源量子计算库来编写一个魔法数字猜测游戏。

我们的游戏非常简单，但将由量子计算的特性驱动。

我们的游戏将有以下设计：

1.  量子计算机将生成一个从 0 到 15 的随机数字。

1.  玩家需要猜测这个数字。
