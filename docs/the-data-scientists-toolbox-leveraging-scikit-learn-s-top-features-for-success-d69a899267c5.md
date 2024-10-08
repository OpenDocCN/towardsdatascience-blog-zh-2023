# 数据科学家的工具箱：利用 scikit-learn 的顶级功能取得成功

> 原文：[`towardsdatascience.com/the-data-scientists-toolbox-leveraging-scikit-learn-s-top-features-for-success-d69a899267c5`](https://towardsdatascience.com/the-data-scientists-toolbox-leveraging-scikit-learn-s-top-features-for-success-d69a899267c5)

## 理解 scikit-learn：统一的机器学习方法

[](https://federicotrotta.medium.com/?source=post_page-----d69a899267c5--------------------------------)![Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----d69a899267c5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d69a899267c5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d69a899267c5--------------------------------) [Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----d69a899267c5--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d69a899267c5--------------------------------) ·12 min read·2023 年 6 月 12 日

--

![](img/f2fe0e32cf21d7fba7d734fb9ffbf0c9.png)

图片来源：[Tayeb MEZAHDIA](https://pixabay.com/it/users/tayebmezahdia-4194100/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3174729) 在 [Pixabay](https://pixabay.com/it//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3174729)

Python 有很多库，使其成为最常用的编程语言之一。它们大多数具有类似的功能，可以相互使用并达到相同的结果。但当涉及到机器学习时，我们唯一可以谈论的库就是`sklearn`。

无论你是 ML 从业者还是初学者，我知道你对`sklearn`有一定的了解。但有时我们使用某些工具只是因为大家都在用，而没有提出任何问题。

比如，你是否曾经问过自己：“*`sklearn`中最重要的 5 个特性是什么*？” 好吧，让我告诉你一件事：我大约花了 2 年的时间才问自己这个问题，所以如果你还没有问，也不必感到羞愧。

所以，在这篇文章中，我将回答这个问题；以下是你将在这里找到的内容：

```py
**Table of contents:** 
Feature #1: consistency
Feature #2: wide range of algorithms
Feature #3: data preprocessing and feature engineering
Feature #4: model evaluation and validation
Feature #5: integration with the Python data science ecosystem
Coding examples
```

# 特性 #1：一致性

`sklearn`的强大之处在于它是一致的库。这是什么意思？这意味着它具有：

1.  **轻松与外部 DL/ML 库集成**。它可以与外部 ML/DL 库集成，例如`Keras`、`Tensorflow`和`XGBoost`，仅举几个例子。这种兼容性使我们能够将`sklearn`的功能与这些库的功能结合起来，使得我们数据专业人员的开发阶段变得轻松无痛。

1.  **使用简便性**。`sklearn` 在其提供的各种机器学习算法中保持统一的接口。这样我们不需要学习新的语法或模式：我们只需遵守“相同的规则”并思考开发我们的机器学习算法。你能想象在处理不同算法时需要处理不同的语法吗？我可不想这样（对不起，如果我不能！）。

1.  **文档完善**。`sklearn` 提供了详尽的文档，包括教程和使用示例。文档还涵盖了库的各个方面，从数据预处理到模型评估。你可以在[这里](https://scikit-learn.org/stable/index.html)参考。

# 特征 #2：广泛的算法范围

`sklearn` 库提供了非常广泛的机器学习算法，从监督学习到无监督学习，包括回归和分类模型。

我们作为数据科学家所拥有的这种广泛可能性，使得 `sklearn` 成为机器学习的完美库：一个适用于我们日常工作的所有模型的环境。

我们只需要选择最适合我们特定问题领域的算法，而不必抱怨集成和其他类似问题：这是一个巨大的优势！

# 特征 #3：数据预处理和特征工程

数据预处理和特征工程是机器学习问题的重要部分，因为它们涉及为机器学习算法准备数据。以下是 `sklearn` 提供的用于这些任务的特征：

## 数据预处理

1.  **处理缺失数据**。如果你练习机器学习一段时间，我相信你会发现处理缺失数据有些痛苦。或者，至少对我来说是这样。`sklearn` 提供了不同的处理缺失数据的特征，例如：a) `SimpleImputer()` 类，它允许我们使用不同的策略（如均值、中位数、众数或常数值）来填补缺失值。填补可以应用于特定列或整个数据集。b) `IterativeImputer()` 类，它利用机器学习模型迭代填补缺失值。它使用选定模型的预测来填补缺失值。c) `MissingIndicator()` 类，用于创建数据集中缺失值的二进制指示器。它添加了一个新特征，标记值是否原本缺失，为模型提供额外的信息。

1.  **特征缩放**。具有不同尺度的特征可能会以不良方式影响许多机器学习算法的性能。幸运的是，`sklearn` 提供了一些缩放特征的方法，例如：a) 标准化（`StandardScaler()`），它将特征缩放到均值为零、方差为一的状态，b) 归一化（`MinMaxScaler()`），它将特征缩放到特定范围内。

