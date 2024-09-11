# 使用符号回归为 Elo 著名的评分系统添加不确定性

> 原文：[`towardsdatascience.com/using-symbolic-regression-to-add-uncertainty-to-elos-famous-ratings-system-796d54f2b478?source=collection_archive---------16-----------------------#2023-07-06`](https://towardsdatascience.com/using-symbolic-regression-to-add-uncertainty-to-elos-famous-ratings-system-796d54f2b478?source=collection_archive---------16-----------------------#2023-07-06)

## 以及创建一个意想不到有用的评分算法

[](https://medium.com/@BlakeAtkinson?source=post_page-----796d54f2b478--------------------------------)![Blake Atkinson](https://medium.com/@BlakeAtkinson?source=post_page-----796d54f2b478--------------------------------)[](https://towardsdatascience.com/?source=post_page-----796d54f2b478--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----796d54f2b478--------------------------------) [Blake Atkinson](https://medium.com/@BlakeAtkinson?source=post_page-----796d54f2b478--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6dc08bbba014&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-symbolic-regression-to-add-uncertainty-to-elos-famous-ratings-system-796d54f2b478&user=Blake+Atkinson&userId=6dc08bbba014&source=post_page-6dc08bbba014----796d54f2b478---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----796d54f2b478--------------------------------) ·12 分钟阅读·2023 年 7 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F796d54f2b478&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-symbolic-regression-to-add-uncertainty-to-elos-famous-ratings-system-796d54f2b478&user=Blake+Atkinson&userId=6dc08bbba014&source=-----796d54f2b478---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F796d54f2b478&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-symbolic-regression-to-add-uncertainty-to-elos-famous-ratings-system-796d54f2b478&source=-----796d54f2b478---------------------bookmark_footer-----------)![](img/2873d9a5d01ad773ef96a85f7f528359.png)

图片由[JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，来自[Unsplash](https://unsplash.com/s/photos/video-games?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 一种通用评分系统

Elo 等级系统在一些场合变得非常有名。也许最著名的是，它自 1960 年代以来一直是国际象棋等级评定的基础。此外，网站 538 成功地对其进行了修改，用于他们大多数知名的体育评级。更少为人知的是，许多视频游戏开发者在他们的匹配系统后台使用了 Elo 系统的变种。如果你正在阅读这篇文章，我会假设你对这个系统有一些了解。它为什么在这么多场合被广泛使用？我认为是因为它的计算扩展性、通用性和简单性。然而，它也有一些缺点。在本文中，我们将解决一个非常关键的问题，同时保持上述优点。

# 符号回归

尽管大型语言模型目前备受关注（开个玩笑），但还有其他令人兴奋的模型正在独立开发，它们有着截然不同的应用场景。符号回归通常更适合发现封闭形式的分析规则，而不是处理像图像分类或音频翻译这样的深度学习任务……
