# 处理 Python 中分类变量的指南

> 原文：[`towardsdatascience.com/guide-to-handling-categorical-variables-in-python-854d0b65a6ae`](https://towardsdatascience.com/guide-to-handling-categorical-variables-in-python-854d0b65a6ae)

## *如何处理分类变量以用于机器学习和数据科学的指南*

[](https://medium.com/@theDrewDag?source=post_page-----854d0b65a6ae--------------------------------)![Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----854d0b65a6ae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----854d0b65a6ae--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----854d0b65a6ae--------------------------------) [Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----854d0b65a6ae--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----854d0b65a6ae--------------------------------) ·13 分钟阅读·2023 年 6 月 16 日

--

![](img/e99d8b89a83f713a851916579c9a484c.png)

图片由 [Thomas Haas](https://unsplash.com/@thomashaas?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) 提供

在数据科学或机器学习项目中处理分类变量并非易事。这种工作需要对应用领域有深入的了解，并且需要对多种可用方法有广泛的理解。

出于这个原因，本文将重点解释以下概念

+   **什么是分类变量**及如何将它们划分为不同类型

+   如何根据它们的类型将它们转换为**数值**

+   处理这些变量的工具和技术主要使用**Sklearn**

适当处理分类变量可以大大改善我们预测模型或分析的结果。事实上，学习和理解数据的大部分相关信息可能都包含在可用的分类变量中。

想想表格数据，按变量`gender`或某种`color`进行划分。这些划分基于类别数量，可以揭示组之间的显著差异，从而为分析师或学习算法提供信息。

我们从定义分类变量是什么以及它们如何表现自己开始。

# 分类变量的定义

分类变量是用于统计学和数据科学中的一种变量，用于表示定性或名义数据。这些变量可以定义为一种类别或数据类别，这些数据无法连续量化，而只能离散表示。

例如，一个分类变量的例子可能是**一个人的眼睛颜色，** 可能是蓝色、绿色或棕色。

> 大多数学习模型无法处理分类格式的数据。我们必须先将它们转换为数字格式，以便信息得以保留。

分类变量可以分为两种类型：

+   名义型

+   有序

**名义变量**是没有严格顺序限制的变量。性别、颜色或品牌就是名义变量的例子，因为它们不能排序。

**有序变量**是将分类变量划分为逻辑上可排序的级别的变量。数据集中包含*第一、第二和第三*等级别的列可以被认为是有序的分类变量。

你可以通过考虑**二元变量和循环变量**来深入了解分类变量的分解。

**二元变量**很简单理解：它是一个只能取两个值的分类变量。

**循环变量**则具有值的重复特征。例如，星期几是循环的，季节也是如此。

# 如何转换分类变量

现在我们已经定义了分类变量是什么以及它们的样子，让我们通过一个实际的例子来解决转化它们的问题——一个名为*cat-in-the-dat*的 Kaggle 数据集。

## 数据集

这是一个开源数据集，作为对分类变量管理和建模的入门竞赛基础，称为分类特征编码挑战 II。你可以从下面的链接直接下载数据。

[](https://www.kaggle.com/c/cat-in-the-dat-ii?source=post_page-----854d0b65a6ae--------------------------------) [## 分类特征编码挑战 II

### 二元分类，每个特征都是分类的（以及交互！）

www.kaggle.com](https://www.kaggle.com/c/cat-in-the-dat-ii?source=post_page-----854d0b65a6ae--------------------------------)

这个数据集的特殊性在于它仅包含分类数据。因此，它成为本指南的完美使用案例。它包括名义型、序数型、循环型和二元变量。

我们将看到将每个变量转换为学习模型可用格式的技术。

数据集如下所示：

![](img/7dfe5816387a98c71f30b084a1b0cf26.png)

作者提供的图片。

由于目标变量只能取两个值，因此这是一个二元分类任务。我们将使用 AUC 指标来评估我们的模型。

现在我们将应用处理分类变量的技术，使用上述提到的数据集。

## 1\. 标签编码（映射到任意数字）

将分类转换为可用格式的最简单方法是将每个类别分配一个任意数字。

例如，`ord_2`列包含的类别

```py
array(['Hot', 'Warm', 'Freezing', 'Lava Hot', 'Cold', 'Boiling Hot', nan],
      dtype=object)
```

可以使用 Python 和 Pandas 进行这样的映射：

```py
df_train = train.copy()

mapping = {
    "Cold": 0,
    "Hot": 1,
    "Lava Hot": 2,
    "Boiling Hot": 3,
    "Freezing": 4,
    "Warm": 5
}

df_train["ord_2"].map(mapping)

>> 
0         1.0
1         5.0
2         4.0
3         2.0
4         0.0
         ... 
599995    4.0
599996    3.0
599997    4.0
599998    5.0
599999    3.0
Name: ord_2, Length: 600000, dtype: float64
```

然而，这种方法有一个问题：你必须手动声明映射。对于少量类别这不是问题，但对于大量类别可能会有问题。

为此，我们将使用 Scikit-Learn 和 `LabelEncoder` 对象以更灵活的方式实现相同的结果。

```py
from sklearn import preprocessing

# we handle missing values
df_train["ord_2"].fillna("NONE", inplace=True)
# init the sklearn encoder
le = preprocessing.LabelEncoder()
# fit + transform
df_train["ord_2"] = le.fit_transform(df_train["ord_2"])
df_train["ord_2"]

>>
0         3
1         6
2         2
3         4
4         1
         ..
599995    2
599996    0
599997    2
599998    6
599999    0
Name: ord_2, Length: 600000, dtype: int64
```

映射由 Sklearn 控制。我们可以这样可视化它：

```py
mapping = {label: index for index, label in enumerate(le.classes_)}
mapping

>>
{'Boiling Hot': 0,
 'Cold': 1,
 'Freezing': 2,
 'Hot': 3,
 'Lava Hot': 4,
 'NONE': 5,
 'Warm': 6}
```

注意上面的代码片段中的 `.fillna(“NONE")`。实际上，Sklearn 的标签编码器不处理空值，如果发现任何空值，应用时会产生错误。

> 正确处理分类变量时最重要的一点是始终处理空值。实际上，大多数相关技术在未处理这些值时无法正常工作。

标签编码器将任意数字映射到列中的每个类别，而无需显式声明映射。这很方便，但对某些预测模型引入了一个问题：*如果列不是目标列，它引入了需要对数据进行缩放的需求*。

实际上，机器学习初学者经常问标签编码器和 one hot 编码器之间的区别，我们很快会看到。**标签编码器从设计上应应用于标签，即我们想要预测的目标变量，而不是其他列。**

说到这一点，一些在领域内非常相关的模型即使使用这种类型的编码也能很好地工作。我指的是树模型，其中 XGBoost 和 LightGBM 脱颖而出。

所以，如果你决定使用树模型，可以自由使用标签编码器，但否则，我们必须使用 one hot 编码。

## 2\. One Hot 编码

正如我在我的文章中提到的[机器学习中的向量表示](https://medium.com/towards-data-science/vector-representations-for-machine-learning-5047c50aaeff)，one hot 编码是一种非常常见且著名的向量化技术（即将文本转换为数字）。

它的工作原理是：对于每个存在的类别，创建一个仅可能包含 0 和 1 的方阵。这个矩阵通知模型，在所有可能的类别中，这一观察到的行具有由 1 表示的值。

一个例子：

```py
 |   |   |   |   |   |   
-------------|---|---|---|---|---|---
 Freezing    | 0 | 0 | 0 | 0 | 0 | 1 
 Warm        | 0 | 0 | 0 | 0 | 1 | 0 
 Cold        | 0 | 0 | 0 | 1 | 0 | 0 
 Boiling Hot | 0 | 0 | 1 | 0 | 0 | 0 
 Hot         | 0 | 1 | 0 | 0 | 0 | 0 
 Lava Hot    | 1 | 0 | 0 | 0 | 0 | 0 
```

数组的大小为 *n_categories*。这是非常有用的信息，因为 one hot 编码通常需要**稀疏表示**转换后的数据。

这意味着什么？这意味着对于大量类别，矩阵也可能变得非常大。由于矩阵仅由 0 和 1 组成，并且只有一个位置可以被 1 填充，这使得 one hot 表示非常冗余和繁琐。

稀疏矩阵解决了这个问题——只保存 1 的位置，而不保存值为 0 的位置。这简化了上述问题，并使我们能够以极少的内存使用量保存大量的信息。

让我们看看在 Python 中这样一个数组的样子，再次应用之前的代码。

```py
from sklearn import preprocessing

# we handle missing values
df_train["ord_2"].fillna("NONE", inplace=True)
# init sklearn's encoder
ohe = preprocessing.OneHotEncoder()
# fit + transform
ohe.fit_transform(df_train["ord_2"].values.reshape(-1, 1))

>>
<600000x7 sparse matrix of type '<class 'numpy.float64'>'
 with 600000 stored elements in Compressed Sparse Row format>
```

Python 默认返回一个对象，而不是值列表。要获得这样的列表，你需要使用 `.toarray()`

```py
ohe.fit_transform(df_train["ord_2"].values.reshape(-1, 1)).toarray()

>>
array([[0., 0., 0., ..., 0., 0., 0.],
       [0., 0., 0., ..., 0., 0., 1.],
       [0., 0., 1., ..., 0., 0., 0.],
       ...,
       [0., 0., 1., ..., 0., 0., 0.],
       [0., 0., 0., ..., 0., 0., 1.],
       [1., 0., 0., ..., 0., 0., 0.]])
```

如果你不完全理解这个概念也不用担心：我们很快会看到如何将标签编码和独热编码应用于数据集，以训练预测模型。

> 标签编码和独热编码是处理分类变量的最重要技术。掌握这两种技术将使你能够处理大多数涉及分类变量的情况。

## 3\. 转换和聚合。

将分类数据转换为数值格式的另一种方法是对变量进行转换或聚合。

通过`.groupby()`分组，可以将列中存在值的计数用作转换的输出。

```py
df_train.groupby(["ord_2"])["id"].count()

>>
ord_2
Boiling Hot     84790
Cold            97822
Freezing       142726
Hot             67508
Lava Hot        64840
Warm           124239
Name: id, dtype: int64
```

使用`.transform()`我们可以将这些数字替换到相应的单元格中。

```py
df_train.groupby(["ord_2"])["id"].transform("count")

>>
0          67508.0
1         124239.0
2         142726.0
3          64840.0
4          97822.0
            ...   
599995    142726.0
599996     84790.0
599997    142726.0
599998    124239.0
599999     84790.0
Name: id, Length: 600000, dtype: float64
```

也可以使用其他数学操作应用这种逻辑——应测试最能提高模型性能的方法。

## 4\. 从分类变量创建新的分类特征。

我们将 ord_1 列与 ord_2 一起查看。

![](img/b9ed1890cb1951a94860379758143a9f.png)

图片来源于作者。

我们可以通过合并现有变量来创建新的分类变量。例如，我们可以将 ord_1 与 ord_2 合并，以创建一个新特征。

```py
df_train["new_1"] = df_train["ord_1"].astype(str) + "_" + df_train["ord_2"].astype(str)
df_train["new_1"]

>>
0                 Contributor_Hot
1                Grandmaster_Warm
2                    nan_Freezing
3                 Novice_Lava Hot
4                Grandmaster_Cold
                   ...           
599995            Novice_Freezing
599996         Novice_Boiling Hot
599997       Contributor_Freezing
599998                Master_Warm
599999    Contributor_Boiling Hot
Name: new_1, Length: 600000, dtype: object
```

这种技术几乎可以应用于任何情况。指导分析师的思想是通过向学习模型中添加原本难以理解的信息来提高模型的性能。

## 5\. 将 NaN 作为分类变量使用。

很多时候，空值会被删除。我通常不推荐这样做，**因为 NaNs 可能包含对我们的模型有用的信息**。

一种解决方案是将 NaNs 视为一个独立的类别。

再次查看 ord_2 列。

```py
df_train["ord_2"].value_counts()

>>
Freezing       142726
Warm           124239
Cold            97822
Boiling Hot     84790
Hot             67508
Lava Hot        64840
Name: ord_2, dtype: int64
```

现在我们尝试应用`.fillna(“NONE")`，以查看有多少个空单元格存在。

```py
df_train["ord_2"].fillna("NONE").value_counts()

>>
Freezing       142726
Warm           124239
Cold            97822
Boiling Hot     84790
Hot             67508
Lava Hot        64840
NONE            18075
```

作为百分比，NONE 约占整列的 3%。这是一个相当显著的量。利用 NaN 更有意义，并且可以使用之前提到的独热编码来完成。

# 跟踪稀有类别。

让我们记住 OneHotEncoder 的作用：它创建一个稀疏矩阵，其列数和行数等于引用列中唯一类别的数量。*这意味着我们还必须考虑可能存在于测试集中但可能在训练集中缺失的类别*。

对于 LabelEncoder 情况类似——测试集中可能存在但训练集中没有的类别，这可能在转换过程中造成问题。

我们通过连接数据集来解决这个问题。这将允许我们将编码器应用于所有数据，而不仅仅是训练数据。

```py
test["target"] = -1
data = pd.concat([train, test]).reset_index(drop=True)
features = [f for f in train.columns if f not in ["id", "target"]]
for feature in features:
    le = preprocessing.LabelEncoder()
    temp_col = data[feature].fillna("NONE").astype(str).values
    data.loc[:, feature] = le.fit_transform(temp_col)

train = data[data["target"] != -1].reset_index(drop=True)
test = data[data["target"] == -1].reset_index(drop=True)
```

![](img/e8556ff3c302b82438c4094d26f5789f.png)

图片来源于作者。

如果我们有测试集，这种方法对我们很有帮助。如果没有测试集，当新类别成为训练集的一部分时，我们将考虑使用类似 NONE 的值。

# 模型分类数据。

现在让我们开始训练一个简单的模型。我们将按照以下链接中关于如何设计和实现交叉验证的步骤进行 👇

[](/what-is-cross-validation-in-machine-learning-14d2a509d6a5?source=post_page-----854d0b65a6ae--------------------------------) ## 什么是机器学习中的交叉验证

### 了解什么是交叉验证——构建通用模型的基本技术

towardsdatascience.com

我们从头开始，导入数据并使用 Sklearn 的`StratifiedKFold`创建折叠。

```py
train = pd.read_csv("/kaggle/input/cat-in-the-dat-ii/train.csv")
test = pd.read_csv("/kaggle/input/cat-in-the-dat-ii/test.csv")

df = train.copy()

df["kfold"] = -1
df = df.sample(frac=1).reset_index(drop=True)
y = df.target.values

kf = model_selection.StratifiedKFold(n_splits=5)

for f, (t_, v_) in enumerate(kf.split(X=df, y=y)):
  df.loc[v_, 'kfold'] = f
```

这段小代码将创建一个包含 5 个组的 Pandas 数据框，以测试我们的模型。

![](img/bb127860185f608bac6b4229aa4ab9cf.png)

作者提供的图像。

现在让我们定义一个函数，用于测试每个组上的逻辑回归模型。

```py
def run(fold: int) -> None:
    features = [
        f for f in df.columns if f not in ("id", "target", "kfold")
    ]

    for feature in features:
        df.loc[:, feature] = df[feature].astype(str).fillna("NONE")

    df_train = df[df["kfold"] != fold].reset_index(drop=True)
    df_valid = df[df["kfold"] == fold].reset_index(drop=True)

    ohe = preprocessing.OneHotEncoder()

    full_data = pd.concat([df_train[features], df_valid[features]], axis=0)
    print("Fitting OHE on full data...")
    ohe.fit(full_data[features])

    x_train = ohe.transform(df_train[features])
    x_valid = ohe.transform(df_valid[features])
    print("Training the classifier...")
    model = linear_model.LogisticRegression()
    model.fit(x_train, df_train.target.values)

    valid_preds = model.predict_proba(x_valid)[:, 1]

    auc = metrics.roc_auc_score(df_valid.target.values, valid_preds)

    print(f"FOLD: {fold} | AUC = {auc:.3f}")

run(0)

>>
Fitting OHE on full data...
Training the classifier...
FOLD: 0 | AUC = 0.785
```

我邀请感兴趣的读者阅读有关交叉验证的文章，以更详细地理解所展示代码的工作原理。

现在让我们看看如何应用像 XGBoost 这样的树模型，它也与 LabelEncoder 配合得很好。

```py
def run(fold: int) -> None:
    features = [
        f for f in df.columns if f not in ("id", "target", "kfold")
    ]

    for feature in features:
        df.loc[:, feature] = df[feature].astype(str).fillna("NONE")

    print("Fitting the LabelEncoder on the features...")
    for feature in features:
        le = preprocessing.LabelEncoder()
        le.fit(df[feature])
        df.loc[:, feature] = le.transform(df[feature])

    df_train = df[df["kfold"] != fold].reset_index(drop=True)
    df_valid = df[df["kfold"] == fold].reset_index(drop=True)

    x_train = df_train[features].values
    x_valid = df_valid[features].values

    print("Training the classifier...")
    model = xgboost.XGBClassifier(n_jobs=-1, n_estimators=300)
    model.fit(x_train, df_train.target.values)

    valid_preds = model.predict_proba(x_valid)[:, 1]

    auc = metrics.roc_auc_score(df_valid.target.values, valid_preds)

    print(f"FOLD: {fold} | AUC = {auc:.3f}")

# execute on 2 folds
for fold in range(2):
    run(fold)

>>
Fitting the LabelEncoder on the features...
Training the classifier...
FOLD: 0 | AUC = 0.768
Fitting the LabelEncoder on the features...
Training the classifier...
FOLD: 1 | AUC = 0.765
```

# 结论

总之，还有其他值得一提的技术来处理分类变量：

+   **基于目标的编码**，即将类别转换为目标变量在对应类别下的平均值

+   [**神经网络的嵌入**](https://medium.com/towards-data-science/vector-representations-for-machine-learning-5047c50aaeff)，可用于表示文本实体

总结一下，正确管理分类变量的关键步骤如下

+   总是处理空值

+   根据变量的类型和我们想使用的模板应用 LabelEncoder 或 OneHotEncoder

+   从变量丰富的角度考虑，将 NaN 或 NONE 视为可以告知模型的分类变量

+   建模数据！

感谢你的时间，

安德烈亚

# 推荐阅读

对于感兴趣的读者，以下是我推荐的每个机器学习相关主题的书单。这些书在我看来是**必不可少**的，并且对我的职业生涯产生了重大影响。

*免责声明：这些是亚马逊的附属链接。我将从亚马逊获得少量佣金，以推荐这些商品给你。你的体验不会改变，你也不会被多收费，但这将帮助我扩大业务，并制作更多关于 AI 的内容。*

+   **机器学习简介：** [*自信的数据技能：掌握数据工作的基础，提升你的职业生涯*](https://amzn.to/3ZzKTz6)作者 Kirill Eremenko

+   **Sklearn / TensorFlow：** [*用 Scikit-Learn、Keras 和 TensorFlow 进行实践机器学习*](https://amzn.to/433F4Nm)作者 Aurelien Géron

+   **NLP：** [*文本作为数据：机器学习和社会科学的新框架*](https://amzn.to/3zvH43j)作者 Justin Grimmer

+   **Sklearn / PyTorch：** [*用 PyTorch 和 Scikit-Learn 进行机器学习：用 Python 开发机器学习和深度学习模型*](https://amzn.to/3Gcavve) 作者：Sebastian Raschka

+   **数据可视化：** [*用数据讲故事：商务专业人士的数据可视化指南*](https://amzn.to/3HUtGtB) 作者：Cole Knaflic

# 有用的链接（由我编写）

+   **学习如何在 Python 中进行顶级的探索性数据分析**：*Python 中的探索性数据分析——逐步过程*

+   **学习 PyTorch 的基础知识：** [*PyTorch 入门：从训练循环到预测*](https://medium.com/towards-data-science/introduction-to-pytorch-from-training-loop-to-prediction-a70372764432)

+   **学习 TensorFlow 的基础知识**：[*从 TensorFlow 2.0 开始——深度学习入门*](https://medium.com/towards-data-science/a-comprehensive-introduction-to-tensorflows-sequential-api-and-model-for-deep-learning-c5e31aee49fa)

+   **用 TF-IDF 在 Python 中进行文本聚类**：[*Python 中的 TF-IDF 文本聚类*](https://medium.com/mlearning-ai/text-clustering-with-tf-idf-in-python-c94cd26a31e7)

**如果你想支持我的内容创作活动，请随意通过下面的推荐链接加入 Medium 的会员计划**。我将从你的投资中获得一部分，同时你也可以无缝访问 Medium 上关于数据科学等领域的大量文章。

[## 通过我的推荐链接加入 Medium - Andrea D'Agostino](https://medium.com/@theDrewDag/membership?source=post_page-----854d0b65a6ae--------------------------------)

### 作为 Medium 会员，你的部分会员费用将分配给你阅读的作者，并且你可以全面访问每一个故事……

[medium.com](https://medium.com/@theDrewDag/membership?source=post_page-----854d0b65a6ae--------------------------------)
