# 决策分析和Python中的决策树 —— 奥克兰A队的案例

> 原文：[https://towardsdatascience.com/decision-analysis-and-trees-in-python-the-case-of-the-oakland-as-786d746cdfb2?source=collection_archive---------6-----------------------#2023-05-24](https://towardsdatascience.com/decision-analysis-and-trees-in-python-the-case-of-the-oakland-as-786d746cdfb2?source=collection_archive---------6-----------------------#2023-05-24)

## 在Python中使用决策树来获取对A队搬到拉斯维加斯的决策的洞察

[](https://medium.com/@gspmalloy?source=post_page-----786d746cdfb2--------------------------------)[![乔凡尼·马洛伊](../Images/e94218e244fab1af845c68e63e5706a1.png)](https://medium.com/@gspmalloy?source=post_page-----786d746cdfb2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----786d746cdfb2--------------------------------)[![走向数据科学](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----786d746cdfb2--------------------------------) [乔凡尼·马洛伊](https://medium.com/@gspmalloy?source=post_page-----786d746cdfb2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa0442a984e63&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecision-analysis-and-trees-in-python-the-case-of-the-oakland-as-786d746cdfb2&user=Giovanni+Malloy&userId=a0442a984e63&source=post_page-a0442a984e63----786d746cdfb2---------------------post_header-----------) 发表在[走向数据科学](https://towardsdatascience.com/?source=post_page-----786d746cdfb2--------------------------------) ·17 min 读·2023年5月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F786d746cdfb2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecision-analysis-and-trees-in-python-the-case-of-the-oakland-as-786d746cdfb2&user=Giovanni+Malloy&userId=a0442a984e63&source=-----786d746cdfb2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F786d746cdfb2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecision-analysis-and-trees-in-python-the-case-of-the-oakland-as-786d746cdfb2&source=-----786d746cdfb2---------------------bookmark_footer-----------)![](../Images/fda332094a2d44ad8e99df3197d1ac42.png)

照片由[里克·罗德里格斯](https://unsplash.com/@rickro?utm_source=medium&utm_medium=referral)提供[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

最近，奥克兰运动员棒球队的老板约翰·费舍尔宣布球队已在内华达州拉斯维加斯购买了近50英亩的土地。[1] 这将使奥克兰最后一支职业体育队的未来岌岌可危。在过去的5年里，奥克兰已经见证了金州勇士队（NBA）和拉斯维加斯突袭者队（NFL）离开，前往其他城市的新球场（尽管金州勇士队只是横跨湾区到旧金山）。尽管奥克兰运动员棒球队的决策过程对我来说仍然是个谜，但数据科学和决策分析的结合可以揭示约翰·费舍尔搬到拉斯维加斯的动机。

决策分析对所有数据科学家来说都很重要，因为它是概率和统计模型的高度技术性工作与业务决策之间的桥梁。了解业务决策的形成方式可以帮助我们在提供可操作的建议和发现时，框定我们的工作和向非技术观众展示我们的发现。运筹学与管理科学研究所（INFORMS）甚至拥有一个[完整的学会](https://connect.informs.org/das/home)……
