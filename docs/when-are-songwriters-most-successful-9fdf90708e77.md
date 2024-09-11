# 歌词创作歌手在何时最成功？

> 原文：[https://towardsdatascience.com/when-are-songwriters-most-successful-9fdf90708e77?source=collection_archive---------11-----------------------#2023-08-22](https://towardsdatascience.com/when-are-songwriters-most-successful-9fdf90708e77?source=collection_archive---------11-----------------------#2023-08-22)

## 让我们通过 KDE 图来了解一下

[](https://medium.com/@lee_vaughan?source=post_page-----9fdf90708e77--------------------------------)[![Lee Vaughan](../Images/9f6b90bb76102f438ab0b9a4a62ffa3f.png)](https://medium.com/@lee_vaughan?source=post_page-----9fdf90708e77--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9fdf90708e77--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9fdf90708e77--------------------------------) [Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----9fdf90708e77--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5d604015c08b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-are-songwriters-most-successful-9fdf90708e77&user=Lee+Vaughan&userId=5d604015c08b&source=post_page-5d604015c08b----9fdf90708e77---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9fdf90708e77--------------------------------) ·11 min read·2023年8月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9fdf90708e77&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-are-songwriters-most-successful-9fdf90708e77&user=Lee+Vaughan&userId=5d604015c08b&source=-----9fdf90708e77---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9fdf90708e77&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-are-songwriters-most-successful-9fdf90708e77&source=-----9fdf90708e77---------------------bookmark_footer-----------)![](../Images/91cfa91f27be316505c84cd95be8b058.png)

一位歌手在五彩纸屑飞舞中领取格莱美奖（由 DALL-E2 协助创作）

歌词创作歌手在什么年龄段最成功？我前几天听到一首老的史蒂维·旺德的歌曲时想到了这个问题。我的*印象*是，像数学家一样，歌词创作歌手在二十多岁中后期达到巅峰。但*数据*显示了什么呢？

在这个*快速成功数据科学*项目中，我们将使用 Python、pandas 和 Seaborn 绘图库来调查这个问题。我们将查看16位知名歌词创作歌手的职业生涯，他们总共拥有超过500首热门歌曲。我们还将将一个称为*核密度估计*的图形纳入分析中。

# 方法论

为了确定词曲作者最成功的时期，我们需要一些指导方针。计划是检查：

+   **包括那些与合作者合作的创作歌手。**

+   **有着数十年职业生涯的创作歌手。**

+   **各种创作歌手和音乐流派的多样选择。**

+   **登上了*Billboard Hot 100*排行榜的创作歌手。**

[*Hot 100*](https://www.billboard.com/charts/hot-100/)是由*Billboard*杂志每周发布的排行榜，排名依据美国最受欢迎的歌曲。排名基于实体和数字销售、广播播放和在线流媒体。我们将以此作为…
