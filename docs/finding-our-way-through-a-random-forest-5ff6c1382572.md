# 穿越随机森林的道路

> 原文：[https://towardsdatascience.com/finding-our-way-through-a-random-forest-5ff6c1382572?source=collection_archive---------11-----------------------#2023-04-19](https://towardsdatascience.com/finding-our-way-through-a-random-forest-5ff6c1382572?source=collection_archive---------11-----------------------#2023-04-19)

## 或者在一个被僵尸肆虐的假设世界中，决策树如何能够决定是否能够摆脱困境

[](https://medium.com/@manfred.james?source=post_page-----5ff6c1382572--------------------------------)[![Diego Manfre](../Images/2189d8e63df449a869526bf8b6c50440.png)](https://medium.com/@manfred.james?source=post_page-----5ff6c1382572--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5ff6c1382572--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5ff6c1382572--------------------------------) [Diego Manfre](https://medium.com/@manfred.james?source=post_page-----5ff6c1382572--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6e3d8f9df1a5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-our-way-through-a-random-forest-5ff6c1382572&user=Diego+Manfre&userId=6e3d8f9df1a5&source=post_page-6e3d8f9df1a5----5ff6c1382572---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----5ff6c1382572--------------------------------) ·17分钟阅读·2023年4月19日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5ff6c1382572&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-our-way-through-a-random-forest-5ff6c1382572&source=-----5ff6c1382572---------------------bookmark_footer-----------)![](../Images/42de49ee660a66ca0f4cef619e194d77.png)

图像由作者使用 Midjourney 制作

*在车库外，咆哮声和低吼声没有停止。他简直不敢相信自己在系列剧和电影中看了许多次的僵尸末日居然出现在他的门前。他可以在车库里躲一段时间，但最终还是得出来。他应该带上斧头还是步枪就够了？他可以试着找些食物，但该不该一个人去？他努力回忆自己看过的所有僵尸电影，但无法决定一个单一的策略。如果他能记住每一个角色被僵尸杀死的场景，这是否足以增加他的生存几率？如果他只有一个决策指南，一切会简单很多……*

# 介绍

你是否看过那种僵尸末日电影，其中总有一个角色似乎总能知道僵尸藏在哪里，或者是否应该与僵尸战斗或逃跑？这个人真的知道将会发生什么吗？有人提前告诉过他/她吗？也许这并没有什么神奇之处，也许这个人读过很多关于僵尸的漫画，并且真的很擅长知道该如何……
