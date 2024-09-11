# 马尔可夫不等式和比内–切比雪夫不等式

> 原文：[https://towardsdatascience.com/the-markov-and-the-bienaym%C3%A9-chebyshev-inequalities-cbf7ccc856f9?source=collection_archive---------3-----------------------#2023-09-12](https://towardsdatascience.com/the-markov-and-the-bienaym%C3%A9-chebyshev-inequalities-cbf7ccc856f9?source=collection_archive---------3-----------------------#2023-09-12)

![](../Images/58636f18625abacc1e9b66e680a0d327.png)

“拉巴耶尔的克利希防线。1814年3月30日巴黎防御” (**The Clichy Barrier. Defense of Paris, March 30, 1814**)（画家：[霍拉斯·费尔内](https://en.wikipedia.org/wiki/en:Horace_Vernet)）([公共领域](https://commons.wikimedia.org/wiki/File:La_Barri%C3%A8re_de_Clichy._D%C3%A9fense_de_Paris,_le_30_mars_1814_-_Horace_Vernet_-_Mus%C3%A9e_du_Louvre_Peintures_RF_126.jpg) 画作)

## 深入探讨这两个界限的意义以及导致它们发现的非凡事件系列

[](https://timeseriesreasoning.medium.com/?source=post_page-----cbf7ccc856f9--------------------------------)[![Sachin Date](../Images/bd023298b414caf88f79b00ef032d065.png)](https://timeseriesreasoning.medium.com/?source=post_page-----cbf7ccc856f9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cbf7ccc856f9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cbf7ccc856f9--------------------------------) [Sachin Date](https://timeseriesreasoning.medium.com/?source=post_page-----cbf7ccc856f9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb75b5b1730f3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-markov-and-the-bienaym%C3%A9-chebyshev-inequalities-cbf7ccc856f9&user=Sachin+Date&userId=b75b5b1730f3&source=post_page-b75b5b1730f3----cbf7ccc856f9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cbf7ccc856f9--------------------------------) ·18分钟阅读·2023年9月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcbf7ccc856f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-markov-and-the-bienaym%25C3%25A9-chebyshev-inequalities-cbf7ccc856f9&user=Sachin+Date&userId=b75b5b1730f3&source=-----cbf7ccc856f9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcbf7ccc856f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-markov-and-the-bienaym%25C3%25A9-chebyshev-inequalities-cbf7ccc856f9&source=-----cbf7ccc856f9---------------------bookmark_footer-----------)

宇宙并不常常告诉你某些事情根本无法完成。不管你多聪明，资金多雄厚，或是你在宇宙的哪个角落，当宇宙说“不可行”时，没有任何办法绕过它。在科学中，这种不可能性通常以某些量的值的限制来表达。一个著名的例子是爱因斯坦在1905年发现的，当你让光子在真空中自由运动时，没有任何东西——毫无疑问，没有任何东西——可以超越它。已经发现并证明了数百个这样的限制或界限。它们共同形成了现实的围栏。

马尔可夫不等式和比涅–切比雪夫不等式是两个深刻地影响了我们对自然界如何限制随机事件发生频率的理解的界限。

马尔可夫不等式的发现和证明归功于**安德烈·安德烈耶维奇·马尔可夫**（1856–1922），这位才华横溢、充满激情的俄罗斯数学家。
