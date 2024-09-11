# 高基数分类变量的混合效应机器学习 — 第I部分：不同方法的实证比较

> 原文：[https://towardsdatascience.com/mixed-effects-machine-learning-for-high-cardinality-categorical-variables-part-i-an-empirical-df0a43dce059?source=collection_archive---------11-----------------------#2023-07-07](https://towardsdatascience.com/mixed-effects-machine-learning-for-high-cardinality-categorical-variables-part-i-an-empirical-df0a43dce059?source=collection_archive---------11-----------------------#2023-07-07)

## 为什么随机效应对机器学习模型有用

[](https://medium.com/@fabsig?source=post_page-----df0a43dce059--------------------------------)[![Fabio Sigrist](../Images/f7bc2adc17255ae1efd0886a19ec202c.png)](https://medium.com/@fabsig?source=post_page-----df0a43dce059--------------------------------)[](https://towardsdatascience.com/?source=post_page-----df0a43dce059--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----df0a43dce059--------------------------------) [Fabio Sigrist](https://medium.com/@fabsig?source=post_page-----df0a43dce059--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb5b503a0c329&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmixed-effects-machine-learning-for-high-cardinality-categorical-variables-part-i-an-empirical-df0a43dce059&user=Fabio+Sigrist&userId=b5b503a0c329&source=post_page-b5b503a0c329----df0a43dce059---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----df0a43dce059--------------------------------) ·10分钟阅读·2023年7月7日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdf0a43dce059&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmixed-effects-machine-learning-for-high-cardinality-categorical-variables-part-i-an-empirical-df0a43dce059&source=-----df0a43dce059---------------------bookmark_footer-----------)![](../Images/e27c9e777e29c1064a414d99ae363e3b.png)

图1：二元数据和具有不同基数的分类变量中树增强方法（“GPBoost”与“LogitBoost”）的对比。**基数越高（即每个水平的样本数越少），随机效应模型的表现越好（即测试误差的减少）** — 图片由作者提供

高基数分类变量是指不同级别的数量相对于数据集的样本大小很大，换句话说，每个分类变量级别的数据点很少。机器学习方法可能在处理高基数变量时遇到困难。在本文中，我们认为随机效应是机器学习模型中建模高基数分类变量的有效工具。特别是，我们通过对比在多个具有高基数分类变量的表格数据集上，两种最成功的机器学习方法——树提升和深度神经网络——以及线性混合效应模型的不同版本。我们的结果表明，首先，具有随机效应的机器学习模型表现优于没有随机效应的模型，其次，具有随机效应的树提升优于具有随机效应的深度神经网络。

## **目录**

· [1 介绍](#1986)

· [2 高基数分类变量的随机效应建模](#9ecd)

· [3 使用真实数据集的不同方法比较](#ab90)

· [4 结论](#9bfc)

· [参考文献](#9aca)

# 1 介绍

处理分类变量的一种简单策略是使用独热编码或虚拟变量。但由于下面描述的原因，这种方法对于高基数分类变量通常效果不好。对于神经网络，一个常用的解决方案是实体嵌入 [Guo and Berkhahn, 2016]，它将分类变量的每个级别映射到低维欧几里得空间。对于树提升，一种简单的方法是给分类变量的每个级别分配一个数字，然后将其视为一维数值变量。在`LightGBM`提升库 [Ke et al., 2017] 中实现的替代方案通过使用近似方法 [Fisher, 1958] 将所有级别分成两个子集来进行树构建算法中的分裂。进一步地，`CatBoost`提升库 [Prokhorenkova et al., 2018] 实现了一种基于有序目标统计的方法，该方法通过使用训练数据的随机划分来处理分类预测变量。

# 2 高基数分类变量的随机效应建模

随机效应可以作为建模高基数分类变量的有效工具。在具有单一高基数分类变量的回归情况下，随机效应模型可以表示为

![](../Images/ba900ba4edc62246b80da8f488ab0c40.png)

其中 j=1,…,ni 是第 i 层内的样本索引，其中 ni 表示分类变量达到第 i 层的样本数量，而 i 表示层级，q 为分类变量的总层数。因此，总样本数为 n = n0 + n1 + … + nq。这样的模型也被称为混合效应模型，因为它包含了固定效应 F(xij) 和随机效应 bi。xij 是固定效应预测变量或特征。混合效应模型可以扩展到其他响应变量分布（例如，分类）和多个分类变量。

传统上，随机效应被用于线性模型中，其中假设 F 是一个线性函数。在过去几年中，线性混合效应模型已经扩展到使用随机森林 [Hajjem et al., 2014]、树提升 [Sigrist, [2022](https://www.jmlr.org/papers/v23/20-322.html), [2023a](https://ieeexplore.ieee.org/document/9759834)]，以及最近（从首次公开预印本的角度来看）深度神经网络 [Simchoni and Rosset, [2021](https://proceedings.neurips.cc/paper/2021/hash/d35b05a832e2bb91f110d54e34e2da79-Abstract.html), [2023](https://www.jmlr.org/papers/v24/22-0501.html)]。与经典的独立机器学习模型相比，随机效应引入了样本之间的依赖性。

**为什么随机效应对高基数分类变量有用？**

对于高基数分类变量，每个层级的数据很少。从直观上讲，如果响应变量在许多层级上具有不同的（条件）均值，传统的机器学习模型（例如，独热编码、嵌入或简单的一维数值变量）可能会在此类数据上出现过拟合或欠拟合问题。从经典的偏差-方差权衡的角度来看，独立的机器学习模型可能难以平衡这种权衡并找到适当的正则化量。例如，可能会发生过拟合，这意味着模型具有低偏差但高方差。

从广义上讲，随机效应作为先验或正则化器，建模函数的难点部分，即其“维度”类似于总样本大小，并且通过这种方式提供了一种在过拟合与欠拟合或偏差与方差之间找到平衡的有效方法。例如，对于单一的分类变量，随机效应模型将会将组拦截效应的估计值收缩向全局均值。这一过程有时也称为“信息汇聚”。它代表了完全忽略分类变量（= 欠拟合 / 高偏差和低方差）与在估计中给予分类变量的每个级别“完全自由”（= 过拟合 / 低偏差和高方差）之间的权衡。重要的是，正则化的程度，由模型的方差参数决定，是从数据中学习得到的。具体来说，在上述单级随机效应模型中，对于具有预测变量xp和分类变量级别i的样本，响应变量的（点）预测值由下式给出

![](../Images/a222d8c7f356c47b699dc508e821c912.png)

其中F(xp)是评估在xp处的训练函数，σ²_1和σ²是方差估计值，而yi和Fi分别是级别i下yij和F(xij)的样本均值。忽略分类变量会得到预测值yp = F(xp)，而一个完全灵活的无正则化模型给出的预测值是yp = F(xp) + (yi — Fi)。即，这两种极端情况与随机效应模型之间的差异是收缩因子σ²_1 / (σ²/ni + σ²_1)（如果级别i的样本数量ni很大，这个因子趋近于零）。与此相关，随机效应模型允许更高效（即更低方差）的固定效应函数F(.)的估计[Sigrist, 2022]。

根据这一论点，Sigrist [2023a，第4.1节]在实证实验中发现，树提升与随机效应结合（“GPBoost”）的表现优于经典的独立树提升（“LogitBoost”），尤其是在分类变量的每个级别样本数量较少，即分类变量的基数较高时。上述结果在图1中进行了再现。这些结果是通过模拟具有5000个样本的二分类数据、一个非线性预测函数和一个逐步增加级别的分类变量（即每个级别样本更少）获得的；有关更多细节，请参见Sigrist [2023a]。结果表明，GPBoost和LogitBoost的测试误差差异越大，当每个级别的样本数量越少（即级别数量越多）时。

# 3 使用真实数据集的不同方法比较

接下来，我们使用多个具有高卡方类别变量的真实数据集比较几种方法。我们使用Simchoni和Rosset [2021, 2023]提供的所有公开表格数据集，以及与Simchoni和Rosset [2021, 2023]相同的实验设置。此外，我们还包括Sigrist [2022]分析的Wages数据集。

我们考虑以下方法：

+   `Linear’：线性混合效应模型

+   `NN Embed’：具有嵌入的深度神经网络

+   `LMMNN’：结合深度神经网络和随机效应 [Simchoni和Rosset, 2021, 2023]

+   `LGBM_Num’：通过为每个类别变量的每个水平分配一个数字，并将这些数字视为一维数值变量的树提升

+   `LGBM_Cat’：使用`LightGBM` [Ke et al., 2017]的方法进行树提升

+   `CatBoost’：使用`CatBoost` [Prokhorenkova et al., 2018]的方法进行树提升

+   `GPBoost’：结合树提升和随机效应 [Sigrist, 2022, 2023a]

请注意，最近（版本1.6及以后），`XGBoost`库 [Chen和Guestrin, 2016] 也实现了与`LightGBM`相同的处理类别变量的方法。我们在这里不将其视为单独的方法。

我们使用以下数据集：

![](../Images/788213f92c9b8a0af74ff7efe79da785.png)

表1：数据集总结。n是样本数量，p是预测变量的数量（不包括高卡方类别变量），K是高卡方类别变量的数量，‘Cat. var.’描述类别变量 — 作者提供的图像

对于所有随机效应方法，我们包括表1中提到的每个类别变量的随机效应，随机效应之间没有先验相关性。Rossmann、AUImport和Wages数据集是纵向数据集。对于这些数据集，我们还包括线性和二次随机斜率；请参见本系列的[第三部分](https://medium.com/towards-data-science/mixed-effects-machine-learning-for-longitudinal-panel-data-with-gpboost-part-iii-523bb38effc)。有关数据集的更多详细信息，请参见Simchoni和Rosset [2021, 2023]以及Sigrist [2023b]。

我们对每个数据集进行5折交叉验证（CV），使用测试均方误差（MSE）来衡量预测准确性。有关实验设置的详细信息，请参见Sigrist [2023b]。数据预处理的代码以及下载数据和运行实验的代码可以在[这里](https://github.com/fabsig/Compare_ML_HighCardinality_Categorical_Variables)找到。对于许可证允许的原始数据源，建模所用的预处理数据也可以在上述网页找到。

![](../Images/e70115790a2114b16bdc770634c2be48.png)

图2：相对最低测试MSE的平均差异。由于不是所有方法都在Wages数据集上运行，因此该数据集未用于计算 — 作者提供的图像

结果汇总在图2中，该图显示了与最低测试均方误差（MSE）的平均相对差异。这是通过首先计算每个数据集的测试MSE与最低MSE的相对差异，然后对所有数据集进行平均得到的。详细结果可以在 [Sigrist [2023b]](https://arxiv.org/abs/2307.02071) 中找到。我们观察到，结合树提升和随机效应（GPBoost）具有最高的预测准确性，与最佳结果的平均相对差异约为7%。第二好的结果是由`LightGBM`（LGMB_Cat）的分类变量方法和具有随机效应的神经网络（LMMNN）获得，它们的平均相对差异约为17%。CatBoost和线性混合效应模型的表现则明显较差，与最佳方法的平均相对差异接近50%。鉴于 [CatBoost 特别设计用来处理分类特征](https://proceedings.neurips.cc/paper/2018/hash/14491b756b3a51daac41c24863285549-Abstract.html)，这令人有些失望。总体上，表现最差的是具有嵌入的神经网络，其与最佳结果的平均相对差异超过150%。将分类变量转换为一维数值变量的树提升（LGBM_Num）的表现略好一些，与最佳结果的平均相对差异约为100%。在其在线文档中，`LightGBM` 推荐 [*“对于高基数的分类特征，通常将特征视为数值特征效果最佳”（截至2023年7月6日）*](https://lightgbm.readthedocs.io/en/latest/Advanced-Topics.html#categorical-feature-support)。我们显然得出了不同的结论。

# 4 结论

我们在具有高基数分类变量的表格数据上对几种方法进行了实证比较。我们的结果显示，首先，具有随机效应的机器学习模型的表现优于没有随机效应的模型；其次，具有随机效应的树提升优于具有随机效应的深度神经网络。虽然可能有几种原因可以解释这一发现，但这与Grinsztajn等人 [2022] 的最新工作一致，他们发现树提升在没有高基数分类变量的表格数据上优于深度神经网络（以及随机森林）。类似地，Shwartz-Ziv和Armon [2022] 也得出结论，树提升“在表格数据上优于深度模型。”

在 [Part II](https://medium.com/towards-data-science/mixed-effects-machine-learning-for-high-cardinality-categorical-variables-part-ii-gpboost-3bdd9ef74492) 中，我们将展示如何使用 `[GPBoost](https://github.com/fabsig/GPBoost)` [库](https://github.com/fabsig/GPBoost) 进行演示，使用上述提到的真实数据集之一。在 [Part III](https://medium.com/towards-data-science/mixed-effects-machine-learning-for-longitudinal-panel-data-with-gpboost-part-iii-523bb38effc) 中，我们将展示如何使用 `GPBoost` 库来建模纵向数据，即面板数据。

# 参考文献

+   T. Chen 和 C. Guestrin. XGBoost：一个可扩展的树提升系统。发表于第22届 ACM SIGKDD 国际知识发现与数据挖掘会议，页码 785–794。ACM，2016年。

+   W. D. Fisher. 论最大同质性分组。美国统计协会期刊，53(284):789–798，1958年。

+   L. Grinsztajn, E. Oyallon 和 G. Varoquaux. 为什么基于树的模型在典型的表格数据上仍然优于深度学习？在 S. Koyejo, S. Mohamed, A. Agarwal, D. Belgrave, K. Cho 和 A. Oh 编辑的《神经信息处理系统进展》卷35，页码 507–520。Curran Associates, Inc., 2022年。

+   C. Guo 和 F. Berkhahn. 分类变量的实体嵌入。arXiv 预印本 arXiv:1604.06737，2016年。

+   A. Hajjem, F. Bellavance 和 D. Larocque. 用于聚类数据的混合效应随机森林。统计计算与模拟期刊，84(6):1313–1328，2014年。

+   G. Ke, Q. Meng, T. Finley, T. Wang, W. Chen, W. Ma, Q. Ye 和 T.-Y. Liu. LightGBM：一种高效的梯度提升决策树。发表于神经信息处理系统进展，页码 3149–3157，2017年。

+   L. Prokhorenkova, G. Gusev, A. Vorobev, A. V. Dorogush 和 A. Gulin. CatBoost：具有分类特征的无偏提升。发表于神经信息处理系统进展，页码 6638–6648，2018年。

+   R. Shwartz-Ziv 和 A. Armon. 表格数据：深度学习并不是你所需要的一切。信息融合，81:84–90，2022年。

+   F. Sigrist. 高斯过程提升。机器学习研究期刊，23(1):10565–10610，2022年。

+   F. Sigrist. 潜在高斯模型提升。IEEE 模式分析与机器智能学报，45(2):1894–1905，2023a年。

+   F. Sigrist. 针对高维分类变量的数据的机器学习方法比较。arXiv 预印本 arXiv::2307.02071 2023b。

+   G. Simchoni 和 S. Rosset. 使用随机效应来考虑深度神经网络中的高维分类特征和重复测量。神经信息处理系统进展，34:25111–25122，2021年。

+   G. Simchoni 和 S. Rosset. 深度神经网络中的随机效应整合。机器学习研究期刊，24(156):1–57，2023年。

+   [https://github.com/fabsig/Compare_ML_HighCardinality_Categorical_Variables](https://github.com/fabsig/Compare_ML_HighCardinality_Categorical_Variables)
