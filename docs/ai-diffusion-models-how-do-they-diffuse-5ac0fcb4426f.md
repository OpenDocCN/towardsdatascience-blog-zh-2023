# 扩散模型：它们是如何扩散的？

> 原文：[https://towardsdatascience.com/ai-diffusion-models-how-do-they-diffuse-5ac0fcb4426f?source=collection_archive---------11-----------------------#2023-11-04](https://towardsdatascience.com/ai-diffusion-models-how-do-they-diffuse-5ac0fcb4426f?source=collection_archive---------11-----------------------#2023-11-04)

## 理解生成 AI 背后的核心过程

[](https://medium.com/@OnurGun?source=post_page-----5ac0fcb4426f--------------------------------)[![Onur Yuce Gun, PhD](../Images/aeb533625cbe7f40dd9f541e473975f6.png)](https://medium.com/@OnurGun?source=post_page-----5ac0fcb4426f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5ac0fcb4426f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5ac0fcb4426f--------------------------------) [Onur Yuce Gun, PhD](https://medium.com/@OnurGun?source=post_page-----5ac0fcb4426f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8ec932e84e54&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fai-diffusion-models-how-do-they-diffuse-5ac0fcb4426f&user=Onur+Yuce+Gun%2C+PhD&userId=8ec932e84e54&source=post_page-8ec932e84e54----5ac0fcb4426f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5ac0fcb4426f--------------------------------) ·7 分钟阅读·2023年11月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5ac0fcb4426f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fai-diffusion-models-how-do-they-diffuse-5ac0fcb4426f&user=Onur+Yuce+Gun%2C+PhD&userId=8ec932e84e54&source=-----5ac0fcb4426f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5ac0fcb4426f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fai-diffusion-models-how-do-they-diffuse-5ac0fcb4426f&source=-----5ac0fcb4426f---------------------bookmark_footer-----------)

# 扩散模型

机器学习中的“扩散模型”这一名称源于扩散过程的统计概念。

那个 *统计概念* 是什么？

在自然科学中，扩散是指粒子从高浓度区域扩散到低浓度区域的过程，这一过程通常通过物理学和数学中的扩散方程来描述。

反应-扩散是一个很好的例子。

# 反应-扩散

反应-扩散是一个相当复杂的过程；如果你想阅读数学逻辑，你可以访问 RD 大师 Karl Sims 的网站：

[](https://www.karlsims.com/rd.html?source=post_page-----5ac0fcb4426f--------------------------------) [## 反应-扩散教程

### 格雷-斯科特反应-扩散模型的插图教程。

[www.karlsims.com](https://www.karlsims.com/rd.html?source=post_page-----5ac0fcb4426f--------------------------------)

让我们从一个简单的类比开始：

反应-扩散系统是一种描述事物如何变化和移动的方式……
