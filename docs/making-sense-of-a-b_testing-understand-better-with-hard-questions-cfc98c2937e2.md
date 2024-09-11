# 了解A/B测试的意义：通过困难问题更好地理解

> 原文：[https://towardsdatascience.com/making-sense-of-a-b_testing-understand-better-with-hard-questions-cfc98c2937e2?source=collection_archive---------2-----------------------#2023-07-04](https://towardsdatascience.com/making-sense-of-a-b_testing_understand-better-with-hard-questions-cfc98c2937e2?source=collection_archive---------2-----------------------#2023-07-04)

## 通过具有挑战性的问题揭示A/B测试中反直觉的方面，提升你的理解，并避免错误

[](https://medium.com/@aliaksandrkazlou?source=post_page-----cfc98c2937e2--------------------------------)[![Aliaksandr Kazlou](../Images/e50023954c125cdd1dbf85834b84f1d8.png)](https://medium.com/@aliaksandrkazlou?source=post_page-----cfc98c2937e2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cfc98c2937e2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cfc98c2937e2--------------------------------) [Aliaksandr Kazlou](https://medium.com/@aliaksandrkazlou?source=post_page-----cfc98c2937e2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa3b0b8410b61&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmaking-sense-of-a-b_testing-understand-better-with-hard-questions-cfc98c2937e2&user=Aliaksandr+Kazlou&userId=a3b0b8410b61&source=post_page-a3b0b8410b61----cfc98c2937e2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cfc98c2937e2--------------------------------) · 6分钟阅读 · 2023年7月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcfc98c2937e2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmaking-sense-of-a-b_testing-understand-better-with-hard-questions-cfc98c2937e2&user=Aliaksandr+Kazlou&userId=a3b0b8410b61&source=-----cfc98c2937e2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcfc98c2937e2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmaking-sense-of-a-b_testing-understand-better-with-hard-questions-cfc98c2937e2&source=-----cfc98c2937e2---------------------bookmark_footer-----------)![](../Images/c91ff8d84b406d2088289d5b649693f8.png)

照片由 [ALAN DE LA CRUZ](https://unsplash.com/es/@alandelacruz4?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这篇文章强调了实验中常见的统计错误。它以五个问题和答案的形式展开，这些答案常常让人感到直觉上不对劲。它专为那些已经熟悉A/B测试但希望扩展理解的人群量身定制。这可以帮助你在日常工作中避免常见错误或在面试中表现出色。

**问题 1：你进行了一个A/B测试（α = 0.05, β = 0.2），结果具有统计学意义。在这种情况下，它是真阳性的可能性有多大？**

想象一下，如果你只测量有效的假设。那么，100%的成功A/B测试将是真阳性。当你的所有假设都无效时，100%的成功A/B测试将是假阳性。

这两个极端的例子旨在说明，没有额外的步骤——对假设分布的假设——是不可能回答这个问题的。

再试一次，假设我们测试的10%的假设是有效的。那么，从A/B测试中观察到统计显著的结果意味着有64%（根据[贝叶斯定理](https://en.wikipedia.org/wiki/Bayes%27_theorem)，(1–0.2)*0.1 / ((1–0.2)*0.1 + 0.05*(1–0.1)))的机会它是真阳性。

![](../Images/1ff5e68d42664d80321c6a6ec84eb47b.png)

作者提供的图像

**问题 2：假设原假设成立。在这种情况下，更高还是更低的p值更可能出现？**

很多人认为是前者。这似乎是直观的：当没有效应时，结果更可能远离统计显著性，因此p值更高。

然而，答案是否定的。当原假设成立时，p值是均匀分布的。

混淆的原因在于，人们通常通过z分数、样本均值或样本均值的差异来可视化这些概念。所有这些都是正态分布的。因此，可能很难理解p值的一致性。

让我们通过一个模拟来说明这一点。假设治疗组和对照组均来自同一个正态分布（μ = 0, σ = 1），这意味着原假设成立。我们将比较它们的均值，计算p值，并多次重复这个过程。为了简化起见，我们只关注治疗组均值较大的情况。然后，我们再看看p值在0.9到0.8和0.2到0.1之间的情况。

当我们将这些p值区间映射到我们模拟的分布上时，图像变得更加清晰。尽管接近零的分布峰值较高，但此处区间的宽度较窄。相反，当我们远离零时，峰值缩小，但区间的宽度增加。这是因为p值的计算方式使得等长区间包含曲线下相同的面积。

![](../Images/8690a40606b009dae1d35fc1a5d7a024.png)

作者提供的图像

**问题3：由于一些技术或业务限制，你进行了一个样本量较小的A/B测试。结果勉强显著。然而，效应值很大，比你在类似A/B测试中通常看到的更大。效应值的增大是否能增强你对结果的信心？**

不一定。为了将效应归类为显著，它必须与零的距离为正负2个标准误差（当α = 0.05）。随着样本量的缩小，标准误差通常会增加。这意味着在小样本中观察到的统计显著效应往往更大。

以下模拟演示了：这些是当两个组（N=1000）都从相同的正态分布（μ = 0，σ = 1）中抽样时的显著A/B测试的绝对效应值。

![](../Images/93df01c68694d9286f170085e0a7454e.png)

作者提供的图像

**问题4：让我们基于之前问题中获得的理解继续探讨**。**是否可以检测到小于2个标准误差的真实效应？**

是的，尽管这里的语义比较模糊**。** 真实效应值可能显著小于2个标准误差。即便如此，你仍然会预期一定比例的A/B测试显示统计显著性。

然而，在这些条件下，你检测到的效应值总是被夸大的。假设真正的效应是0.4，但你检测到的效应是0.5，且p值为0.05。你会认为这是真阳性吗？如果真正的效应值只有0.1，但你又检测到效应为0.5呢？如果真正的效应仅为0.01，那这仍然是一个真阳性吗？

让我们可视化这个场景。对照组（N=100）从正态分布（μ = 0，σ = 2）中抽样，而处理组（N=100）则从相同的分布中抽样，但μ从0.1到1变化。无论真实效应大小如何，成功的A/B测试生成的估计效应值至少为0.5。当真实效应小于这个值时，结果估计显然被夸大了。

![](../Images/b1c9d35cac60a6a66dee7ce516a5e20d.png)

作者提供的图像

这就是为什么一些统计学家避免将结果划分为“真阳性”或“假阳性”这样的二元类别。他们更倾向于以更连续的方式处理这些结果[1]。

**问题5：你进行了A/B测试，结果显著，p值为0.04。然而，你的老板仍然不相信，并要求再做一次测试。这个后续测试没有产生显著结果，p值为0.25。这是否意味着原始效应不真实，初始结果是一个假阳性？**

将p值解释为二元的、词典式的决策规则总是存在风险。让我们回顾一下p值实际上是什么。它是惊讶的度量。它是随机的，是连续的。它只是证据中的一部分。

设想第一个实验（p=0.04）是在 1,000 人中进行的。第二个实验（p=0.25）则是在 10,000 人中进行的。除了质量上的明显差异外，正如我们在问题 3 和 4 中讨论的，第二个 A/B 测试可能估计的效应大小要小得多，可能在实际中不再具有显著意义。

让我们倒过来看这个场景：第一个（p=0.04）是在 10,000 人中进行的，第二个（p=0.25）则是在 1,000 人中进行的。在这种情况下，我们对效果‘存在’的信心更大。

现在，假设两个 A/B 测试是相同的。在这种情况下，你观察到了两个相当相似的、稍微令人惊讶的结果，它们都与零假设不太一致。它们落在 0.05 的两侧并不是特别重要。重要的是，在零假设为真时，连续观察到两个小 p 值是不太可能的。

我们可能需要考虑的一个问题是，这个差异本身是否具有统计显著性。以二元方式分类 p 值会扭曲我们的直觉，让我们相信不同侧的 p 值之间存在巨大甚至本体上的差异。然而，p 值是一个相当连续的函数，尽管两个 A/B 测试的 p 值不同，但它们可能对零假设提供了非常相似的证据[2]。

另一种看待这个问题的方法是结合证据。假设两个测试的零假设都为真，根据[Fisher 方法](https://en.wikipedia.org/wiki/Fisher%27s_method)，组合后的 p 值为 0.05。还有其他方法可以结合 p 值，但基本逻辑相同：在大多数情况下，尖锐的零假设并不现实。因此，即使这些结果在统计上没有单独显著，足够多的‘令人惊讶’的结果也可能足以拒绝零假设。

![](../Images/ca549bdd0f58d7ea341632905c7e5070.png)

通过使用 Fisher 方法融合两个 p 值。图片来源：Chen-Pan Liao，来自[维基百科](https://en.wikipedia.org/wiki/Fisher%27s_method)

# **结论**

我们常用的零假设检验框架，在分析 A/B 测试时并不是特别直观。没有经常的心理训练，我们常常会退回到‘直观’的理解，这可能会误导我们。我们也可能会形成一些例行程序以减轻这种认知负担。不幸的是，这些例行程序往往变得有些仪式化，遵守正式程序的过程掩盖了实际推断的目标。

# 参考文献

1.  McShane, B. B., Gal, D., Gelman, A., Robert, C., & Tackett, J. L. (2019). 放弃统计显著性。*美国统计学家*, *73*(sup1), 235–245.

1.  Gelman, A., & Stern, H. (2006). “显著”和“非显著”之间的差异本身并不具有统计显著性。*美国统计学家*, *60*(4), 328–331.
