# 类别不平衡策略 — 带代码的视觉指南

> 原文：[https://towardsdatascience.com/class-imbalance-strategies-a-visual-guide-with-code-8bc8fae71e1a?source=collection_archive---------2-----------------------#2023-04-24](https://towardsdatascience.com/class-imbalance-strategies-a-visual-guide-with-code-8bc8fae71e1a?source=collection_archive---------2-----------------------#2023-04-24)

## 了解随机欠采样、过采样、SMOTE、ADASYN 和 Tomek 链接

[](https://travis-tang.medium.com/?source=post_page-----8bc8fae71e1a--------------------------------)[![Travis Tang](../Images/8372ea73b8cf8fe344de6274b5d9ad17.png)](https://travis-tang.medium.com/?source=post_page-----8bc8fae71e1a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8bc8fae71e1a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8bc8fae71e1a--------------------------------) [Travis Tang](https://travis-tang.medium.com/?source=post_page-----8bc8fae71e1a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F169b6a57c01e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclass-imbalance-strategies-a-visual-guide-with-code-8bc8fae71e1a&user=Travis+Tang&userId=169b6a57c01e&source=post_page-169b6a57c01e----8bc8fae71e1a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8bc8fae71e1a--------------------------------) ·13分钟阅读·2023年4月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8bc8fae71e1a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclass-imbalance-strategies-a-visual-guide-with-code-8bc8fae71e1a&user=Travis+Tang&userId=169b6a57c01e&source=-----8bc8fae71e1a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8bc8fae71e1a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclass-imbalance-strategies-a-visual-guide-with-code-8bc8fae71e1a&source=-----8bc8fae71e1a---------------------bookmark_footer-----------)

类别不平衡发生在分类问题中一个类别显著多于另一个类别。这在许多机器学习问题中很常见。例子包括欺诈检测、异常检测和医学诊断。

# 类别不平衡的诅咒

在不平衡的数据集上训练的模型在少数类别上表现不佳。在最佳情况下，这可能会导致业务损失，如客户流失分析。而在最糟糕的情况下，它可能会蔓延到面部识别系统的系统性偏见中。

![](../Images/7e0577b405b22eaa703f8b6695043510.png)

一个平衡的数据集可能就是缺少的关键（来源：

Elena Mozhvilo 在 [Unsplash](https://unsplash.com/photos/j06gLuKK0GM))

处理类别不平衡的常见方法是重采样。这可能包括对多数类别进行过采样、对少数类别进行欠采样，或两者兼而有之。

在本文中，我使用生动的可视化和代码来说明处理类别不平衡的策略：

1.  随机过采样

1.  随机欠采样

1.  使用SMOTE进行过采样

1.  使用 ADASYN 进行过采样

1.  使用Tomek Link进行欠采样

1.  使用SMOTE过采样，然后使用TOMEK Link进行欠采样（SMOTE-Tomek）

我还将在真实世界数据集上使用这些策略，并评估它们对机器学习模型的影响。让我们开始吧。

> 所有源代码都在这里。

# 使用Imbalance-learn

我们将使用Python中的`imbalanced-learn`包来解决类别不平衡的问题。这是一个开源库，依赖于scikit-learn，并提供在处理不平衡类别分类时的工具。

要安装它，请使用以下命令。

```py
pip install -U imbalanced-learn
```

# 数据集

我们使用的数据集是[**UCI社区与犯罪数据集（CC BY 4.0）**](https://archive.ics.uci.edu/ml/datasets/communities+and+crime)，包含1994年美国社区的100个属性。我们可以用它来预测是否**犯罪率高**（定义为**人均暴力犯罪**超过0.65）。数据来源于UCI机器学习库，由La Salle大学的Michael Redmond于2009年发布。

> 数据集中包含的变量涉及社区，如被视为城市的人口比例和家庭收入中位数，以及涉及执法，如人均警官数量和分配给毒品单位的警官比例。

此数据集存在类别不平衡。对于每一个高犯罪率社区，有12个低犯罪率社区。这非常适合我们的案例说明。

```py
>>> from imblearn.datasets import fetch_datasets

>>> # Fetch dataset from imbalanced-learn library 
>>> # as a dictionary of numpy array
>>> us_crime = fetch_datasets()['us_crime']
>>> us_crime

{'data': array([[0.19, 0.33, 0.02, ..., 0.26, 0.2 , 0.32],
        [0\.  , 0.16, 0.12, ..., 0.12, 0.45, 0\.  ],
        [0\.  , 0.42, 0.49, ..., 0.21, 0.02, 0\.  ],
        ...,
        [0.16, 0.37, 0.25, ..., 0.32, 0.18, 0.91],
        [0.08, 0.51, 0.06, ..., 0.38, 0.33, 0.22],
        [0.2 , 0.78, 0.14, ..., 0.3 , 0.05, 1\.  ]]),
 'target': array([-1,  1, -1, ..., -1, -1, -1]),
 'DESCR': 'us_crime'}
```

我们将把这个字典转换成Pandas数据框架，然后分割成训练-测试集。

```py
# Convert the dictionary to a pandas dataframe
crime_df = pd.concat([pd.DataFrame(us_crime['data'], columns = [f'data_{i}' for i in range(us_crime.data.shape[1])]),
           pd.DataFrame(us_crime['target'], columns = ['target'])], axis = 1)

# Split data into train test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(crime_df.drop('target', axis = 1), 
                                                    crime_df['target'], 
                                                    test_size = 0.4, 
                                                    random_state = 42)
```

注意，我们将仅对训练数据集执行欠采样和过采样。我们将*不会*对测试集进行欠采样和过采样。

## 数据集预处理

我们的目标是可视化一个不平衡的数据集。为了在二维图中可视化128维的数据集，在训练集上进行以下操作。

+   缩放数据集，

+   对特征执行主成分分析（PCA），将100个特征转换为2个主成分，

+   可视化数据。

这是数据在2D中的可视化。

![](../Images/86931b188116c9c87693d8ef51c041c2.png)

作者提供的图片

上述图的代码：

```py
from sklearn.preprocessing import MinMaxScaler
from sklearn.decomposition import PCA
import matplotlib.pyplot as plt

# Scale the dataset on both train and test sets.
# Note that we fit MinMaxScaler on X_train only, not on the entire dataset.
# This prevents data leakage from test set to train set.
scaler = MinMaxScaler()
scaler.fit(X_train)
X_train = scaler.transform(X_train)
X_test = scaler.transform(X_test)

# Perform PCA Decomposition on both train and test sets
# Note that we fit PCA on X_train only, not on the entire dataset.
# This prevents data leakage from test set to train set.
pca = PCA(n_components=2)
pca.fit(X_train)
X_train_pca = pca.transform(X_train)
X_test_pca = pca.transform(X_test)

# Function for plotting dataset 
def plot_data(X,y,ax,title):
    ax.scatter(X[:, 0], X[:, 1], c=y, alpha=0.5, s = 30, edgecolor=(0,0,0,0.5))
    ax.set_ylabel('Principle Component 1')
    ax.set_xlabel('Principle Component 2')
    if title is not None:
        ax.set_title(title)

# Plot dataset
fig,ax = plt.subplots(figsize=(5, 5))
plot_data(X_train_pca, y_train, ax, title='Original Dataset')
```

预处理完成后，我们准备对数据集进行重采样。

# 策略1\. 随机过采样

随机过采样通过替换从少数类别中复制现有样本来增加例子。每个少数类别中的数据点有相同的复制概率。

![](../Images/ceecf5a6b76f1149935c8efbbb4780d8.png)

作者提供的图片

这是我们如何在数据集上进行过采样的方法。

```py
from imblearn.over_sampling import RandomOverSampler

# Perform random oversampling
ros = RandomOverSampler(random_state=0)
X_train_ros, y_train_ros = ros.fit_resample(X_train_pca, y_train)
```

让我们比较随机过采样前（左）和随机过采样后（右）的数据。

![](../Images/e8d5473774d60edf316cace663f86b4a.png)

在Github上绘图的代码。图片来源：作者

唯一的区别？在随机过采样之后，少数类中的**重叠数据点更多**。因此，少数类的数据点看起来更暗。

# **策略2：随机欠采样**

相反，随机欠采样会从多数类中移除现有样本。多数类中的每个数据点被移除的机会是相等的。

![](../Images/8eeea9953f7b07db8e240e34bbf4b009.png)

图片来源：作者

我们可以用以下代码来实现这一点。

```py
from imblearn.under_sampling import RandomUnderSampler

# Perform random sampling
rus = RandomUnderSampler(random_state=0)
X_train_rus, y_train_rus = rus.fit_resample(X_train_pca, y_train)

# Function for plotting is in Notebook.
# Insert link here.
```

让我们比较随机欠采样前（左）和后（右）的数据。

![](../Images/783b6e0d87a1ebec3168b582b8d21975.png)

图片来源：作者

欠采样后，数据点的总体数量显著减少。这是因为在类平衡之前，随机移除多数类中的数据点。

## 将机器学习应用于欠采样和过采样数据集

让我们比较在上面三个数据集（未修改数据集、欠采样数据集和过采样数据集）上训练的分类机器学习模型（SVM模型）的表现

在这里，我们在三个数据集上训练了三种支持向量机分类器（SVC）：

+   原始数据

+   随机过采样的数据

+   随机欠采样的数据

```py
from sklearn.svm import SVC

# Train SVC on original data
clf = SVC(kernel='linear',probability=True)
clf_ros.fit(X_train_pca, y_train)

# Train SVC on randomly oversampled data
clf_ros = SVC(kernel='linear',probability=True)
clf_ros.fit(X_train_ros, y_train_ros)

# Train SVC on randomly undersampled data
clf_rus = SVC(kernel='linear',probability=True)
clf_rus.fit(X_train_rus, y_train_rus)

# Function for plotting is in Notebook.
# Insert link here.
```

然后，我们可以可视化每个SVC从数据集中学到的内容。

![](../Images/c790f8a28ebcf0e5a45dbd055b11cc7f.png)

图片来源：作者

上面的图总结了算法从数据集中学到的内容。特别是，它们学到了：

+   一个落入**黄色**区域的新点被预测为**黄色**点（‘*高犯罪率社区*’）

+   一个落入**紫色**区域的新点被预测为**紫色**点（‘*低犯罪率社区*’）

这里有一些观察：

+   训练在*原始*数据集上的SVC…相当无用。它基本上将所有社区预测为紫色。它学会忽视所有黄色点。

+   训练在过采样和欠采样数据集上的SVC的偏差较小。它们更不容易错误分类少数类。

+   训练在过采样和欠采样数据集上的SVC的决策边界有所不同。

## 使用ROC评估重采样模型

为了评估哪个SVC最佳，我们将评估SVC在测试集上的表现。我们将使用的指标是接收者操作特征曲线（ROC），以找到曲线下面积（AUC）。请搜索（Cmd+F）“**附录1**”以了解ROC的介绍。

![](../Images/efe1e7a50daeb633bc071de887eb0593.png)

图片来源：作者

```py
from sklearn.svm import SVC
from sklearn import metrics
import matplotlib.pyplot as plt

# Helper function for plotting ROC
def plot_roc(ax, X_train, y_train, X_test, y_test, title):
    clf = SVC(kernel='linear',probability=True)
    clf.fit(X_train, y_train)
    y_test_pred = clf.predict_proba(X_test)[:,1]
    fpr, tpr, thresh = metrics.roc_curve(y_test, y_test_pred)
    auc = metrics.roc_auc_score(y_test, y_test_pred)
    ax.plot(fpr,tpr,label=f"{title} AUC={auc:.3f}")

    ax.set_title('ROC Curve')
    ax.set_xlabel('False Positive Rate')
    ax.set_ylabel('True Positive Rate')
    ax.legend(loc=0)

# Plot all ROC into one graph
fig,ax = plt.subplots(1,1,figsize=(8,5))
plot_roc(ax, X_train_pca, y_train, X_test_pca, y_test, 'Original Dataset')
plot_roc(ax, X_train_ros, y_train_ros, X_test_pca, y_test, 'Randomly Oversampled Dataset')
plot_roc(ax, X_train_rus, y_train_rus, X_test_pca, y_test, 'Randomly Undersampled Dataset') 
```

在原始数据上训练的SVC表现不佳。它的表现比我们随机猜测结果还要差。

随机过采样的数据集优于欠采样的数据集。一个可能的原因是，从欠采样过程中移除数据点会丧失信息。相反，过采样不会丢失信息。

现在我们对过采样和欠采样技术有了理解，让我们深入探讨过采样和欠采样。

# 策略 3\. 使用SMOTE进行过采样

SMOTE是一种过采样方法。直观地说，SMOTE通过在彼此之间插值的少数数据点之间创建合成数据点。

这是SMOTE工作的简化说明。

1.  随机选择少数类中的一些数据点。

1.  对于每个选定的点，识别其*k*个最近邻居。

1.  对于每个邻居，添加一个新点，该点位于数据点和邻居之间的某处。

1.  重复步骤2到4，直到生成足够的合成数据点。

*请搜索（Cmd+F）“附录2”以查找其创作者对SMOTE算法的确切描述。*

这是一个可视化。

让我们使用SMOTE对数据集进行过采样，并在其上训练一个SVC。

```py
from imblearn.over_sampling import SMOTE
from sklearn.svm import LinearSVC

# Perform random sampling
smote = SMOTE(random_state=0)
X_train_smote, y_train_smote = smote.fit_resample(X_train_pca, y_train)

# Train linear SVC
clf_smote = SVC(kernel='linear',probability=True)
clf_smote.fit(X_train_smote, y_train_smote)

# Plot decision boundary
# Function for plotting decision boundary is in Notebook
# Link: 
```

这是结果。

![](../Images/93afb26c0ba1103f7dee31647bb1d0ea.png)

作者提供的图像

# 策略 4\. 使用ADASYN进行过采样（+它与SMOTE的不同之处）

ADASYN是SMOTE的一个变种：SMOTE和ADASYN都通过插值生成新样本。

但有一个关键的区别。ADASYN会在被KNN分类器错误分类的原始样本旁边生成样本。相反，SMOTE区分了被KNN分类器正确或错误分类的样本。

这是ADASYN工作原理的可视化。

让我们使用ADASYN对数据集进行过采样，并在其上训练一个SVC。

```py
from imblearn.over_sampling import ADASYN

# Perform random sampling
adasyn = ADASYN(random_state=0)
X_train_adasyn, y_train_adasyn = adasyn.fit_resample(X_train_pca, y_train)

# Train linear SVC
from sklearn.svm import SVC
clf_adasyn = SVC(kernel='linear',probability=True)
clf_adasyn.fit(X_train_adasyn, y_train_adasyn)

# Plot decision boundary
# Function for plotting decision boundary is in Notebook
# Link:
```

让我们比较SMOTE、ADASYN和原始数据集。

![](../Images/b0508f63d53bdbdac8559a5ced5afb8c.png)

作者提供的图像

这里有几点观察。

首先，两种过采样方法都会在原始数据点之间创建更多的合成数据点。这是因为SMOTE和ADASYN都使用插值来创建新的数据点。

其次，比较SMOTE和ADASYN时，我们注意到ADASYN会在少数（*黄色*）点附近的多数（*紫色*）数据点创建数据点。

+   比较上面用蓝色圈出的区域，ADASYN在只有少数紫色数据点的区域创建了*较少*的黄色数据点。

+   比较上面用棕色圈出的区域，ADASYN在紫色数据点较多的区域创建了*更多*的黄色数据点。

让我们比较到目前为止我们描述的所有过采样方法的ROC曲线。在这个例子中，它们表现同样出色。

![](../Images/3efbe5c0f92b3f979af1a495623abe81.png)

作者提供的图像

# 策略 5\. 使用汤姆克链接进行欠采样

汤姆克链接是一对非常接近但属于不同类别的点。*汤姆克链接的数学定义可以在附录3中找到。*

这是一个可视化。

要使用汤姆克链接进行欠采样，我们将识别数据集中所有的汤姆克链接。对于汤姆克链接中的每对数据点，我们将删除多数类别。

这里有一个动画，说明了使用汤姆克链接进行欠采样。

我们将对我们的数据集应用汤姆克链接欠采样。

```py
from imblearn.under_sampling import TomekLinks
from sklearn.svm import LinearSVC

# Perform Tomek Link undersampling
tomek = TomekLinks()
X_train_tomek, y_train_tomek = tomek.fit_resample(X_train_pca, y_train)

# Train linear SVC
clf_tomek = SVC(kernel='linear',probability=True)
clf_tomek.fit(X_train_tomek, y_train_tomek)

# Code for plotting graph in notebook.
# Notebook link:
```

现在让我们比较 Tomek 欠采样和随机欠采样。

![](../Images/a0d832d1141b4bd1673e53d1bef2c71f.png)

图片来源于作者

在我们的数据集中，移除 Tomek Link 对缓解类别不平衡几乎没有作用。这是因为数据集中 Tomek Links 的数量有限。

让我们看看 Tomek Link 欠采样的表现与随机欠采样有何不同。

![](../Images/b45b058004a2d9c4af8eec5d33d36a39.png)

图片来源于作者

我们观察到**随机欠采样比 Tomek Link 欠采样效果更好。** 这是因为 Tomek Link 没有像随机欠采样那样完全消除类别不平衡。

# 策略 6\. SMOTEK：先使用 SMOTE 过采样，然后使用 Tomek Links 欠采样

现在我们已经了解了过采样和欠采样。我们可以将这些技术结合起来吗？

当然！**SMOTE-TOMEK** 是一种结合了过采样（SMOTE）和欠采样（通过 Tomek Links）的技术。

我们将其应用到我们的数据集上。

```py
from imblearn.combine import SMOTETomek
from sklearn.svm import LinearSVC

# Perform random sampling
smotetomek = SMOTETomek(random_state=0)
X_train_smotetomek, y_train_smotetomek = smotetomek.fit_resample(X_train_pca, y_train)

# Plot linear SVC
clf_smotetomek = SVC(kernel='linear',probability=True)
clf_smotetomek.fit(X_train_smotetomek, y_train_smotetomek)
```

让我们比较 SMOTE、Tomek 和 SMOTE-Tomek。

![](../Images/798077ce2d76cec375453ddcf4493c0a.png)

图片来源于作者

比较 SMOTE-Tomek 和仅 SMOTE，我们可以看到差异被圈出了棕色的圈。SMOTE-Tomek 移除了接近边界的点。

最终，我们将比较上述描述的所有技术。结果是，SMOTE-TOMEK 表现最佳。

![](../Images/1bf9ed2d4e34e8377859560edbbfc7c3.png)

图片来源于作者

# 结束

总体来说，你可以使用过采样、欠采样或两者的组合来处理数据不平衡问题。如果你有计算资源，通常更好的方法是使用过采样和欠采样的组合；当数据点较少时，过采样是一种不错的策略；而当有许多类似数据点时，欠采样则表现较好。

处理不平衡数据集并不容易。我鼓励你探索许多其他重采样策略（包括不同的 [欠采样方法](https://imbalanced-learn.org/dev/references/under_sampling.html) 和 [过采样方法](https://imbalanced-learn.org/dev/references/over_sampling.html)），以查看哪种策略在你的数据集上表现最好。

此外，评估不平衡数据集的性能可能会很棘手。确保使用正确的分类指标。幸运的是，[ROC 曲线](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.RocCurveDisplay.html)、[F1 分数](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html) 和 [几何均值分数](https://imbalanced-learn.org/dev/references/metrics.html) 等指标已经可供使用。

我是 Travis Tang。我在 LinkedIn 和 Medium 上发布数据科学内容。关注我以获取更多内容 :)

# 附录

## 附录 1\. 使用 ROC 评估类别不平衡问题中的模型

ROC 对类别不平衡不敏感，使其成为评估类别不平衡模型的绝佳工具。它不依赖于类别的普遍性。这与像准确率这样的评估指标形成对比，后者在类别不平衡时可能会产生误导。

![](../Images/ea7535e53d252dd13478c5d82f190260.png)

由 CMG Lee 绘制，基于 [http://commons.wikimedia.org/wiki/File:roc-draft-xkcd-style.svg](https://commons.wikimedia.org/wiki/File:roc-draft-xkcd-style.svg)。

ROC 曲线绘制了 y 轴上的真实正例率 (TPR) 对 x 轴上的假正例率 (FPR) 的图像，用于所有可能的分类阈值。TPR 是正确分类为正例的正例实例的比例，而 FPR 是错误分类为正例的负例实例的比例。

一个性能良好的模型将具有接近图表左上角的 ROC 曲线，因为这表示更高的 TPR 和更低的 FPR。一个完全随机猜测的模型将落在 TPR = FPR 的线条上。

## 附录 2\. SMOTE 算法的确切算法

> 少数类通过取每个少数类样本并沿连接任意/所有 k 个少数类最近邻的线段引入合成样本来进行过采样。根据所需的过采样量，从 k 个最近邻中随机选择邻居。我们目前的实现使用五个最近邻。例如，如果所需的过采样量为 200%，则从五个最近邻中选择两个邻居，并在每个邻居的方向上生成一个样本。合成样本的生成方式如下：取考虑中的特征向量（样本）与其最近邻之间的差异。将此差异乘以一个 0 到 1 之间的随机数，并将其加到考虑中的特征向量上。这会在两个特定特征之间的线段上选择一个随机点。这种方法有效地使少数类的决策区域变得更一般化。 [2]

## 附录 3\. **Tomek Links** 的定义

> 给定两个属于不同类别的样本 Ei 和 Ej，d(Ei, Ej) 是 Ei 和 Ej 之间的距离。如果不存在样本 El，使得 d(Ei, El) < d(Ei, Ej) 或 d(Ej, El) < d(Ei, Ej)，则 (Ei, Ej) 对被称为 Tomek link。*[1]*

# 参考文献

[1] Batista, Gustavo EAPA, Ronaldo C. Prati, 和 Maria Carolina Monard. “[A Study of the Behavior of Several Methods for Balancing Machine Learning Training Data](https://www.researchgate.net/publication/220520041_A_Study_of_the_Behavior_of_Several_Methods_for_Balancing_machine_Learning_Training_Data)” *ACM SIGKDD explorations newsletter* 6.1 (2004): 20–29。

[2] Chawla, Nitesh V., 等. “[SMOTE: synthetic minority over-sampling technique.](https://dl.acm.org/doi/10.5555/1622407.1622416)” *Journal of artificial intelligence research* 16 (2002): 321–357。
