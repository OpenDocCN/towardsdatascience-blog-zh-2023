# 线性回归 — 预测机器学习建模的奥卡姆剃刀

> 原文：[`towardsdatascience.com/linear-regression-occams-razor-of-predictive-machine-learning-modeling-f2ba5b144a2b`](https://towardsdatascience.com/linear-regression-occams-razor-of-predictive-machine-learning-modeling-f2ba5b144a2b)

## 使用 Python 进行线性回归的机器学习建模

[](https://medium.com/@fmnobar?source=post_page-----f2ba5b144a2b--------------------------------)![Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----f2ba5b144a2b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f2ba5b144a2b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f2ba5b144a2b--------------------------------) [Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----f2ba5b144a2b--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f2ba5b144a2b--------------------------------) ·阅读时间 16 分钟·2023 年 1 月 3 日

--

![](img/2494e60058d5a0eb46decef5f125cf20.png)

水晶球，由 [DALL.E 2](https://openai.com/dall-e-2/)

你熟悉奥卡姆剃刀吗？我记得在《生活大爆炸》电视剧中提到过它！奥卡姆剃刀的观点是，在其他条件相同的情况下，现象的最简单解释更可能是真实的，而不是更复杂的解释（即最简单的解决方案几乎总是最佳解决方案）。我认为机器学习预测建模中的奥卡姆剃刀就是线性回归，这几乎是使用的最简单建模方法，并且对于某些任务可能是最佳解决方案。本文将介绍线性回归的概念和实现。

与我的其他帖子类似，通过实践问题和答案来实现学习。我会在问题中提供提示和解释，以便让过程更简单。最后，我使用的创建此练习的笔记本也链接在文章底部，你可以下载、运行并跟随学习。

开始吧！

*(除非另有说明，否则所有图片均由作者提供。)*

[](https://medium.com/@fmnobar/membership?source=post_page-----f2ba5b144a2b--------------------------------) [## 通过我的推荐链接加入 Medium - Farzad Mahmoodinobar

### 阅读 Farzad 的每一个故事（以及 Medium 上其他作者的故事）。你的会员费直接支持 Farzad 和其他人…

medium.com](https://medium.com/@fmnobar/membership?source=post_page-----f2ba5b144a2b--------------------------------)

# 数据集

为了练习线性回归，我们将使用来自 [UCI 机器学习库](https://archive-beta.ics.uci.edu/dataset/10/automobile)（CC BY 4.0）的汽车价格数据集。我已经清理了部分数据供我们使用，可以从 [这个链接](https://gist.github.com/fmnobar/c9b4029e08e97978a9a53f4eb034b16f) 下载。

> 我将解释我们在练习中使用的线性回归模型背后的数学。虽然理解这些数学内容不是成功理解本文内容的必要条件，但我建议你了解它，以便更好地理解创建线性回归模型时的幕后过程。

# 线性回归基础

线性回归是使用线性预测变量（或自变量）来预测因变量。一种简单的例子是直线公式：

![](img/681b884e40b7a6b0db5530abb337ed9b.png)

在这种情况下，`y` 是因变量，而 `x` 是自变量（`c` 是一个常数）。线性回归模型的目标是确定最佳系数（上例中的 `a`），以使 `x` 能最准确地预测 `y`。

现在，让我们将这个例子推广到多元线性回归。在多元线性回归模型中，目标是找到描述因变量与多个自变量之间关系的最佳拟合线。

![](img/daebd57c67021726b1c7287621920276.png)

在这种情况下，我们有多个自变量（或预测变量），从 `x_1` 到 `x_n`，每个自变量都乘以它自己的系数，以预测因变量 `y`。在线性回归模型中，我们将尝试确定系数 `a_1` 到 `a_n` 的值，以获得对因变量 `y` 的最佳预测。

现在我们了解了线性回归的概念，让我们转向普通最小二乘（OLS）回归，这是一种线性回归形式。

# 普通最小二乘回归

普通最小二乘回归模型通过最小化残差平方和来估计回归模型的系数。残差是回归线（即预测值）与实际值之间的垂直距离，如下图所示。这些残差被平方，以便错误不会相互抵消（当一个预测值高于实际值，而另一个预测值低于实际值时，这两个仍然是错误，不应相互抵消）。

![](img/814567d23e6731a092e28fec2bdddb61.png)

普通最小二乘回归——回归线和残差

现在我们理解了基本概念，我们将开始探索数据和我们可能用来预测汽车价格的变量（或特征）。然后，我们将数据分为训练集和测试集，以建立回归模型。接下来，我们将查看回归模型的性能，最后绘制结果。

让我们开始吧！

# 1\. 探索性分析

让我们首先查看数据，可以从[这里](https://gist.github.com/fmnobar/c9b4029e08e97978a9a53f4eb034b16f)下载。首先，我们将导入 Pandas 和 NumPy。然后，我们将读取包含数据集的 CSV 文件，并查看数据集的前五行。

```py
# Import libraries
import pandas as pd
import numpy as np

# Show all columns/rows of the dataframe
pd.set_option("display.max_columns", None)
pd.set_option("display.max_rows", None)

# To show all columns in one view
from IPython.display import display, HTML
display(HTML("<style>.container { width:100% !important; }</style>"))

# Read the csv into a dataframe
df = pd.read_csv('auto-cleaned.csv')

# Display top five rows
df.head()
```

结果：

![](img/eb8ad12479cdd91d62cb8c73d320fd73.png)

列名大多是显而易见的，所以我只会添加那些对我不太明显的列名。你现在可以忽略这些，并在需要列名定义时参考它们。

+   symboling: 保险公司根据车辆的风险感知分配的值。+3 表示车辆有风险，-3 表示车辆安全

+   aspiration: 标准或涡轮

+   drive-wheels: rwd 表示后驱；fwd 表示前驱；4wd 表示四轮驱动

+   wheel-base: 前后轮中心之间的距离，以厘米为单位

+   engine-type: dohc 表示双顶置凸轮；dohcv 表示双顶置凸轮和气门；l 表示 L 型发动机；ohc 表示顶置凸轮；ohcf 表示顶置凸轮和气门 F 型发动机；ohcv 表示顶置凸轮和气门；rotor 表示旋转发动机

+   bore: 汽缸的内径，以厘米为单位

+   stroke: 汽缸的运动

**问题 1：**

数据框中是否有缺失值？

**回答：**

```py
df.info()
```

结果：

![](img/6d3e3b7d855ecea3cbdd3e6f348f436b.png)

如我们所见，共有 25 列（注意列编号从 0 到 24）和 193 行。列中没有空值。

# 2\. 特征选择

特征选择是识别和选择一组相关特征（也称为“预测变量”、“输入”或“属性”）以构建机器学习模型的过程。特征选择的目标是通过减少模型的复杂性和消除不相关、冗余或噪声特征，从而提高模型的准确性和可解释性。

**问题 2：**

创建一个表格，显示数据框中列之间的相关性。

**回答：**

我们将使用`pandas.DataFrame.corr`，它计算列之间的逐对相关性。需要考虑两个要点：

1.  `pandas.DataFrame.corr`将排除空值。我们确认我们的数据集不包含任何空值，但在处理含有空值的练习中，这可能很重要。

1.  我们将仅限制数值的相关性，并将在后续练习中讨论分类值。

作为回顾，让我们在继续之前复习一下分类变量和数值变量是什么。

在机器学习中，分类变量是可以取有限数量值的变量。这些值表示不同的类别，值本身没有固有的顺序或数值意义。分类变量的例子包括性别（男性或女性）、婚姻状况（已婚、单身、离婚等）。

数值变量是可以在特定范围内取任意数值的变量。这些变量可以是连续的（意味着它们可以在特定范围内取任意值）或离散的（意味着它们只能取特定的、预定的值）。数值变量的例子包括年龄、身高、体重等。

处理完这些后，让我们计算相关性。

```py
corr = np.round(df.corr(numeric_only = True), 2)
corr
```

结果：

![](img/0b403d5f49deaa35f582b0961ea6feb2.png)

**问题 3：**

在上一问题中生成了许多相关值。我们更关心与汽车价格的相关性。展示与汽车价格的相关性，并按从大到小的顺序排列。

**答案：**

```py
price_corr = corr['price'].sort_values(ascending = False)
price_corr
```

结果：

![](img/e8d1690d750aa81648fbc0a35675b7eb.png)

这非常有趣。例如，“engine-size”似乎与价格的相关性最高，这在预期之中，而“compression-ratio”似乎与价格的相关性不那么高。另一方面，“symboling”，我们记得这是衡量汽车风险的指标，与汽车价格呈负相关，这也符合直觉。

**问题 4：**

为了更专注于相关特征以构建汽车价格模型，筛选出与价格相关性较弱的列，我们将定义任何相关性绝对值小于 0.2 的特征（这是为了本练习而任意选择的值）。

**答案：**

```py
# Set the threshold
threshold = 0.2

# Drop columns with a correlation less than the threshold
df.drop(price_corr.where(lambda x: abs(x) < threshold).dropna().index, axis = 1, inplace = True)

df.info()
```

结果：

![](img/aea2051e3164f7de810698a44ff777cd.png)

我们看到由于这个原因，我们现在剩下 19 个特征（总共有 20 列，但其中一个是价格本身，所以剩下 19 个特征或预测变量）。

**问题 5：**

现在我们有了更可管理的特征数量，重新查看一下它们，看看是否需要删除其中任何一个。

***提示：*** *一些特征可能非常相似，可能是冗余的。有些可能实际上并不重要。*

**答案：**

让我们查看数据框，然后查看剩余特征之间的相关性。

```py
df.head()
```

结果：

![](img/00d5ea313f493625011f16024e2576ea.png)

```py
# Calculate correlations
round(df.corr(numeric_only = True), 2)
```

结果：

![](img/b5ac276e4dab731b663e69f57182034d.png)

“wheel-base”（前后轮之间的距离）和“length”（汽车的总长度）高度相关，似乎传达了相同的信息。此外，“city-mpg”和“highway-mpg”也高度相关，因此我们可以考虑删除其中一个。让我们继续删除“wheel-base”和“city-mpg”，然后再查看数据框的前五行。

```py
# Drop the columns
df.drop(['wheel-base', 'city-mpg'], axis = 1, inplace = True)

# Return top five rows of the remaining dataframe
df.head()
```

结果：

![](img/d2eaea36a671a657462c276b6c803e81.png)

正如我们所见，新的数据框更小，不包括我们刚刚删除的两个列。接下来，我们将讨论分类变量。

## 2.1\. 虚拟编码

让我们更仔细地查看“make”和“fuel-type”列的值。

```py
df['make'].value_counts()
```

结果：

![](img/7c5e9096f7d2a55e7c573d50e607d725.png)

```py
df['fuel-type'].value_counts()
```

结果：

![](img/2c920a9e96fd9e2516ccb31cc3eaff17.png)

这两列是分类值（例如丰田或柴油），而不是数值。

为了将这些分类变量纳入我们的回归模型中，我们将为这些分类变量创建“虚拟编码”。

虚拟编码是将一个列中的分类变量（或预测变量）替换为多个二进制列。例如，假设我们有一个如表中所示的分类变量：

![](img/be2d6ff2c74f37d7aa906bc1d8c006b4.png)

分类变量 — 虚拟编码前

如上表所示，“random_categorical_variable”可以有 A、B 和 C 三个分类值。我们希望通过虚拟编码将分类变量转换为一种更易于在回归模型中使用的格式，这将把它转换为 A、B 和 C 三个单独的列，并带有二进制值，如下所示：

![](img/745f3d431c3ab9d3debc858990d29472.png)

分类变量 — 虚拟编码后

让我们看看如何在 Python 中实现虚拟编码。

**问题 6：**

对我们数据框的分类列进行虚拟编码。

答案：

让我们首先看看数据框在虚拟编码前的样子。

```py
df.head()
```

结果：

![](img/e06f558283eea02f5eacd756842ecd35.png)

从之前的问题我们知道，“fuel-type”列可以取 2 个不同的值（即汽油和柴油）。因此，在虚拟编码后，我们期望将“fuel-type”列替换为 2 个单独的列。其他分类列也是如此，具体取决于每个列有多少个唯一值。

首先，我们只对“fuel-type”列进行虚拟编码作为示例，并观察数据框的变化，然后我们可以继续对其他分类列进行虚拟编码。

```py
# Dummy code df['fuel-type']
df = pd.get_dummies(df, columns = ['fuel-type'], prefix = 'fuel-type')

# Return top five rows of the updated dataframe
df.head()
```

结果：

![](img/0be5e2605762055a81e9e8b81816af61.png)

正如预期的那样，我们现在有两个原始的“fuel-type”列，分别命名为“fuel-type_gas”和“fuel-type_diesel”。

接下来，让我们识别所有分类列并对其进行虚拟编码。

```py
# Select "object" data types
columns = df.select_dtypes(include='object').columns

# Dummy code categorical columns
for column in columns:
    df = pd.get_dummies(df, columns = [column], prefix = column)

# Return top five rows of the resulting dataframe
df.head()
```

结果：

![](img/477e0a07ebf3588fbfc63241c71f8ab6.png)

请注意，上述快照未覆盖虚拟编码后的所有列，因为现在我们有 63 列，这在快照中展示会显得太小。

最后，现在我们创建了所有这些新列，让我们重新创建价格与所有其他列之间的相关性，并从高到低进行排序。

```py
# Re-create the correlation matrix
corr = np.round(df.corr(numeric_only = True), 2)

# Return correlation with price from highest to the lowest
price_corr = corr['price'].sort_values(ascending = False)
price_corr
```

结果：

![](img/9c7563699f6719c7abc2c85654efa98c.png)

如上所示，一些分类变量与价格有较高的相关性，例如“drive-wheels”和“num-of-cylinders”。

到此为止，我们已经对数据有所了解，并在一定程度上清理了数据，现在让我们继续进行创建模型以根据这些属性预测汽车价格的主要目标。

# 3\. 将数据分割为训练集和测试集

此时，我们将首先将数据拆分为因变量和自变量。因变量或“y”是我们要预测的内容，在这个练习中是“价格”。它被称为因变量，因为它的值依赖于自变量的值。自变量或“X”是目前数据框中所有其他变量或特征，包括“发动机尺寸”、“马力”等。

接下来，我们将数据拆分为训练集和测试集。顾名思义，训练数据集将用于训练回归模型，然后我们将使用测试集来测试模型的性能。我们拆分数据是为了确保模型在训练过程中看不到测试集，从而使测试集能够很好地代表模型的性能。将数据拆分为训练集和测试集很重要，因为使用相同的数据来拟合模型和评估其性能可能导致过拟合。过拟合发生在模型过于复杂时，它学习了数据中的噪声和随机波动，而不是潜在的模式。因此，模型可能在训练数据上表现良好，但在新见数据上表现不佳。

**问题 7：**

将因变量（目标）分配给 y，将自变量（或特征）分配给 X。

**答案：**

```py
X = df.drop(['price'], axis = 1)
y = df['price']
```

**问题 8：**

将数据拆分为训练集和测试集。使用 30% 的数据作为测试集，并使用 `random_state` 为 1234。

**答案：**

```py
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.3, random_state = 1234)
```

**问题 9：**

使用训练集训练线性回归模型。

**答案：**

```py
from sklearn.linear_model import LinearRegression

# First create an object of the class
lr = LinearRegression()

# Now use the object to train the model
lr.fit(X_train, y_train)

# Let's look at the coefficients of the trained model
lr.coef_
```

结果：

![](img/f621a2e7175b2d4c43945becfb948c12.png)

我们将讨论这里发生了什么，但首先让我们看看如何评估机器学习模型。

# 4\. 模型评估

## 4.1\. R²

**问题 10：**

训练模型的得分是多少？

**答案：**

为此，我们可以使用 LinearRegression 的 “score()” 方法，它返回预测的决定系数或 R²$，计算如下：

![](img/6dc564ec022cd3d147dcff7c4bdea2bc.png)![](img/11dffd9b28f0fd2a5421f3642d067a80.png)

最好的得分是 1.0\. 一个始终预测“y”期望值的常数模型，无论输入特征如何，都将得到 R² 分数 0.0。

了解这些知识后，我们来看看实现过程。

```py
score = lr.score(X_train, y_train)

print(f"Training score of the model is {score}.")
```

结果：

![](img/24e8597820edac349384c602fbe2d4f3.png)

**问题 11：**

预测测试集的值，然后评估训练模型在测试集上的表现。

**答案：**

```py
# Predict y for X_test
y_pred = lr.predict(X_test)

score_test = lr.score(X_test, y_test)
print(f"Test score of the trained model is {score_test}.")
```

结果：

![](img/6e762acf16b46983385cf997279802de.png)

## 4.2\. 均方误差

均方误差（Mean Squared Error, MSE）是平方误差的平均值，计算如下：

![](img/80dbaace5ecde5a193981630d22b1075.png)

**问题 12：**

计算测试集预测结果的均方误差（Mean Squared Error）和决定系数（R²）。

**答案：**

```py
from sklearn.metrics import mean_squared_error, r2_score

print(f"R²: {r2_score(y_pred, y_test)}")
print(f"MSE: {mean_squared_error(y_pred, y_test)}")
```

结果：

![](img/38284a52fe093e63223b6a7b1948926b.png)

**问题 13：**

你如何解读前一个问题的结果？对下一步有什么建议？

**答案：**

R² 相对较高，但均方误差（MSE）也很高，这可能表明误差可能过大——注意这实际上取决于业务需求以及模型的用途。在某些情况下，90.6% 的 R² 可能足够满足业务需求，而在其他情况下，这个数字可能还不够好。这种性能水平可能受到一些不是强预测价格的特征的影响。让我们看看能否识别出哪些特征不是强预测因子并将其删除。然后我们可以重新训练并再次查看评分，以查看是否能够改善我们的模型。

为了尝试一些新的方法，我们将使用 statsmodels 库中的普通最小二乘法（OLS）。训练和预测测试集值的步骤与之前相同。

```py
# Import libraries
import statsmodels.api as sm

# Initialize the model
sm_model = sm.OLS(y_train, X_train).fit()

# Create the predictions
sm_predictions = sm_model.predict(X_test)

# Return the summary results
sm_model.summary()
```

结果：

![](img/072aed0efd18d7c8f0abafb182807484.png)

这个方法提供了特征的良好展示和该特征显著性的 p 值测量。例如，如果我们使用 0.05 或 5% 的显著性水平（或 95% 的置信水平），我们可以排除那些“P > |t|”大于 0.05 的特征。

```py
len(sm_model.pvalues.where(lambda x: x > 0.05).dropna().index)
```

结果：

```py
43
```

共有 43 列这样的数据。让我们删除这些列，看看结果是否有所改善。

```py
# Create a list of columns that meee the criteria
columns = list(sm_model.pvalues.where(lambda x: x > 0.05).dropna().index)

# Drop those columns
df.drop(columns, axis = 1, inplace = True)

# Revisit the process to create a new model summary
X = df.drop(['price'], axis = 1)
y = df['price']

# Split data into train and test sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.3, random_state = 1234)

# Train the model
sm_model = sm.OLS(y_train, X_train).fit()

# Create predictions using the trained model
sm_predictions = sm_model.predict(X_test)

# Return model summary
sm_model.summary()
```

结果：

![](img/8ec99628d8067f6c8c8e04199dadefb4.png)

根据 R 平方值判断，整体性能从 0.967 提升至 0.972，并且我们减少了列数，这使得我们的模型和分析更加高效。

# 5\. 预测值与实际值的图

**问题 14：**

创建预测值与实际值的散点图。如果所有预测值都与实际值相匹配，我们会期望所有点都沿着一条直线如 `f(x) = x` 分布。为比较添加一条红色的直线。

**答案：**

```py
# Import libraries
import matplotlib.pyplot as plt
%matplotlib inline

# Define figure size
plt.figure(figsize = (7, 7))

# Create the scatterplot
plt.scatter(y_pred, y_test)
plt.plot([y_pred.min(), y_pred.max()], [y_pred.min(), y_pred.max()], color = 'r')

# Add x and y labels
plt.xlabel("Predictions")
plt.ylabel("Actuals")

# Add title
plt.title("Predictions vs. Actuals")
plt.show()
```

结果：

![](img/bcee29cf4f4b942a17d73866cf3a019a.png)

训练模型的预测值与实际值的散点图

正如我们预期的那样，值围绕直线散布，显示出模型产生了较好的预测水平。点在红线的右侧，意味着模型预测的价格高于实际价格，而在红线左侧的点则表示相反的情况。

# 带有练习问题的笔记本

以下是包含问题和答案的笔记本，您可以下载并进行练习。

# 结论

在这篇文章中，我们讨论了在某些情况下，最简单的解决方案可能是最合适的解决方案，并介绍了如何将线性回归作为预测机器学习任务中的一种解决方案进行实现。我们首先学习了线性回归背后的数学原理，然后实现了一个模型来根据现有的汽车属性预测汽车价格。接着，我们测量了模型的性能，并采取了某些措施以提高模型的性能，最后通过散点图可视化了训练模型预测值与实际值的比较。

# 感谢阅读！

如果你觉得这篇文章对你有帮助，请 [关注我的 Medium](https://medium.com/@fmnobar) 并订阅以接收我的最新文章！
