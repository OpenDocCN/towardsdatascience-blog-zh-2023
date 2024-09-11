# 图示比利·科根的网络：使用 Python 的 NetworkX 库分析和绘制社交关系 — 第 4 部分

> 原文：[https://towardsdatascience.com/graphing-billy-corgans-network-analyzing-and-mapping-social-relationships-with-python-s-networkx-724de1e431ac?source=collection_archive---------14-----------------------#2023-07-21](https://towardsdatascience.com/graphing-billy-corgans-network-analyzing-and-mapping-social-relationships-with-python-s-networkx-724de1e431ac?source=collection_archive---------14-----------------------#2023-07-21)

## 继续学习如何使用 NetworkX 和 Python 进行社交网络分析

[](https://christineegan42.medium.com/?source=post_page-----724de1e431ac--------------------------------)[![Christine Egan](../Images/d0a11bde52ceaa53d7162f2dd77c8041.png)](https://christineegan42.medium.com/?source=post_page-----724de1e431ac--------------------------------)[](https://towardsdatascience.com/?source=post_page-----724de1e431ac--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----724de1e431ac--------------------------------) [Christine Egan](https://christineegan42.medium.com/?source=post_page-----724de1e431ac--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e9b4d1cb38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgraphing-billy-corgans-network-analyzing-and-mapping-social-relationships-with-python-s-networkx-724de1e431ac&user=Christine+Egan&userId=8e9b4d1cb38&source=post_page-8e9b4d1cb38----724de1e431ac---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----724de1e431ac--------------------------------) ·11 分钟阅读·2023 年 7 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F724de1e431ac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgraphing-billy-corgans-network-analyzing-and-mapping-social-relationships-with-python-s-networkx-724de1e431ac&user=Christine+Egan&userId=8e9b4d1cb38&source=-----724de1e431ac---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F724de1e431ac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgraphing-billy-corgans-network-analyzing-and-mapping-social-relationships-with-python-s-networkx-724de1e431ac&source=-----724de1e431ac---------------------bookmark_footer-----------)

[在我们对比利·科根的影响范围进行调查的开端](https://medium.com/towards-data-science/visualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-efeb82ab853e)中，我们介绍了社交网络分析及基本概念，如节点和边。在 [第2部分](https://medium.com/towards-data-science/visualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-f4a9cf6b6d57)中，我们通过绘制乐队[Smashing Pumpkins](https://smashingpumpkins.com/)和[Zwan](https://en.wikipedia.org/wiki/Zwan)成员之间的关系，扩展了对社交网络分析的理解。接着，我们检视了度中心性和中介中心性等指标，以探讨不同乐队成员之间的关系。同时，我们讨论了领域知识如何帮助我们理解结果。

在[第3部分](https://medium.com/towards-data-science/closeness-and-communities-analyzing-social-networks-with-python-and-networkx-part-3-c19feeb38223)中，我们引入了第三个中心性度量，[接近中心性](https://neo4j.com/docs/graph-data-science/current/algorithms/closeness-centrality/)。我们还开始讨论社区和子群体的概念，并展示了不同的社区图以及如何利用接近中心性来指导我们的解释。利用Zwan和Smashing Pumpkins乐队成员的网络，我们对成员之间的关系进行了推断。

这一次，我们将通过扩展网络并添加额外的乐队来使结果更有趣。同时，我们将扩展对中心性度量和社区概念的理解，同时提升我们的Matplotlib技能，以便更好地制作…
