# 利用因果机器学习识别 Spotify 歌曲流行的驱动因素

> 原文：[https://towardsdatascience.com/identifying-drivers-of-spotify-song-popularity-with-causal-ml-934e8347d2aa?source=collection_archive---------5-----------------------#2023-02-01](https://towardsdatascience.com/identifying-drivers-of-spotify-song-popularity-with-causal-ml-934e8347d2aa?source=collection_archive---------5-----------------------#2023-02-01)

## 什么使一首歌“成功”？

[](https://medium.com/@aashishnair?source=post_page-----934e8347d2aa--------------------------------)[![Aashish Nair](../Images/23f4b3839e464419332b690a4098d824.png)](https://medium.com/@aashishnair?source=post_page-----934e8347d2aa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----934e8347d2aa--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----934e8347d2aa--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----934e8347d2aa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3087ba81e065&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fidentifying-drivers-of-spotify-song-popularity-with-causal-ml-934e8347d2aa&user=Aashish+Nair&userId=3087ba81e065&source=post_page-3087ba81e065----934e8347d2aa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----934e8347d2aa--------------------------------) ·10 min 阅读·2023年2月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F934e8347d2aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fidentifying-drivers-of-spotify-song-popularity-with-causal-ml-934e8347d2aa&user=Aashish+Nair&userId=3087ba81e065&source=-----934e8347d2aa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F934e8347d2aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fidentifying-drivers-of-spotify-song-popularity-with-causal-ml-934e8347d2aa&source=-----934e8347d2aa---------------------bookmark_footer-----------)![](../Images/3e158bbe5c7a9493fddc127a1acad384.png)

图片来源：[C D-X](https://unsplash.com/de/@cdx2?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 目录

∘ [介绍](#0a12)

∘ [问题陈述](#0d78)

∘ [为什么选择因果机器学习？](#b34e)

∘ [数据收集](#fa56)

∘ [探索性数据分析（EDA）](#ad8f)

∘ [数据建模](#1a29)

∘ [模型解释](#26c0)

∘ [构建 Web 应用](#a2ee)

∘ [限制](#8f75)

∘ [结论](#6379)

## 介绍

什么使一首歌成功？当艺术家高音演唱或朗诵引人深思的诗句时，很容易为你对一首歌的喜爱找到理由。仅仅因为一首歌是由你最喜欢的艺术家演唱也很容易喜欢这首歌。然而，这还不足以解释当前的音乐格局。在这个饱和的市场中，尽管有无数的曲目具有相似的声音、风格和类型，但有些曲目就是比其他曲目表现得更好。

这就引出了一个问题：是否存在更多隐藏/潜在的音频因素影响我们对某些曲目的偏好？这个项目试图通过利用因果机器学习来构建一个工具，以帮助识别Spotify歌曲受欢迎的潜在驱动因素。
