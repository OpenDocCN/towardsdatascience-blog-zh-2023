# 用 Python 实现的机器学习特征编码技术

> 原文：[`towardsdatascience.com/feature-encoding-techniques-in-machine-learning-with-python-implementation-dbf933e64aa`](https://towardsdatascience.com/feature-encoding-techniques-in-machine-learning-with-python-implementation-dbf933e64aa)

## 数据科学工作流中需要考虑的 6 种特征编码技术

[](https://kayjanwong.medium.com/?source=post_page-----dbf933e64aa--------------------------------)![Kay Jan Wong](https://kayjanwong.medium.com/?source=post_page-----dbf933e64aa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dbf933e64aa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dbf933e64aa--------------------------------) [Kay Jan Wong](https://kayjanwong.medium.com/?source=post_page-----dbf933e64aa--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dbf933e64aa--------------------------------) ·阅读时间 10 分钟·2023 年 1 月 10 日

--

![](img/f0cc329e9db60722e304729377be4544.png)

由 [Susan Holt Simpson](https://unsplash.com/@shs521?utm_source=medium&utm_medium=referral) 提供的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

***特征编码*** 将分类变量转换为数值变量，作为特征工程的一部分，使数据与机器学习模型兼容。根据分类变量的类型和其他考虑因素，有多种方法可以执行特征编码。

本文介绍了执行特征编码的一般技巧，详细阐述了 6 种特征编码技术，您可以在数据科学工作流中考虑这些技术，提供了使用时机的评论，最后介绍了如何在 Python 中实现这些技术。

![](img/b373a0a726614c75547f877115df740f.png)

图 1：特征编码技术总结 — 作者提供的图像

6 种特征编码技术的备忘单总结见图 1；请继续阅读以获取每种方法的详细解释和实现。

# 目录

1.  标签 / 有序编码

1.  One-Hot / 虚拟编码

1.  目标编码

1.  计数 / 频率编码

1.  二进制 / BaseN 编码

1.  哈希编码

# 特征编码技巧

## 提示 1：防止数据泄露

考虑到特征编码的目的是将分类变量转换为数值变量，我们可以将“cat”、“dog”和“horse”等类别编码为 0、1、2 等数字。然而，我们必须记住**数据泄露**的问题，即测试数据中的信息泄露到训练数据中。

> 编码器必须仅在训练数据上进行拟合，使编码器仅学习训练集中存在的类别，然后用于转换验证/测试数据。 **不要在整个数据集上拟合编码器！**

问题自然接踵而至：“*如果在验证/测试数据中有缺失或新类别怎么办？*”，我们可以通过两种方式来处理——由于模型未在这些未见类别上训练，因此可以删除这些未见类别。或者，我们可以将它们编码为`-1`或其他任意值，以指示这些是未见类别。

## 提示 2：保存你的编码器

如前所述，编码器在训练数据上进行拟合（使用`.fit`方法），并用于转换验证/测试数据（使用`.transform`方法）。最好保存编码器以便稍后转换验证/测试数据。

保存编码器的其他好处包括能够检索类别或将编码的值转换回其类别（使用`.inverse_transform`方法），如果适用且需要的话。

现在让我们深入探讨特征编码技术！

# №1. 标签 / 顺序编码器

> 标签编码器和顺序编码器将类别直接编码为数值（参见图 2）。

标签编码器用于***名义***分类变量（没有顺序的类别，例如红色、绿色、蓝色），而顺序编码器用于***顺序***分类变量（有顺序的类别，例如小、中、大）。

![](img/2e8d46ea0373347ddaf5bfe7bfcef4ab.png)

图 2：标签编码示例 — 作者提供的图片

给定基数（类别数量）为`n`，标签和顺序编码器将值编码从`0`到`n-1`。

## 何时使用标签 / 顺序编码器

+   **名义/顺序变量**：标签编码器用于名义分类变量，而顺序编码器用于顺序分类变量

+   **支持高基数**：标签和顺序编码器可以用于类别非常多的情况

+   **未见变量**：顺序编码器可以用任意值编码验证/测试集中的未见变量（参见下面的代码示例），默认情况下会抛出值错误

## 标签 / 顺序编码器的缺点

+   **未见变量**：标签编码器不对验证/测试集中的未见变量进行编码，并会抛出值错误，必须进行特殊的错误处理以避免此错误

+   **将类别解释为数值**：机器学习模型会将编码的列读取为数值变量，而不是将其解释为不同的类别

对于标签编码器，它一次只能编码一列，每个分类列必须初始化多个标签编码器。

```py
from sklearn.preprocessing import LabelEncoder

# Initialize Label Encoder
encoder = LabelEncoder()

# Fit encoder on training data
data_train["type_encoded"] = encoder.fit_transform(data_train["type"])

# Transform test data
data_test["type_encoded"] = encoder.transform(data_test["type"])

# Retrieve the categories (returns list)
list(encoder.classes_)

# Retrieve original values from encoded values
data_train["type2"] = encoder.inverse_transform(data_train["type_encoded"])
```

对于顺序编码器，它可以一次编码多个列，并且可以指定类别的顺序。

```py
from sklearn.preprocessing import OrdinalEncoder

# Initialize Ordinal Encoder
encoder = OrdinalEncoder(
    categories=[["small", "medium", "large"]],
    handle_unknown="use_encoded_value",
    unknown_value=-1,
)
data_train["size_encoded"] = encoder.fit_transform(data_train[["size"]])
data_test["size_encoded"] = encoder.transform(data_test[["size"]])

# Retrieve the categories (returns list of lists)
encoder.categories

# Retrieve original values from encoded values
data_train["size2"] = encoder.inverse_transform(data_train[["size_encoded"]])
```

# №2. 独热编码 / 虚拟编码

> 在 One-Hot 编码和虚拟编码中，分类列被拆分为多个包含 1 和 0 的列（参见图 3）。

这解决了标签编码和序数编码的缺点，即由于编码数据被表示为多个布尔列，列现在被视为分类列。

![](img/0204fe69d55eca38233d97ce66a8d435.png)

图 3：One-Hot 编码示例 — 图片由作者提供

给定基数（类别数）为 `n`，One-Hot 编码器通过创建 `n` 个额外的列来对数据进行编码。在虚拟编码中，我们可以去掉最后一列，因为它是虚拟变量，这将导致 `n-1` 列。

## 何时使用 One-Hot / 虚拟编码器

+   **名义变量**：One-Hot 编码器用于名义分类变量。

+   **低到中等基数**：由于为每个类别创建了新的列，因此建议在类别数目较少到中等时使用 One-Hot 编码，以免结果数据过于稀疏。

+   **缺失或未见变量**：sklearn 包中的 One-Hot 编码器可以通过为缺失的变量创建列、忽略未见的变量列来处理缺失或未见的变量，以保持特征列的一致性，默认情况下会引发值错误。

## One-Hot / 虚拟编码器的缺点

+   **虚拟变量陷阱**：由于编码数据稀疏，可能会导致特征之间高度相关的现象。

+   **大数据集**：One-hot 编码会增加数据集中的列数，这可能会影响训练速度，并且不适用于基于树的模型。

One-hot 编码可以通过 sklearn 包中的 `OneHotEncoder` 或使用 pandas 的 `get_dummies` 方法来完成。

```py
from sklearn.preprocessing import OneHotEncoder

# Initialize One-Hot Encoder
encoder = OneHotEncoder(handle_unknown="ignore")

# Fit encoder on training data (returns a separate DataFrame)
data_ohe = pd.DataFrame(encoder.fit_transform(data_train[["type"]]).toarray())
data_ohe.columns = [col for cols in encoder.categories_ for col in cols]

# Join encoded data with original training data
data_train = pd.concat([data_train, data_ohe], axis=1)

# Transform test data
data_ohe = pd.DataFrame(encoder.transform(data_test[["type"]]).toarray())
data_ohe.columns = [col for cols in encoder.categories_ for col in cols]
data_test = pd.concat([data_test, data_ohe], axis=1)
```

使用 pandas 内置的 `get_dummies` 方法时，必须手动处理验证/测试数据中的缺失和未见变量。

```py
data_ohe = pd.get_dummies(data_train["type"])
data_train = pd.concat([data_train, data_ohe], axis=1)
```

# №3. 目标编码

> 目标编码使用贝叶斯后验概率将分类变量编码为目标变量（数值变量）的均值。应用平滑技术以防止目标泄漏。

与标签编码和序数编码相比，目标编码用解释目标的值对数据进行编码，而不是用任意的数字 0、1、2 等。其他类似的编码方法可以使用信息值（IV）或证据权重（WOE）对分类变量进行编码。

![](img/554464b6423e3f286920f32e04b4ea9a.png)

图 4：目标编码示例 — 图片由作者提供

实现目标编码有两种方法

+   均值编码：编码值是目标值的均值，并应用了平滑。

+   Leave-One-Out 编码：编码值是目标值的均值，但不包括我们要预测的数据点。

## 何时使用目标编码器

+   **名义变量**：目标编码器用于名义分类变量。

+   **支持高基数**：目标编码器可以用于类别很多的情况，如果每个类别有多个数据样本则更好

+   **未见变量**：目标编码器可以通过用目标变量的均值编码未见变量来处理它们

## 目标编码器的缺点

+   **目标泄露**：即使进行平滑处理，这也可能导致目标泄露和过拟合。可以使用留一法编码和在目标变量中引入高斯噪声来解决过拟合问题

+   **类别分布不均**：类别分布在训练和验证/测试数据中可能不同，从而导致类别被编码为不正确或极端的值

目标编码需要使用命令 `pip install category_encoders` 安装 `category_encoders` python 包。

```py
import category_encoders as ce

# Target (Mean) Encoding - fit on training data, transform test data
encoder = ce.TargetEncoder(cols="type", smoothing=1.0)
data_train["type_encoded"] = encoder.fit_transform(data_train["type"], data_train["label"])
data_test["type_encoded"] = encoder.transform(data_test["type"], data_test["label"])

# Leave One Out Encoding
encoder = ce.LeaveOneOutEncoder(cols="type")
data_train["type_encoded"] = encoder.fit_transform(data_train["type"], data_train["label"])
data_test["type_encoded"] = encoder.transform(data_test["type"], data_test["label"])
```

# №4\. 计数 / 频率编码

> 计数和频率编码将分类变量分别编码为出现次数和频率（归一化计数）。

![](img/5fac27741fe852ba6bb4d84099316ccf.png)

图 5: 计数和频率编码示例 — 作者提供的图像

## 何时使用计数 / 频率编码器

+   **名义变量**：频率和计数编码器对名义分类变量有效

+   **未见变量**：频率和计数编码器可以通过用 `0` 值编码未见变量来处理它们

## 计数 / 频率编码器的缺点

+   **相似编码**：如果所有类别的计数相似，则编码后的值将相同。

```py
import category_encoders as ce

# Count Encoding - fit on training data, transform test data
encoder = ce.CountEncoder(cols="type")
data_train["type_count_encoded"] = encoder.fit_transform(data_train["type"])
data_test["type_count_encoded"] = encoder.transform(data_test["type"])

# Frequency (normalized count) Encoding
encoder = ce.CountEncoder(cols="type", normalize=True)
data_train["type_frequency_encoded"] = encoder.fit_transform(data_train["type"])
data_test["type_frequency_encoded"] = encoder.transform(data_test["type"])
```

# №5\. 二进制 / BaseN 编码

> 二进制编码将分类变量编码为整数，然后转换为二进制代码。输出类似于独热编码，但创建的列更少。

这解决了独热编码的缺点，其中 `n` 的基数不会导致 `n` 列，而是 `log2(n)` 列。BaseN 编码遵循相同的理念，但使用其他基数值而不是 2，从而得到 `logN(n)` 列。

![](img/0e81081ecfdb661632192a68fea03c21.png)

图 6: 二进制编码示例 — 作者提供的图像

## 何时使用二进制编码器

+   **名义变量**：二进制和 BaseN 编码器用于名义分类变量

+   **高基数**：二进制和 BaseN 编码在类别数量很高时表现良好

+   **缺失或未见变量**：二进制和 BaseN 编码器可以通过在所有列中用 `0` 值编码未见变量来处理它们

```py
import category_encoders as ce

# Binary Encoding - fit on training data, transform test data
encoder = ce.BinaryEncoder()
data_encoded = encoder.fit_transform(data_train["type"])
data_train = pd.concat([data_train, data_encoded], axis=1)

data_encoded = encoder.transform(data_test["type"])
data_test = pd.concat([data_test, data_encoded], axis=1)

# BaseN Encoding - fit on training data, transform test data
encoder = ce.BaseNEncoder(base=5)
data_encoded = encoder.fit_transform(data_train["type"])
data_train = pd.concat([data_train, data_encoded], axis=1)

data_encoded = encoder.transform(data_test["type"])
data_test = pd.concat([data_test, data_encoded], axis=1)
```

# №6\. 哈希编码

> 哈希编码将分类变量编码为使用哈希函数生成的不同哈希值。输出类似于独热编码，但可以选择创建的列数。

哈希编码与二进制编码类似，因为它们比独热编码更节省空间，但哈希编码使用哈希函数而不是二进制数。

![](img/465b520aae5b9b37b3af018d62048f45.png)

图 7: 使用 2 列的哈希编码示例 — 作者提供的图像

哈希编码可以将高基数数据编码为固定大小的数组，因为新列的数量是手动指定的。这也类似于[TSNE](https://scikit-learn.org/stable/modules/generated/sklearn.manifold.TSNE.html)或[谱嵌入](https://scikit-learn.org/stable/modules/generated/sklearn.manifold.SpectralEmbedding.html)等降维算法，这些算法通过特征值和其他距离度量构建固定大小的数组。

## 何时使用哈希编码器

+   **名义变量**：哈希编码器用于名义分类变量。

+   **高基数**：哈希编码在类别数量较多时表现良好。

+   **缺失或未见变量**：哈希编码器可以通过在所有列中用空值编码未见变量来处理这些变量。

## 哈希编码器的缺点

+   **不可逆**：哈希函数是单向的，即原始输入可以被哈希为哈希值，但无法从哈希值中恢复原始输入。

+   **信息丢失或冲突**：如果创建的列数过少，哈希编码可能会导致信息丢失，因为多个不同的输入可能会导致哈希函数产生相同的输出。

哈希编码可以使用来自 sklearn 包的`FeatureHasher`或来自分类编码器包的`HashingEncoder`来完成。

```py
from sklearn.feature_extraction import FeatureHasher

# Hash Encoding - fit on training data, transform test data
encoder = FeatureHasher(n_features=2, input_type="string")
data_encoded = pd.DataFrame(encoder.fit_transform(data_train["type"]).toarray())
data_train = pd.concat([data_train, data_encoded], axis=1)

data_encoded = pd.DataFrame(encoder.transform(data_test).toarray())
data_test = pd.concat([data_test, data_encoded], axis=1)
```

使用`category_encoders`，

```py
import category_encoders as ce

# Hash Encoding - fit on training data, transform test data
encoder = ce.HashingEncoder(n_components=2)
data_encoded = encoder.fit_transform(data_train["type"])
data_train = pd.concat([data_train, data_encoded], axis=1)

data_encoded = encoder.transform(data_test["type"])
data_test = pd.concat([data_test, data_encoded], axis=1)
```

希望你对将分类数据编码为数值数据的不同方法有更多了解。选择使用哪种特征编码技术时，重要的是考虑**分类数据的类型**（名义或序数）、**使用的机器学习模型**以及每种方法的**优缺点**。

由于模式或趋势的变化，在测试期间也要考虑缺失或未见的变量，以确保数据科学工作流在生产中不会失败！

# 有用的链接

+   `sklearn` 文档：[`scikit-learn.org/stable/modules/classes.html#module-sklearn.preprocessing`](https://scikit-learn.org/stable/modules/classes.html#module-sklearn.preprocessing)

+   `category_encoders` 文档：[`contrib.scikit-learn.org/category_encoders/`](https://contrib.scikit-learn.org/category_encoders/)
