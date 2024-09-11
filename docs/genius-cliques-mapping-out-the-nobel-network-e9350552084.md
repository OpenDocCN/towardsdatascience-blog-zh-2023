# 天才小圈子：揭示诺贝尔网络

> 原文：[https://towardsdatascience.com/genius-cliques-mapping-out-the-nobel-network-e9350552084?source=collection_archive---------3-----------------------#2023-09-27](https://towardsdatascience.com/genius-cliques-mapping-out-the-nobel-network-e9350552084?source=collection_archive---------3-----------------------#2023-09-27)

![](../Images/5d29b37b8e5320a0d54a5a0f91f9d638.png)

诺贝尔奖获得者网络。图片由作者提供。

[](https://medium.com/@janosovm?source=post_page-----e9350552084--------------------------------)[![Milan Janosov](../Images/77b62460041f66ec4585a81baef81a03.png)](https://medium.com/@janosovm?source=post_page-----e9350552084--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e9350552084--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e9350552084--------------------------------) [Milan Janosov](https://medium.com/@janosovm?source=post_page-----e9350552084--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F838408aa2ad4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenius-cliques-mapping-out-the-nobel-network-e9350552084&user=Milan+Janosov&userId=838408aa2ad4&source=post_page-838408aa2ad4----e9350552084---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e9350552084--------------------------------) ·阅读时长7分钟·2023年9月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe9350552084&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenius-cliques-mapping-out-the-nobel-network-e9350552084&user=Milan+Janosov&userId=838408aa2ad4&source=-----e9350552084---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe9350552084&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenius-cliques-mapping-out-the-nobel-network-e9350552084&source=-----e9350552084---------------------bookmark_footer-----------)

# **结合网络科学、数据可视化和维基百科，揭示所有诺贝尔奖获得者之间的隐藏联系。**

*这篇文章最初发表于数据可视化协会的印刷杂志《夜莺》第二区。*

# 诺贝尔圈内外的激励科学家

尽管我在网络和数据科学方面获得了博士学位，但我始终与我的根源保持紧密联系，特别是在寻找灵感时。我在匈牙利长大，对“火星人”这一群体的成就特别惊叹，他们是二战期间从匈牙利移居到美国的著名科学家。有趣的是，他们中的一些人甚至上过同一所高中。

火星人中包括了例如**利奥·西拉德**，他不仅发现了核链反应理论，还与**阿尔伯特·爱因斯坦**和**尤金·维格纳**共同申请了冰箱专利。维格纳是曼哈顿计划的关键科学家之一，领导了第一台核反应堆的发展。由于他的贡献，维格纳于1963年获得了诺贝尔物理学奖，这18个与匈牙利来源的思想家相关的诺贝尔奖中之一。
