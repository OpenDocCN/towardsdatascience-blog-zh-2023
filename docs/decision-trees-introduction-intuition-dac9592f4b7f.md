# 决策树：介绍与直观理解

> 原文：[`towardsdatascience.com/decision-trees-introduction-intuition-dac9592f4b7f`](https://towardsdatascience.com/decision-trees-introduction-intuition-dac9592f4b7f)

## 使用 Python 做数据驱动决策

[](https://shawhin.medium.com/?source=post_page-----dac9592f4b7f--------------------------------)![Shaw Talebi](https://shawhin.medium.com/?source=post_page-----dac9592f4b7f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dac9592f4b7f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dac9592f4b7f--------------------------------) [Shaw Talebi](https://shawhin.medium.com/?source=post_page-----dac9592f4b7f--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dac9592f4b7f--------------------------------) ·阅读时间 10 分钟·2023 年 2 月 10 日

--

![](img/13f81f75cc709437ff2192216ceadf8c.png)

图片由 [niko photos](https://unsplash.com/@niko_photos?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)，并配有思考的表情符号。

这是关于决策树系列文章中的第一篇。在这篇文章中，我介绍了决策树，并描述了如何使用数据来*生成*它们。文章最后包含了示例 Python 代码，展示了如何创建和使用决策树来帮助进行医学预测。

## 重点：

+   决策树是一种广泛使用且直观的机器学习技术，用于解决预测问题。

+   我们可以从数据中*生成*决策树。

+   超参数调整可以用来帮助避免*过拟合*问题。

# **什么是决策树？**

**决策树**是一种**广泛使用且直观的机器学习技术**。通常，它们**用于解决预测问题**。例如，预测明天的天气预报或估算一个人患心脏病的概率。

决策树通过一系列是非问题进行工作，这些问题用于缩小可能的选择范围并得出结果。下面展示了一个简单的决策树示例。

![](img/19d95fea62f210ec0bc0b398fdea68a8.png)

示例决策树预测我是否会喝茶或咖啡。图片由作者提供。

如上图所示，决策树由通过有向边连接的节点组成。决策树中的每个节点对应于一个基于预测变量的条件语句。

上面显示的决策树的顶部是**根节点**，它**设置了数据记录的初始分裂**。在这里，我们评估时间是否在下午 4 点之后。每个可能的响应（是或否）在树中遵循不同的路径。

如果是，我们沿着左侧分支前进，最终到达一个**叶节点**（也称为终端节点）。**在这种类型的节点上不需要进一步分裂来确定结果**。在这种情况下，我们选择茶而不是咖啡，以便能在合理的时间上床睡觉。

相反，如果时间是下午 4 点或更早，我们会沿着右侧分支前进，最终到达一个所谓的**分裂节点**。这些节点**进一步分割数据记录**，基于条件语句进行划分。接下来，我们评估昨晚的睡眠时间是否超过 6 小时。如果是，我们继续选择茶，但如果不是，我们则选择咖啡☕️。

## 使用决策树

在实际操作中，我们通常不会像刚才那样使用决策树（即查看决策树并跟随特定数据记录）。相反，我们让计算机为我们评估数据。我们只需将所需数据以表格形式提供给计算机即可。

下面是一个示例。这里我们有两个变量的表格数据：时间和前一晚的睡眠小时数（蓝色列）。然后，使用上述决策树，我们可以为每条记录分配适当的含咖啡因饮料（绿色列）。

![](img/5fcefe3250fcb94b3929efba85702486.png)

输入数据的示例表格及其生成的决策树预测。图片由作者提供。

## **决策树的图形视图**

另一种思考决策树的方法是**图形化**。*（这是我个人对决策树的直观理解。）*

想象一下，我们将示例决策树中的两个预测变量绘制在一个二维图上。然后，我们可以将决策树的分裂表示为将图划分为不同区域的线条。这样，我们可以通过简单地查看数据点所在的象限来确定饮料选择。

从直观上看，这就是决策树的作用。**将预测空间划分为不同的部分，并为每个部分分配一个标签（或概率）**。

![](img/1dca41b0e04b37add727a37582954df4.png)

决策树对茶或咖啡的预测的图形视图。图片由作者提供。

# **如何构建决策树？**

决策树是一种直观的数据划分方法。然而，使用数据手动绘制一个合适的决策树可能并不容易。在这种情况下，**我们可以使用机器学习**策略来学习适用于给定数据集的“最佳”决策树。

数据可以用于在一种称为**训练**的优化过程中构建决策树。训练需要一个训练数据集，其中的预测变量预先标记了目标值。

一种标准的训练决策树的策略使用被称为**贪婪搜索**的方法。这是一种在优化中流行的技术，我们通过找到*局部*最优解来简化更复杂的优化问题，而不是*全局*最优解。(*我在之前的一篇关于* [*因果发现*](https://medium.com/towards-data-science/causal-discovery-6858f9af6dcb)*的文章中给出了贪婪搜索的直观解释。*)

在决策树的情况下，贪婪搜索确定每个可能的分裂选项的**增益**，然后选择提供最大增益的那个选项[1,2]。这里的“**增益**”由**分裂准则**决定，这可以基于几种不同的量度，例如**基尼 impurity**、**信息增益**、**均方误差 (MSE)**等。这个过程会递归地重复，直到决策树完全生成。

例如，如果使用基尼 impurity，数据记录会递归地分成两个组，以使**结果组的加权平均 impurity 最小化**。这个分裂过程可以继续，直到所有数据分区都是*纯净的*，即每个分区中的所有数据记录都对应于一个单一的目标值。

尽管这意味着决策树可以是*完美*的估计器，但这种方法可能会导致**过拟合**。训练好的**决策树在与训练数据集有显著不同的数据上表现不佳**。

## **超参数调优**

对抗过拟合问题的一种方法是超参数调优。**超参数**是**限制决策树增长的值**。

常见的决策树超参数包括最大分裂次数、最小叶子节点大小和分裂变量的数量。设置决策树超参数的**关键结果**是**限制树的大小**，这有助于避免过拟合并提高泛化能力。

## **替代训练策略**

虽然我上述描述的训练过程在决策树中广泛使用，但我们可以使用其他替代方法。

**剪枝**——一种这样的方式叫做剪枝[3]。从某种意义上讲，剪枝是决策树生长的反面。我们不是从根节点开始递归地添加节点，而是从完全生成的树开始，逐步去除节点。

虽然剪枝过程可以通过多种方式进行，但通常会删除那些不会显著增加模型误差的节点。这是一种替代超参数调优来限制树的生长以避免过拟合的方法[3]。

**最大似然**——我们可以使用最大似然框架训练决策树[4]。虽然这种方法不太为人知，但它基于一个强大的理论框架。它允许我们使用信息准则如 AIC 和 BIC 来客观优化树中的参数数量及其性能，这有助于避免广泛的超参数调优。

# 示例代码：使用决策树进行脓毒症生存预测

现在，了解了决策树的基本概念以及如何从数据中构建决策树后，让我们使用 Python 深入探讨一个具体的例子。在这里，我们将使用来自[UCI 机器学习库](https://archive.ics.uci.edu/ml/datasets/Sepsis+survival+minimal+clinical+records)的数据集来训练一个决策树，以预测患者是否会生存，基于他们的年龄、性别和经历的脓毒症发作次数[5,6]。

对于决策树的训练，我们将使用 sklearn Python 库[7]。此示例的代码可以在[GitHub 仓库](https://github.com/ShawhinT/YouTube-Blog/tree/main/decision-tree/decision_tree)中自由获取。

我们从导入一些有用的库开始。

```py
# import modules
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

from sklearn.model_selection import train_test_split
from sklearn import tree
from sklearn.metrics import confusion_matrix, ConfusionMatrixDisplay
from sklearn.metrics import precision_score, recall_score, f1_score
from imblearn.over_sampling import SMOTE
```

接下来，我们从.csv 文件中加载数据并进行一些数据准备。

```py
# read data from csv
df = pd.read_csv('raw/s41598-020-73558-3_sepsis_survival_primary_cohort.csv')

# look at data distributions
plt.rcParams.update({'font.size': 16})

# plot histograms
df.hist(figsize=(12,8))
```

![](img/fea3c309833e691f553f7aaf79ce4e0e.png)

数据集中每个变量的直方图。图片由作者提供。

请注意，在右下角的直方图中，我们有更多的存活记录而非死亡记录。这被称为**不平衡数据集**。对于一个简单的决策树分类器，从不平衡的数据中学习可能会导致**决策树*过度预测*多数类**。

为了处理这种情况，我们可以通过过采样少数类来使数据更平衡。一种方法是使用一种叫做**合成少数类过采样技术**（**SMOTE**）的技术。虽然我会在未来的文章中详细介绍 SMOTE，但目前只需知道这有助于我们平衡数据并改进决策树模型即可。

```py
# Balance data using SMOTE

# define predictor and target variable names
X_var_names = df.columns[:3]
y_var_name = df.columns[3]

# create predictor and target arrays
X = df[X_var_names]
y = df[y_var_name]

# oversample minority class using smote
X_resampled, y_resampled = SMOTE().fit_resample(X, y)

# plot resulting outcome histogram
y_resampled.hist(figsize=(6,4))
plt.title('hospital_outcome_1alive_0dead \n (balanced)')
```

![](img/045d096e7d85b27e2f9ba1701d3cb90b.png)

SMOTE 之后的结果直方图。图片由作者提供。

数据准备的最后一步是将我们的重采样数据拆分为训练和测试数据集。**训练数据**将用于**构建决策树**，而**测试数据**将用于**评估其性能**。这里我们使用 80–20 的训练-测试拆分。

```py
# create train and test datasets
X_train, X_test, y_train, y_test = \
      train_test_split(X_resampled, y_resampled, test_size=0.2, random_state=0)
```

现在有了训练数据，我们可以创建我们的决策树。Sklearn 使这一过程非常简单，仅用两行代码，我们就能得到一个决策树。

```py
# Training
clf = tree.DecisionTreeClassifier(random_state=0)
clf = clf.fit(X_train, y_train)
```

让我们来看一下结果。

```py
# Display decision tree
plt.figure(figsize=(24,16))

tree.plot_tree(clf)
plt.savefig('visuals/fully_grown_decision_tree.png',facecolor='white',bbox_inches="tight")
plt.show()
```

![](img/cad6d35f2ad5a40779dfc1f54b3ee1e9.png)

完全生长的决策树。图片由作者提供。

不用说，这是一棵非常大的决策树，这可能会使解释结果变得困难。然而，让我们暂时搁置这一点，评估模型的性能。

为了评估性能，我们使用**混淆矩阵**，**它显示了真正例（TP）、真负例（TN）、假正例（FP）和假负例（FN）的数量**。

我不会在这里讨论混淆矩阵，但目前**我们希望**的是**对角线上的数字要大**，而**非对角线上的数字要小**。

![](img/847c78b12ad89d7bf2bf0e5f6bc326b9.png)

完全生长的决策树的混淆矩阵。（左）训练数据集。（右）测试数据集。图片由作者提供。

我们可以从混淆矩阵中获取数据，并计算三个不同的性能指标：精确度、召回率和 f1-score**。** 简单来说，**精确度** = TP / (TP + FP)，**召回率** = TP / (TP + FN)，**f1-score** 是精确度和召回率的调和均值。

![](img/867103da82f0cb2b241406a704a3ed82.png)

完全成长的决策树的三个性能指标。 图片由作者提供。

生成这些结果的代码如下。

```py
# Function to plot confusion matrix and print precision, recall, and f1-score
def evaluateModel(clf, X, y):

    # confusion matrix
    y_pred = clf.predict(X)
    cm = confusion_matrix(y, y_pred)
    cm_disp = ConfusionMatrixDisplay(confusion_matrix=cm, display_labels=['dead', 'alive'])
    cm_disp.plot()

    # print metrics
    print("Precision = " + str(np.round(precision_score(y, y_pred),3)))
    print("Recall = " + str(np.round(recall_score(y, y_pred),3)))
    print("F1 = " + str(np.round(f1_score(y, y_pred),3)))
```

## 超参数调整

虽然决策树在这里使用的数据上表现不错，但仍然存在**可解释性**的问题。查看之前展示的决策树，临床医生从决策树的逻辑中提取有意义的见解将是具有挑战性的。

这就是超参数调整可以发挥作用的地方。为了使用 sklearn 完成这一点，我们只需在决策树训练步骤中添加输入参数即可。

在这里我们将尝试设置 max_depth = 3。

```py
# train model with max_depth set to 3
clf_tuned = tree.DecisionTreeClassifier(random_state=0, max_depth=3)
clf_tuned = clf_tuned.fit(X_train, y_train)
```

现在，让我们看看结果决策树。

![](img/f9b0eaf963d0a43c1b277088afe9ea3f.png)

调整后的决策树，max_depth=3。 图片由作者提供。

由于我们限制了树的最大深度，我们可以清楚地看到这里发生了哪些分裂。

我们再次使用混淆矩阵和之前相同的三个性能指标来评估模型的表现。

![](img/9540e446233230f2be271a1a4f1045aa.png)

超参数调整决策树的混淆矩阵。 （左）训练数据集。 （右）测试数据集。 图片由作者提供

![](img/0246f2318fcfd809c78fd1598db48ef0.png)

超参数调整后的决策树的三个性能指标。 图片由作者提供。

虽然完全成长的树可能看起来比超参数调整的树更可取，但这回到了过拟合的讨论。是的，完全成长的树在当前数据上的表现更好，但我不认为这适用于其他数据。

换句话说，**虽然简单的决策树在这里的表现较差，但它可能比完全成长的树更具泛化能力。**

这个假设可以通过将每个模型应用于 GitHub [repo](https://github.com/ShawhinT/YouTube-Blog/tree/main/decision-tree/decision_tree) 中的其他两个数据集来进行测试。

[](https://github.com/ShawhinT/YouTube-Blog/tree/main/decision-tree/decision_tree?source=post_page-----dac9592f4b7f--------------------------------) [## YouTube-Blog/decision-tree/decision_tree at main · ShawhinT/YouTube-Blog

### 代码补充了 YouTube 视频和 Medium 上的博客文章。 - YouTube-Blog/decision-tree/decision_tree at main ·…

github.com](https://github.com/ShawhinT/YouTube-Blog/tree/main/decision-tree/decision_tree?source=post_page-----dac9592f4b7f--------------------------------)

## 决策树集成

虽然超参数调优可以提高决策树的泛化能力，但在性能方面仍有不足。在上面的例子中，经过超参数调优后，决策树仍然将训练数据**35%**的时间标记错误，这在讨论生死问题时（*如此处的例子所示*）是一个大问题。

解决这个问题的一种流行方法是使用**多个决策树而不是单一的决策树来进行预测**。这些被称为**决策树集成**，将成为本系列的[下一篇文章](https://medium.com/towards-data-science/10-decision-trees-are-better-than-1-719406680564)的主题。

[](/10-decision-trees-are-better-than-1-719406680564?source=post_page-----dac9592f4b7f--------------------------------) ## 10 决策树比 1 个更好

### 解析 bagging、boosting、随机森林和 AdaBoost

towardsdatascience.com

# 资源

**联系**: [我的网站](https://shawhintalebi.com/) | [预约电话](https://calendly.com/shawhintalebi)

**社交媒体**: [YouTube 🎥](https://www.youtube.com/channel/UCa9gErQ9AE5jT2DZLjXBIdA) | [LinkedIn](https://www.linkedin.com/in/shawhintalebi/) | [Twitter](https://twitter.com/ShawhinT)

**支持**: [请我喝咖啡](https://www.buymeacoffee.com/shawhint) ☕️

[](https://shawhin.medium.com/subscribe?source=post_page-----dac9592f4b7f--------------------------------) [## 免费获取我写的每个新故事

### 免费获取我写的每个新故事 P.S. 我不会将你的邮箱与任何人分享。通过注册，你将创建一个…

shawhin.medium.com](https://shawhin.medium.com/subscribe?source=post_page-----dac9592f4b7f--------------------------------)

[1] [*分类与回归树* 由 Breiman 等人著](https://doi.org/10.1201/9781315139470)

[2] [决策树：Kotsiantis, S. B. 最近的概述](https://link.springer.com/article/10.1007/s10462-011-9272-4)

[3] [Esposito 等人对剪枝决策树方法的比较分析](https://doi.org/10.1109/34.589207)

[4] [Su 等人提出的最大似然回归树](https://doi.org/10.1198/106186004X2165)

[5] Dua, D. 和 Graff, C. (2019). [UCI 机器学习库](https://archive.ics.uci.edu/ml/datasets/census+income) [http://archive.ics.uci.edu/ml]。加利福尼亚州尔湾: 加州大学信息与计算机科学学院。（CC BY 4.0）

[6] [Chicco 和 Jurman 通过年龄、性别和脓毒症发作次数来预测脓毒症患者的生存率](https://www.nature.com/articles/s41598-020-73558-3)

[7] [Scikit-learn: Python 中的机器学习](https://jmlr.csail.mit.edu/papers/v12/pedregosa11a.html)，Pedregosa *等人*，JMLR 12，2825–2830 页，2011 年。