1.  **编码分类变量**。许多机器学习算法无法直接处理分类变量。因此，`sklearn` 提供了几种编码技术，例如：a）独热编码（`OneHotEncoder()`）和 b）有序编码（`OrdinalEncoder()`），将分类变量转换为数值表示。这使得处理字符串形式的数据并将其转换为数字变得简单，以便我们可以将其输入到机器学习算法中。

1.  **处理异常值**。异常值对数据科学家来说是一个普遍的难题，因为它们会影响机器学习模型的性能，因此需要特别处理。`sklearn` 提供了一些异常值检测和去除的技术，例如局部离群因子（`LOF()`）算法和孤立森林算法（`IsolationForest()`）。注意，这些都是无监督的机器学习算法。

## 特征工程：

1.  **特征提取**。`sklearn` 提供了不同的方法来从数据中提取有意义的特征。实际上，并不是我们拥有的所有特征对于机器学习问题都是有意义的，通常我们只处理有意义的特征是个普遍的经验法则。在特征提取方面，`sklearn` 提供了：a）`CountVectorizer()` 类，它可以将文本文档转换为令牌计数矩阵；b）`TfidfVectorizer()` 类，它计算词频-逆文档频率（TF-IDF）来表示文本数据。

1.  **降维**。`sklearn` 提供了各种降维技术，这些技术对于减少特征数量同时保留关键信息非常有用。两种流行的方法是：a）主成分分析（`PCA()`）和 b）t-分布随机邻居嵌入（`TSNE()`）。

1.  **特征选择**。最后，`sklearn` 还提供了多种特征选择方法，例如：a）单变量特征选择（`SelectKBest()`），b）递归特征消除（`RFE()`），和 c）基于树模型的特征重要性排名（`SelectFromModel()`）。

# 特征 #4：模型评估和验证

模型评估和验证是机器学习中至关重要的活动。实际上，作为数据科学家，我们的角色是找到最适合数据的模型；但这到底意味着什么呢？

这意味着我们需要训练一些机器学习模型，并找到那些在未见数据上做出最佳预测（或分类）的模型。这意味着我们首先需要在训练数据上评估一些机器学习模型，然后在新的、未见的数据上验证它们。`sklearn` 提供了不同的功能来帮助我们完成这些基本任务。

1.  **交叉验证技术**。交叉验证是一种重要的模型评估技术，帮助我们应对过拟合。它通过将数据划分为多个子集，在这些子集的不同组合上训练模型，并平均结果来估计模型的性能。`sklearn` 为我们提供了以下交叉验证方法：a）k 折交叉验证，b）分层 k 折交叉验证，以及 c）留一交叉验证。

1.  **评估指标**。`sklearn` 提供了广泛的评估指标，让我们能够数值化分类和回归模型的性能。对于分类任务，我们可以提到，如 `accuracy`、`precision`、`recall` 和 AUC/ROC。回归任务则可以使用均方误差（`MSE`）、均绝对误差（`MAE`）、R² 等指标进行评估。如果你需要深入了解如何使用这些指标（以及 `sklearn` 提供的所有主要指标），这里有 分类指标的终极指南。这里还有 回归任务的终极指南。

1.  **超参数调优**。模型评估和验证与超参数调优密切相关，后者涉及选择最佳的模型参数组合以获得 ML 算法的最佳性能。`sklearn` 提供了像 `GridSearchCV` 和 `RandomizedSearchCV` 这样的工具用于使用交叉验证进行超参数调优。这些技术系统地探索超参数空间，识别出最佳的超参数组合，从而获得最佳模型性能，基于所选择的评估指标。

# 特性 #5：与 Python 数据科学生态系统的集成

`sklearn` 的超能力之一是与 Python 数据科学生态系统的集成。这使得数据科学家能够利用 `sklearn` 的力量与数据处理、可视化和分析的库和工具结合。以下是一些例子：

1.  **Pandas 和 Numpy**。`sklearn` 与 `Pandas` 数据框集成的可能性使用户能够将数据框直接传递给 `sklearn` 的函数和类。这使得在将数据输入 `sklearn` 模型之前，使用 `Pandas` 进行数据预处理和清理变得非常方便。`sklearn` 也在数据表示上严重依赖 `NumPy` 数组。实际上，它可以直接接受 `NumPy` 数组作为训练任务的输入。

