# 理解KL散度

> 原文：[https://towardsdatascience.com/understanding-kl-divergence-f3ddc8dff254?source=collection_archive---------0-----------------------#2023-02-02](https://towardsdatascience.com/understanding-kl-divergence-f3ddc8dff254?source=collection_archive---------0-----------------------#2023-02-02)

![](../Images/23ed4c262576e3be5227e532e0eedf31.png)

图片由作者提供

## KL散度的数学、直观理解和实际应用指南——包括如何在漂移监测中最佳使用它

[](https://aparnadhinak.medium.com/?source=post_page-----f3ddc8dff254--------------------------------)[![Aparna Dhinakaran](../Images/e431ee69563ecb27c86f3428ba53574c.png)](https://aparnadhinak.medium.com/?source=post_page-----f3ddc8dff254--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f3ddc8dff254--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f3ddc8dff254--------------------------------) [Aparna Dhinakaran](https://aparnadhinak.medium.com/?source=post_page-----f3ddc8dff254--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff32f85889f3a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-kl-divergence-f3ddc8dff254&user=Aparna+Dhinakaran&userId=f32f85889f3a&source=post_page-f32f85889f3a----f3ddc8dff254---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f3ddc8dff254--------------------------------) ·7分钟阅读·2023年2月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff3ddc8dff254&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-kl-divergence-f3ddc8dff254&user=Aparna+Dhinakaran&userId=f32f85889f3a&source=-----f3ddc8dff254---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff3ddc8dff254&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-kl-divergence-f3ddc8dff254&source=-----f3ddc8dff254---------------------bookmark_footer-----------)

**Kullback-Leibler散度**度量（相对熵）是信息理论中的一种统计测量，通常用于量化一个概率分布与参考概率分布之间的差异。

尽管KL散度很受欢迎，但有时会被误解。在实践中，有时也很难知道何时应选择一种统计距离检查而非另一种。

本博客介绍了如何使用KL散度，它在实践中的工作原理，以及何时应使用或不应使用KL散度来监测漂移。

# 你如何计算KL散度？

KL 散度是一种非对称度量， [测量相对熵](http://hanj.cs.illinois.edu/cs412/bk3/KL-divergence.pdf) 或两个分布所表示的信息差异。它可以被视为测量两个数据分布之间的距离，显示这两个分布彼此之间的差异。

KL 散度有连续形式

![](../Images/6ad674da352f58e57e2b6d350c36194a.png)

作者提供的图片

以及离散形式的 KL 散度：

![](../Images/8ab7c7798e76224f948d0de7b8caf822.png)

作者提供的图片

在模型监控中，大多数实践者几乎专门使用离散形式的 KL 散度，并通过 [数据分箱](https://arize.com/blog-course/data-binning-production/) 来获得离散分布。离散形式的 KL 散度与连续形式在样本数量和箱子限制趋向于无限时会收敛。存在最佳的 [选择方法](https://stats.stackexchange.com/questions/510699/discrete-kl-divergence-with-decreasing-bin-width) 来接近连续形式。在实践中，箱子的数量可能远少于上述数字所示的数量——如何创建这些箱子以处理零样本箱的情况在实际操作中比其他任何事情更为重要（未来的文章将用代码处理如何自然处理零箱问题）。

# KL 散度如何用于模型监控？

在模型监控中，KL 散度用于监控生产环境，特别是围绕特征和预测数据。KL 散度被用来确保生产中的输入或输出数据不会与基准发生剧烈变化。基准可以是训练生产数据窗口，或者是训练或验证数据集。

漂移监控对于那些接收延迟的真实数据来与生产模型决策进行比较的团队尤其有用。这些团队可以依赖预测和特征分布的变化作为性能的代理。

KL 散度通常应用于每个特征，独立地进行计算；它不是作为协方差特征度量的设计，而是显示每个特征如何独立于基准值发生偏离的度量。

![](../Images/52a909cc15e9bc7d6942a8712757c40b.png)

作者提供的图片

上面橙色条纹中显示的 p(x) 是参考或基准分布。最常见的基准要么是生产数据的滞后窗口，要么是训练数据集。每个箱子会累加地贡献于 KL 散度。这些箱子共同加总到总的百分比分布中。

*✏️* ***注意***:* 有时非实践者可能有一种过于热心的目标，即完美捕捉数据变化的数学方法。在实践中，需要记住，实际数据在生产中总是不断变化的，许多模型对这种修改后的数据有很好的适应性。使用漂移度量的目标是拥有一个稳固、稳定且非常有用的度量，以便进行故障排除。

# KL 散度是非对称度量吗？

是的。如果你交换基线分布 p(x) 和样本分布 q(x)，你会得到不同的数字。作为一个非对称度量，KL 散度在团队使用它进行数据模型比较时有一些缺点。有时团队希望在故障排除工作流程中用不同的分布替换比较基线，而拥有一个*A / B* 与*B / A* 不同的度量可能会使结果比较变得困难。

![](../Images/3cb72a873aabe21985a675c56cba853e.png)

作者提供的图像

这也是模型监控工具如 Arize（完全披露：我共同创立了 Arize）将[总体稳定性指数](https://arize.com/blog-course/population-stability-index-psi/)（PSI），KL 散度的对称衍生物，作为模型监控分布的主要指标之一的原因。

# 连续数值特征与分类特征之间的差异

KL 散度可以用于测量数值分布与分类分布之间的差异。

![](../Images/521325cae5eaa261781ae9deab73eb81.png)

作者提供的图像

## **数值**

对于数值分布，数据根据切割点、箱体大小和箱体宽度被分成多个箱。箱体策略可以是均匀箱体、分位数或者复杂的混合策略，这会对 KL 散度产生显著影响。

## **分类**

KL 散度的监测跟踪分类数据集中的大规模分布变化。对于分类特征，通常有一个大小，使得基数变得太大，导致度量的实用性降低。理想的大小是约 50–100 个独特值——当分布具有更高基数时，两个分布之间的差异及其重要性的问题会变得模糊。

# 高基数

在高基数字特征监测的情况下，现成的统计距离通常效果不好——我们通常推荐两个选项：

1.  **嵌入**：在一些高基数的情况下，使用的值——如用户 ID 或内容 ID——已经被用于内部创建嵌入。[嵌入漂移监测](/measuring-embedding-drift-aa9b7ddb84ae)可以提供帮助。

1.  **纯高基数分类**：在其他情况下，当模型将输入编码到较大的空间时，使用 KL 散度监测前 50–100 个顶级项以及所有其他值作为“其他”可能会很有用。

最后，有时你想监测的是某个特定的内容，如在某个周期内的新值或箱体的百分比。这些可以通过数据质量监控工具进行更具体的设置。

# KL 散度示例

这是一个[KL 散度](https://arize.com/blog-course/kl-divergence/)的示例。

![](../Images/f22bd63ba1fc4b894ad6c59afb8d4bfc.png)

作者提供的图像

想象一下，我们有一个用于预测信用卡欺诈的模型的数值分布。该模型是基于上图所示的基准进行训练的。我们可以看到收费的分布发生了变化。行业标准对阈值有一些规定，实际上我们建议使用生产中滞后的值来设置自动阈值。在生产中，许多固定设置的<>并不合适。

# KL 散度的直觉

对于度量及其基于分布变化的变化，拥有一定的直觉非常重要。

![](../Images/c7e9b9e7cfb3d9d19d5af4f923ae61db.png)

作者提供的图像

上述例子展示了从一个类别分箱移动到另一个分箱的情况。以“医疗”作为特征（贷款用途）的预测从 2% 增加到 8%，而以“度假”为特征的预测从 23% 减少到 17%。

在这个例子中，与“医疗”相关的 KL 散度组件是 -0.028，且小于“度假”百分比变化的 0.070 组件。

一般来说，减少百分比并将其向 0 移动的变化对该统计量的影响大于百分比的增加。

![](../Images/5b4a8fcc6994cdbaa6158d4deaa6b24a.png)

作者提供的图像

相对于行业标准 0.2，要获得较大的变动，唯一的方法是将一个分箱向 0 移动。在这个例子中，将一个分箱从 9% 移动到 0.5% 会大幅改变 KL 散度。

[这里是一个电子表格](https://docs.google.com/spreadsheets/d/1BNjGmJJOZpKdOhHHskVl9yT5lhhyPfNaI6KbDmb3R9Q/edit?usp=sharing)，供那些想要操作和修改这些百分比以更好地理解直觉的人使用。值得注意的是，这种直觉与 PSI 的直觉非常不同。

# 结论

如果你考虑使用 KL 散度来衡量漂移，需要记住几件重要的事情。首先，大多数实践者发现使用 KL 散度的离散形式最为简单，并通过对数据进行分箱来获取离散分布（有关分箱挑战的更多信息将在未来的帖子中介绍）。其次，虽然理解 KL 散度背后的直觉和数学很重要，但有时其他度量如 PSI —— KL 散度的对称推导 —— 或其他方法可能更适合你的用例。
