# 大学橄榄球会议重新调整——回归分析

> 原文：[`towardsdatascience.com/college-football-conference-realignment-regression-8f0776278d55?source=collection_archive---------7-----------------------#2023-08-06`](https://towardsdatascience.com/college-football-conference-realignment-regression-8f0776278d55?source=collection_archive---------7-----------------------#2023-08-06)

[](https://medium.com/@gspmalloy?source=post_page-----8f0776278d55--------------------------------)![Giovanni Malloy](https://medium.com/@gspmalloy?source=post_page-----8f0776278d55--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8f0776278d55--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8f0776278d55--------------------------------) [Giovanni Malloy](https://medium.com/@gspmalloy?source=post_page-----8f0776278d55--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa0442a984e63&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcollege-football-conference-realignment-regression-8f0776278d55&user=Giovanni+Malloy&userId=a0442a984e63&source=post_page-a0442a984e63----8f0776278d55---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8f0776278d55--------------------------------) · 7 分钟阅读 · 2023 年 8 月 6 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8f0776278d55&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcollege-football-conference-realignment-regression-8f0776278d55&user=Giovanni+Malloy&userId=a0442a984e63&source=-----8f0776278d55---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8f0776278d55&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcollege-football-conference-realignment-regression-8f0776278d55&source=-----8f0776278d55---------------------bookmark_footer-----------)

欢迎来到我系列文章的第二部分，主题是会议重新调整！去年夏天，当会议重新调整如火如荼时，Tony Altimore 在 Twitter 上发布了一项[研究](https://twitter.com/TJAltimore/status/1536438310809247745)，这激励了我进行自己的会议重新调整分析。该系列分为四个部分（完整的动机在第一部分中找到）：

1.  [大学橄榄球会议重新调整——Python 中的探索性数据分析](https://medium.com/towards-data-science/college-football-conference-realignment-exploratory-data-analysis-in-python-6f4a74037572)

1.  大学橄榄球会议重新调整——回归分析

1.  [大学橄榄球会议重组 — 聚类分析](https://medium.com/p/6ca16840ed3d#0733-ef9637a21b53)

1.  [大学橄榄球会议重组 — node2vec](https://medium.com/towards-data-science/college-football-conference-realignment-node2vec-ba2e931bb1c)

![](img/9ac3d6a3c19c4114817821937ef1c400.png)

照片由[诺伯特·布劳恩](https://unsplash.com/@medion4you?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

希望系列中的每一部分都能为你提供一个关于大学橄榄球这项受人喜爱的运动的全新视角。对于那些没有阅读第一部分的读者，简要总结是我创建了自己从网络各处收集的数据集。这些数据包括[每个 FBS 项目的基本信息](https://en.wikipedia.org/wiki/List_of_NCAA_Division_I_FBS_football_programs)、所有[大学橄榄球对抗赛](https://en.wikipedia.org/wiki/List_of_NCAA_college_football_rivalry_games)的非标准近似、[体育场容量](https://en.wikipedia.org/wiki/List_of_NCAA_Division_I_FBS_football_stadiums)、[历史表现](https://en.wikipedia.org/wiki/List_of_NCAA_Division_I_FBS_football_bowl_records)、[AP 前 25 名投票频率](https://collegepollarchive.com/football/index.cfm)、学校是否为[AAU](https://www.aau.edu/)或[R1](https://en.wikipedia.org/wiki/List_of_research_universities_in_the_United_States)机构（历史上对加入 Big Ten 和 Pac 12 很重要）、[NFL 选秀球员数量](https://www.ncaa.com/news/football/article/2020-04-27/college-football-teams-most-nfl-draft-picks-2000)、2017–2019 年的[项目收入数据](https://graphics.wsj.com/table/NCAA_2019)以及关于大学橄榄球粉丝群体规模的[最新估算](https://drive.google.com/file/d/1MiUOwx8X3H2bSkUOz8a1YhceyJWLLCoJ/view)。由于…