1.  **Matplotlib 和 Seaborn**。`sklearn` 提供了内置支持，用于使用 `Matplotlib` 绘制指标和模型评估结果。这使用户能够可视化模型性能、特征重要性和其他相关统计数据，帮助我们理解我们的 ML 模型。此外，由于 `Seaborn` 是一个建立在 `Matplotlib` 之上的数据可视化库，`sklearn` 也与之兼容，使用户能够将 `sklearn` 的 ML 算法的力量与 `Seaborn` 的美观图表相结合，用于探索性数据分析和模型评估。

# 代码示例

在最后一段中，我想强调一些使用示例。如果你有使用 `sklearn` 的经验，我知道你知道如何使用它。但请考虑到，这里重点不是展示这些特性如何工作，而是强调工作流程的运作方式，基于我们在前面段落中所说的内容。因为，通常我们使用一些工具只是为了使用它们，且因为每个人都这样做，而不提出任何问题。

## 示例 #1：与 Pandas 和 NumPy 的集成

让我们看看如何与 `sklearn`、`Pandas` 和 `NumPy` 集成。为了简单起见，我们可以使用 ChatGPT 的提示。但在此之前，你可能需要了解如何作为数据科学家有效地使用提示工程：

[](/unleashing-the-power-of-prompt-engineering-for-data-scientists-16b6d1f2bf85?source=post_page-----d69a899267c5--------------------------------) ## 释放数据科学家提示工程的力量

### 如果你从事数据工作，如何以及为什么编写有效的提示

