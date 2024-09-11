# NBA 分析使用 PySpark

> 原文：[https://towardsdatascience.com/nba-analytics-using-pyspark-a1699ae1117a?source=collection_archive---------17-----------------------#2023-04-14](https://towardsdatascience.com/nba-analytics-using-pyspark-a1699ae1117a?source=collection_archive---------17-----------------------#2023-04-14)

## 胜率，背靠背比赛的得分均值和标准差，更多内容请参见Python代码

[](https://jin-cui.medium.com/?source=post_page-----a1699ae1117a--------------------------------)[![Jin Cui](../Images/e5ddcbaa6d7da38f960d2c5fea71b538.png)](https://jin-cui.medium.com/?source=post_page-----a1699ae1117a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a1699ae1117a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a1699ae1117a--------------------------------) [Jin Cui](https://jin-cui.medium.com/?source=post_page-----a1699ae1117a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F333e9ae026bc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnba-analytics-using-pyspark-a1699ae1117a&user=Jin+Cui&userId=333e9ae026bc&source=post_page-333e9ae026bc----a1699ae1117a---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a1699ae1117a--------------------------------) · 阅读时长7分钟·2023年4月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa1699ae1117a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnba-analytics-using-pyspark-a1699ae1117a&user=Jin+Cui&userId=333e9ae026bc&source=-----a1699ae1117a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa1699ae1117a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnba-analytics-using-pyspark-a1699ae1117a&source=-----a1699ae1117a---------------------bookmark_footer-----------)![](../Images/0fb8acf97beb22f7a127864f2d599ba7.png)

照片由 [Emanuel Ekström](https://unsplash.com/@emanuelekstrom?utm_source=medium&utm_medium=referral) 提供，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 动机

一周前，我观看了一场密尔沃基雄鹿队与波士顿凯尔特人队的NBA比赛。这场比赛是联盟中排名前两的球队之间的对决，许多人认为这是东部决赛的前奏。作为一个篮球和NBA的狂热粉丝，这场比赛令我感到相当失望，因为密尔沃基雄鹿队以140–99输给了波士顿凯尔特人队，这对密尔沃基来说是一次罕见的惨败，而密尔沃基在2022–2023赛季拥有最佳（常规赛）战绩。

尽管这场比赛的结果对密尔沃基来说有些不寻常，尤其是考虑到这是在主场的惨败，但比赛的评论员提醒我，他们实际上是在打背靠背的比赛，也就是在前一天比赛之后进行的比赛（在这种情况下，就是前一天在印第安纳的比赛）。换句话说，疲劳可能在他们的失败中发挥了作用，因为打背靠背的比赛对运动员来说体力要求很高，这种情况可能因两场比赛之间的旅行（从印第安纳到密尔沃基）而加剧。

从球队的赛程来看，在一个赛季的80多场比赛中，NBA球队确实会有一些背靠背的比赛。你觉得…
