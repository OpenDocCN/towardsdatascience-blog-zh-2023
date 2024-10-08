# 用这四个鲜为人知的 Scikit-Learn 类简化你的数据准备

> 原文：[`towardsdatascience.com/simplify-your-data-preparation-with-these-4-lesser-known-scikit-learn-classes-70270c94569f`](https://towardsdatascience.com/simplify-your-data-preparation-with-these-4-lesser-known-scikit-learn-classes-70270c94569f)

## 忘掉 train_test_split：Pipeline、ColumnTransformer、FeatureUnion 和 FunctionTransformer 即使在使用 XGBoost 或 LGBM 时也是不可或缺的

[](https://medium.com/@mattchapmanmsc?source=post_page-----70270c94569f--------------------------------)![Matt Chapman](https://medium.com/@mattchapmanmsc?source=post_page-----70270c94569f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----70270c94569f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----70270c94569f--------------------------------) [Matt Chapman](https://medium.com/@mattchapmanmsc?source=post_page-----70270c94569f--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----70270c94569f--------------------------------) ·10 分钟阅读·2023 年 6 月 1 日

--

![](img/4d616242f15fc7f0163616870c608e44.png)

图片由[Victor](https://unsplash.com/@victor_g)提供，来源于[Unsplash](https://unsplash.com/photos/UoIiVYka3VY)

数据准备被公认为数据科学中最不受欢迎的部分。然而，如果做得正确，它其实不必那么令人头疼。

尽管近年来由于 PyTorch、LightGBM 和 XGBoost 的迅猛崛起，scikit-learn 作为*建模*库的流行度有所下降，但它仍然是最佳的*数据准备*库之一。

我不仅仅是指那老掉牙的`train_test_split`。如果你愿意深入挖掘，你会发现一系列对高级数据准备技术非常有用的工具，这些工具与其他库如`lightgbm`、`xgboost`和`catboost`完美兼容，用于后续建模。

在本文中，我将介绍四个 scikit-learn 类，这些类显著加快了我作为数据科学家日常工作中的数据准备流程。

# 1\. Pipeline：无缝结合预处理步骤

Scikit-learn 的`Pipeline`类使你能够将不同的预处理器或模型组合成一个可调用的代码块：

![](img/6aa21afcbb7383b916da6ee40e39478f.png)

作者提供的图片

管道可以由两种不同的东西组成：

+   **变换器**：任何具有`fit()`和`transform()`方法的对象。你可以把变换器看作是用于处理数据的对象，通常在数据准备工作流中会有多个变换器。例如，你可以使用一个变换器来填补缺失值，另一个来缩放特征或对分类变量进行独热编码。`MinMaxScaler()`、`SimpleImputer()`和`OneHotEncoder()`都是变换器的例子。

+   **估计器**：在 scikit-learn 术语中，“估计器”通常指机器学习模型；即具有`fit()`和`predict()`方法的对象。`LinearRegression()`和`RandomForestClassifier()`是估计器的例子。

在一个管道中，你可以链式连接任意数量的**变换器**，使你能够顺序地应用不同的数据预处理步骤。如果愿意，你还可以在末尾添加一个**估计器**（ML 模型），以便利用新变换的数据进行预测，但这不是强制性的。

例如，你可以构建一个管道，首先用零填补缺失值，然后对变量进行独热编码：

![](img/1755b225522bf4b8a22ed49b4b58cdcf.png)

作者提供的图像

或者，如果你想直接在管道中包含建模步骤，你可以构建一个管道，该管道用均值填补缺失值、缩放特征，然后使用`RandomForestRegressor()`进行预测：

![](img/d2adfd6be2a9c50f3bbede182aadabd8.png)

作者提供的图像

使用 scikit-learn 构建管道是非常简单的。

为了说明这一点，我将首先加载一些数据并将其分为训练集和测试集。在这个例子中，我将使用[糖尿病数据集](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.load_diabetes.html)，该数据集由 scikit-learn 提供，包含 442 名糖尿病患者的十个预测变量（*年龄、性别、体质指数、平均血压以及六个血清测量值*）和一个响应变量，表示这些预测变量记录一年后每位患者糖尿病的进展情况。

```py
import pandas as pd
from sklearn.datasets import load_diabetes
from sklearn.model_selection import train_test_split

# Load diabetes dataset into pandas DataFrames
X, y = load_diabetes(scaled=False, return_X_y=True, as_frame=True)

# Split into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3)
display(X_train.head())
display(y_train.head())
```

![](img/01d0df4d0bb03649f88d9ebe00313a71.png)

作者提供的图像。糖尿病数据集和 scikit-learn 在 BSD-3 许可证下发布

接下来，我们定义我们的`Pipeline`。目前，我将定义一个简单的预处理`Pipeline`，包括两个步骤——用均值填补缺失值，并重新缩放所有特征——且不会包含估计器/模型。然而，无论是否包含估计器，原理都是一样的。

```py
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import MinMaxScaler
from sklearn import set_config

# Return pandas DataFrames instead of numpy arrays
set_config(transform_output="pandas")

# Build pipeline
pipe = Pipeline(steps=[
    ('impute_mean', SimpleImputer(strategy='mean')),
    ('rescale', MinMaxScaler())
])
```

一旦我们定义了`Pipeline`，我们就“拟合”它到训练数据集，并用它来转换训练集和测试集：

```py
# Fit the pipeline to the training data
pipe.fit(X_train)

# Transform data using the fitted pipeline
X_train_transformed = pipe.transform(X_train)
X_test_transformed = pipe.transform(X_test)
```

这将给我们两个预处理的数据集（`X_train_transformed`和`X_test_transformed`），准备好进行建模或特征选择等后续步骤。

使用`Pipeline`来处理这些预处理步骤的优势有两个方面：

1.  **防止信息泄露**：由于预处理器是拟合于训练数据集`X_train`的，因此在填补缺失值或创建独热编码特征时，测试集的信息不会“泄露”。

1.  **避免重复**：如果我们不使用`Pipeline`来处理这些预处理步骤，我们最终会多次转换`X_test`数据集（每次我们想应用一个预处理步骤时）。在这种小规模下，重复可能看起来不是太糟糕。但在复杂的机器学习工作流中，你很容易增加到 5、10，甚至 20 个预处理步骤。使用`Pipeline`可以使这一过程变得简单，因为我们可以添加任意多的步骤，仍然只需对`X_train`和`X_test`进行一次转换：

```py
preprocessor = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='mean')),
    ('scaler', MinMaxScaler()),
    ('step_3', ...),
    ('step_4', ...),
    ...,
    ('step_k', ...)
])

preprocessor.fit(X_train)

X_train_transformed = pipe.transform(X_train)
X_test_transformed = pipe.transform(X_test)
```

# 2\. ColumnTransformer：对不同特征子集应用不同的转换器

在之前的示例中，我们对所有特征应用了相同的预处理步骤。但如果我们有异质的数据类型，并且希望对不同的特征应用不同的预处理器呢？例如，如果我们只想对数值特征进行缩放，或者如果我们想对分类特征进行独热编码？

这时`ColumnTransformer`派上用场了。`ColumnTransformer`允许你将不同的转换器应用于数组或 pandas DataFrame 的不同列。

在下面的代码中，我们首先定义了不同的列组，并且对于每个组，我们使用`Pipeline`来构建一个将作用于该特定组的预处理器。最后，我们将所有转换器链在一个`ColumnTransformer`中。

```py
# This code will only work if you've already run the code in the previous sections

from sklearn.pipeline import Pipeline, make_pipeline
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder, FunctionTransformer, MinMaxScaler
from sklearn.impute import SimpleImputer

# Categorical columns transformer - (a) impute NAs with the mode, and (b) one-hot encode
categorical_features = ['sex']
categorical_transformer = Pipeline(steps=[
    ('impute_mode', SimpleImputer(strategy='mode')),
    ('ohe', OneHotEncoder(handle_unknown='ignore', sparse=False, drop='first')) # handle_unknown='ignore' ensures that any values not encountered in the training dataset are ignored (i.e. all ohe columns will be set to zero)
])

# Numerical columns transformer - (a) impute NAs with the mean, and (b) rescale
numerical_features = ['bp', 'bmi', 's1', 's2', 's3', 's4', 's5', 's6'] # All except 'age' and 'sex'
numerical_transformer = Pipeline(steps=[
    ('impute_mean', SimpleImputer(strategy='mean')),
    ('rescale', MinMaxScaler())
])

# Combine the individual transformers into a single ColumnTransformer
preprocessor = ColumnTransformer(

    # Chain together the individual transformers
    transformers = [
        ('categorical_transformer', categorical_transformer, categorical_features),
        ('numerical_transformer', numerical_transformer, numerical_features),
    ],

    # By default, columns which are not transformed by the ColumnTransformer 
    # will be dropped. By setting remainder='passthrough', we ensure that
    # these columns are retained, in their original form.
    remainder='passthrough',

    # Prefix feature names with the name of the transformer that generated them (optional)
    verbose_feature_names_out=True
)
# Get visual representation of the preprocessing/feature engineering pipeline
preprocessor
```

![](img/73a71294eda47d1fe395f59bfcae38dd.png)

图片作者

要将`ColumnTransformer`应用于数据，我们使用与应用第一个`Pipeline`时相同的代码：

```py
# Fit the preprocessor to the training data
preprocessor.fit(X_train)

# Transform data using the fitted preprocessor
X_train_transformed = preprocessor.transform(X_train)
X_test_transformed = preprocessor.transform(X_test)
```

# 3\. FeatureUnion：并行应用多个转换器

`Pipeline`和`ColumnTransformer`是非常棒的工具，但它们有一个显著的限制。你发现了吗？

它们只能***顺序***应用转换器。

换句话说，当你使用`Pipeline`/`ColumnTransformer`转换特征`Column1`时，scikit-learn 首先会将`transformer_1`应用于`Column1`，然后将`transformer_2`应用于`Column1`的转换版本，以此类推。这对于按顺序预处理数据是可以的（例如“首先填补缺失值，然后进行独热编码”），但在我们希望并行应用不同的预处理步骤时就不理想了（例如“同时从相同的基础列创建两个新特征”）。在这些情况下，使用标准的`Pipeline`或`ColumnTransformer`就不够用了，因为一旦序列中的第一个转换器应用，`Column1`的原始“原始”值就会丢失。

如果我们想对相同的基础特征应用多个变换并行，我们需要使用另一个工具：`FeatureUnion`。

我们可以将`FeatureUnion`视为一个工具，它创建了你底层数据的“副本”，并并行地对这些副本应用转换器，然后将结果拼接在一起。每个转换器都接收原始的底层数据，因此我们不会遇到顺序转换的问题。

要使用`FeatureUnion`，我们只需要添加几行代码：

```py
# This code will only work if you've already run the code in the previous sections

from sklearn.pipeline import FeatureUnion
from sklearn.decomposition import PCA, TruncatedSVD

# Define a feature_union object which will create reduced-dimensionality features
union = FeatureUnion(transformer_list=[
    ("pca", PCA(n_components=1)),
    ("svd", TruncatedSVD(n_components=2))
])

# Adapt the numerical transformer so that it includes the FeatureUnion
numerical_features = ['bp', 'bmi', 's1', 's2', 's3', 's4', 's5', 's6'] # All except 'age' and 'sex'
numerical_transformer = Pipeline(steps=[
    ('impute_mean', SimpleImputer(strategy='mean')),
    ('rescale', MinMaxScaler()),
    ('reduce_dimensionality', union)
])

# Categorical columns transformer - same as above
categorical_features = ['sex']
categorical_transformer = Pipeline(steps=[
    ('impute_mode', SimpleImputer(strategy='mode')),
    ('ohe', OneHotEncoder(handle_unknown='ignore', sparse=False, drop='first')) # handle_unknown='ignore' ensures that any values not encountered in the training dataset are ignored (i.e. all ohe columns will be set to zero)
])

# Build the ColumnTransformer
preprocessor = ColumnTransformer(
    transformers = [
        ('categorical_transformer', categorical_transformer, categorical_features),
        ('numerical_transformer', numerical_transformer, numerical_features),
    ],
    remainder='passthrough',
    verbose_feature_names_out=True
)
preprocessor
```

![](img/e4dace85b9345490fdb7eea945042d0c.png)

作者提供的图片

在这个图示中，我们可以看到`FeatureUnion`步骤是并行应用的，而不是顺序应用的。就像之前一样，我们将`preprocessor`拟合到训练数据上，然后使用它来转换我们想用于建模/预测的任何数据集。

```py
# Fit the preprocessor to the training data
preprocessor.fit(X_train)

# Transform data using the fitted preprocessor
X_train_transformed = preprocessor.transform(X_train)
X_test_transformed = preprocessor.transform(X_test)
```

# 4. `FunctionTransformer`：无缝集成特征工程

上面讨论的所有转换器和工具都使用 scikit-learn 中预构建的类来对数据进行标准转换（例如，缩放、独热编码、插补等）。

如果你想应用自定义函数——例如在特征工程期间——那么你需要使用`FunctionTransformer`。就个人而言，我非常喜欢这个类——它使得将自定义函数集成到`Pipeline`中变得非常简单，而无需从头编写新的转换器类。

创建`FunctionTransformer`非常简单。你可以按照标准的 Python 风格定义你的函数，然后创建一个管道。在这里，我定义了两个简单的函数：一个将两列相加，另一个将两列相减。

```py
from sklearn.preprocessing import FunctionTransformer

def add_features(X):
    X['feature_1_2'] = X['feature_1'] + X['feature_2']
    return X

def subtract_features(X):
    X['feature_3_4'] = X['feature_3'] - X['feature_4']
    return X

# Put into a pipeline
feature_engineering = Pipeline(steps=[
    ('add_features', FunctionTransformer(add_features)),
    ('subtract_features', FunctionTransformer(subtract_features))
])
```

为了进一步简化，你可以在同一个函数中包含多个转换：

```py
def add_subtract_features(X):
    X['feature_1_2'] = X['feature_1'] + X['feature_2'] # Add features
    X['feature_3_4'] = X['feature_3'] - X['feature_4'] # Subtract features
    return X

# Put into a pipeline
feature_engineering = Pipeline(steps=[
    ('add_subtract_features', FunctionTransformer(add_subtract_features)),
])
```

最后，将`feature_engineering`管道添加到我们之前定义的`preprocessing`管道中：

```py
# Combine preprocessing and feature engineering in a single pipeline
pipe = Pipeline([
    ('preprocessing', preprocessor),
    ('feature_engineering', feature_engineering),
])

pipe
```

![](img/509a78b53b47ac12a99a82738b0a2207.png)

作者提供的图片

并使用这个新的管道对所有数据集应用相同的预处理/特征工程步骤：

```py
# Fit the preprocessor to the training data
pipe.fit(X_train)

# Transform data using the fitted preprocessor
X_train_transformed = pipe.transform(X_train)
X_test_transformed = pipe.transform(X_test)
```

# 附加提示：保存你的管道以实现真正可重复的工作流程

在机器学习的企业应用中，单次使用模型或预处理工作流的情况非常少见。更常见的是，你需要定期每周/月重新运行模型，并为新数据生成新的预测。

在这些情况下，与其每次从头编写新的预处理管道，不如每次使用相同的管道。为此，在开发好管道后，使用`joblib`库保存管道，以便将来能够使用相同的转换来处理数据集：

```py
import joblib

# Save pipeline
joblib.dump(pipe, "pipe.pkl")

# Assume that the below steps are applied in another notebook/script

# Load pipeline
pretrained_pipe = joblib.load("pipe.pkl")

# Apply pipeline to a new dataset, X_test_new
X_test_new_transformed = pretrained_pipe.transform(X_test_new)
```

# 结论

总结：

+   `Pipeline`提供了一种快速的方法来依次将不同的预处理转换器应用于你的数据

+   使用`ColumnTransformer`是一种绝佳的方法，可以将不同的预处理步骤依次应用于不同的特征子集

+   `FeatureUnion`使你能够并行应用不同的预处理转换

+   `FunctionTransformer`提供了一种超级简单的方法来编写自定义特征工程函数，并将它们集成到你的管道中

如果你使用这些工具，我向你承诺它们会帮助你写出更优雅、可重复且符合 Python 风格的代码。你的机器学习工程师们会喜欢你的！

如果你喜欢这篇文章，关注我将对我意义重大。如果你想无限制地访问我所有的故事（以及 Medium.com 的其他内容），你可以通过我的[推荐链接](https://medium.com/@mattchapmanmsc/membership)每月支付 $5 注册。与通过普通注册页面注册相比，这不会增加你的额外费用，并且帮助支持我的写作，因为我会获得一小笔佣金。

感谢阅读！
