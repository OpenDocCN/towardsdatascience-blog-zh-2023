# 透视镜与谷歌在眼睛中发现的东西

> 原文：[https://towardsdatascience.com/through-the-looking-glass-and-what-google-find-there-in-the-eye-e7a836eb9571?source=collection_archive---------4-----------------------#2023-03-30](https://towardsdatascience.com/through-the-looking-glass-and-what-google-find-there-in-the-eye-e7a836eb9571?source=collection_archive---------4-----------------------#2023-03-30)

## | 计算机视觉 | 健康护理中的人工智能 | CNN

## 或谷歌如何使用深度学习来诊断眼部照片中的疾病

[](https://salvatore-raieli.medium.com/?source=post_page-----e7a836eb9571--------------------------------)[![Salvatore Raieli](../Images/6bb4520e2df40d20283e7283141b5e06.png)](https://salvatore-raieli.medium.com/?source=post_page-----e7a836eb9571--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e7a836eb9571--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e7a836eb9571--------------------------------) [Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----e7a836eb9571--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff1a08d9452cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthrough-the-looking-glass-and-what-google-find-there-in-the-eye-e7a836eb9571&user=Salvatore+Raieli&userId=f1a08d9452cd&source=post_page-f1a08d9452cd----e7a836eb9571---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e7a836eb9571--------------------------------) ·12 min read·2023年3月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe7a836eb9571&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthrough-the-looking-glass-and-what-google-find-there-in-the-eye-e7a836eb9571&user=Salvatore+Raieli&userId=f1a08d9452cd&source=-----e7a836eb9571---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe7a836eb9571&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthrough-the-looking-glass-and-what-google-find-there-in-the-eye-e7a836eb9571&source=-----e7a836eb9571---------------------bookmark_footer-----------)![](../Images/f4f232c4e1c318f5fce9ec857c2775d1.png)

作者使用 OpenAI DALL-E 创作的图像

谷歌最近发布了一篇科学论文，展示了一种人工智能模型如何通过简单的眼睛照片预测多种系统生物标志物。

> 这如何运作？这些结果是如何得出的？为什么这很重要？我们在本文中讨论了这些问题。

# 眼睛中的隐藏宝藏

![](../Images/35db24ac8670c2348b942cd5ebabcba1.png)

图像由 [v2osk](https://unsplash.com/it/@v2osk) 提供，来源于 Unsplash

疾病的诊断通常需要使用昂贵的仪器进行检查，然后由经过培训的医疗专业人员进行解释。这并不总是可行的。并非所有医院都有相同的仪器，并且…
