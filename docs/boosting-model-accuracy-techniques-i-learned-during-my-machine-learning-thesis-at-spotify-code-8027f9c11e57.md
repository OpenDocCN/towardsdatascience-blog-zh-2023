# 提升模型准确性：我在 Spotify 机器学习论文中学到的技巧（+代码片段）

> 原文：[https://towardsdatascience.com/boosting-model-accuracy-techniques-i-learned-during-my-machine-learning-thesis-at-spotify-code-8027f9c11e57?source=collection_archive---------1-----------------------#2023-08-24](https://towardsdatascience.com/boosting-model-accuracy-techniques-i-learned-during-my-machine-learning-thesis-at-spotify-code-8027f9c11e57?source=collection_archive---------1-----------------------#2023-08-24)

## 提高顽固 ML 模型的技术数据科学家工具栈

[](https://medium.com/@elalamik?source=post_page-----8027f9c11e57--------------------------------)[![Khouloud El Alami](../Images/58840bfe28a60892b51d40ad6ba7f5e8.png)](https://medium.com/@elalamik?source=post_page-----8027f9c11e57--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8027f9c11e57--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8027f9c11e57--------------------------------) [Khouloud El Alami](https://medium.com/@elalamik?source=post_page-----8027f9c11e57--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c6a36490614&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fboosting-model-accuracy-techniques-i-learned-during-my-machine-learning-thesis-at-spotify-code-8027f9c11e57&user=Khouloud+El+Alami&userId=9c6a36490614&source=post_page-9c6a36490614----8027f9c11e57---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8027f9c11e57--------------------------------) ·12分钟阅读·2023年8月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8027f9c11e57&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fboosting-model-accuracy-techniques-i-learned-during-my-machine-learning-thesis-at-spotify-code-8027f9c11e57&user=Khouloud+El+Alami&userId=9c6a36490614&source=-----8027f9c11e57---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8027f9c11e57&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fboosting-model-accuracy-techniques-i-learned-during-my-machine-learning-thesis-at-spotify-code-8027f9c11e57&source=-----8027f9c11e57---------------------bookmark_footer-----------)

*这篇文章是记录我在 Spotify 机器学习论文中学到的知识的两部分文章之一。请务必查看* [*第二篇关于我在这项研究中如何实现特征重要性*](/feature-importance-analysis-with-shap-i-learned-at-spotify-aacd769831b4)*。*

[## 我在Spotify学到的SHAP特征重要性分析（在复仇者的帮助下）](https://towardsdatascience.com/feature-importance-analysis-with-shap-i-learned-at-spotify-aacd769831b4?source=post_page-----8027f9c11e57--------------------------------)

### 确定关键特征，并理解它们如何影响机器学习模型的预测结果，使用SHAP

[towardsdatascience.com](https://towardsdatascience.com/feature-importance-analysis-with-shap-i-learned-at-spotify-aacd769831b4?source=post_page-----8027f9c11e57--------------------------------)

在2021年，我花了8个月时间构建一个预测模型，以测量*用户满意度*，这是我在Spotify的论文的一部分。

![](../Images/2106c3fd9d7bd76cadf0157aecf85277.png)

图片来源于作者

我的目标是了解用户对音乐体验满意的原因。为此，我建立了一个LightGBM分类器，其输出是一个二元响应：

*y = 1 → 用户似乎满意

y = 0 → 不太满意*

预测人类满意度是一项挑战，因为人类本质上是不满足的。即使是机器也不太适合解读人类心理的奥秘。所以自然地，我的模型也尽可能地困惑。
