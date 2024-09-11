# 在Spotify学到的SHAP特征重要性分析（在复仇者的帮助下）

> 原文：[https://towardsdatascience.com/feature-importance-analysis-with-shap-i-learned-at-spotify-aacd769831b4?source=collection_archive---------0-----------------------#2023-08-23](https://towardsdatascience.com/feature-importance-analysis-with-shap-i-learned-at-spotify-aacd769831b4?source=collection_archive---------0-----------------------#2023-08-23)

## 识别顶级特征并了解它们如何影响机器学习模型的预测结果，使用SHAP

[](https://medium.com/@elalamik?source=post_page-----aacd769831b4--------------------------------)[![Khouloud El Alami](../Images/58840bfe28a60892b51d40ad6ba7f5e8.png)](https://medium.com/@elalamik?source=post_page-----aacd769831b4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----aacd769831b4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----aacd769831b4--------------------------------) [Khouloud El Alami](https://medium.com/@elalamik?source=post_page-----aacd769831b4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c6a36490614&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffeature-importance-analysis-with-shap-i-learned-at-spotify-aacd769831b4&user=Khouloud+El+Alami&userId=9c6a36490614&source=post_page-9c6a36490614----aacd769831b4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----aacd769831b4--------------------------------) · 13分钟阅读 · 2023年8月23日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Faacd769831b4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffeature-importance-analysis-with-shap-i-learned-at-spotify-aacd769831b4&user=Khouloud+El+Alami&userId=9c6a36490614&source=-----aacd769831b4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faacd769831b4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffeature-importance-analysis-with-shap-i-learned-at-spotify-aacd769831b4&source=-----aacd769831b4---------------------bookmark_footer-----------)

*本文是两部分文章中的一部分，记录了我在Spotify机器学习论文中的学习经历。请务必查看第二篇文章，了解我如何成功显著优化模型以进行这项研究。*

[](/boosting-model-accuracy-techniques-i-learned-during-my-machine-learning-thesis-at-spotify-code-8027f9c11e57?source=post_page-----aacd769831b4--------------------------------) [## 提升模型准确性：我在Spotify机器学习论文中学到的技术（附代码…

### 一个技术数据科学家用来改进顽固机器学习模型的工具栈

[towardsdatascience.com](/boosting-model-accuracy-techniques-i-learned-during-my-machine-learning-thesis-at-spotify-code-8027f9c11e57?source=post_page-----aacd769831b4--------------------------------)

两年前，我在Spotify进行了一项引人入胜的研究项目，作为我的硕士论文的一部分。我学习了几种有用的机器学习技术，我认为任何数据科学家都应该将其纳入工具箱。今天，我在这里为你介绍其中之一。

在那段时间，我花了6个月试图建立一个预测模型，然后解读其内部运作。*我的目标是理解是什么让用户对他们的音乐体验感到满意*。

这不仅仅是预测用户是否开心，而是理解那些*潜在*的因素，它们促成了用户的幸福（或缺乏幸福）。
