# R 中的多项逻辑回归

> 原文：[`towardsdatascience.com/multinomial-logistic-regression-in-r-428d9bb7dc70`](https://towardsdatascience.com/multinomial-logistic-regression-in-r-428d9bb7dc70)

## R 中的统计系列

[](https://mdsohel-mahmood.medium.com/?source=post_page-----428d9bb7dc70--------------------------------)![Md Sohel Mahmood](https://mdsohel-mahmood.medium.com/?source=post_page-----428d9bb7dc70--------------------------------)[](https://towardsdatascience.com/?source=post_page-----428d9bb7dc70--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----428d9bb7dc70--------------------------------) [Md Sohel Mahmood](https://mdsohel-mahmood.medium.com/?source=post_page-----428d9bb7dc70--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----428d9bb7dc70--------------------------------) ·6 分钟阅读·2023 年 1 月 29 日

--

![](img/4b92b97f4d508c05364527387fd1e0bd.png)

图片由 [Edge2Edge Media](https://unsplash.com/@edge2edgemedia?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/uKlneQRwaxY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> **介绍**

如果你阅读了我之前关于“R 中的统计系列”的文章，你可能对 R 中的逻辑回归实现有了较好的理解，也对不同类型的逻辑回归模型有了基本了解。我们在回归领域已经走了很长一段路，覆盖了二项、比例优势 (PO)、广义以及部分比例优势 (PPO) 模型。在这篇文章中，我将讨论多项逻辑回归，它是特定应用的，并且可能对理解具有多个无序响应变量的逻辑回归模型至关重要。例如，可能出现的回归类型是人们对政党的归属，例如共和党、民主党、独立党等。

> **简要回顾**

首先，我们来回顾一下我们已经覆盖的内容。起初，我们介绍了二项逻辑回归，它没有有序响应。在这里，我们只有两个类别，例如健康或不健康。接着，我们深入探讨了具有多个有序响应的有序逻辑回归模型。在这里，我们对健康状态进行了更多有序分类，并将其输入到有序模型中。该模型的一个基本假设是系数独立于类别，并且它们在响应之间不变化。例如，如果我们有四个健康状态类别，并按 1、2、3、4 排列。我们假设每个独立变量的系数在这四个类别中保持不变。这被称为比例优势 (PO) 假设。

后来我们进入了广义逻辑模型，并允许所有系数在各类别之间变化。例如，当我们考虑健康状态从 1 变为 2 时，某个特定自变量的系数将与健康状态从 2 变为 3 时的系数值不同。

之前讨论过的另一种模型是部分比例奇数（PPO）模型。在这种模型中，仅允许违反比例奇数假设的变量在各类别之间变化。例如，如果我们将健康状态作为响应变量，教育年限、婚姻状态和家庭收入作为预测变量，如果我们发现只有教育状态违反了比例奇数假设，我们可以为此提出 PPO 模型，而不是广义有序回归模型。我知道这可能听起来有点复杂，但如果你查看我之前关于“R 语言统计系列”的文章，希望事情会变得更加易懂。目前，下面的表格展示了我们在模型定义和执行方面所涵盖的内容。

![](img/8f9e1f54afdfc6343b3f9550100b6068.png)

> 什么是多项式回归？

简单来说，多项式回归模型通过对数比或对数赔率方法估计个体落入特定类别的可能性，相对于基准类别。它类似于当名义响应变量有超过 2 个结果时对二项分布的扩展。在多项式回归中，我们需要定义一个参考类别，模型将确定相对于参考类别的多个二项分布参数。

在我们的示例中，我们将设置一个特定的健康状态作为基准类别，并以此进行多项式逻辑回归。简言之，多项式回归就像一次性执行多个二项回归模型。

> **数据集**

[UCI 仓库中的成人数据集](https://archive.ics.uci.edu/ml/datasets/adult)将再次用于多项式逻辑回归的实现。下面是数据集的快速查看。

![](img/2cfd35028c97cea40f1df9dcfbacd968.png)

成人数据集来自 [UCI 仓库](https://archive.ics.uci.edu/ml/datasets/adult)。

+   教育：数值型和连续型

+   婚姻状态：二元（0 为未婚，1 为已婚）

+   性别：二元（0 为女性，1 为男性）

+   家庭收入：二元（0 为平均或低于平均，1 为高于平均）

+   健康状态：有序（1 为差，2 为一般，3 为好，4 为优秀）

在这里，尽管健康状态是有序的，但我们实际上不需要它，因为我们将执行多个二项逻辑回归。

> **R 语言中的实现**

要实现多项式逻辑回归，我将使用 VGAM 包中的 vglm() 命令。代码片段如下。

如前所述，我们需要定义一个参考类别。在这里，我将健康状态水平=1 定义为参考类别。

> **结果解释**

![](img/d9ea6eb6bf81a219bb8797dc2cbc96ca.png)

模型总结

好的。我们已经总结了上述定义的模型。这是一个具有多个结果类别的预测模型。预测变量是教育年限（连续变量），结果是类别变量，实际上是有序的，但我们不需要类似的有序响应。我们可以简单地将数值分配给这些类别。

总结与二元逻辑回归类似，但我们有多个二元回归模型，所有模型都与参考类别进行比较。让我们参考上述系数块。

第一个模型使用 intercept1 和 educ1 比较类别 2 与参考类别 1（模型定义中已定义）。第二个模型使用 intercept2 和 educ2 比较类别 3 与参考类别 1。第三个模型使用 intercept3 和 educ3 比较类别 4 与参考类别 1。由于有四个类别，因此只能进行三种二元比较。

model1:

![](img/902ddfa90cb1c4dac5e703380e7f671a.png)

model2:

![](img/1ffe499a83809e51c16934216d161aa4.png)

model3:

![](img/332333c92603a9ddf76c5ba018e7829a.png)

相同的二元逻辑回归解释也适用于此。例如，在 model1 中，我们可以说，每增加一个单位的 educ 变量，类别 2 相对于类别 1 的对数几率增加 0.08969，其余两个模型也是如此。所有情况下的斜率都是正的，因为随着教育水平的提高，具有更好健康状态的对数几率也会增加。这是显而易见的，但我们通过给定的数据集量化了这一事实。

R 还对每个预测变量进行了假设检验。原假设声明预测变量在预测对数几率中不是显著变量。educ1 的相关 p 值为 0.00853，小于 0.01。因此，我们可以拒绝原假设，并得出结论，educ1 在确定类别 2 相对于参考类别 1 的对数几率中发挥了显著作用。

我们还可以在 R 中确定奇数比，如下所示。

![](img/0c4827519038f33867b9d488edecc8ed.png)

奇数比

这基本上是系数的指数值。这告诉我们，每增加一个单位的 educ，处于类别 2 的几率相比于类别 1 增加了 1.09350 倍。换句话说，每增加一个单位的 educ，处于类别 2 的几率增加了 9.35%。

对于其他模型，百分比变得显著。每增加一个单位的教育，处于类别 3 的概率增加 25.23%，而每增加一个单位的教育，处于类别 4 的概率增加 35.82%，相对于参考类别 1。

> **结论**

在这篇文章中，我讨论了多项逻辑回归模型的必要性，并在 R 中进行了实现。这种回归类型类似于二项回归，只是我们进行的是多个二元比较。当我们有像政党归属这样的无序类别时，可以进行多项逻辑回归。

> **数据集致谢**

[Dua, D. 和 Graff, C. (2019). UCI 机器学习库 [http://archive.ics.uci.edu/ml]。加州尔湾：加州大学信息与计算机科学学院 (CC BY 4.0)](https://archive.ics.uci.edu/ml/datasets/adult)

[](https://mdsohel-mahmood.medium.com/membership?source=post_page-----428d9bb7dc70--------------------------------) [## 使用我的推荐链接加入 Medium - Md Sohel Mahmood

### 阅读 Md Sohel Mahmood（以及 Medium 上的其他成千上万名作者）的每一篇故事。您的会员费用将直接...

[获取 Md Sohel Mahmood 发布时的电子邮件通知](https://mdsohel-mahmood.medium.com/membership?source=post_page-----428d9bb7dc70--------------------------------) [](https://mdsohel-mahmood.medium.com/subscribe?source=post_page-----428d9bb7dc70--------------------------------) [## 订阅 Md Sohel Mahmood 的所有故事

### 每当 Md Sohel Mahmood 发布新内容时，你将收到电子邮件通知。通过注册，如果你还没有 Medium 帐户，将创建一个...

[订阅 Medium 使用我的推荐链接 - Md Sohel Mahmood](https://mdsohel-mahmood.medium.com/subscribe?source=post_page-----428d9bb7dc70--------------------------------)
