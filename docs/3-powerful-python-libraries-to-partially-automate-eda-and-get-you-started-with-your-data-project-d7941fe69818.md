# 3个强大的Python库来（部分）自动化EDA，并帮助你启动数据项目

> 原文：[https://towardsdatascience.com/3-powerful-python-libraries-to-partially-automate-eda-and-get-you-started-with-your-data-project-d7941fe69818?source=collection_archive---------0-----------------------#2023-12-02](https://towardsdatascience.com/3-powerful-python-libraries-to-partially-automate-eda-and-get-you-started-with-your-data-project-d7941fe69818?source=collection_archive---------0-----------------------#2023-12-02)

## 所有的机器学习问题都是数据问题。

[](https://medium.com/@juanjosemunozp?source=post_page-----d7941fe69818--------------------------------)[![Juan Jose Munoz](../Images/b42d72e9e2a2eaf11da5465e9b041d53.png)](https://medium.com/@juanjosemunozp?source=post_page-----d7941fe69818--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d7941fe69818--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d7941fe69818--------------------------------) [Juan Jose Munoz](https://medium.com/@juanjosemunozp?source=post_page-----d7941fe69818--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc4fd0e1cff25&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-powerful-python-libraries-to-partially-automate-eda-and-get-you-started-with-your-data-project-d7941fe69818&user=Juan+Jose+Munoz&userId=c4fd0e1cff25&source=post_page-c4fd0e1cff25----d7941fe69818---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d7941fe69818--------------------------------) ·4 min read·2023年12月2日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd7941fe69818&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-powerful-python-libraries-to-partially-automate-eda-and-get-you-started-with-your-data-project-d7941fe69818&user=Juan+Jose+Munoz&userId=c4fd0e1cff25&source=-----d7941fe69818---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd7941fe69818&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-powerful-python-libraries-to-partially-automate-eda-and-get-you-started-with-your-data-project-d7941fe69818&source=-----d7941fe69818---------------------bookmark_footer-----------)

**为了避免“垃圾进，垃圾出”的老话，花费相当的时间理解和清理数据是很有必要的**。我最近读了Konrad Banachewicz和Luca Massaron的《Kaggle书》，其中采访了许多Kaggle大师。有趣的是，匆忙或跳过EDA是他们和初学者最常犯的错误。

![](../Images/70fdaf03bcbe1a9f1816719ad668c270.png)

图片来源：[Choong Deng Xiang](https://unsplash.com/@dengxiangs?utm_source=medium&utm_medium=referral) 上传于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**我们都知道EDA的重要性，但我们仍然跳过这个步骤**。这可能是因为很难知道从哪里开始，应该问什么问题，或者我们可能过于急于投入建模。

**以下是3个Python库，你可以用来部分自动化你的探索性数据分析，并开始你的数据项目。**

*以下分析的数据来源于Kaggle的房价——高级回归技术竞赛。*

# YData Profiling

这是新的Pandas分析版本，支持Spark，并且现在超越了仅仅支持Pandas DataFrame的范围。

**然而，最终目标保持不变：提供一个一行的探索性数据分析（EDA）体验。** 这个包…
