# 理解预测性维护 — 波数据：特征工程（第二部分）

> 原文：[https://towardsdatascience.com/understanding-predictive-maintenance-wave-data-feature-engineering-part-2-spectral-3eced3bdbb3e?source=collection_archive---------13-----------------------#2023-12-01](https://towardsdatascience.com/understanding-predictive-maintenance-wave-data-feature-engineering-part-2-spectral-3eced3bdbb3e?source=collection_archive---------13-----------------------#2023-12-01)

## 频谱数据的特征工程

[](https://marcin-staskopl.medium.com/?source=post_page-----3eced3bdbb3e--------------------------------)[![Marcin Stasko](../Images/5142b9a260a1cce7c6a2ebcc16f46fbb.png)](https://marcin-staskopl.medium.com/?source=post_page-----3eced3bdbb3e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3eced3bdbb3e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3eced3bdbb3e--------------------------------) [Marcin Stasko](https://marcin-staskopl.medium.com/?source=post_page-----3eced3bdbb3e--------------------------------)

·

[跟随链接](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd8736adba55&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-predictive-maintenance-wave-data-feature-engineering-part-2-spectral-3eced3bdbb3e&user=Marcin+Stasko&userId=d8736adba55&source=post_page-d8736adba55----3eced3bdbb3e---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----3eced3bdbb3e--------------------------------) ·12 分钟阅读·2023 年 12 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3eced3bdbb3e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-predictive-maintenance-wave-data-feature-engineering-part-2-spectral-3eced3bdbb3e&user=Marcin+Stasko&userId=d8736adba55&source=-----3eced3bdbb3e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3eced3bdbb3e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-predictive-maintenance-wave-data-feature-engineering-part-2-spectral-3eced3bdbb3e&source=-----3eced3bdbb3e---------------------bookmark_footer-----------)![](../Images/f135b31634608fd3f588fd8593f13524.png)

[Evie S.](https://unsplash.com/@evieshaffer?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上的照片

# 文章目的

这是关于波数据特征工程文章的第二部分。我们将专注于频谱特征。有什么想法可以补充吗？欢迎分享！

本文是系列文章《理解预测性维护》的一部分。

[查看完整系列文章](https://marcin-staskopl.medium.com/list/understanding-predictive-maintenance-series-e1f44d8a0cc3)。确保通过关注我来获取最新文章。所有没有说明的图片均由我创作。

# 频域特征

在转换到频域时，我们采用如 `快速傅里叶变换 (FFT)` 等技术来转换时域信号。提取的特征包括 `主频率`、`谱熵` 和 `谱峰度`。`功率谱密度 (PSD)` 和 `谐波比` 提供了关于功率分布和谐波关系的见解。

+   `FFT (快速傅里叶变换)` 将时域信号转换为频域。提取从结果谱中得到的特征，如 `主频率`、`谱熵` 和 `谱峰度`。

+   `功率谱密度 (PSD)` 描述了信号的功率如何在频率上分布。
