# 映射全球自然再造林项目的潜力

> 原文：[https://towardsdatascience.com/mapping-the-global-potential-of-natural-reforestation-projects-c425d2998fa5?source=collection_archive---------7-----------------------#2023-08-21](https://towardsdatascience.com/mapping-the-global-potential-of-natural-reforestation-projects-c425d2998fa5?source=collection_archive---------7-----------------------#2023-08-21)

## 使用地面观察、遥感和机器学习

[](https://steve-klosterman.medium.com/?source=post_page-----c425d2998fa5--------------------------------)[![Steve Klosterman](../Images/4a7e2ebb41964275bac5f97372df36e3.png)](https://steve-klosterman.medium.com/?source=post_page-----c425d2998fa5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c425d2998fa5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c425d2998fa5--------------------------------) [Steve Klosterman](https://steve-klosterman.medium.com/?source=post_page-----c425d2998fa5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F20266e1c64db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmapping-the-global-potential-of-natural-reforestation-projects-c425d2998fa5&user=Steve+Klosterman&userId=20266e1c64db&source=post_page-20266e1c64db----c425d2998fa5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c425d2998fa5--------------------------------) ·11分钟阅读·2023年8月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc425d2998fa5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmapping-the-global-potential-of-natural-reforestation-projects-c425d2998fa5&user=Steve+Klosterman&userId=20266e1c64db&source=-----c425d2998fa5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc425d2998fa5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmapping-the-global-potential-of-natural-reforestation-projects-c425d2998fa5&source=-----c425d2998fa5---------------------bookmark_footer-----------)

由Stephen Klosterman及Earthshot科学团队撰写。内容最初呈现于[2022年12月美国地球物理联盟秋季会议](https://agu.confex.com/agu/fm22/meetingapp.cgi/Paper/1185690)。

# 引言

生态修复项目通常需要投资以启动活动。为了为森林增长和保护项目创造碳金融机会，必须能够预测木质生物质中碳的积累量，或者在防止森林砍伐的情况下预测避免的排放量。这还包括试图理解其他各种生态系统特性的可能变化，例如植物和动物物种组成以及水质。为了创建碳积累预测，一种常见的方法是将个别注意力和研究精力投入到特定地点的项目中，这些地点可能分布在全球各地。因此，拥有一个本地准确且全球范围的增长率或其他感兴趣的参数值地图，将有助于快速“勘探”生态系统修复机会。本文描述了创建这样一张地图的方法，该方法源自于一个在先前发布的文献综述数据上训练的机器学习模型。[我们接着演示了在Google Earth Engine应用中对非洲的地图实施](https://steve-klosterman.users.earthengine.app/view/africaglobalunrmodel)。

# 数据和方法
