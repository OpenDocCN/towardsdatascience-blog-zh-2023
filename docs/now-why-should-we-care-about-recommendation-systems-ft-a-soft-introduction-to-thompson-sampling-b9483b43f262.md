# 现在，我们为什么应该关注推荐系统…？特别介绍一下**汤普森抽样**

> 原文：[https://towardsdatascience.com/now-why-should-we-care-about-recommendation-systems-ft-a-soft-introduction-to-thompson-sampling-b9483b43f262?source=collection_archive---------13-----------------------#2023-11-07](https://towardsdatascience.com/now-why-should-we-care-about-recommendation-systems-ft-a-soft-introduction-to-thompson-sampling-b9483b43f262?source=collection_archive---------13-----------------------#2023-11-07)

## 一个正在进行的推荐系统系列

[](https://irenechang1510.medium.com/?source=post_page-----b9483b43f262--------------------------------)[![Irene Chang](../Images/02280890ed87239c75cbcbfa7c5d686c.png)](https://irenechang1510.medium.com/?source=post_page-----b9483b43f262--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b9483b43f262--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b9483b43f262--------------------------------) [Irene Chang](https://irenechang1510.medium.com/?source=post_page-----b9483b43f262--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6d381bb66361&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnow-why-should-we-care-about-recommendation-systems-ft-a-soft-introduction-to-thompson-sampling-b9483b43f262&user=Irene+Chang&userId=6d381bb66361&source=post_page-6d381bb66361----b9483b43f262---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----b9483b43f262--------------------------------) ·12分钟阅读·2023年11月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb9483b43f262&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnow-why-should-we-care-about-recommendation-systems-ft-a-soft-introduction-to-thompson-sampling-b9483b43f262&user=Irene+Chang&userId=6d381bb66361&source=-----b9483b43f262---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb9483b43f262&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnow-why-should-we-care-about-recommendation-systems-ft-a-soft-introduction-to-thompson-sampling-b9483b43f262&source=-----b9483b43f262---------------------bookmark_footer-----------)![](../Images/a8dea9c6cebe66313b191c252f95b74c.png)

照片由[Myke Simon](https://unsplash.com/@myke_simon?utm_source=medium&utm_medium=referral)提供，拍摄于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

今天我又一次意识到，已经是第100...01天，我一边浏览Netflix寻找要观看的节目，一边拿着迟迟未开封的晚餐盒，一边吃着我的食物。我的推荐列表里充满了过多的亚洲浪漫剧和美国成长故事的建议，可能是基于我一个月前或两个月前看过的一两个系列。“这里没什么好看的……”——我在读完所有简介后叹了口气，感觉自己能够预测情节的发展。我掏出了另一个备用娱乐选项，Tiktok，同时下意识地想着，我可能需要*不感兴趣*一些视频，*喜欢*，*收藏*其他视频，以…推荐算法今天给我一些新的内容流。

推荐系统（RecSys）可以被认为是一种成熟的算法，它已经深深植根于我们的日常生活中，以至于在1到Chat-GPT的尺度上，它在学术界和非学术界几乎都像是80年代的趋势。然而，这绝不是一个近乎完美的算法。运营推荐系统时面临的伦理、社会、技术和法律挑战…
