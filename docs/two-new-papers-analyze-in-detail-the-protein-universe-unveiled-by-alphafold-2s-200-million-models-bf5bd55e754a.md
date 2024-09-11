# 两篇新论文详细分析了AlphaFold 2的两亿模型揭示的蛋白质宇宙

> 原文：[https://towardsdatascience.com/two-new-papers-analyze-in-detail-the-protein-universe-unveiled-by-alphafold-2s-200-million-models-bf5bd55e754a?source=collection_archive---------7-----------------------#2023-09-21](https://towardsdatascience.com/two-new-papers-analyze-in-detail-the-protein-universe-unveiled-by-alphafold-2s-200-million-models-bf5bd55e754a?source=collection_archive---------7-----------------------#2023-09-21)

## 他们不得不创建新的工具来处理如此大规模的蛋白质结构模型

[](https://lucianosphere.medium.com/?source=post_page-----bf5bd55e754a--------------------------------)[![LucianoSphere (Luciano Abriata, PhD)](../Images/a8ae3085d094749bbdd1169cca672b86.png)](https://lucianosphere.medium.com/?source=post_page-----bf5bd55e754a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bf5bd55e754a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bf5bd55e754a--------------------------------) [LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----bf5bd55e754a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd28939b5ab78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwo-new-papers-analyze-in-detail-the-protein-universe-unveiled-by-alphafold-2s-200-million-models-bf5bd55e754a&user=LucianoSphere+%28Luciano+Abriata%2C+PhD%29&userId=d28939b5ab78&source=post_page-d28939b5ab78----bf5bd55e754a---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bf5bd55e754a--------------------------------) ·7分钟阅读·2023年9月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbf5bd55e754a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwo-new-papers-analyze-in-detail-the-protein-universe-unveiled-by-alphafold-2s-200-million-models-bf5bd55e754a&user=LucianoSphere+%28Luciano+Abriata%2C+PhD%29&userId=d28939b5ab78&source=-----bf5bd55e754a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbf5bd55e754a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwo-new-papers-analyze-in-detail-the-protein-universe-unveiled-by-alphafold-2s-200-million-models-bf5bd55e754a&source=-----bf5bd55e754a---------------------bookmark_footer-----------)![](../Images/79ab585bd4935c9a93826c1a29254aa1.png)

在讨论的文章中呈现的资源之一，[https://uniprot3d.org/](https://uniprot3d.org/) 展示了蛋白质的现代宇宙视图，其中较亮的簇包含更多成员。通过缩放，用户可以检查相关蛋白质，直到点击特定节点显示关于 Uniprot 上蛋白质家族的信息（此处仅显示结构模型）。图像由作者在浏览网站时制作。

**DeepMind 的 AlphaFold 2 与欧洲生物信息学研究所合作，最近发布了超过 2 亿个预测的蛋白质结构，开启了蛋白质研究的新纪元。在这里，我将呈现本周发表在*自然*上的两篇开创性论文的总结，这些论文深入探讨了这一蛋白质宇宙的深处。这些论文采用了创新的聚类算法、结构比较以及其他现有工具的适应方法，以处理大数据量，从而揭示了蛋白质的结构多样性、进化关系和功能潜力，规模前所未有。**

蛋白质是生物学中的重要“工匠”，管理着从能量生成到细胞分裂的众多细胞过程。尽管得益于基因组学的进步，蛋白质的测序在近年来得到了迅速发展，但由于缺乏可扩展的实验方法，其 3D 结构的确定仍然滞后。然而，随着 DeepMind 开发的革命性 AI 系统 AlphaFold 2 的问世，蛋白质结构预测的格局已经发生了…
