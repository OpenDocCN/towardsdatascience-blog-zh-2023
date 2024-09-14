# SciKit 管道简要介绍

> 原文：[`towardsdatascience.com/a-brief-introduction-to-scikit-pipelines-888edc86da9b`](https://towardsdatascience.com/a-brief-introduction-to-scikit-pipelines-888edc86da9b)

## 以及你为什么应该开始使用它们。

[](https://medium.com/@jodancker?source=post_page-----888edc86da9b--------------------------------)![Jonte Dancker](https://medium.com/@jodancker?source=post_page-----888edc86da9b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----888edc86da9b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----888edc86da9b--------------------------------) [Jonte Dancker](https://medium.com/@jodancker?source=post_page-----888edc86da9b--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----888edc86da9b--------------------------------) ·阅读时间 7 分钟·2023 年 8 月 23 日

--

![](img/f2987996b1c9e2cc8de667615c02c9c0.png)

图片由[Sigmund](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

你是否曾经训练过机器学习模型，结果看起来太过理想？但后来你发现你的训练数据和测试数据之间存在一些数据泄露？

或者你是否有许多预处理步骤来准备数据，以至于难以将预处理步骤从模型训练迁移到生产中以进行实际预测？

或者你的预处理变得混乱，难以以可读和易于理解的方式分享你的代码？

然后你可能想尝试`scikit-learn`的管道。管道是设置你的机器学习训练、测试和生产工作流的优雅解决方案，使你的工作更加轻松，结果更具可重复性。

那么什么是管道？它有哪些好处？如何设置管道？我将探讨这些问题，并给出一些构建模块的代码示例。通过结合这些构建模块，你可以构建更复杂的管道，这些管道是针对你需求量身定制的。

# 什么是管道？

管道允许你在机器学习工作流中组装多个步骤，这些步骤按顺序转换数据，然后将数据传递给估算器。因此，管道可以包括预处理、特征工程和特征选择步骤，然后将数据传递给最终的估算器进行分类或回归任务。

# 我为什么要使用管道？

总的来说，使用管道会使你的工作更轻松，并加快机器学习模型的开发。这是因为管道

+   导致更干净、更易于理解的代码

+   易于复制和理解数据工作流

+   更易于阅读和调整

+   使数据准备更快，因为管道自动化了数据准备

+   有助于避免数据泄露

+   允许对管道中的所有估计器和参数进行一次超参数优化

+   方便的是，你只需调用一次`fit()`和`predict()`就能运行整个数据管道

在训练和优化模型并对结果满意后，你可以轻松保存训练好的管道。然后，每当你想运行模型时，只需加载预训练的管道，你就可以开始推理。这样，你可以以非常干净的方式分享你的模型，易于复制和理解。

# 如何设置管道？

设置`scikit-learn`的管道非常简单直接。

`scikit-learn`的`Pipeline`使用包含你要应用于数据的变换器的键值对列表。你可以任意选择键。键可以用来访问变换器的参数，例如，在超参数优化过程中运行网格搜索时。由于变换器存储在列表中，你也可以通过索引访问变换器。

要在管道中拟合数据并进行预测，你可以像使用任何`scikit-learn`中的变换器或回归器一样运行`fit()`和`predict()`。

一个非常简单的管道可能如下所示：

```py
from sklearn.impute import SimpleImputer
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import MinMaxScaler
from sklearn.linear_model import LinearRegression

pipeline = Pipeline(
   steps=[("imputer", SimpleImputer()), 
          ("scaler", MinMaxScaler()), 
          ("regression", LinearRegression())
   ]
)

pipeline.fit(X_train, y_train)
y_pred = pipeline.predict(X_test)
```

不过，`scikit-learn`还让你的生活更轻松，如果你不想为变换器输入键值，可以使用`make_pipeline()`函数，`scikit-learn`会根据变换器的类名设置名称。

```py
from sklearn.impute import SimpleImputer
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import MinMaxScaler
from sklearn.linear_model import LinearRegression

pipeline = make_pipeline(steps=[
    SimpleImputer(), 
    MinMaxScaler(), 
    LinearRegression()
    ]
)
```

就这样。这样你就快速设置了一个简单的管道，可以开始用它来训练模型和运行预测。如果你想查看管道的样子，可以直接打印管道，`scikit-learn`会向你展示管道的交互视图。

但如果你想构建更复杂和可自定义的东西怎么办？例如，不同地处理分类值和数值，添加特征或转换目标值。

不用担心，`scikit-learn`提供了额外的功能，可以创建更自定义的管道，并将管道提升到更高的水平。这些功能包括：

+   `ColumnTransformer`

+   `FeatureUnion`

+   `TransformedTargetRegressor`

我将逐一讲解它们，并展示如何使用的示例。

# 转换选定的特征

如果你有不同类型的特征，例如，连续和分类特征，你可能会希望对这些特征进行不同的转换。例如，对连续特征进行缩放，而对分类特征进行独热编码。

你可以在将特征传递到管道之前进行这些预处理步骤。但这样做的话，你无法将这些预处理步骤和参数包含在以后的超参数搜索中。而且，将它们包含在管道中可以使处理你的机器学习模型更为简便。

要对选择的列应用变换或一系列变换，可以使用`ColumnTransformer`。其使用方式与`Pipeline`非常相似，只不过我们将相同的键值对传递给`transformers`而不是`steps`。然后，我们可以将创建的变换器作为管道中的一个步骤包含进来。

```py
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import OneHotEncoder

categorical_transformer = ColumnTransformer(
    transformers=[("encode", OneHotEncoder())]
)

pipeline = Pipeline(steps=[
    ("categorical", categorical_transformer, ["col_name"])
    ]
)
```

由于我们只想对某些列进行变换，我们需要在管道中传递这些列。此外，我们还可以让`ColumnTransformer`知道我们希望对其余列做什么。例如，如果你想保留那些未被变换器更改的列，你需要将`remainder`设置为`passthrough`。否则，这些列将被丢弃。除了什么都不做或丢弃列，你还可以通过传递一个变换器来变换剩余的列。

```py
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import MinMaxScaler, OneHotEncoder

categorical_transformer = ColumnTransformer(
 transformers=[("encode", OneHotEncoder(), ["col_name"])], remainder="passthrough"
)

categorical_transformer = ColumnTransformer(
 transformers=[("encode", OneHotEncoder(), ["col_name"])], remainder=MinMaxScaler()
)
```

```py

Since `scikit-learn` allows Pipeline stacking we could even pass a Pipeline to the `ColumnTransformer` instead of stating each transformation we want to do in the `ColumnTransformer` itself.

```

from sklearn.compose import ColumnTransformer

from sklearn.impute import SimpleImputer

from sklearn.pipeline import Pipeline

from sklearn.preprocessing import MinMaxScaler, OneHotEncoder

categorical_transformer = Pipeline(steps=[("encode", OneHotEncoder())])

numerical_transformer = Pipeline(

steps=[("imputation", SimpleImputer()), ("scaling", MinMaxScaler())]

)

preprocessor = ColumnTransformer(

transfomers=[

    ("numeric", numerical_transformer),

    ("categoric", categorical_transformer, ["col_name"]),

]

)

pipeline = Pipeline(steps=["preprocesssing", preprocessor])

```py

# Combining features

Now, you are able to run different pre-processing steps on different columns, but what if you want to derive new features from the data and add them to you feature set?

For this, you can use `FeatureUnion`, which combines transformer objects into a new transformer with the combined objects. Running a pipeline with a `FeatureUnion` fits each transformer independently and then joins their output.

For example, assume we want to add the Moving Average as a feature, we could do this:

```

from sklearn.compose import FeatureUnion

from sklearn.pipeline import Pipeline

preprocessor = (

FeatureUnion(

    [

    ("moving_Average", MovingAverage(window=30)),

    ("numerical", numerical_pipeline),

    ]

),

)

pipeline = Pipeline(steps=["preprocesssing", preprocessor])

```py

# Transforming the target value

If you have a regression problem sometimes it can help to transform the target before fitting a regression.

You can include such a transformation using the `TransformedTargetRegressor` class. With this class you can either use transformers provided by `scikit-learn` like a MinMax scaler or write your own transformation functions.

One huge advantage of the `TransformedTargetRegressor` is that it automatically maps the predictions back to the original space by an inverse transform. So, you do not need to care about this later on when you move from model training to making predictions in production.

```

from sklearn.compose import TransformedTargetRegressor

from sklearn.impute import SimpleImputer

from sklearn.pipeline import Pipeline

from sklearn.preprocessing import MinMaxScaler

regressor = TransformedTargetRegressor(

    regressor=model,

    func=np.log1p,

    inverse_func=np.expm1

)

pipeline = Pipeline(

steps=[

    ("imputer", SimpleImputer()),

    ("scaler", MinMaxScaler()),

    ("regressor", regressor)

    ]

)

pipeline.fit(X_train, y_train)

y_pred = pipeline.predict(X_test)

```py

# Building your own custom functions

Sometimes it is not enough to use pre-processing methods `scikit-learn` provides. This, however, should not hold you back when using Pipelines. You can easily create your own functions that you can then include in the pipeline.

For this, you need to build a class that contains a `fit()` and `transform()` method as these are called when running the pipeline. However, these methods do no necessarily need to do anything. Moreover, we can let the class inherit from `scikit-learn`’s `BaseEstimator` and `TransformerMixin` class to give us some basic functionality that our pipeline needs.

For example, assume we want to make predictions on a time series and we want to smooth all features by a moving average. For this, we just set up a class with a `transform` method that contains the smoothing part.

```

from sklearn.base import BaseEstimator, TransformerMixin

from sklearn.impute import SimpleImputer

from sklearn.pipeline import Pipeline

from sklearn.preprocessing import MinMaxScaler

class MovingAverage(BaseEstimator, TransformerMixin):

    def __init__(self, window=30):

        self.window = window

    def fit(self, X, y=None):

        return self

    def transform(self, X, y=None):

        return X.rolling(window=self.window, min_periods=1, center=False).mean()

pipeline = Pipeline(

steps=[

    ("ma", MovingAverage(window=30)),

    ("imputer", SimpleImputer()),

    ("scaler", MinMaxScaler()),

    ("regressor", model),

]

)

pipeline.fit(X_train, y_train)

y_pred = pipeline.predict(X_test)

```py

# What else is there to know?

The default return of transformers in `scikit-learn` is a numpy array. This can lead to problems in your pipeline if you only want to apply a transformation on certain features in the second step of the pipeline, e.g., only categorical features.

However, to prevent your pipeline from breaking you can change the default return value of all transformers to a dataframe by stating

```

from sklearn import set_config

set_config(transform_output = "pandas")

```

在进行超参数优化或检查 Pipeline 的单个参数时，直接访问参数可能会很有帮助。要访问参数，你可以使用`<estimator>__<parameter>`语法。例如，在上述示例中，我们可以通过调用`pipeline.set_params(pipeline__ma_window=7)`来访问 MovingAverage 转换器的窗口宽度。

# 结论

使用`scikit-learn`的 Pipeline 可以大大简化你在开发新机器学习模型和设置预处理步骤时的工作。除了拥有许多好处之外，设置 Pipeline 也很简单直接。不过，你可以构建复杂且可定制的预处理 Pipeline，其中只有你的创造力限制了边界。

如果你喜欢这篇文章或有任何问题，随时留言或联系我。我也对你使用`scikit-learn`的 Pipeline 的经验感兴趣。

想要了解更多关于 Pipeline 的信息，请查看以下链接：

+   [`scikit-learn.org/stable/modules/compose.html#pipeline`](https://scikit-learn.org/stable/modules/compose.html#pipeline)