[towardsdatascience.com

提示可能是以下内容：“*创建一个用于回归分析的 Pandas 数据框，并包含 5 个特征。然后，对数据进行归一化，并将其分为训练集和测试集。使用线性回归模型拟合测试集。打印训练集和测试集的 R² 值*”。

然后我们得到：

> 当然！我将创建一个包含五个特征的示例 Pandas 数据框，对数据进行归一化，将其分为训练集和测试集，拟合一个线性回归模型，并打印训练集和测试集的 R² 分数。这是一个示例实现：

```py
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import r2_score
from sklearn.preprocessing import StandardScaler

# Generating random features with at least 50 occurrences each
np.random.seed(42)

feature1 = np.random.randint(0, 10, size=100)
feature2 = np.random.randint(0, 10, size=100)
feature3 = np.random.randint(0, 10, size=100)
feature4 = np.random.randint(0, 10, size=100)
feature5 = np.random.randint(0, 10, size=100)
target = np.random.randint(0, 100, size=100)

# Creating a Pandas data frame
data = {
    'Feature1': feature1,
    'Feature2': feature2,
    'Feature3': feature3,
    'Feature4': feature4,
    'Feature5': feature5,
    'Target': target
}

df = pd.DataFrame(data)

# Splitting the data into features and target
X = df.drop('Target', axis=1)
y = df['Target']

# Normalizing the data using StandardScaler
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Splitting the normalized data into train and test sets
X_train, X_test, y_train, y_test = train_test_split(X_scaled, y, test_size=0.2, random_state=42)

# Fitting the linear regression model
reg_model = LinearRegression()
reg_model.fit(X_train, y_train)

# Predicting the target variable for both train and test sets
y_train_pred = reg_model.predict(X_train)
y_test_pred = reg_model.predict(X_test)

# Calculating R² scores for train and test sets
r2_train = r2_score(y_train, y_train_pred)
r2_test = r2_score(y_test, y_test_pred)

# Printing the R² scores
print("R² score for train set:", r2_train)
print("R² score for test set:", r2_test)
```

> 在这个例子中，我创建了一个包含五个特征（`Feature1` 到 `Feature5`）和一个目标变量（`Target`）的 Pandas 数据框。然后，我使用 scikit-learn 的 `StandardScaler` 对特征数据进行了归一化。之后，我将归一化的数据按 80:20 的比例分为训练集和测试集。接着，我在训练集上训练了一个线性回归模型，并预测了训练集和测试集的目标变量。最后，我计算并打印了训练集和测试集的 R² 分数。

所以，这里是在工作流程层面发生的事情：

1.  使用 `NumPy` 创建特征和标签。

1.  特征和标签成为了一个 `Pandas` 数据框。

1.  `Pandas`数据框变成了`X`和`y`：特征的数据框和标签的`Pandas`系列。

1.  `X`和`y`已被缩放和拆分。然后，线性回归模型进行预测，并使用 R²指标在训练集和测试集上进行评估。

所以，如我们所见，我们有一个从`NumPy`开始，到`sklearn`结束的直接工作流，给出所需的结果。不需要任何“奇怪的操作”或集成。

## 示例 #2：与 Matplotlib 的集成

为了展示`sklearn`与`Matplotlib`之间的集成，我们再次利用 ChatGPT，使用以下提示：“*使用 NumPy 生成的数据在 Python 中创建一个二分类问题。标准化并将数据拆分为训练集和测试集。使用随机森林和 KNN 分类器拟合训练集。打印训练集的 ROC 曲线*。”

我们得到：

> 当然！我们将使用 Python 生成随机数据来创建一个二分类问题，然后我们将标准化并将数据拆分为训练集和测试集。我们将使用随机森林和 K 近邻（KNN）分类器拟合训练集，最后，我们将绘制训练集的 ROC 曲线。
> 
> 这是代码：

```py
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.neighbors import KNeighborsClassifier
from sklearn.metrics import roc_curve, roc_auc_score
import matplotlib.pyplot as plt

# Set a random seed for reproducibility
np.random.seed(42)

# Generate random data
num_samples = 1000
num_features = 10

# Generate features (X)
X = np.random.randn(num_samples, num_features)

# Generate labels (y)
y = np.random.randint(2, size=num_samples)

# Standardize the features
X = (X - np.mean(X, axis=0)) / np.std(X, axis=0)

# Split the data into train and test sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Fit the train set using random forest classifier
rf_clf = RandomForestClassifier()
rf_clf.fit(X_train, y_train)

# Fit the train set using K-nearest neighbors (KNN) classifier
knn_clf = KNeighborsClassifier()
knn_clf.fit(X_train, y_train)

# Predict probabilities for the positive class (class 1)
y_train_rf_probs = rf_clf.predict_proba(X_train)[:, 1]
y_train_knn_probs = knn_clf.predict_proba(X_train)[:, 1]

# Calculate the false positive rate (FPR), true positive rate (TPR), and thresholds for the ROC curve
rf_fpr, rf_tpr, rf_thresholds = roc_curve(y_train, y_train_rf_probs)
knn_fpr, knn_tpr, knn_thresholds = roc_curve(y_train, y_train_knn_probs)

# Calculate the AUC score for the ROC curve
rf_auc = roc_auc_score(y_train, y_train_rf_probs)
knn_auc = roc_auc_score(y_train, y_train_knn_probs)

# Plot the ROC curve
plt.plot(rf_fpr, rf_tpr, label=f"Random Forest (AUC = {rf_auc:.2f})")
plt.plot(knn_fpr, knn_tpr, label=f"KNN (AUC = {knn_auc:.2f})")
plt.plot([0, 1], [0, 1], 'k--')  # Diagonal line for random classifier
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('ROC Curve - Train Set')
plt.legend(loc='lower right')
plt.show()
```

> 这段代码生成了 10 个特征和 1000 个样本的随机数据。然后标准化特征，将数据拆分为训练集和测试集，使用随机森林和 KNN 分类器拟合训练集，预测正类的概率，计算每个分类器的 ROC 曲线和 AUC 评分，最后绘制训练集的 ROC 曲线。

如果我们运行上面生成的代码，我们得到：

![](img/73c6667f9d931b6f8b5728116639ca2f.png)

上述生成的代码产生的 ROC 曲线。图片由 Federico Trotta 提供。

所以，这里是过程：

1.  使用`NumPy`创建特征和标签。

1.  特征和标签数组变成了`X`和`y`。

1.  特征和标签数组被分割成训练集和测试集。然后，训练集已使用 KNN 和随机森林分类器进行拟合。

1.  `Matplotlib`库已被用于绘制 ROC 曲线，使我们能够轻松比较两个分类器的性能。

再次，我们有一个直接的工作流，使用了所有我们需要的库，打印出我们想要的结果，无需“额外操作”。

# 结论

在这篇文章中，我们已经看到使`sklearn`成为完美 ML 库的前 5 个特点。如在示例中所见，工作流清晰直接，使我们能够在没有额外操作的情况下进行分析。

虽然我们领域中一些其他库（如`PyCaret`，它毕竟是一个低代码库）正在增长，但`sklearn`无疑拥有我们开发算法所需的一切，不必担心其他方面，如集成、操作等。

**免费 Python 电子书：**

开始学习 Python 数据科学但感到困难？ [***订阅我的新闻通讯，获取我的免费电子书：这将为你提供正确的学习路径，以便通过实际操作学习 Python 数据科学。***](https://federico-trotta.ck.page/a3970f33f4)

喜欢这个故事？通过我的推荐链接成为 Medium 会员，每月只需 5 美元 [点击这里](https://medium.com/@federicotrotta/membership)：我将赚取一小部分佣金，你无需支付额外费用。

[](https://federicotrotta.medium.com/membership?source=post_page-----d69a899267c5--------------------------------) [## 通过我的推荐链接加入 Medium - Federico Trotta

### 阅读 Federico Trotta 的每一个故事（以及 Medium 上成千上万其他作家的故事）。你的会员费将直接支持…

federicotrotta.medium.com](https://federicotrotta.medium.com/membership?source=post_page-----d69a899267c5--------------------------------)
