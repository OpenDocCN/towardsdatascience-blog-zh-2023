# 利用LLMs完善推荐知识图谱

> 原文：[https://towardsdatascience.com/augmenting-intelligence-leveraging-llms-to-complete-recommendation-knowledge-graphs-a0585e311d3f?source=collection_archive---------2-----------------------#2023-11-06](https://towardsdatascience.com/augmenting-intelligence-leveraging-llms-to-complete-recommendation-knowledge-graphs-a0585e311d3f?source=collection_archive---------2-----------------------#2023-11-06)

[](https://medium.com/@alcarazanthony1?source=post_page-----a0585e311d3f--------------------------------)[![安东尼·阿尔卡拉斯](../Images/6a71a1752677bd07c384246fb0c7f7e8.png)](https://medium.com/@alcarazanthony1?source=post_page-----a0585e311d3f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a0585e311d3f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a0585e311d3f--------------------------------) [安东尼·阿尔卡拉斯](https://medium.com/@alcarazanthony1?source=post_page-----a0585e311d3f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F30bc9ffd2f4b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faugmenting-intelligence-leveraging-llms-to-complete-recommendation-knowledge-graphs-a0585e311d3f&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=post_page-30bc9ffd2f4b----a0585e311d3f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a0585e311d3f--------------------------------) ·6分钟阅读·2023年11月6日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa0585e311d3f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faugmenting-intelligence-leveraging-llms-to-complete-recommendation-knowledge-graphs-a0585e311d3f&source=-----a0585e311d3f---------------------bookmark_footer-----------)

*本文文本的语法、流畅性和可读性由人工智能软件进行了增强。*

随着互联网和在线平台的迅猛增长，用户面临着大量选择。推荐系统在通过预测用户偏好和建议相关内容来突破信息过载方面变得至关重要。然而，提供准确和个性化的推荐仍然是一个持续的挑战。

问题的核心在于通过建模用户行为来理解用户的真实兴趣和意图。推荐系统依赖于从用户数据中获得的模式，例如浏览历史、购买记录、评分和互动。但现实中的用户数据往往稀疏且有限，缺乏捕捉用户意图细微差别所需的关键上下文信号。

因此，推荐模型无法学习全面的用户和项目表示。它们的建议最终变得过于泛泛、重复或不相关。冷启动问题使得新用户活动历史有限的情况更加复杂。企业也因客户体验不佳而遭受收入损失。

这需要能够从用户数据中挖掘更深入见解的解决方案。一种新兴的方法是使用知识图谱，它 encapsulate 事实和实体之间的连接。构建良好的知识图谱在解决推荐系统中的关键挑战方面具有巨大的潜力。
