# 到1还是到0：图像分类中的像素攻击

> 原文：[https://towardsdatascience.com/to-1-or-to-0-pixel-attacks-in-image-classification-ec323555a11a?source=collection_archive---------8-----------------------#2023-11-23](https://towardsdatascience.com/to-1-or-to-0-pixel-attacks-in-image-classification-ec323555a11a?source=collection_archive---------8-----------------------#2023-11-23)

## 探索对抗性机器学习的领域

[](https://medium.com/@MahamsMultiverse?source=post_page-----ec323555a11a--------------------------------)[![马哈姆·哈鲁恩](../Images/5a9ac82369ecbf7719b765ec160a70ef.png)](https://medium.com/@MahamsMultiverse?source=post_page-----ec323555a11a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ec323555a11a--------------------------------)[![走向数据科学](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ec323555a11a--------------------------------) [马哈姆·哈鲁恩](https://medium.com/@MahamsMultiverse?source=post_page-----ec323555a11a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F398c9514a58b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fto-1-or-to-0-pixel-attacks-in-image-classification-ec323555a11a&user=Maham+Haroon&userId=398c9514a58b&source=post_page-398c9514a58b----ec323555a11a---------------------post_header-----------) 发表在 [走向数据科学](https://towardsdatascience.com/?source=post_page-----ec323555a11a--------------------------------) · 13分钟阅读 · 2023年11月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fec323555a11a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fto-1-or-to-0-pixel-attacks-in-image-classification-ec323555a11a&user=Maham+Haroon&userId=398c9514a58b&source=-----ec323555a11a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fec323555a11a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fto-1-or-to-0-pixel-attacks-in-image-classification-ec323555a11a&source=-----ec323555a11a---------------------bookmark_footer-----------)![](../Images/5969ea7ddc81cf61597e791f7e4a79c5.png)

图片由 [皮耶特罗·詹](https://unsplash.com/@pietrozj?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，来源于 [Unsplash](https://unsplash.com/photos/green-and-red-light-wallpaper-n6B49lTx7NM?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

嗨，大家好！

今年，我参加了我第一次由 AI Village @ DEFCON 31 举办的[夺旗赛 (CTF) 比赛](https://www.kaggle.com/competitions/ai-village-capture-the-flag-defcon31/overview)，经历可以说相当引人入胜。特别是涉及像素攻击的挑战引起了我的注意，并成为了这篇文章的主要焦点。虽然我最初打算分享一个简单版本的像素攻击案例，但这篇文章的目标还包括深入探讨如何加强 ML 模型，以更好地抵御像比赛中遇到的像素攻击。

在我们深入理论之前，让我们通过一个能引起你注意的场景来设定背景。

想象一下：我们的公司 MM Vigilant 正在致力于开发一款前沿的物体检测产品。这个概念既简单又具有革命性——客户拍摄所需物品的照片，然后几天后物品就会送到他们家门口。作为幕后的**数据科学家**，你打造了终极的基于图像的物体分类模型。分类结果无可挑剔，模型评估指标一流，利益相关者也非常满意。模型投入生产，客户们都很高兴——直到一波……
