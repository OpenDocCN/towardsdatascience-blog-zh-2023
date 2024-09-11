# 微软的新人工智能方法用于预测分子的运动和功能

> 原文：[https://towardsdatascience.com/microsofts-new-ai-method-to-predict-how-molecules-move-and-function-93d47e246b5d?source=collection_archive---------6-----------------------#2023-07-20](https://towardsdatascience.com/microsofts-new-ai-method-to-predict-how-molecules-move-and-function-93d47e246b5d?source=collection_archive---------6-----------------------#2023-07-20)

## 被称为“Distributional Graphormer”，它可能会启动人工智能在化学和生物科学领域带来的革命的下一步。

[](https://lucianosphere.medium.com/?source=post_page-----93d47e246b5d--------------------------------)[![LucianoSphere (Luciano Abriata, PhD)](../Images/a8ae3085d094749bbdd1169cca672b86.png)](https://lucianosphere.medium.com/?source=post_page-----93d47e246b5d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----93d47e246b5d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----93d47e246b5d--------------------------------) [LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----93d47e246b5d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd28939b5ab78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmicrosofts-new-ai-method-to-predict-how-molecules-move-and-function-93d47e246b5d&user=LucianoSphere+%28Luciano+Abriata%2C+PhD%29&userId=d28939b5ab78&source=post_page-d28939b5ab78----93d47e246b5d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----93d47e246b5d--------------------------------) ·9分钟阅读·2023年7月20日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F93d47e246b5d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmicrosofts-new-ai-method-to-predict-how-molecules-move-and-function-93d47e246b5d&user=LucianoSphere+%28Luciano+Abriata%2C+PhD%29&userId=d28939b5ab78&source=-----93d47e246b5d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F93d47e246b5d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmicrosofts-new-ai-method-to-predict-how-molecules-move-and-function-93d47e246b5d&source=-----93d47e246b5d---------------------bookmark_footer-----------)![](../Images/1f7a0288e0acf2dfa3610977083b03c4.png)

图1\. 分子（橙色）采用三种不同结构（x轴），每种结构具有不同的能量（y轴）的示意图。这被称为分子由于运动而可以探索的“构象景观”。局部或全局最小值的能量越低，分子在不同结构之间交换时会停留的时间越长（绿色箭头）。虽然像AlphaFold这样的常规结构预测方法旨在找到中央最小值，即最低能量结构，但最终目标是找到可能结构的完整分布及其概率。有关更多解释，请参见图2和文本。图片由作者绘制。

*“我们能否不仅预测静态蛋白质结构，还能预测它们的结构多样性？”*——这是我大约一年前在AlphaFold 2的兴奋逐渐平息时提出的问题：

[](https://medium.com/advances-in-biological-science/can-we-predict-not-only-static-protein-structures-but-also-their-structural-diversity-fa1d9380fc34?source=post_page-----93d47e246b5d--------------------------------) [## 我们能否不仅预测静态蛋白质结构，还能预测它们的结构多样性？

### 最近一项以特殊方式应用AlphaFold 2的研究表明这可能是可能的。

[medium.com](https://medium.com/advances-in-biological-science/can-we-predict-not-only-static-protein-structures-but-also-their-structural-diversity-fa1d9380fc34?source=post_page-----93d47e246b5d--------------------------------)

现在，显然微软的团队在将AI应用于科学领域（与一位著名教授合作，见文末说明）可能对我的问题有了首个“是”的答案。他们刚刚展示了他们的新型“分布图形变换器”（Distributional Graphormer），它不仅能预测单一分子结构（如蛋白质、其他分子或材料），而且能实际预测分子或材料在3D中可能采用的多种替代结构（或“构象”）。也就是说，原子的不同可能排列。更重要的是，这个新的AI模型还“理解”不同结构的含义……
