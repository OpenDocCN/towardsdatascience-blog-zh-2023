# 对AUC和Harrell’s C的直观理解

> 原文：[https://towardsdatascience.com/get-an-intuition-for-auc-and-harrells-c-18df56220969?source=collection_archive---------7-----------------------#2023-09-15](https://towardsdatascience.com/get-an-intuition-for-auc-and-harrells-c-18df56220969?source=collection_archive---------7-----------------------#2023-09-15)

## 图形化方法

[](https://elena-jolkver.medium.com/?source=post_page-----18df56220969--------------------------------)[![Elena Jolkver](../Images/ec9e69fb674ce968174886dfa48ed292.png)](https://elena-jolkver.medium.com/?source=post_page-----18df56220969--------------------------------)[](https://towardsdatascience.com/?source=post_page-----18df56220969--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----18df56220969--------------------------------) [Elena Jolkver](https://elena-jolkver.medium.com/?source=post_page-----18df56220969--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd6af391a93a9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fget-an-intuition-for-auc-and-harrells-c-18df56220969&user=Elena+Jolkver&userId=d6af391a93a9&source=post_page-d6af391a93a9----18df56220969---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----18df56220969--------------------------------) ·6分钟阅读·2023年9月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F18df56220969&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fget-an-intuition-for-auc-and-harrells-c-18df56220969&user=Elena+Jolkver&userId=d6af391a93a9&source=-----18df56220969---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F18df56220969&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fget-an-intuition-for-auc-and-harrells-c-18df56220969&source=-----18df56220969---------------------bookmark_footer-----------)![](../Images/863341544228b533c5ea23dd56dbdb87.png)

作者拍摄的照片

每个进入机器学习或预测建模领域的人都会遇到模型性能测试的概念。教科书通常仅在读者首先学习的内容上有所不同：回归及其均方误差（MSE），还是分类及其众多性能指标，如准确率、灵敏度或精确度等。尽管准确率可以简单地通过正确/错误预测的比率来计算，因此非常直观，但ROC AUC初看可能会让人感到困惑。然而，它也是评估预测器质量的常用参数。让我们首先解开其机制，以了解细节。

# 首先了解AUC

假设我们建立了一个二分类器来预测样本属于某个类别的概率。我们的测试数据集具有已知类别，得到了以下结果，这些结果可以总结在混淆矩阵中，并在表格中详细报告，其中样本按预测为 P（正例）类别的概率排序。

![](../Images/730deb60b522213f6c9455ab92ad3ecb.png)

混淆矩阵和详细预测表，显示了各个样本的概率。图片由作者提供。

ROC AUC 定义为 ROC（接收者操作特征）曲线下的面积。ROC 曲线是将真实正率（TPR）与假正率（FPR）绘制在一起的图 [[Wikipedia](https://en.wikipedia.org/wiki/Receiver_operating_characteristic)]。TPR（也称为敏感性）是正确识别的正例数与所有正例数的比率。在我们的例子中，TPR 计算为 4/5（五个样本中有四个被正确分类为正例）。FPR 计算为错误分类为正例的负例数与实际负例总数的比率。在我们的例子中，FPR 计算为 2/6（如果我们将“阳性”阈值设置为 0.5，那么六个负样本中有两个被误分类为正例）。

我们可以根据 TPR 和 FPR 值绘制 ROC 曲线，并计算 AUC（曲线下面积）：

![](../Images/16259017e360e5c029118e0ac5e2df98.png)

基于预测概率的 ROC 曲线。图片由作者提供。

个别 TPR/FPR 值是如何得到的？为此，我们考虑我们的概率表并计算每个样本的 TPR/FPR，将样本被视为正例的概率设置为表中给出的值。即使我们超过了通常的 0.5 阈值（样本通常被视为“负例”），我们仍继续将其视为正例。让我们在例子中遵循这一过程：

![](../Images/acf683b88a386589d3518851c7e7f96f.png)

图片由作者提供

在阈值 0.81 时，五个正例中的一个被正确分类为正例，没有样本被预测为负例。我们继续，直到遇到第一个负例：

![](../Images/4a127b56f2b430a3c55e884228fddb2c.png)

图片由作者提供

在这里，我们的 TPR 停滞在之前的值（五个正例中的三个被正确预测），但 FPR 增加，我们错误地将六个负样本中的一个分配给了正类。我们继续直到最后：

![](../Images/6c1c1f15c5819c1d6ad87eea7eee76c9.png)

图片由作者提供

Voilà：我们得到了用于创建 ROC 曲线的完整表格。

# 为什么 Harrell 的 C 只是 AUC

那么 Harrell 的 C 指数（也称为一致性指数或 C 指数）呢？考虑预测特定疾病（例如癌症）发生后的死亡任务。最终，所有患者都会死，无论是否患有癌症——一个简单的二分类器帮助不大。生存模型考虑了结果（死亡）发生的持续时间。事件发生越早，个体遇到结果的风险越高。如果你要评估生存模型的质量，你会查看 C 指数（也称为一致性、也称为 Harrell 的 C）。

为了理解 C 指数的计算，我们需要引入两个新概念：允许对和一致对。允许对是指在观察期间有不同结果的样本对（例如：患者），即在实验进行时，其中一个患者经历了结果，而另一个则被删失（即尚未达到结果）。然后分析这些允许对，查看具有较高风险评分的个体是否经历了事件，而删失的个体没有。这些情况称为一致对。

简化一点，C 指数计算为一致对数量与允许对数量的比率（为了简化，我省略了风险相等的情况）。让我们通过一个例子来说明，假设我们使用了一个计算风险而不是概率的生存模型。以下表格仅包含允许对。如果具有较高风险评分的患者经历了事件（属于我们的“积极”组），则“Concordance”列设置为1。id 只是之前表格中的行号。特别注意个体 4 与 5 或 7 的比较。

![](../Images/fd387b2d9378b7e7041c43bfdc4b4793.png)

作者提供的图片

这使我们在 30 个允许对中得到 27 个一致对。比率（简化的 Harrell 的 C）是 C = 0.9，这让我们不禁想起之前计算的 AUC。

我们可以构建一个一致性矩阵，来可视化 C 统计量的计算过程，如 [Carrington et al](https://doi.org/10.1186/s12911-019-1014-6) 所建议的那样。该图显示了实际正例的风险评分与实际负例的风险评分，并显示了所有对（绿色 + 红色）中正确排名对（绿色）的比例，如果我们将每个网格方块视为样本的表示：

![](../Images/ff9123665080b7d1a50219e7c44f331a.png)

Harrell 的 C 计算的一致性矩阵。作者提供的图片

一致性矩阵显示了底部右侧的正确排名对、顶部左侧的错误排名对，以及正好对应于我们之前看到的 ROC 曲线的边界。

在拆解构建 ROC 曲线和一致性矩阵的过程中，我们发现了一个相似点：在这两种情况下，我们都根据样本的概率/风险得分对其进行排序，并检查排序是否与实际情况相符。我们设置的分类概率阈值越高，假阳性越多。实际阳性病例的风险越低，实际阴性病例被误分类为阳性的可能性就越大。根据我们的排名数据绘制曲线，会得到形状和面积相同的曲线，这就是我们称之为 AUC 或 Harrell’s C 的曲线，具体取决于上下文。

我希望这个例子有助于培养对 AUC 和 Harrell’s C 的直观理解。

# 致谢

比较这两个参数的想法来源于一次富有成效的讨论，讨论发生在高级机器学习学习小组的聚会中，致敬 Torsten！

***参考文献***: Carrington, A.M., Fieguth, P.W., Qazi, H. 等人。用于评估机器学习算法中的不平衡数据的新一致部分 AUC 和部分 c 统计量。*BMC Med Inform Decis Mak* ***20***, 4 (2020)。[*https://doi.org/10.1186/s12911-019-1014-6*](https://doi.org/10.1186/s12911-019-1014-6)
