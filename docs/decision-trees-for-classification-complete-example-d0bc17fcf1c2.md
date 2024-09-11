# 分类决策树——完整示例

> 原文：[https://towardsdatascience.com/decision-trees-for-classification-complete-example-d0bc17fcf1c2?source=collection_archive---------1-----------------------#2023-01-01](https://towardsdatascience.com/decision-trees-for-classification-complete-example-d0bc17fcf1c2?source=collection_archive---------1-----------------------#2023-01-01)

## 关于如何构建分类决策树的详细示例

[](https://medium.com/@pumaline?source=post_page-----d0bc17fcf1c2--------------------------------)[![Datamapu](../Images/63b0c7f9a3d160c5bb039bbebd791f7e.png)](https://medium.com/@pumaline?source=post_page-----d0bc17fcf1c2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d0bc17fcf1c2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d0bc17fcf1c2--------------------------------) [Datamapu](https://medium.com/@pumaline?source=post_page-----d0bc17fcf1c2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffcd72d75ae6e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecision-trees-for-classification-complete-example-d0bc17fcf1c2&user=Datamapu&userId=fcd72d75ae6e&source=post_page-fcd72d75ae6e----d0bc17fcf1c2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d0bc17fcf1c2--------------------------------) ·8分钟阅读·2023年1月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd0bc17fcf1c2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecision-trees-for-classification-complete-example-d0bc17fcf1c2&user=Datamapu&userId=fcd72d75ae6e&source=-----d0bc17fcf1c2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd0bc17fcf1c2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecision-trees-for-classification-complete-example-d0bc17fcf1c2&source=-----d0bc17fcf1c2---------------------bookmark_footer-----------)![](../Images/a45b59b3ef0c4737f791d8f710bbf118.png)

图片由Fabrice Villard提供，来自Unsplash

本文解释了我们如何使用决策树来解决分类问题。在解释重要术语后，我们将为一个简单的示例数据集构建一个决策树。

# 介绍

> [决策树是一种决策支持工具，它使用树状模型展示决策及其可能的后果，包括随机事件结果、资源成本和效用。这是一种仅包含条件控制语句的算法展示方式。](https://en.wikipedia.org/wiki/Decision_tree)

传统上，决策树是手动绘制的，但可以通过机器学习进行学习。它们可用于回归和分类问题。在这篇文章中，我们将重点关注分类问题。让我们考虑以下示例数据：

![](../Images/0ff178c454666a8b72804aec2f7de623.png)

示例数据（由作者构建）

使用这个简化的例子，我们将预测一个人是否会成为宇航员，取决于他们的年龄、是否喜欢狗以及是否喜欢重力。在讨论如何构建决策树之前，让我们看一下我们示例数据的最终决策树。

![](../Images/36dbbe9e24de192c40ec14269cf0efa3.png)

示例数据的最终决策树

我们可以跟踪路径来做出决定。例如，我们可以看到，不喜欢重力的人不会成为宇航员，与其他特征无关。另一方面，我们也可以看到，喜欢重力和喜欢狗的人将会成为宇航员，与年龄无关。

在详细讨论如何构建这棵树之前，让我们定义一些重要的术语。

# 术语

## 根节点

顶级节点。做出的第一个决策。在我们的例子中，根节点是“喜欢重力”。

## 分支

分支代表子树。我们的例子有两个分支。例如，一个分支是从“喜欢狗”开始的子树，另一个是从“年龄 < 40.5”开始的子树。

## 节点

一个节点代表进一步（子）节点的切分。在我们的例子中，节点有“喜欢重力”、“喜欢狗”和“年龄 < 40.5”。

## 叶子节点

叶子节点位于分支的末端，即不再进行切分。它们代表每个动作的可能结果。在我们的例子中，叶子节点由“是”和“否”表示。

## 父节点

一个在（子）节点之前的节点称为父节点。在我们的例子中，“喜欢重力”是“喜欢狗”的父节点，而“喜欢狗”是“年龄 < 40.5”的父节点。

## 子节点

一个节点在另一个节点下方称为子节点。在我们的例子中，“喜欢狗”是“喜欢重力”的子节点，而“年龄 < 40.5”是“喜欢狗”的子节点。

## 切分

将一个节点划分为两个（子）节点的过程。

## 剪枝

移除父节点的（子）节点称为剪枝。树通过切分生长，通过剪枝缩小。在我们的例子中，如果我们移除节点“年龄 < 40.5”，我们将对树进行剪枝。

![](../Images/1556654976019b99810dc7f15c6695ca.png)

决策树插图

我们还可以观察到，决策树允许我们混合数据类型。我们可以在同一棵树中使用数值数据（“年龄”）和分类数据（“喜欢狗”、“喜欢重力”）。

# 创建决策树

创建决策树中最重要的步骤是对数据的 *分裂*。我们需要找到一种方法将数据集 (*D*) 分裂为两个数据集 (*D_1*) 和 (*D_2*)。可以使用不同的标准来寻找下一个分裂，概览见例如 [这里](https://www.analyticsvidhya.com/blog/2020/06/4-ways-split-decision-tree/#:~:text=Steps%20to%20split%20a%20decision%20tree%20using%20Information%20Gain%3A,entropy%20or%20highest%20information%20gain)。我们将集中于其中一个标准：[基尼不纯度](https://www.learndatasci.com/glossary/gini-impurity/#:~:text=a%20simple%20dataset-,What%20is%20Gini%20Impurity%3F,nodes%20to%20form%20the%20tree.)，它是一个用于分类目标变量的标准，也是 Python 库 [scikit-learn](https://scikit-learn.org/stable/modules/tree.html#classification) 使用的标准。

## 基尼不纯度

数据集 *D* 的基尼不纯度计算如下：

![](../Images/dcd4ca062f20d515c6c01822250af8fa.png)

其中 n = n_1 + n_2 表示数据集 (D) 的大小，并且

![](../Images/55268b5346592788b91d1ebd51acdd75.png)

*D_1* 和 *D_2* 是 *D* 的子集，𝑝_𝑗 是在给定节点上样本属于类别 𝑗 的概率，𝑐 是类别的数量。基尼不纯度越低，节点的同质性越高。纯节点的基尼不纯度为零。为了使用基尼不纯度来分裂决策树，需要执行以下步骤。

1.  对于每个可能的分裂，计算每个子节点的基尼不纯度

1.  计算每个分裂的基尼不纯度，作为子节点基尼不纯度的加权平均值

1.  选择基尼不纯度值最低的分裂

重复步骤 1–3 直到不能再分裂为止。

为了更好地理解这一点，让我们来看一个例子。

## 第一个示例：具有两个二元特征的决策树

在为整个数据集创建决策树之前，我们将首先考虑一个子集，该子集仅考虑两个特征：“喜欢重力”和“喜欢狗”。

我们首先需要决定哪个特征将作为 *根节点*。我们通过仅用一个特征来预测目标，然后选择基尼不纯度最低的特征作为根节点。也就是说，在我们的案例中，我们构建了两个浅层树，只有根节点和两个叶子。在第一个案例中，我们使用“喜欢重力”作为根节点，在第二个案例中使用“喜欢狗”。然后我们计算两个树的基尼不纯度。这些树的样子如下：

![](../Images/a7cbf81c91434e4974dd0fa52e21864a.png)

图片由作者提供

这些树的基尼不纯度计算如下：

**案例 1：**

数据集 1：

![](../Images/079a8cfb48bd78791b73edbf569b2bd3.png)

数据集 2：

![](../Images/39cfa1633b7fbf37959ff0cbc29f8383.png)

基尼不纯度是两者的加权均值：

![](../Images/bc97ce3e7eae3f23478251857c2e9b13.png)

**案例 2：**

数据集 1：

![](../Images/bee46fd6beab0ec0c42e071c3899739c.png)

数据集 2：

![](../Images/220e31774ee921f5ec8ecd3b0865c8a8.png)

基尼不纯度是两者的加权均值：

![](../Images/7b00d2fb5b4c651548228a07a25bdcd0.png)

即，第一个案例的基尼不纯度较低，是选择的拆分。在这个简单的示例中，只剩下一个特征，我们可以构建最终的决策树。

![](../Images/74ab2bd34dbd66e8504ea62a5a88a2b2.png)

只考虑特征‘喜欢重力’和‘喜欢狗’的最终决策树

## 第二个示例：添加一个数值变量

到现在为止，我们只考虑了数据集的一个子集——分类变量。现在我们将添加数值变量‘年龄’。拆分的标准相同。我们已经知道‘喜欢重力’和‘喜欢狗’的基尼不纯度。数值变量的基尼不纯度计算类似，但决策需要更多计算。需要执行以下步骤

1.  按数值变量（‘年龄’）对数据框进行排序

1.  计算邻近值的均值

1.  计算这些均值的所有拆分的基尼不纯度

这又是我们的数据，按年龄排序，左侧给出了邻近值的均值。

![](../Images/d825cb583abd383d6979f1e7996dddaf.png)

按年龄排序的数据集。左侧显示了年龄的邻近值的均值。

我们得到以下可能的拆分。

![](../Images/260539e5a4dd3b1394dc0e5015ae71d8.png)

年龄的可能拆分及其基尼不纯度。

我们可以看到，所有可能的‘年龄’拆分的基尼不纯度都高于‘喜欢重力’和‘喜欢狗’的基尼不纯度。当使用‘喜欢重力’时，基尼不纯度最低，即这是我们的*根节点*和第一次拆分。

![](../Images/cd3c81e701f1cdd6bf1fe61bddf6ce7d.png)

树的第一次拆分。‘喜欢重力’是根节点。

子集数据集2已经是纯净的，即这个节点是一个*叶子节点*，无需进一步拆分。左侧的*分支*，数据集1不是纯净的，可以进一步拆分。我们像之前一样计算每个特征的基尼不纯度：‘喜欢狗’和‘年龄’。

![](../Images/70d756f4890d3d097c10e1e251c8af18.png)

数据集2的可能拆分。

我们看到最低的基尼不纯度是由“喜欢狗”的拆分给出的。我们现在可以构建我们的最终决策树。

![](../Images/36dbbe9e24de192c40ec14269cf0efa3.png)

最终决策树。

## 使用Python

在Python中，我们可以使用scikit-learn方法[DecisionTreeClassifier](https://scikit-learn.org/stable/modules/tree.html#classification)来构建分类决策树。请注意，scikit-learn还提供了[DecisionTreeRegressor](https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeRegressor.html#sklearn.tree.DecisionTreeRegressor)，这是一个用于回归的决策树方法。假设我们的数据存储在数据框‘df’中，我们可以使用‘fit’方法进行训练：

```py
from sklearn.tree import DecisionTreeClassifier
clf = DecisionTreeClassifier()
X = df['age', 'likes dogs', 'likes graviy']
y = df['going_to_be_an_astronaut']
clf.fit(X,y)
```

我们可以使用‘*plot_tree*’方法可视化生成的树。这与我们构建的树相同，只是分割标准用‘<=’代替了‘<’，而‘true’和‘false’路径的方向相反。也就是说，外观上存在一些差异。

```py
plot_tree(clf, feature_names=[‘age’,‘likes_dogs’,‘likes_gravity’], fontsize=8);
```

![](../Images/4acd653ec0c964848a0e2b58da27225f.png)

使用scikit-learn生成的决策树。

# 决策树的优缺点

在使用决策树时，了解其优缺点很重要。以下是一些优缺点的列表，但这个列表并不完全。

## 优势

+   决策树直观、易于理解和解释。

+   决策树不受异常值和缺失值的影响。

+   数据不需要进行缩放。

+   数值数据和分类数据可以结合使用。

+   决策树是非参数算法。

## 缺点

+   过拟合是一个常见问题。剪枝可能有助于克服这个问题。

+   虽然决策树可以用于回归问题，但它们不能真正预测连续变量，因为预测必须以类别形式分隔。

+   训练决策树相对昂贵。

# 结论

在这篇文章中，我们讨论了一个简单但详细的示例，说明了如何为分类问题构建决策树，以及如何利用它进行预测。创建决策树的关键步骤是找到将数据分成两个子集的最佳分割方式。常用的方法是基尼不纯度。这也被Python中的scikit-learn库所使用，该库在实际中常用于构建决策树。重要的是要记住决策树的局限性，其中最突出的就是过拟合的倾向。

# 参考文献

+   克里斯·尼科尔森，《决策树》（2020），pathmind — A.I. Wiki，《AI、机器学习和深度学习中的重要主题初学者指南》https://wiki.pathmind.com/decision-tree。

+   阿比谢克·夏尔马，[4种简单的方式来拆分决策树](https://www.analyticsvidhya.com/blog/2020/06/4-ways-split-decision-tree/#:~:text=Steps%20to%20split%20a%20decision%20tree%20using%20Information%20Gain%3A,entropy%20or%20highest%20information%20gain) （2020），analyticsvidhya

除非另有说明，所有图片均为作者所用。

在这里找到更多数据科学和机器学习的文章：

[## 更多

### 数据科学和机器学习博客

datamapu.com](https://datamapu.com/?source=post_page-----d0bc17fcf1c2--------------------------------) [](https://medium.com/@pumaline/subscribe?source=post_page-----d0bc17fcf1c2--------------------------------) [## 订阅Pumaline发布的内容时会收到电子邮件。

### 订阅Pumaline发布的内容时会收到电子邮件。通过注册，如果你还没有Medium账号，将会创建一个…

[medium.com](https://medium.com/@pumaline/subscribe?source=post_page-----d0bc17fcf1c2--------------------------------) [](https://www.buymeacoffee.com/pumaline?source=post_page-----d0bc17fcf1c2--------------------------------) [## Pumaline

### 嗨，我喜欢学习和分享关于数据科学和机器学习的知识。

[www.buymeacoffee.com](https://www.buymeacoffee.com/pumaline?source=post_page-----d0bc17fcf1c2--------------------------------)
