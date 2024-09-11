# 利用一种能够理解与任何类型分子相互作用的新 AI 模型突破蛋白质设计的界限

> 原文：[https://towardsdatascience.com/breaking-boundaries-in-protein-design-with-a-new-ai-model-that-understands-interactions-with-any-388fd747ee40?source=collection_archive---------9-----------------------#2023-06-21](https://towardsdatascience.com/breaking-boundaries-in-protein-design-with-a-new-ai-model-that-understands-interactions-with-any-388fd747ee40?source=collection_archive---------9-----------------------#2023-06-21)

## 这一新模型有助于**扩展机器学习模型在工程化蛋白质方面的适用性，能够通过调整其与任何类型分子的特定相互作用，从而有效影响生物技术和临床应用**

[](https://lucianosphere.medium.com/?source=post_page-----388fd747ee40--------------------------------)[![LucianoSphere (Luciano Abriata, PhD)](../Images/a8ae3085d094749bbdd1169cca672b86.png)](https://lucianosphere.medium.com/?source=post_page-----388fd747ee40--------------------------------)[](https://towardsdatascience.com/?source=post_page-----388fd747ee40--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----388fd747ee40--------------------------------) [LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----388fd747ee40--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd28939b5ab78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbreaking-boundaries-in-protein-design-with-a-new-ai-model-that-understands-interactions-with-any-388fd747ee40&user=LucianoSphere+%28Luciano+Abriata%2C+PhD%29&userId=d28939b5ab78&source=post_page-d28939b5ab78----388fd747ee40---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----388fd747ee40--------------------------------) ·5 min read·2023年6月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F388fd747ee40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbreaking-boundaries-in-protein-design-with-a-new-ai-model-that-understands-interactions-with-any-388fd747ee40&user=LucianoSphere+%28Luciano+Abriata%2C+PhD%29&userId=d28939b5ab78&source=-----388fd747ee40---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F388fd747ee40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbreaking-boundaries-in-protein-design-with-a-new-ai-model-that-understands-interactions-with-any-388fd747ee40&source=-----388fd747ee40---------------------bookmark_footer-----------)![](../Images/e3fcdb27db7b7b4c76aa9f48fa1aacbb.png)

作者通过编辑Dall-E-2生成的图像（最初使用 [这里](https://medium.com/advances-in-biological-science/how-computer-modeling-simulations-and-artificial-intelligence-impact-protein-engineering-in-4d8473bd59ff)）创作的“蛋白质工程”概念艺术。

**在深度学习的力量推动下，Deepmind的AlphaFold在结构生物学上掀起的革命，使得与之密切相关的蛋白质设计领域最近进入了一个新纪元。然而，现有的蛋白质设计机器学习（ML）模型在将非蛋白质实体纳入设计过程中的能力上存在限制，只能处理蛋白质成分。在我们的新预印本中，我们介绍了一种新的深度学习模型，“CARBonAra”，它考虑了任何类型的分子环境，并因此可以设计与任何类型的分子结合的蛋白质：类似药物的配体、辅因子、底物、核酸，甚至是其他蛋白质。通过利用我们之前的ML模型中的几何变换器架构，CARBonAra能够在了解**…
