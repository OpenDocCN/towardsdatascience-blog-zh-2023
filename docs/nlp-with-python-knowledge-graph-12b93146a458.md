# 使用Python的自然语言处理：知识图谱

> 原文：[https://towardsdatascience.com/nlp-with-python-knowledge-graph-12b93146a458?source=collection_archive---------1-----------------------#2023-04-19](https://towardsdatascience.com/nlp-with-python-knowledge-graph-12b93146a458?source=collection_archive---------1-----------------------#2023-04-19)

## SpaCy，句子分割，词性标注，D**依存解析，**命名实体识别，等等……

[](https://maurodp.medium.com/?source=post_page-----12b93146a458--------------------------------)[![毛罗·迪·皮埃特罗](../Images/3586d9d3238d904a1e1fa39c77b59d3f.png)](https://maurodp.medium.com/?source=post_page-----12b93146a458--------------------------------)[](https://towardsdatascience.com/?source=post_page-----12b93146a458--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----12b93146a458--------------------------------) [毛罗·迪·皮埃特罗](https://maurodp.medium.com/?source=post_page-----12b93146a458--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F44a176cd070a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnlp-with-python-knowledge-graph-12b93146a458&user=Mauro+Di+Pietro&userId=44a176cd070a&source=post_page-44a176cd070a----12b93146a458---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----12b93146a458--------------------------------) ·14分钟阅读·2023年4月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F12b93146a458&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnlp-with-python-knowledge-graph-12b93146a458&user=Mauro+Di+Pietro&userId=44a176cd070a&source=-----12b93146a458---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F12b93146a458&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnlp-with-python-knowledge-graph-12b93146a458&source=-----12b93146a458---------------------bookmark_footer-----------)

## 摘要

在本文中，我将展示如何使用Python和自然语言处理构建知识图谱。

![](../Images/321c5d15bba2bcd22a3328c17f2827ac.png)

照片由[莫里茨·金德勒](https://unsplash.com/@moritz_photography?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上。

一个[**网络图**](https://en.wikipedia.org/wiki/Graph_theory)是一个数学结构，用于展示点之间的关系，可以通过无向图/有向图结构可视化。这是一种将连接节点映射到数据库的形式。

一个[**知识库**](https://en.wikipedia.org/wiki/Knowledge_base)是来自不同来源的信息的统一存储库，比如*维基百科*。

一个 [**知识图谱**](https://en.wikipedia.org/wiki/Knowledge_graph) 是一种使用图结构数据模型的知识库。简单来说，它是一种特定类型的网络图，展示了现实世界实体、事实、概念和事件之间的定性关系。*Google* 在2012年首次使用了“知识图谱”这一术语，以介绍 [他们的模型](https://en.wikipedia.org/wiki/Google_Knowledge_Graph)。

![](../Images/1c6b4397f08ef1f66cf0e7624bae5507.png)

图片来自作者

目前，大多数公司正在构建 [数据湖](https://en.wikipedia.org/wiki/Data_lake)，一个中央数据库，用于存储从不同来源获取的各种原始数据（即结构化和非结构化数据）……
