# 在 Scikit-Learn 中使用自定义 Transformers 进行高级数据准备

> 原文：[`towardsdatascience.com/advanced-data-preparation-using-custom-transformers-in-scikit-learn-5e58d2713ac5`](https://towardsdatascience.com/advanced-data-preparation-using-custom-transformers-in-scikit-learn-5e58d2713ac5)

## 超越“初学者模式”，充分利用 scikit-learn 更强大的功能

[](https://medium.com/@mattchapmanmsc?source=post_page-----5e58d2713ac5--------------------------------)![Matt Chapman](https://medium.com/@mattchapmanmsc?source=post_page-----5e58d2713ac5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5e58d2713ac5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5e58d2713ac5--------------------------------) [Matt Chapman](https://medium.com/@mattchapmanmsc?source=post_page-----5e58d2713ac5--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5e58d2713ac5--------------------------------) ·阅读时间 10 分钟·2023 年 6 月 15 日

--

![](img/2d8d8b06eb619180898fe4e29f587be9.png)

图片由 [Daniel K Cheung](https://unsplash.com/@danielkcheung) 提供，来源于 [Unsplash](https://unsplash.com/photos/cPF2nlWcMY4)

Scikit-Learn 提供了许多有用的数据准备工具，但有时内置的选项并不够。

在本文中，我将向你展示如何使用自定义 Transformers 创建高级数据准备工作流。如果你已经使用 scikit-learn 一段时间并且希望提升技能，学习 Transformers 是超越“初学者模式”并了解现代数据科学团队所需的一些更高级功能的绝佳方式。

如果这个话题听起来有点高级，不用担心——本文充满了示例，这些示例将帮助你对代码和概念充满信心。

我将从 scikit-learn 的`Transformer`类的简要概述开始，然后介绍两种构建自定义 Transformers 的方法：

1.  使用 `FunctionTransformer`

1.  从零开始编写自定义`Transformer`

# Transformers：在 scikit-learn 中预处理数据的最佳方法

`Transformer` 是 scikit-learn 的核心构建块之一。事实上，它如此基础，以至于你很可能已经在使用它却没有意识到。

在 scikit-learn 中，`Transformer` 是任何具有 `fit()` 和 `transform()` 方法的对象。简单来说，这意味着 Transformer 是一个类（即可重用的代码块），它将你的原始数据集作为输入并返回该数据集的转换版本。

![](img/7dc632c664aee56d6b737766b6bc2db0.png)

图片作者

重要的是，scikit-learn 的 `Transformers` 与大语言模型（LLMs）如 BERT 和 GPT-4 中使用的`transformers`并不相同，或通过 HuggingFace `transformers` 库提供的模型也不相同。在 LLMs 的上下文中，“transformer”（小写‘t’）是深度学习模型；而 scikit-learn 的 `Transformer`（大写‘T’）则是完全不同（且更简单）的实体。你可以简单地把它看作是典型 ML 工作流中的数据预处理工具。

当你导入 scikit-learn 时，你可以自动访问一系列为常见 ML 数据预处理任务设计的预构建 Transformers，如填补缺失值、特征缩放和独热编码。一些最受欢迎的 Transformers 包括：

1.  `sklearn.impute.SimpleImputer` - 一个 Transformer，用于替换数据集中缺失的值

1.  `sklearn.preprocessing.MinMaxScaler` - 一个 Transformer，可以重新缩放数据集中数值特征

1.  `sklearn.preprocessing.OneHotEncoder` - 一个 Transformer，用于对分类特征进行独热编码

使用 scikit-learn 的 `sklearn.pipeline.Pipeline`，你甚至可以将多个 Transformers 链接在一起，构建多步骤的数据准备工作流，为后续的 ML 建模做好准备：

![](img/03fbf1b205ea500b325e3e7fbfed50d1.png)

作者提供的图片

如果你对 Pipelines 或 ColumnTransformers 不熟悉，它们是简化 ML 代码的好方法，你可以在我之前的文章中了解更多：

[](/simplify-your-data-preparation-with-these-4-lesser-known-scikit-learn-classes-70270c94569f?source=post_page-----5e58d2713ac5--------------------------------) ## 使用这 4 个鲜为人知的 Scikit-Learn 类简化你的数据准备

### 忘记 train_test_split：Pipeline、ColumnTransformer、FeatureUnion 和 FunctionTransformer 是不可或缺的，即使……

towardsdatascience.com

# 预构建的 scikit-learn Transformers 有什么问题？

完全没有！

如果你正在处理简单的数据集并执行标准的数据准备步骤，那么 scikit-learn 的预构建 Transformers 可能会完全足够。无需从头编写自定义 Transformers 来重新发明轮子。

但—说实话—现实生活中的数据集什么时候真的简单过？

（剧透：绝对不会。）

如果你正在处理真实世界的数据或需要实现一些有趣的数据预处理方法，那么 scikit-learn 的内置 Transformers 可能并不总是足够的。迟早，你会需要实现自定义数据转换。

幸运的是，scikit-learn 提供了几种扩展其基本 Transformer 功能并构建更自定义 Transformers 的方法。为了展示这些方法的工作原理，我将使用经典的泰坦尼克号生存预测数据集。即使在这个所谓的“简单”数据集中，你会发现数据准备中还有很多创新的机会。正如我将展示的那样，自定义 Transformers 是实现这一任务的理想工具。

# 数据

首先，让我们加载数据集并将其拆分为训练集和测试集：

```py
import pandas as pd
from sklearn.datasets import fetch_openml
from sklearn.model_selection import train_test_split

# Load data and split into training and testing sets
X, y = fetch_openml("titanic", version=1, as_frame=True, return_X_y=True)
X.drop(['boat', 'body', 'home.dest'], axis=1, inplace=True)
X_train, X_test, y_train, y_test = train_test_split(X, y, stratify=y, test_size=0.2)
X_train.head()
```

![](img/5216168cd8901e8c31de6dc524c6e539.png)

作者提供的图像。 [泰坦尼克号数据集](https://www.openml.org/search?type=data&sort=runs&id=40945&status=active) 使用 CC0 公开领域许可。

因为这篇文章专注于如何构建**自定义**的 Transformers，我不会详细介绍标准的预处理步骤，这些步骤可以通过 scikit-learn 的内置 Transformers（例如，使用 `OneHotEncoder` 对分类变量如 `sex` 进行独热编码，或使用 `SimpleImputer` 替换缺失值）轻松应用于该数据集。

相反，我将重点介绍如何整合更复杂的预处理步骤，这些步骤无法使用“现成的” Transformers 实现。

其中一个步骤是从 `name` 字段中提取每个乘客的头衔（例如 Mr, Mrs, Master）。为什么我们可能想这样做呢？如果我们知道每个乘客的头衔包含了他们的等级/年龄/性别的指示，并且我们假设这些因素影响了乘客登上救生艇的能力，那么合理地假设头衔可能对生存几率有所启示。例如，一个拥有“Master”头衔（表明他们是儿童）的乘客可能比一个拥有“Mr”头衔（表明他们是成人）的乘客更有可能生存。

当然，问题在于，没有内置的 scikit-learn 类可以执行像从 `name` 字段提取头衔这样具体的操作。为了提取头衔，我们需要构建一个自定义 Transformer。

# 1\. FunctionTransformer

构建自定义 Transformer 的最快方法是使用 `FunctionTransformer` 类，它允许你直接从普通的 Python 函数创建 Transformers。

使用 `FunctionTransformer` 时，你首先需要定义一个函数，该函数接受一个输入数据集 `X`，执行所需的转换，并返回 `X` 的转换版本。然后，将你的函数封装在 `FunctionTransformer` 中，scikit-learn 将创建一个实现你函数的自定义 Transformer。

例如，以下是一个可以从我们的泰坦尼克号数据集的 `name` 字段中提取乘客头衔的函数：

```py
from sklearn.preprocessing import FunctionTransformer

def extract_title(X):
    """Extract the title from each passenger's `name`."""
    X['title'] = X['name'].str.split(', ', expand=True)[1].str.split('.', expand=True)[0]
    return X

extract_title_transformer = FunctionTransformer(extract_title)
print(type(extract_title_transformer))
# <class 'sklearn.preprocessing._function_transformer.FunctionTransformer'>
```

如你所见，将函数封装在 `FunctionTransformer` 中将其转换为 scikit-learn Transformer，使其具备 `.fit()` 和 `.transform()` 方法。

然后，我们可以将这个 Transformer 与我们想要包含的任何额外预处理步骤/transformers 一起纳入数据准备管道中：

```py
from sklearn.pipeline import Pipeline

preprocessor = Pipeline(steps=[
  ('extract_title', extract_title_transformer),
  # ... any other transformers we want to include, e.g. SimpleImputer or MinMaxScaler
])

X_train_transformed = preprocessor.fit_transform(X_train)
X_train_transformed
```

![](img/a966e4f57584d07d7af04bfd212711ae.png)

图片由作者提供

如果你想定义一个更复杂的函数/转换器，该函数需要额外的参数，你可以通过将这些参数纳入`FunctionTransformer`的`kw_args`参数来传递这些参数。例如，我们可以定义另一个函数，根据乘客的称谓来识别他们是否来自上层阶级/专业背景：

```py
def extract_title(X):
    """Extract the title from each passenger's `name`."""
    X['title'] = X['name'].str.split(', ', expand=True)[1].str.split('.', expand=True)[0]

def is_upper_class(X, upper_class_titles):
    """If the passenger's title is in the list of `upper_class_titles`, return 1, else 0."""
    X['upper_class'] = X['title'].apply(lambda x: 1 if x in upper_class_titles else 0)
    return X

preprocessor = Pipeline(steps=[
  ('extract_title', FunctionTransformer(extract_title)),
  ('is_upper_class', FunctionTransformer(is_upper_class,
                      kw_args={'upper_class_titles':['Dr', 'Col', 'Major', 'Lady', 'Rev', 'Sir', 'Capt']})),
    # ... any other transformers we want to include, e.g. SimpleImputer or MinMaxScaler
])

X_train_transformed = preprocessor.fit_transform(X_train)
X_train_transformed
```

![](img/7dd3fd7c128c72d53e24e9a5b3068bde.png)

图片由作者提供

正如你所看到的，使用`FunctionTransformer`是一种非常简单的方法，可以将这些复杂的预处理步骤纳入 Pipeline 中，而不会从根本上改变我们代码的结构。

# 1.1 `FunctionTransformer`的局限性：有状态转换

`FunctionTransformer`是一个强大而优雅的解决方案，但它仅适用于你想应用***无状态***转换（即基于规则的转换，不依赖于从训练数据中计算的先前值）的情况。如果你想定义一个自定义转换器，可以根据训练数据集中观察到的值来转换测试数据集，那么你不能使用`FunctionTransformer`，你需要采取不同的方法。

如果这听起来有点混乱，花一分钟重新考虑一下我们刚刚编写的提取乘客称谓的函数：

```py
def extract_title(X):
    """Extract the title from each passenger's `name`."""
    X['title'] = X['name'].str.split(', ', expand=True)[1].str.split('.', expand=True)[0]
```

这个函数是***无状态***的，因为它没有对过去的记忆；在操作过程中不使用任何预计算的值。每次调用这个函数时，它都会从头开始应用，就像是第一次执行一样。

相反，**有状态**函数保留先前操作的信息，并在实施当前操作时使用这些信息。为了说明这一点，这里有两个函数，它们用均值替换数据集中缺失的值：

```py
# Stateless - no prior information is used in the transformation
def impute_mean_stateless(X):
    X['column1'] = X['column1'].fillna(X['column1'].mean())

# Stateful - prior information about the training set is used in the transformation
column1_mean_train = np.mean(X_train['column1'])
def impute_mean(X):
    X['column1'] = X['column1'].fillna(column1_mean_train)
    return X
```

第一个函数是一个*无状态*函数，因为在转换过程中没有使用先前的信息；均值仅仅是利用传递给函数的数据集`X`计算的。

第二个是一个*有状态*函数，它使用`column1_mean_train`（即`column1`在训练集`X_train`中的均值）来替换`X`中的缺失值。

无状态和有状态转换之间的区别可能看起来有些晦涩，但在机器学习任务中这是一个非常重要的概念，尤其是在我们有独立的训练和测试数据集时。每当我们想要替换缺失值、缩放特征或对测试数据集进行独热编码时，我们希望这些转换基于训练数据集中观察到的值。换句话说，我们希望我们的转换器能***适配***训练集。以用均值填补缺失值为例，我们希望“均值”是训练集的均值。

使用 `FunctionTransformer` 的问题在于它不能用于实现有状态的转换。即使一个用 `FunctionTransformer` 创建的 `Transformer` *在技术上* 拥有 `.fit()` 方法，[调用它不会有任何效果](https://stackoverflow.com/a/62225211)，因此我们不能真正将这个 Transformer “拟合”到训练数据上。为什么？因为在 `FunctionTransformer` 创建的 Transformer 中，转换始终依赖于函数的输入值 `X`。我们的 `Transformer` 将始终使用传递给它的数据集重新计算值；它无法用预计算的值进行插补/转换。

# 1.2 一个示例来说明 FunctionTransformer 的局限性

为了说明这一点，这里有一个示例，其中我尝试将一个基于 FunctionTransformer 的 Transformer “拟合”到训练集上，然后使用这个假定已“拟合”的 Transformer 对测试集进行转换。如你所见，测试集中的缺失值没有被用训练集中的均值替代；它们是根据测试集重新计算的。换句话说，Transformer 无法应用有状态的转换。

```py
# Show the test set, pre-transformation
X_test.head(3)
```

![](img/648add161d7746faa7471bc53ccb2e89.png)

图片由作者提供，显示了测试集中“年龄”列的第三行中的缺失值

```py
print("X_train mean: ", X_train['age'].mean())
# X_train mean:  29.857414148681055

print("X_test mean: ", X_test['age'].mean())
# X_test mean:  29.97444952830189
```

```py
def impute_mean(X):
    X['age'] = X['age'].fillna(X['age'].mean())
    return X

impute_mean_FT = FunctionTransformer(impute_mean) # Convert function to Transformer
prepro = impute_mean_FT.fit(X_train) # The Transformer is "fitted" to the train set
prepro.transform(X_test) # The fitted Transformer is used to transform the test set
```

![](img/7973d4ccbc7bcff0978c0416e85a741a.png)

图片由作者提供。第三行中的缺失值被替换为测试集的均值，而不是训练集的均值，说明了 FunctionTransformer 无法生成能够进行有状态转换的 Transformer。

如果这一切听起来有点混乱，不用担心。关键的信息是：如果你想定义一个能够基于训练数据集观察到的值对测试数据集进行预处理的自定义 Transformer，你不能使用 `FunctionTransformer`，你需要采取不同的方法。

# 2\. 从头开始创建自定义 Transformer

一种替代方法是定义一个新的 Transformer 类，该类继承自 `sklearn.base` 模块中的类：`TransformerMixin`。这个新类将作为一个 Transformer 使用，适用于无状态的 *以及* 有状态的转换。

以下是我们如何将 `extract_title` 代码片段转化为一个 Transformer 的方法：

```py
from sklearn.base import TransformerMixin

class ExtractTitle(TransformerMixin):
    def fit(self, X, y=None):
        return self
    def transform(self, X, y=None):
        X['title'] = X['name'].str.split(', ', expand=True)[1].str.split('.', expand=True)[0]
        return X

preprocessor = Pipeline(steps=[
    ('extract_title', ExtractTitle()),
])

X_train_transformed = preprocessor.fit_transform(X_train)
X_train_transformed.head()
```

![](img/a966e4f57584d07d7af04bfd212711ae.png)

图片由作者提供

如你所见，我们达到了与构建使用 FunctionTransformer 的 Transformer 时完全相同的转换效果。

## 2.1 向自定义 Transformer 传递参数

如果你需要将数据传递给自定义 Transformer，只需在定义 `fit()` 和 `transform()` 方法之前定义一个 `__init__()` 方法：

```py
class IsUpperClass(TransformerMixin):
    def __init__(self, upper_class_titles):
        self.upper_class_titles = upper_class_titles        

    def fit(self, X, y=None):
        return self

    def transform(self, X, y=None):
        X['upper_class'] = X['title'].apply(lambda x: 1 if x in self.upper_class_titles else 0)
        return X

preprocessor = Pipeline(steps=[
    ('IsUpperClass', IsUpperClass(upper_class_titles=['Dr', 'Col', 'Major', 'Lady', 'Rev', 'Sir', 'Capt'])),
])

X_train_transformed = preprocessor.fit_transform(X_train)
X_train_transformed.head()
```

![](img/7dd3fd7c128c72d53e24e9a5b3068bde.png)

图片由作者提供

就这样：在 scikit-learn 中构建自定义 Transformers 的两种方法。能够使用这些方法是非常有价值的，这也是我作为数据科学家在日常工作中经常用到的。我希望你觉得这很有用。

如果你喜欢这篇文章并希望获得更多数据科学方面的技巧和见解，可以考虑在 Medium 上关注我或[LinkedIn](https://www.linkedin.com/in/matt-chapman-ba8488118/)。如果你希望无限访问我所有的故事（以及 Medium.com 的其他内容），你可以通过我的[推荐链接](https://medium.com/@mattchapmanmsc/membership)以每月 $5 的价格注册。与通过普通注册页面相比，这不会额外增加你的费用，并且支持我的写作，因为我将获得一小部分佣金。

感谢阅读！
