# MLX vs MPS vs CUDA：一项基准测试

> 原文：[https://towardsdatascience.com/mlx-vs-mps-vs-cuda-a-benchmark-c5737ca6efc9?source=collection_archive---------3-----------------------#2023-12-15](https://towardsdatascience.com/mlx-vs-mps-vs-cuda-a-benchmark-c5737ca6efc9?source=collection_archive---------3-----------------------#2023-12-15)

## 苹果新的ML框架MLX的首次基准测试

[](https://tristanbilot.medium.com/?source=post_page-----c5737ca6efc9--------------------------------)[![Tristan Bilot](../Images/64c2628ae710042d80ca2ee2feb3da37.png)](https://tristanbilot.medium.com/?source=post_page-----c5737ca6efc9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c5737ca6efc9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c5737ca6efc9--------------------------------) [Tristan Bilot](https://tristanbilot.medium.com/?source=post_page-----c5737ca6efc9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2f664301d394&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmlx-vs-mps-vs-cuda-a-benchmark-c5737ca6efc9&user=Tristan+Bilot&userId=2f664301d394&source=post_page-2f664301d394----c5737ca6efc9---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c5737ca6efc9--------------------------------) ·6 min read·Dec 15, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc5737ca6efc9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmlx-vs-mps-vs-cuda-a-benchmark-c5737ca6efc9&user=Tristan+Bilot&userId=2f664301d394&source=-----c5737ca6efc9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc5737ca6efc9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmlx-vs-mps-vs-cuda-a-benchmark-c5737ca6efc9&source=-----c5737ca6efc9---------------------bookmark_footer-----------)![](../Images/e7da28e0049ecdc0858d1969c4856536.png)

[哈维尔·阿列格·巴罗斯](https://unsplash.com/@soymeraki?utm_source=medium&utm_medium=referral)的照片，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如果你是Mac用户且是深度学习爱好者，你可能曾经希望过你的Mac可以处理那些复杂的模型，对吧？那么，猜猜看？苹果刚刚发布了[MLX](https://ml-explore.github.io/mlx/build/html/index.html)，这是一个在Apple Silicon上高效运行ML模型的框架。

最近在PyTorch 1.12中引入的[MPS后端](https://developer.apple.com/metal/pytorch/)已经是一大胆举措，但随着MLX的宣布，苹果似乎要在开源深度学习领域迈出重要的一步。

在本文中，我们将对这些新方法进行严格测试，将它们与传统的CPU后端在三款不同的Apple Silicon芯片和两款支持CUDA的GPU上进行基准对比。通过这样做，我们旨在揭示这些新型Mac兼容方法在2024年用于深度学习实验的潜力。

作为一个以GNN为导向的研究者，我将把基准测试集中在图卷积网络（GCN）模型上。但由于该模型主要由线性层组成，我们的发现对那些不专门研究GNN领域的人也可能具有洞察力。

## 构建环境

要为MLX构建环境，我们必须指定是使用i386还是arm架构。使用conda可以通过以下方式完成：
