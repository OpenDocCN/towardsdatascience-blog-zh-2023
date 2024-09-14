# 在 Python 中创建合成数据集的简单方法

> 原文：[`towardsdatascience.com/simple-ways-to-create-synthetic-dataset-in-python-76a8e9a2f35c`](https://towardsdatascience.com/simple-ways-to-create-synthetic-dataset-in-python-76a8e9a2f35c)

## 数据科学基础

## 创建模拟表格数据的初学者指南

[](https://zluvsand.medium.com/?source=post_page-----76a8e9a2f35c--------------------------------)![Zolzaya Luvsandorj](https://zluvsand.medium.com/?source=post_page-----76a8e9a2f35c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----76a8e9a2f35c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----76a8e9a2f35c--------------------------------) [Zolzaya Luvsandorj](https://zluvsand.medium.com/?source=post_page-----76a8e9a2f35c--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----76a8e9a2f35c--------------------------------) ·阅读时间 7 分钟·2023 年 1 月 12 日

--

在开发代码时，有时我们需要一个虚拟数据集。例如，我们想分享代码和底层数据，但实际数据集是机密的，因此不适合分享。一个选项是找到并使用合适的玩具数据集或公开可用的数据集。另一个选项是创建一个足够满足使用案例的合成数据集。在本文中，我们将探讨一些在 Python 中创建合成数据集的简单方法。

![](img/9298cc1e32a2bf82fd4e570d2c3cb6c6.png)

图片来自[Jackie Tsang](https://unsplash.com/es/@jickii?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 🔧 设置

我们将从加载必要的库开始：

```py
import numpy as np
import pandas as pd
from faker import Faker
from scipy.stats import skewnorm
from datetime import datetime
from sklearn.datasets import (make_regression, make_classification, 
                              make_multilabel_classification, 
                              make_blobs)
from sklearn.model_selection import train_test_split
from sklearn.ensemble import (RandomForestClassifier,
                              RandomForestRegressor)
from sklearn.multioutput import MultiOutputClassifier
from sklearn.cluster import KMeans
from sklearn.metrics import mean_squared_error, roc_auc_score
import matplotlib.pyplot as plt
import seaborn as sns
sns.set(style='darkgrid', context='talk')
```

一切准备就绪，*让我们深入探讨*！

# 📍 Scikit-learn

Scikit-learn 提供了许多有用的函数来创建合成数值数据集。在这一部分，我们将熟悉其中的一些。

## 📌 回归

我们可以使用`[make_regression](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.make_regression.html)`函数创建具有数值特征和连续目标的数据集。我们来创建一个包含 5 个特征和 1000 条记录的连续目标数据集：

```py
n = 1000
n_features = 5
seed = 123X, y = make_regression(n_samples=n, n_features=n_features, 
                       random_state=seed)
columns = [f"feature{i+1}" for i in range(n_features)]
df = pd.concat([pd.DataFrame(X, columns=columns), 
                pd.Series(y, name='target')], axis=1)
print(df.shape)
df.head()
```

![](img/1ce697a3bb5d9663a02392065b694905.png)

图片由作者提供

这非常简单明了！我们将记录数指定为`n_samples`参数，特征数指定为`n_features`参数。我们设置了随机种子，以便合成数据集可以被再现。如果需要，我们可以使用这个数据集来构建回归模型：

```py
X_train, X_test, y_train, y_test = train_test_split(
    X, y, random_state=seed
)
model = RandomForestRegressor(random_state=seed)
model.fit(X_train, y_train)
print(f"Train | MSE: {mean_squared_error(y_train, model.predict(X_train)):.4f}")
print(f"Test | MSE: {mean_squared_error(y_test, model.predict(X_test)):.4f}")
```

![](img/bb573001cfce803a03f371447198ff81.png)

图片由作者提供

在这种情况下，模型似乎在训练数据上过拟合得很严重。

## 📌 分类

类似于上面，我们可以使用`[make_classification](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.make_classification.html)`创建具有所需数量数值特征和离散目标的数据集。我们现在将练习创建一个包含 5 个特征和一个二进制目标的 1000 条记录的数据集：

```py
n_classes = 2
X, y = make_classification(n_samples=n, n_features=n_features, 
                           n_classes=n_classes, random_state=seed)
columns = [f"feature{i+1}" for i in range(n_features)]
df = pd.concat([pd.DataFrame(X, columns=columns), 
                pd.Series(y, name='target')], axis=1)
print(df.shape)
df.head()
```

![](img/cd9e7aea1cf7b833060b18752bea0cca.png)

图片由作者提供

除了在回归部分使用的参数外，我们还将类别数量指定为`n_classes`参数。我们接下来将构建一个分类模型：

```py
X_train, X_test, y_train, y_test = train_test_split(
    X, y, random_state=seed
)
model = RandomForestClassifier(random_state=seed)
model.fit(X_train, y_train)
print(f"Train | ROC-AUC: {roc_auc_score(y_train, model.predict_proba(X_train)[:,1]):.4f}")
print(f"Test | ROC-AUC: {roc_auc_score(y_test, model.predict_proba(X_test)[:,1]):.4f}")
```

![](img/631c2fecf425ff77975b5a4f736c5862.png)

图片由作者提供

从初步观察来看，该模型表现不错。我们还可以使用`[make_multilabel_classification](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.make_multilabel_classification.html#sklearn.datasets.make_multilabel_classification)`创建多标签分类问题的数据集：

```py
X, Y = make_multilabel_classification(n_samples=n, 
                                      n_features=n_features, 
                                      n_classes=n_classes, 
                                      random_state=seed)
x_columns = [f"feature{i+1}" for i in range(n_features)]
y_columns = [f"target{i+1}" for i in range(n_classes)]
df = pd.concat([pd.DataFrame(X, columns=x_columns), 
                pd.DataFrame(Y, columns=y_columns)], axis=1)
print(df.shape)
df.head()
```

![](img/fdadad9605ffc03d33542c85e73cef26.png)

图片由作者提供

在这种情况下，我们有两个目标标签。使用虚拟数据集，我们可以构建一个多标签分类模型：

```py
X_train, X_test, Y_train, Y_test = train_test_split(
    X, Y, random_state=seed
)
model = MultiOutputClassifier(
    RandomForestClassifier(random_state=seed)
)
model.fit(X_train, Y_train)
print(f"Train | Accuracy by class: {np.round(np.mean(Y_train==model.predict(X_train), axis=0),4)}")
print(f"Test | Accuracy by class: {np.round(np.mean(Y_test==model.predict(X_test), axis=0),4)}")
```

![](img/b1282caf0505f33548e8ec72cdbdbaa7.png)

图片由作者提供

## 📌 聚类

另一个有用的函数是`[make_blobs](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.make_blobs.html)`，它用于创建聚类数据：

```py
X, y = make_blobs(n_samples=n, n_features=n_features, 
                  centers=4, random_state=seed)
columns = [f"feature{i+1}" for i in range(n_features)]
df = pd.concat([pd.DataFrame(X, columns=columns), 
                pd.Series(y, name='target')], axis=1)
print(df.shape)
df.head()
```

![](img/bd6787db3cdd1e0e9fe02e28da0cd13f.png)

图片由作者提供

在这种情况下，我们选择了 4 个簇中心作为`centers`参数。尽管聚类是无监督的，即我们没有目标变量，但我们在合成数据集中得到了簇作为目标变量。让我们可视化不同`k`值下的平方距离和：

```py
ks = np.arange(2, 11)
sum_squared_distances = []
for k in ks:
    model = KMeans(k, random_state=seed)
    model.fit(X)
    sum_squared_distances.append(model.inertia_)plt.figure(figsize=(6,4))
sns.lineplot(x=ks, y=sum_squared_distances)
plt.xlabel('k')
plt.ylabel('Sum of squared distances');
```

![](img/c2e5227327336c3b9af19a5918ce2b3f.png)

图片由作者提供

看起来当 k=4 时，平方距离和趋于平稳。还是我们受到了确认偏误的影响？

如果你想了解更多，这四个函数还有其他有用的参数，可以进一步定制和控制如何创建合成数据集。你可以从[这里](https://scikit-learn.org/stable/datasets/sample_generators.html)了解更多关于 Scikit-learn 生成合成数据集的函数。

# 📍 NumPy & pandas

我们还可以使用`numpy`和`pandas`，这两个常用的数据处理库，来创建虚拟数据集：

```py
np.random.seed(123)
df = pd.DataFrame()
df['id'] = np.random.choice(np.arange(10**5, 10**6), n, 
                            replace=False)
df['gender'] = np.random.choice(['female', 'male'], n, 
                                p=[0.6, 0.4])
df['age'] = np.random.randint(18, 80, size=n)
df['spend'] = skewnorm.rvs(100, loc=1000, scale=500, size=n)
df['points'] = np.random.normal(loc=50, scale=10, size=n)start_date = pd.Timestamp("2013-01-01")
end_date = pd.Timestamp("2023-02-01")
delta = (end_date-start_date).days
df['date_joined'] = start_date + pd.to_timedelta(np.random.randint(delta, size=n), 'day')
print(df.shape)
df.head()
```

![](img/8477af89e805ea010c34c309c38393c4.png)

图片由作者提供

## 📌 分类变量

我们创建了一个分类变量：`gender`作为示例。使用`p`参数，我们指定了类别的期望概率。我们可以检查生成的数据是否反映了这一点：

```py
pd.concat([df['gender'].value_counts(normalize=True),
           df['gender'].value_counts()], axis=1)
```

![](img/64033e5ad7b3dd61c0767a9943022e52.png)

图片由作者提供

很好，大致是 60:40。如果我们不指定`p`参数，类别将会均匀分配。

## 📌 数值变量

我们创建了一些数值变量：`id`、`age`、`spend`、`points`。

◼️ `id`：我们通过指定`replace=False`确保 5 位数的 ID 是唯一的。

◼️ `age`：使用了`np.random.randint()`函数在一个范围内生成随机整数。

◼️ `spend`：使用了`scipy.stats.skewnorm.rvs()`函数创建了一个偏斜的随机数值变量。

◼️ `points`：使用了`np.random.normal()`函数创建了一个正态分布的随机数值变量。

让我们比较一下`spend`和`points`的分布，如下所示：

```py
fig, ax = plt.subplots(2, 1, figsize=(6, 7))
sns.histplot(data=df, x='spend', ax=ax[0])
sns.histplot(data=df, x='points', ax=ax[1])
fig.tight_layout();
```

![](img/fc520dc2d1790fa300da83cd42ff15bd.png)

图片由作者提供

我们可以看到，`spend`的分布偏斜且有一个长的右尾，而`points`大致呈正态分布。

## 📌 日期变量

最后，我们使用`pandas`创建了一个日期变量。我们定义了`start_date`和`end_date`，并在这个范围内找到了随机日期。我们可以检查随机抽样日期的分布：

```py
plt.figure(figsize=(6,4))
sns.histplot(data=df, x='date_joined');
```

![](img/9d6d3201680eb57f3336b653b540105a.png)

图片由作者提供

现在，让我们熟悉一个有趣的库。

# 📍 Faker

`Faker`是一个用于创建假数据集的库。我们使用这个库的方法非常简单，我们首先初始化一个`Faker`对象：`fake = Faker()`。然后我们可以通过`fake.<method_name()>`访问它提供的所有方法。例如，查看`fake.name()`。以下是我们可以使用该库创建的样本数据集：

```py
df = pd.DataFrame()
fake = Faker()
fake.seed_instance(seed)
np.random.seed(seed)
start_date = datetime(1940, 1, 1)
end_date = datetime(2005, 2, 1)
for i in range(n):
    df.loc[i, 'birthday'] = fake.date_between(start_date, end_date).strftime('%Y-%m-%d')
    df.loc[i, 'first_name'] = fake.first_name()
    df.loc[i, 'last_name'] = fake.last_name()
    df.loc[i, 'email'] = f"{df.loc[i, 'first_name'].lower()}@{fake.domain_name()}"
    df.loc[i, 'phone_number'] = fake.phone_number()
print(df.shape)
df.head()
```

![](img/e708fa3a8ba3e1bd32d071fed2fdc338.png)

图片由作者提供 | 假人员信息因安全原因已被模糊处理

除了使用`pandas`，我们还可以使用`Faker`添加日期，如`birthday`列所示。我们还生成了一些自由文本列。当你需要虚拟数据时，这个库非常有用，不是吗？如果你想创建虚拟文本，这里有一个示例语法：

```py
corpus = [fake.sentence() for i in range(n)]
corpus[:5]
```

![](img/614a0da3746284cb2955456b2c554b10.png)

图片由作者提供

根据我们的需求，这里还有略有不同的版本：`fake.sentences()`、`fake.paragraph()`或`fake.paragraphs()`。

在这篇文章中，我们只看了一些它的方法。如果你想了解更多关于这个库的信息，请访问[这里](https://github.com/joke2k/faker/blob/master/docs/fakerclass.rst)的 GitHub 文档。

就这样！希望这些生成合成数据集的简单方法对你的 Python 代码开发有所帮助。

![](img/f552a8c38f153675d4ffcdfd99826b98.png)

图片由[Edgar Chaparro](https://unsplash.com/@echaparro?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

感谢阅读这篇文章。如果你感兴趣，这里有我其他一些文章的链接：

◼️️ 从 ML 模型到 ML 管道

◼️️ 使用 SHAP 解释 Scikit-learn 模型

◼️️ Python 中绘制多个图形的 4 个简单技巧

◼️ 美化 pandas DataFrames

◼ 在 Python 中简单的数据可视化，你会发现很有用️

◼️ 在 Seaborn（Python）中绘制更美观和定制化图表的 6 个简单技巧

再见了 🏃 💨
