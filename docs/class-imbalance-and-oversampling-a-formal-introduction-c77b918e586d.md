# 类别不平衡与重采样：正式介绍

> 原文：[https://towardsdatascience.com/class-imbalance-and-oversampling-a-formal-introduction-c77b918e586d?source=collection_archive---------12-----------------------#2023-10-07](https://towardsdatascience.com/class-imbalance-and-oversampling-a-formal-introduction-c77b918e586d?source=collection_archive---------12-----------------------#2023-10-07)

## 让我们深入探讨类别不平衡问题，以及诸如随机过采样等重采样方法如何尝试解决这一问题。

[](https://essamwissam.medium.com/?source=post_page-----c77b918e586d--------------------------------)[![Essam Wisam](../Images/6320ce88ba2e5d56d70ce3e0f97ceb1d.png)](https://essamwissam.medium.com/?source=post_page-----c77b918e586d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c77b918e586d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c77b918e586d--------------------------------) [Essam Wisam](https://essamwissam.medium.com/?source=post_page-----c77b918e586d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fccb82b9f3b87&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclass-imbalance-and-oversampling-a-formal-introduction-c77b918e586d&user=Essam+Wisam&userId=ccb82b9f3b87&source=post_page-ccb82b9f3b87----c77b918e586d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c77b918e586d--------------------------------) ·6 分钟阅读·2023年10月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc77b918e586d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclass-imbalance-and-oversampling-a-formal-introduction-c77b918e586d&user=Essam+Wisam&userId=ccb82b9f3b87&source=-----c77b918e586d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc77b918e586d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclass-imbalance-and-oversampling-a-formal-introduction-c77b918e586d&source=-----c77b918e586d---------------------bookmark_footer-----------)

最近，我在构建一个名为[Imbalance.jl](https://github.com/JuliaAI/Imbalance.jl)的Julia包来解决类别不平衡问题。在构建该包时，我花费了大量的精力阅读论文和查看实现，因此我认为分享我对类别不平衡问题的理解以及一些用于解决该问题的流行算法可能会有所帮助。这些算法包括朴素随机过采样、随机过采样示例（ROSE）、随机游走过采样（RWO）、合成少数类过采样技术（SMOTE）、SMOTE-名义型、SMOTE-名义型连续以及许多欠采样方法。对于这个故事，我们将正式定义类别不平衡问题，并讨论随机过采样作为有效解决方案。在后续文章中，我们将通过考虑其他技术理性地得出更多的解决方案。

## 目录

∘ [类别不平衡问题](#c801)

∘ [解决类别不平衡问题](#425a)

∘ [随机过采样](#bac7)

∘ [为什么要过采样？](#fa3c)

∘ [欠采样](#8ebe)

![](../Images/f6e5f9e8d6f67dd7bb40b63d875b143a.png)

照片由 [Artem Kniaz](https://unsplash.com/@artem_kniaz?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

## 类别不平衡问题

大多数（如果不是全部）机器学习算法可以被视为经验风险最小化的一种形式，其中目标是找到参数θ，使得某些损失函数*L*最小化：

![](../Images/d0d8a2bc15d20e758f02c4b1d36946b4.png)

例如，线性回归将*L*定义为平方损失，逻辑回归将其定义为交叉熵损失，SVM将其定义为铰链损失，自适应提升将其定义为指数损失。

基本假设是，如果*f_θ*的参数允许它在数据集上最小化该经验风险，而数据集可以视为从总体中随机抽取的样本，那么它应该足够接近我们寻求的目标函数*f*，我们寻找的模型是使数据集以及整个总体上的相同量最小化。

在具有K个类别的多类别设置中，我们可以将经验风险写作

![](../Images/3d4322a630bee958b0694cf37dd3e735.png)

当一些类别的样本远少于其他类别时，就会发生类别不平衡问题。在这种情况下，对应于这些类别的术语对总和的贡献最小，这使得任何学习算法都可能找到一个近似解来最小化经验风险，而这个解主要只在显著的和上进行最小化。这会产生一个可能与真实目标*f*相差很大的假设*f_θ*，而这些少数类可能是所讨论应用中最重要的。

总结来说，以下是我们有类别不平衡问题的条件：

1 — 训练集中点在各个类别之间的分布并不“公平”；某些类别的点远少于其他类别。

2 — 模型在训练后对这些少数类别的点表现不佳。也就是说，学习算法没有像上面解释的那样适当地最小化少数类别的损失。

这个问题的严重程度取决于这些少数类别对应用程序的重要性。它们往往比多数类别更重要（例如，分类欺诈交易或稀有疾病）。

## 解决类别不平衡问题

从问题的描述中很明显，一个解决办法是给较小的类别（即少数类别）加权，以便学习算法更容易避免利用它们的不重要性来获得近似解。通常可以很容易地为此目的修改机器学习算法；特别是当它们明确是经验风险最小化的一种形式，而不仅仅是对于某些损失函数的等效形式时。

另一种试图解决问题而不需要对学习算法进行任何修改的方法是重新采样数据。最简单的形式可以看作是赋权的成本敏感方法。考虑以下算法：

**给定：** 一个不平衡的数据集，包含K个类别和每个类别的整数

**需求：** 一个数据集，其中每个类别的数据根据整数进行复制

**操作：** 将类别k中的每个点重复c次，其中c是与该类别相关的整数

插入总和时应该很明显，这相当于成本敏感方法；回想一下，最小化一个函数等同于最小化它的标量正倍数。

## 随机过采样

上述算法存在一个小问题；如果类别A有900个示例，而类别B有600个示例，则没有整数倍数可以用来过采样类别B，使数据集达到完全平衡。我们可以通过随机选择点进行复制，扩展算法以处理非整数的复制比例。例如，如果我们想对类别B进行300个示例的过采样，以使系统达到平衡（相当于比例为1.5），我们可以通过...

1 — 从类别B中随机选择300个点

2 — 复制这些点

这个算法称为朴素随机过采样，它正式的做法是：

1 — 计算每个类别所需生成的点数（根据给定的比例计算）

2 — 假设对于类别k，这个数字是*N_k*，然后从属于该类别的点中随机选择*N_k*个点，并将它们添加以形成新的数据集

显然，这在平均上等同于前述算法，因此也等同于类别权重。如果类别k的比例为2.0，则每个点平均会被随机选择一次。

这是我为三个类别（0、1、2）生成的随机不平衡数据集，展示了类别的直方图以及过采样前后的点的散点图。

![](../Images/1e64ac6158a9431b3104391846e8de6d.png)

作者图

请注意，下方的两个图形没有视觉上的差异，因为所有生成的点都是现有点的复制品。

## 为什么要过采样？

你可能还不太相信重新采样比类别权重在解决类别不平衡问题上更为突出；毕竟，我们已经展示了，朴素的随机过采样可以看作是平均上等同于类别权重的，只是训练时间更长（由于数据量增加）。这个论点唯一的问题是，一般情况下不可能使用类别权重；尤其是，当机器学习模型没有明确最小化损失时。

如果我们通过在数据集中自然地添加每个类别的点来收集更多数据，那么我们获得比朴素随机过采样（或类别权重）更好的结果是有道理的。例如，假设……

+   我们希望检测一个交易是否为欺诈交易。

+   我们收集了一个包含1K欺诈交易和999K有效交易的数据集。

显然，通过收集另外998K欺诈交易来解决不平衡问题比重复现有的1K交易997K次要好得多。特别是，在后者情况下，我们有很高的过拟合特定数据的风险。

现实显然是，通常情况下不可能为少数类收集更多数据；然而，可能可以模拟这样做的效果。也许，如果我们合成生成的数据对于少数类足够代表真实的新例子，那么理想情况下，这比重复例子或类别权重要好得多。这是最常见的过采样形式，其与收集真实数据的关系可能是其优于随机过采样和类别权重的最合理理由；因此，成为解决类别不平衡问题的值得方法。

## 欠采样

最后请注意，如果我们不是随机选择少数类中的例子来复制，而是随机选择多数类中的点进行删除，那么算法变成了朴素随机欠采样。这显然有丧失有用数据的缺点，但有时从“不是特别有用”的多数类中删除数据可以解决不平衡问题，并导致在“更有用”的少数类上的表现更好。还有其他欠采样方法，可以更仔细地选择排除哪些点，例如为了保持数据结构或允许更好的决策边界。

在[进一步的故事](https://medium.com/towards-data-science/class-imbalance-from-random-oversampling-to-rose-517e06d7a9b)中，我们解释了最初在[Imbalance.jl](https://github.com/JuliaAI/Imbalance.jl)中实现的所有算法及其如何解决类别不平衡问题。
