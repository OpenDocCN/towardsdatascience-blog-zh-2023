# 如何解决蛋白质折叠问题：AlphaFold2

> 原文：[https://towardsdatascience.com/how-to-solve-the-protein-folding-problem-alphafold2-6c81faba670d?source=collection_archive---------3-----------------------#2023-03-11](https://towardsdatascience.com/how-to-solve-the-protein-folding-problem-alphafold2-6c81faba670d?source=collection_archive---------3-----------------------#2023-03-11)

## 更深入地了解AlphaFold2及其神经网络架构

[](https://universvm.medium.com/?source=post_page-----6c81faba670d--------------------------------)[![Leonardo Castorina](../Images/74ba4ff34c619576da18dbda354f4982.png)](https://universvm.medium.com/?source=post_page-----6c81faba670d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6c81faba670d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6c81faba670d--------------------------------) [Leonardo Castorina](https://universvm.medium.com/?source=post_page-----6c81faba670d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fff3309c4a1aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-solve-the-protein-folding-problem-alphafold2-6c81faba670d&user=Leonardo+Castorina&userId=ff3309c4a1aa&source=post_page-ff3309c4a1aa----6c81faba670d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6c81faba670d--------------------------------) ·16分钟阅读·2023年3月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6c81faba670d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-solve-the-protein-folding-problem-alphafold2-6c81faba670d&user=Leonardo+Castorina&userId=ff3309c4a1aa&source=-----6c81faba670d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6c81faba670d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-solve-the-protein-folding-problem-alphafold2-6c81faba670d&source=-----6c81faba670d---------------------bookmark_footer-----------)![](../Images/572e0dac09bea160681c694c2b4fb515.png)

蛋白质序列到结构的示意图。白色部分为原始蛋白质，彩虹色为AlphaFold预测的蛋白质[蓝色=最高信心，红色=最低信心]（图示由作者提供）

在这一系列文章中，我将介绍蛋白质折叠及深度学习模型，如AlphaFold、OmegaFold和ESMFold。我们将从AlphaFold2开始！

# 介绍

蛋白质是执行生物体中大多数生化功能的分子。它们参与消化（酶）、结构过程（角蛋白——皮肤）、光合作用，并且在制药行业中被广泛使用 [2]。

蛋白质的三维结构对其功能至关重要。蛋白质由20种叫做氨基酸（或残基）的亚单位组成，每种氨基酸具有不同的特性，如电荷、极性、长度和原子数。氨基酸由所有氨基酸共有的**骨架**和每种氨基酸独特的**侧链**构成。它们通过肽键连接[2]。

![](../Images/bba2585b3f0247659bbdd5ef71269b05.png)

四个残基的蛋白质多肽示意图。（作者插图）

![](../Images/7a58223094f9764d6b7763d827e2ef4e.png)

四个残基的蛋白质骨架示意图。（作者插图）
