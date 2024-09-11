# 当1+1≠2：量子物理如何打破统计学法则

> 原文：[https://towardsdatascience.com/how-quantum-physics-broke-the-laws-of-statistics-86fb8941ed2c?source=collection_archive---------2-----------------------#2023-05-03](https://towardsdatascience.com/how-quantum-physics-broke-the-laws-of-statistics-86fb8941ed2c?source=collection_archive---------2-----------------------#2023-05-03)

## 揭开2022年物理学诺贝尔奖背后的数据科学谜团

[](https://tim-lou.medium.com/?source=post_page-----86fb8941ed2c--------------------------------)[![Tim Lou, PhD](../Images/e4931bb6d59e27730529ceaf00a23822.png)](https://tim-lou.medium.com/?source=post_page-----86fb8941ed2c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----86fb8941ed2c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----86fb8941ed2c--------------------------------) [Tim Lou, PhD](https://tim-lou.medium.com/?source=post_page-----86fb8941ed2c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8d41b438feef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-quantum-physics-broke-the-laws-of-statistics-86fb8941ed2c&user=Tim+Lou%2C+PhD&userId=8d41b438feef&source=post_page-8d41b438feef----86fb8941ed2c---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----86fb8941ed2c--------------------------------) ·12分钟阅读·2023年5月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F86fb8941ed2c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-quantum-physics-broke-the-laws-of-statistics-86fb8941ed2c&user=Tim+Lou%2C+PhD&userId=8d41b438feef&source=-----86fb8941ed2c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F86fb8941ed2c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-quantum-physics-broke-the-laws-of-statistics-86fb8941ed2c&source=-----86fb8941ed2c---------------------bookmark_footer-----------)

统计学是数据科学的核心支柱，但其假设并非总是经过充分检验。这在量子计算的兴起中尤为严重，甚至统计学公理也可能被违反。在本文中，我们探讨了量子物理如何打破统计学，并揭示了使用数据科学类比来理解这一现象的方法。

![](../Images/40fd156b4ea851346d5ed01ce5e16f8a.png)

2022年的物理学诺贝尔奖涉及到抛硬币，并发现量子物理违反了统计学的基本法则（背景：[Dan Dennis](https://unsplash.com/@cameramandan83)，硬币3D模型：[hyperionforge](https://sketchfab.com/hyperionforge)，组合由作者提供）

让我们玩一个掷币游戏：掷三枚硬币，尽量让它们都落在不同的面上。这看似是不可能的任务，因为无论硬币如何设计，它只能有两面。三次掷币都落在不同面上的可能性实在太少。

然而，凭借量子物理的力量，这样一个看似不可能的壮举在统计上是可以实现的：三次掷币都可以落在不同的面上。那么获胜的奖励是什么呢？那就是2022年的诺贝尔物理学奖，授予了**阿兰·阿斯佩克特**、**约翰·克劳瑟**和**安东·蔡林格**，颁奖日期为2022年10月4日。

根据 [nobelprize.org](https://www.nobelprize.org/prizes/physics/2022/press-release/)，他们的成就

> *“是因为他们在纠缠光子实验中，确立了贝尔不等式的违背，并开创了量子信息科学。”*
