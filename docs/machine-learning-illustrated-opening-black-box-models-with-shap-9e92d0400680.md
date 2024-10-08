# 机器学习的可视化：用 SHAP 揭开黑箱模型的面纱

> 原文：[`towardsdatascience.com/machine-learning-illustrated-opening-black-box-models-with-shap-9e92d0400680`](https://towardsdatascience.com/machine-learning-illustrated-opening-black-box-models-with-shap-9e92d0400680)

## 如何使用 SHAP 解释和解读任何机器学习模型

[](https://medium.com/@shreya.rao?source=post_page-----9e92d0400680--------------------------------)![Shreya Rao](https://medium.com/@shreya.rao?source=post_page-----9e92d0400680--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9e92d0400680--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9e92d0400680--------------------------------) [Shreya Rao](https://medium.com/@shreya.rao?source=post_page-----9e92d0400680--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9e92d0400680--------------------------------) ·10 分钟阅读·2023 年 5 月 8 日

--

Shapley Values 是经济学中合作博弈论的一个概念，它根据每个参与者对博弈的贡献为每个玩家分配一个价值。在机器学习领域，这一概念被适配成了 SHAP（SHapley Additive exPlanations）框架，这是一种有效的模型解释技术。

如果你有兴趣深入了解 Shapley Values，我强烈推荐阅读我之前的[文章](https://medium.com/@shreya.rao/economics-illustrated-shapley-values-7d33df43ada8)以了解 Shapley Values 背后的数学和直觉。尽管它已被修改以适应机器学习的需求，但理解其基本原理仍然是有帮助的。

[](https://medium.com/@shreya.rao/economics-illustrated-shapley-values-7d33df43ada8?source=post_page-----9e92d0400680--------------------------------) [## 经济学插图：Shapley Values

### Shapley Values 是博弈论中广泛使用的概念，它提供了一种公平的方式来分配总收益。

[medium.com](https://medium.com/@shreya.rao/economics-illustrated-shapley-values-7d33df43ada8?source=post_page-----9e92d0400680--------------------------------)

SHAP 框架类似于 Shapley 值，因为它计算了特征在一个游戏（即机器学习模型）中的单独影响。然而，机器学习模型是非合作性的游戏，这意味着特征之间不一定像在合作游戏中那样相互作用。相反，每个特征独立地贡献于模型的输出。虽然 Shapley 值公式可以使用，但由于参与者和联盟的数量众多，它可能在计算上非常昂贵且不准确。为了解决这个问题，研究人员开发了替代方法，如蒙特卡洛方法和基于核的方法。在本文中，我们将深入探讨蒙特卡洛方法。

让我们用一个例子来设置。假设我们有一个[数据集](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.fetch_california_housing.html)，包含 20,640 栋加州房屋的价格。

```py
from sklearn.datasets import fetch_california_housing
import pandas as pd

# load dataset
housing = fetch_california_housing()
X, y = housing.data, housing.target
X = pd.DataFrame(X, columns=housing.feature_names)
# feature dataset
X = X.drop(['Population', 'AveBedrms', 'AveOccup'], axis=1)
```

我们将使用特征*MedInc, HouseAge, AveRooms, Latitude,* 和 *Longitude*（**X**）……

![](img/d9e9aa728c9b1f047e2360235dfee519.png)

…预测房价（**y**）。*注意：房价以$100,000 为单位表示。*

我们将建立一个模型。这可以是任何模型，但我们将使用 XGBoost（阅读我的上一篇文章以了解 XGBoost 背后的数学）。按照标准步骤——将数据分为训练集和测试集，并用训练集训练模型。

```py
# split data into train and test
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)

# train an XGBoost model
from xgboost import XGBRegressor
model = xgb.XGBRegressor(objective='reg:squarederror', random_state=4)
model.fit(X_train, y_train)
```

然后使用这个模型对测试集进行预测。

```py
y_pred = model.predict(X_test)
```

让我们进一步分解。我们想要测试集中第一个样本（或房子）的预测房价。所以对于第一个样本，这些特征值是……

```py
X_test.iloc[0,]
```

```py
MedInc         4.151800
HouseAge      22.000000
AveRooms       5.663073
Latitude      32.580000
Longitude   -117.050000
```

…预测的房价是：

```py
y_pred[0]
> 1.596
```

![](img/8d031456937a9971920a73371b7570b4.png)

XGBoost 做出的预测显得神秘，因为它是一个黑箱模型，所以我们看不到**箱子**内部发生了什么。为了更深入地了解预测值是如何得出的，我们需要理解两点：

+   特征对预测房价的影响是正面还是负面？（例如，更高的*MedInc*值是否导致预测房价的增加？）

+   这些影响的程度是多少？（例如，*MedInc*对预测房价的影响是否比*AveRooms*更大？）

为了回答这些问题并确定每个特征对 1.596 这一房价预测的贡献，我们可以依赖特征的 SHAP 值。

让我们通过以下步骤来计算对 1.596 这一预测的***MedInc 的 SHAP 值***：

## 第 0 步：计算预期的预测房价

**预期的预测房价**不过是所有预测值的*平均值*。即：

```py
y_pred.mean()
> 2.07
```

所以，2.07 作为预期的预测房价。我们现在尝试弄清楚为什么预测值 1.596 与预期的 2.07 有偏差。0.476（2.07–1.596）的差异来自哪里？

特征的 SHAP 值衡量了该特征对模型预测值偏离/接近期望预测值的贡献程度。

## 步骤 1：获取特征的随机排列

我们从这个特征的排列开始…

![](img/bdb2f16b821b15a994544bb27c8dba63.png)

…在随机重排后，我们得到这个：

![](img/619fe10cf225d257a337e8fa2d0f76d6.png)

我们在这里关注*MedInc*，所以让我们突出显示*MedInc*以及其右边的任何特征。

![](img/1f4a973514fd5ca9d4a70c16575b98f6.png)

## 步骤 2：从数据集中选择一个随机样本

我们回到我们的测试数据集，从中选择一个随机样本：

![](img/0997fab0f5286f15e5dfdf642226a42f.png)

## 步骤 3：形成两个新样本

这些样本将部分来源于原始样本…

![](img/1eddbf91915ef607150e0be25b47cc11.png)

…以及我们在步骤 2 中刚刚提取的新样本：

![](img/0997fab0f5286f15e5dfdf642226a42f.png)

我们创建的第一个样本，称为 x1，将拥有原始样本中的所有相同值，除了*MedInc*右边的特征。对于*MedInc*右边的特征，我们从新样本中获取。

![](img/8723a7d0e91ad4f884ae6578c5642251.png)

我们创建的第二个样本，称为 x2，将拥有原始样本中的所有值，除了*MedInc*右边的特征***和 MedInc***。

![](img/57a53edd16f43e6ad55865902d89a298.png)

从中我们可以看到，这些样本完全一样，除了一个关键的地方——它们在*MedInc*值上有所不同，这是我们关注的特征。

## 步骤 4：使用新样本进行预测，并找出预测值的差异

现在我们将这些样本输入到模型中，并获取它们的预测值。

![](img/404b7d4e6ef4ab25dba1a1d6d431d147.png)![](img/988b7fbfbe21e81ecb2a0dbd8e86a64b.png)

我们找到这些值之间的差异：

![](img/1f0d0cbdea088dd0c9912c1c49f754d3.png)

由于 x1 和 x2 之间的唯一区别是*MedInc*的值，这个区别表示当*MedInc*的值发生变化时预测的变化。

## 步骤 5：重复步骤 1–4 几次，然后计算差异的平均值

我们现在要做的就是重复上述过程，并计算步骤 4 中找到的所有差异值的平均值。

假设我们将这个过程重复 1000 次（这个数字可能会根据模型和数据集的复杂性而有所不同）。在对结果取平均后，我们得到了一个值为**0.22**。

这个 0.22 表示*MedInc*在第一个样本中的 SHAP 值，这意味着*MedInc*对将期望预测值 2.07 调整到我们预测值 1.596 的贡献为+0.22。

我们可以对其他特征——*HouseAge, AveRooms, Latitude,* 和 *Longitude* 应用相同的过程，以找到它们的 SHAP 值。

现在我们了解了 SHAP 的基本计算，我们可以通过可视化来应用它。为了可视化它们，我们将使用 Python 的***shap***库中的***Explainer***并输入我们的模型。

```py
import shap
explainer = shap.Explainer(model)
shap_values = explainer(X_test)
```

这将为测试集中每个样本的所有特征（*MedInc*、*房龄*、*平均房间数*、*纬度*和*经度*）提供 SHAP 值。利用这些 SHAP 值，我们开始绘图。

## 1\. 瀑布图

这个图帮助我们逐个可视化数据中每个样本的 SHAP 值。让我们可视化第一个样本的 SHAP 值。

```py
# visualize the SHAP values of the first sample
shap.plots.waterfall(shap_values[0])
```

![](img/c495a96aeb6c1dcd086e1ea7f37fa0c7.png)

请注意，预期的预测值 E[f(X)] = 2.07，这是第 0 步计算的值，而第一个房子的预测房价 f(X) = 1.596 是我们对第一个样本的预测。

注意到*MedInc*的 SHAP 值为+0.22（我们在第 5 步中看到），*经度*的 SHAP 值为-2.35，等等。如果我们将所有上述 SHAP 值加到 2.07 中，并从中减去，我们得到第一个房子的预测值 1.596。

![](img/5d56ac53edea49f36e9b272963d0c1d9.png)

忽略符号，*经度*的 SHAP 值的大小为 2.35，大于其他特征的值。这意味着*经度*对这一特定预测的影响最大。

就像我们可视化第一个样本的 SHAP 值一样，我们也可以可视化第二个样本的 SHAP 值。

```py
# visualize the SHAP values of the second sample
shap.plots.waterfall(shap_values[0])
```

![](img/01b917c3c00b0c91f0df282f81694742.png)

比较我们测试数据中第一个和第二个房子的 SHAP 值，我们观察到显著差异。在第一个房子中，*经度*对预测价格的影响最大，而在第二个房子中，*MedInc*的影响最为显著。

> 注意：这些 SHAP 值的差异突出了每个特征对模型输出的独特贡献。理解这些贡献对于建立对模型决策过程的信任至关重要，并确保模型不会存在偏见或歧视。

## 2\. 力图

另一种可视化上述内容的方法是力图。

```py
shap.plots.force(shap_values[0])
```

![](img/55044cac57d3c64cac41ef4327c95dbd.png)

## 3\. 平均 SHAP 图

要确定哪些特征通常对我们模型的预测最重要，我们可以使用所有观测值的平均 SHAP 值的条形图。取绝对值的平均值可以确保正负值不会相互抵消。

```py
shap.plots.bar(shap_values)
```

![](img/65409913e4de84a4ddf04af405b50f4d.png)

每个特征都有一个对应的条形，高度表示平均 SHAP 值。例如，在我们的图中，平均 SHAP 值最大的特征是*纬度*，这表明它对我们模型的预测影响最大。这些信息可以帮助我们了解哪些特征对模型的决策过程至关重要。

## 4\. 蜂群图

蜜蜂图是一种有用的可视化工具，用于检查每个特征的所有 SHAP 值。y 轴按特征将 SHAP 值分组，点的颜色表示相应的特征值。通常，较红的点代表较高的特征值。

蜜蜂图可以帮助识别特征与模型预测之间的重要关系。在这个图中，特征按其平均 SHAP 值排序。

```py
shap.plots.beeswarm(shap_values)
```

![](img/8aa359bf65e22090bf49cb223be05753.png)

通过检查蜜蜂图中的 SHAP 值，我们可以开始理解特征与预测房价之间关系的性质。例如，对于*MedInc*，我们观察到 SHAP 值随着特征值的增加而增加。这表明较高的*MedInc*值有助于提高预测的房价。

相比之下，对于*Latitude*和*Longitude*，我们注意到相反的趋势，即更高的特征值会导致较低的 SHAP 值。这一观察表明较高的*Latitude*和*Longitude*值与较低的预测房价相关联。

## 5\. 依赖图

为了更深入地理解各个特征与其相应的 SHAP 值之间的关系，我们可以创建依赖图。依赖图是一个散点图，展示了单个特征的 SHAP 值与特征值之间的关系。

```py
shap.plots.scatter(shap_values[:,"MedInc"])
```

![](img/8428b1c963f86abd056ae4e55141a60d.png)

```py
shap.plots.scatter(shap_values[:,"Latitude"])
```

![](img/675f2522c40fbd98d264bc70c56b6ee2.png)

通过分析依赖图，我们可以确认在蜜蜂图中所做的观察。例如，当我们为*MedInc*创建一个依赖图时，我们观察到*MedInc*值与 SHAP 值之间存在正相关关系。换句话说，更高的*MedInc*值会导致预测的房价更高。

总体而言，依赖图提供了对单个特征与预测房价之间复杂关系的更详细理解。

总之，SHAP 值让我们窥探了每个特征如何贡献于模型输出。这些见解可以通过可视化进一步增强。我们可以利用 SHAP 做出更明智的特征选择、模型改进决策，并最终在机器学习应用中实现更好的性能。

![](img/a5968fd00bf83cbd6acdff51388eb153.png)

一如既往，感谢你让我参与到你的机器学习旅程中。你可以通过邮件联系我，邮箱是*shreya.statistics@gmail.com*，或者在[LinkedIn](https://www.linkedin.com/in/shreyarao24/)上与我联系，提出你的意见、问题和建议！
