# 与 Pandas AI 一起跑步：对波士顿马拉松的探索

> 原文：[https://towardsdatascience.com/running-with-pandas-ai-an-exploration-of-the-boston-marathon-ad9516b34d8a?source=collection_archive---------6-----------------------#2023-05-22](https://towardsdatascience.com/running-with-pandas-ai-an-exploration-of-the-boston-marathon-ad9516b34d8a?source=collection_archive---------6-----------------------#2023-05-22)

## 穿越波士顿马拉松获胜者数据集，从起点到终点

[](https://sejaldua.medium.com/?source=post_page-----ad9516b34d8a--------------------------------)[![Sejal Dua](../Images/b9ec1f4907da5e6dfa1c922caa5b326d.png)](https://sejaldua.medium.com/?source=post_page-----ad9516b34d8a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ad9516b34d8a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ad9516b34d8a--------------------------------) [Sejal Dua](https://sejaldua.medium.com/?source=post_page-----ad9516b34d8a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe353ddb0c125&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frunning-with-pandas-ai-an-exploration-of-the-boston-marathon-ad9516b34d8a&user=Sejal+Dua&userId=e353ddb0c125&source=post_page-e353ddb0c125----ad9516b34d8a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ad9516b34d8a--------------------------------) ·9 min read·2023年5月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fad9516b34d8a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frunning-with-pandas-ai-an-exploration-of-the-boston-marathon-ad9516b34d8a&user=Sejal+Dua&userId=e353ddb0c125&source=-----ad9516b34d8a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fad9516b34d8a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frunning-with-pandas-ai-an-exploration-of-the-boston-marathon-ad9516b34d8a&source=-----ad9516b34d8a---------------------bookmark_footer-----------)![](../Images/d6458818dcf988949ccea46e6c5c796c.png)

图片由 [Miguel A Amutio](https://unsplash.com/@amutiomi?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/s/photos/boston-marathon?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

当我站在今年波士顿马拉松激动人心的氛围中，兴奋地为跑者加油时，我开始理解波士顿那几乎无法言喻的魔力。马拉松展示了凭借精神坚韧、纪律和决心，普通人也能完成非凡的壮举。更重要的是，马拉松是人类进步和潜力的庆典。波士顿作为世界上最古老的年度马拉松赛之一，拥有最具挑战性的赛道，并且在2013年爆炸事件后，也成为了力量和韧性的象征事件，因此我决定通过数据的视角来探索它。

我找到了一个包含波士顿马拉松获胜者（男女）及其获胜时间的[Kaggle 数据集](https://www.kaggle.com/datasets/zhikchen/boston-marathon-winners-men-and-women)。该数据集的列如下：

+   **年份 (int)**: 波士顿马拉松举办的年份以及获胜者获得称号的年份

+   **获胜者 (str)**: 那一年赢得马拉松的运动员（男性/女性）的名字

+   **国家 (str)**: 运动员代表或来自的国家

+   **时间 (time)**: the…
