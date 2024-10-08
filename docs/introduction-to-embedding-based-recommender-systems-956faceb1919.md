# 嵌入式推荐系统介绍

> 原文：[`towardsdatascience.com/introduction-to-embedding-based-recommender-systems-956faceb1919`](https://towardsdatascience.com/introduction-to-embedding-based-recommender-systems-956faceb1919)

## [推荐系统](https://medium.com/tag/recommendation-system)

## 学习在 TensorFlow 中构建一个简单的矩阵分解推荐系统

[](https://dr-robert-kuebler.medium.com/?source=post_page-----956faceb1919--------------------------------)![Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----956faceb1919--------------------------------)[](https://towardsdatascience.com/?source=post_page-----956faceb1919--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----956faceb1919--------------------------------) [Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----956faceb1919--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----956faceb1919--------------------------------) ·阅读时间 13 分钟·2023 年 1 月 25 日

--

![](img/ff12d44a9188531303b168fa10bb28cf.png)

图片由 [Johannes Plenio](https://unsplash.com/es/@jplenio?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这些推荐在像亚马逊、Netflix 或 Spotify 这样的大型网站上随处可见，有时非常出色，有时却很糟糕，甚至有时很搞笑，告诉你下一步买什么、看什么或听什么。虽然**推荐系统**对我们用户来说很方便——我们可以受到启发去尝试新事物——但**公司特别受益**于它们。

要了解具体程度，我们可以查看 Dietmar Jannach 和 Michael Jugovac 在论文中给出的一些数据，[**测量推荐系统的商业价值**](https://arxiv.org/abs/1908.08328) [1]。从他们的论文中：

+   **Netflix：** “75% 的观看内容来源于某种推荐”（[这个甚至来自 Medium！](https://netflixtechblog.com/netflix-recommendations-beyond-the-5-stars-part-1-55838468f429)）

+   **YouTube：** “60% 的主页点击来自推荐”

+   **亚马逊：** “约 35%的销售来源于交叉销售（即推荐）”，其中*their*指的是亚马逊

在这篇论文 [1] 中，你可以找到更多关于提高点击率、参与度和销售的有趣声明，这些都来源于使用推荐系统。

所以，看起来推荐系统就像是面包切片以来最伟大的发明一样，我也同意推荐系统是机器学习领域中出现的最优秀、最有趣的事物之一。这就是为什么在本文中，我想向你展示

+   如何设计一个简单的*协作*推荐系统（矩阵分解）

+   如何在 TensorFlow 中实现它

+   优缺点是什么。

> 你可以在 [我的 Github](https://github.com/Garve/Towards-Data-Science---Notebooks/blob/main/TDS%20-%20Introduction%20to%20Embedding-Based%20Recommender%20Systems.ipynb) 上找到代码。

在我们开始之前，让我们获取一些可以操作的数据。

# 获取数据

如果你还没有，请通过 `pip install tensorflow-datasets` 获取 **tensorflow_datasets**。你可以下载他们提供的任何数据集，但我们将坚持使用真正经典的 movielens！我们选择 movielens 数据中最小的版本，包含 1,000,000 行，这样后续训练会更快。

```py
import tensorflow_datasets as tfds

data = tfds.load("movielens/1m-ratings")
```

`data` 是一个包含 TensorFlow 数据集的字典，这些数据集非常棒。但为了简化，我们将其转换为 pandas 数据框，这样大家就能保持一致。

> ***注意：*** *通常，你会将其保留为 TensorFlow 数据集，尤其是当数据量变得更大时，因为 pandas 对 RAM 的需求非常高。不要尝试将 movielens 数据集的 25,000,000 版本转换为 pandas 数据框！*

```py
df = tfds.as_dataframe(data["train"])
print(df.head(5))
```

![](img/42723a437fbc07c68cb766cdd8982721.png)

图片来自作者。

> ***⚠️ 警告：*** *不要打印整个数据框，因为这是一个样式化的数据框，默认情况下会显示所有 1,000,000 行！*

我们可以看到大量的数据。每一行包括

+   用户 (**user_id**)，

+   一个电影 (**movie_id**)，

+   用户对电影的评分 (**user_rating**)，以 1 到 5 的整数（星级）表示，并且

+   关于用户和电影的更多特征。

> *在本教程中，让我们只使用最基本的：***user_id***、***movie_id*** 和 ***user_rating***，因为这通常是我们唯一拥有的数据。拥有更多关于用户和电影的特征通常是一种奢侈，所以让我们直接处理更难但广泛适用的情况。*
> 
> 基于这种交互数据训练的推荐系统称为* ***协同过滤 —*** *一个模型在许多用户的交互数据上进行训练，以便为单个用户提供推荐。* 一人为大家，大家为一人！

我们还会保留**时间戳**以进行时间上的训练-测试拆分，因为这类似于我们在现实生活中的训练方式：我们现在训练，但希望模型明天表现良好。所以我们也应该以这种方式评估模型质量。

```py
filtered_data = (
    df
    .filter(["timestamp", "user_id", "movie_id", "user_rating"])
    .sort_values("timestamp")
    .astype({"user_id": int, "movie_id": int, "user_rating": int}) # nicer types
    .drop(columns=["timestamp"]) # don't need the timestamp anymore
)

train = filtered_data.iloc[:900000] # chronologically first 90% of the dataset
test = filtered_data.iloc[900000:]  # chronologically last 10% of the dataset
```

`filtered_data` 包含

![](img/1df5a8858010c3f9d946b01758b00d02.png)

图片来自作者。

## 冷启动问题

如果我们以任何方式拆分数据，我们可能会遇到所谓的**冷启动问题**，这意味着某些用户或电影只出现在测试集中，而不在训练集中。在我们的例子中，有趣的是，用户 1 就是这样一个例子。

```py
print(train.query("user_id == 1").shape[0])
print(test.query("user_id == 1").shape[0])

# Output:
# 0
# 53
```

有点像仅出现在测试集中的分类特征类别。这使得学习更困难，但模型仍然必须以某种方式处理它。我们将要构建的推荐系统很容易受到冷启动问题的影响，但还有其他类型的推荐系统可以更好地处理新用户或电影。这是另一个文章的内容。

让我们构建训练和测试数据框并继续。

```py
X_train = train.drop(columns=["user_rating"])
y_train = train["user_rating"]

X_test = test.drop(columns=["user_rating"])
y_test = test["user_rating"]
```

# 嵌入速成课程

现在我们知道数据的样子了，让我们定义模型的**签名**，即输入和输出是什么。在我们的例子中，这非常简单：输入应该是**user_id**和**movie_id**，输出应该是**user_rating**，即用户对电影的评分。

![](img/9763a829d99b952731e4bd03c81fba56.png)

图片由作者提供。

这样的模型可能是什么样的呢？这很难，尤其是对于数据科学初学者。**用户和电影是类别**，即使我们将它们编码为整数。因此，将它们视为数字并仅用它们训练模型是不够有意义的。

## 糟糕的事情！

对于好奇的读者，我还是会做的。以下是一个**不要**这样做的示例：

```py
# BAD BAD BAD BAD BAD BAD BAD BAD BAD BAD BAD BAD BAD BAD BAD

from sklearn.ensemble import HistGradientBoostingRegressor
from sklearn.metrics import mean_absolute_error

hgb = HistGradientBoostingRegressor(random_state=0)
hgb.fit(X_train, y_train)
print(hgb.score(X_test, y_test), mean_absolute_error(y_test, hgb.predict(X_test)))

# Output:
# 0.07018701410615702 0.8508620798953698

# BAD BAD BAD BAD BAD BAD BAD BAD BAD BAD BAD BAD BAD BAD BAD
```

*r*²约为 0.07，这和一个仅输出评分均值的回归器差不多，不管用户和电影的输入如何。平均绝对误差约为 0.85，这意味着我们平均错过了真实评分约 0.85 星。

我将展示如何使用嵌入构建一个更有意义和更好的模型，而不是这样做。

## 一热编码作为嵌入的特例

编码诸如用户或电影这样的类别变量的一种方式是使用向量，即一组数字——在这个上下文中称为**嵌入**。这是一种有用的技术，不仅仅是推荐系统，**任何涉及类别数据的情况**都可以使用。

![](img/d44666d456372bc6d8e7a40848516e0e.png)

图片由作者提供。

将类别转换为数字的一个非常简单的例子是**一热/虚拟编码**。然而，对于高卡特性类别特征，结果嵌入是**高维**的，这使得我们在尝试使用它们时陷入了[维度诅咒](https://en.wikipedia.org/wiki/Curse_of_dimensionality)的陷阱。

另一个缺点是每对两个向量之间的距离是相同的。例如，如果你取一个具有三个类别的特征，编码为 [1, 0, 0]，[0, 1, 0] 和 [0, 0, 1]，每个类别与其他类别在常见的度量标准（如欧几里得和其他 [闵可夫斯基距离](https://en.wikipedia.org/wiki/Minkowski_distance)）中具有相同的距离。这对于名义特征可能没问题，但对于**序数**特征如*热*、*温暖*或*冷*天气，如果*热*离*温暖*比*冷*更近会更好。

显然，这是不好的，因此我们必须考虑其他方案。

## 实际情况

> **嵌入允许我们创建比一热编码向量更有意义的更短的向量。**

在深度学习框架如 TensorFlow 和 PyTorch 中，它们随时可用。总体上，它们的工作方式如下：

1.  你指定一个**嵌入维度**，即向量的长度。这是一个你可以调整的超参数之一。

1.  每个类别的嵌入被随机初始化，就像神经网络中的其他权重一样。

1.  训练使嵌入对模型更有用。

实际上，这并不是一个概念上新的操作，因为你可以通过**首先进行独热编码**类别，**然后使用没有激活函数和偏置的线性（密集）层**来**模拟**它。嵌入层更高效，因为它只是进行查找，而不是像线性层中那样计算矩阵乘积。

![](img/cc22cb4729f899280b6534830b3460a5.png)

图片由作者提供。

# 构建模型

现在我们已经具备了所有的要素，让我们来构建一个模型吧！首先，我们将定义模型的高层次架构，然后在 TensorFlow 中构建它，虽然如果你更喜欢 PyTorch，它也同样容易。

## 架构

好了，那么两个分类变量（**user_id** 和 **movie_id**）进入模型，然后我们进行嵌入。我们最终得到两个向量，最好是相同长度的。最后，我们希望得到一个单一的数字，即**user_rating**。

> ***注意：*** *我们将其建模为回归问题，但你也可以将其视为分类任务。*

那么，我们如何将两个相同长度的向量合并为一个单一的数字呢？有很多方法，但其中一种最简单且高效的方法是直接计算**点积**。

![](img/a7dbfbaf27676c2a35180020c0a20615.png)

图片由作者提供。

> ***注意：*** *我们将在接下来的内容中采用的方法也称为* ***矩阵分解*** *，因为我们会在各处计算点积，就像你乘以两个矩阵一样。*

我会说这并没有什么太疯狂的地方。现在我们可以看看模型应该如何工作：

![](img/525d2726b9eb8d72477491bf01ae5c53.png)

图片由作者提供。

作为一个公式，我们创建了这个：

![](img/e107de2352c6aa5fc868c96529d954bf.png)

图片由作者提供。

这表示**“用户 *u* 对电影 *m* 的评分等于用户 *u* 的嵌入与电影 *m* 的嵌入的点积”**。

## TensorFlow 实现，第一个版本

如果你了解基本的 TensorFlow，实现实际上是轻而易举的。唯一需要注意的是，嵌入层希望类别被表示为从 1 到 *number_of_categories* 的整数。你经常会看到有人填充一些字典，如 {“user_8323”: 1, “user_1122”: 2, …} 以及一个反向字典，如 {1: “user_8323”, 2: “user_1122”, …} 来实现这一点，但 TensorFlow 也有一些很好的层来处理这些。我们将在这里使用 `[IntegerLookup](https://www.tensorflow.org/api_docs/python/tf/keras/layers/IntegerLookup)`。这个层的一个好特性是：未知类别默认映射为 0。

在开始之前，我们必须首先从**训练集**中获取所有唯一的用户和电影。

```py
all_users = train["user_id"].unique()
all_movies = train["movie_id"].unique()
```

使用 Keras 的功能 API，你可以这样实现上述想法：

```py
import tensorflow as tf

# user pipeline
user_input = tf.keras.layers.Input(shape=(1,), name="user")
user_as_integer = tf.keras.layers.IntegerLookup(vocabulary=all_users)(user_input)
user_embedding = tf.keras.layers.Embedding(input_dim=len(all_users)+1, output_dim=32)(user_as_integer)

# movie pipeline
movie_input = tf.keras.layers.Input(shape=(1,), name="movie")
movie_as_integer = tf.keras.layers.IntegerLookup(vocabulary=all_movies)(movie_input)
movie_embedding = tf.keras.layers.Embedding(input_dim=len(all_movies)+1, output_dim=32)(movie_as_integer)

# dot product
dot = tf.keras.layers.Dot(axes=2)([user_embedding, movie_embedding])
flatten = tf.keras.layers.Flatten()(dot)

# model input/output definition
model = tf.keras.Model(inputs=[user_input, movie_input], outputs=flatten)

model.compile(loss="mse", metrics=[tf.keras.metrics.MeanAbsoluteError()])
```

由于我们给用户和电影输入层起了好名字，我们可以像这样训练模型：

```py
model.fit(
    x={
        "user": X_train["user_id"],
        "movie": X_train["movie_id"]
    },
    y=y_train.values,
    batch_size=256,
    epochs=100,
    validation_split=0.1, # for early stopping
    callbacks=[
        tf.keras.callbacks.EarlyStopping(patience=1, restore_best_weights=True)
    ],
)

# Output (for me):
# ...
# Epoch 18/100
# 3165/3165 [==============================] - 8s 3ms/step - loss: 0.7357 - mean_absolute_error: 0.6595 - val_loss: 11.4699 - val_mean_absolute_error: 2.9923
```

我们现在可以在测试集上评估这个模型，但我们已经可以看到，它可能非常糟糕，因为`val_mean_absolute_error`约为 3。这意味着我们平均偏差为 3 星，在 5 星系统中这是非常糟糕的。这比我们之前的糟糕模型还要差，这是一个了不起的成就。😅 但这为什么呢？让我们在下一部分深入探讨一下。

## TensorFlow 实现，版本二

到目前为止，我们已经构建了一个回归模型，可以输出任何实数。模型很难学习应该输出在 1 到 5 之间的特殊范围的数字，但我们可以用一个简单的技巧来简化这一点：只需像处理逻辑回归一样压缩输出范围。只不过是将[0, 1]区间缩放和移动到[1, 5]。

```py
user_input = tf.keras.layers.Input(shape=(1,), name="user")
user_as_integer = tf.keras.layers.IntegerLookup(vocabulary=all_users)(user_input)
user_embedding = tf.keras.layers.Embedding(input_dim=len(all_users) + 1, output_dim=32)(user_as_integer)

movie_input = tf.keras.layers.Input(shape=(1,), name="movie")
movie_as_integer = tf.keras.layers.IntegerLookup(vocabulary=all_movies)(movie_input)
movie_embedding = tf.keras.layers.Embedding(input_dim=len(all_movies) + 1, output_dim=32)(movie_as_integer)

dot = tf.keras.layers.Dot(axes=2)([user_embedding, movie_embedding])
flatten = tf.keras.layers.Flatten()(dot)

# this is new!
squash = tf.keras.layers.Lambda(lambda x: 4*tf.nn.sigmoid(x) + 1)(flatten) 

model = tf.keras.Model(inputs=[user_input, movie_input], outputs=squash)

model.compile(loss="mse", metrics=[tf.keras.metrics.MeanAbsoluteError()])
```

公式如下：

![](img/b2df904a2383d151cae118d356c93023.png)

图片由作者提供。

其中 *σ* 是 sigmoid 函数。训练如上所述，测试集上的评估结果是

```py
model.evaluate(
    x={"user": X_test["user_id"], "movie": X_test["movie_id"]},
    y=y_test
)

# Output:
# [...] loss: 0.9701 - mean_absolute_error: 0.7683
```

这比之前的模型和我们糟糕的基准要好得多。如果你还想要一个*r*²得分：

```py
from sklearn.metrics import r2_score

r2_score(
    y_test,
    model.predict(
        {"user": X_test["user_id"], "movie": X_test["movie_id"]}
    ).ravel()
)
# Output:
# 0.1767611765807019 
```

让我们做一个小的最终调整，以得到一个更好的模型。

## TensorFlow 实现，最终版本

除了嵌入，我们还可以为每部电影和每个用户关联一个**偏置项**。这捕捉到一些用户往往只给出相对积极（或消极）的评分，以及一些电影往往只收到积极（或消极）的评论。这样，**偏置项**可以进行**粗略调整**，而**嵌入**则进行**精细调整**。例如，一个通常给 4 星的用户将有一个固定的偏置，一个**单一数字，而不是向量**。嵌入仅需专注于解释为什么这个用户有时给 3 星或 5 星。

公式变为

![](img/75abd763d3245efbe4dc491f8eafc77b.png)

图片由作者提供。

其中 *bᵤ* 和 *bₘ* 分别是用户 *u* 和电影 *m* 的偏置项。作为代码：

```py
user_input = tf.keras.layers.Input(shape=(1,), name="user")
user_as_integer = tf.keras.layers.IntegerLookup(vocabulary=all_users)(user_input)
user_embedding = tf.keras.layers.Embedding(input_dim=len(all_users) + 1, output_dim=32)(user_as_integer)
user_bias = tf.keras.layers.Embedding(input_dim=len(all_users) + 1, output_dim=1)(user_as_integer)

movie_input = tf.keras.layers.Input(shape=(1,), name="movie")
movie_as_integer = tf.keras.layers.IntegerLookup(vocabulary=all_movies)(movie_input)
movie_embedding = tf.keras.layers.Embedding(input_dim=len(all_movies) + 1, output_dim=32)(movie_as_integer)
movie_bias = tf.keras.layers.Embedding(input_dim=len(all_movies) + 1, output_dim=1)(movie_as_integer)

dot = tf.keras.layers.Dot(axes=2)([user_embedding, movie_embedding])
add = tf.keras.layers.Add()([dot, user_bias, movie_bias])
flatten = tf.keras.layers.Flatten()(add)
squash = tf.keras.layers.Lambda(lambda x: 4 * tf.nn.sigmoid(x) + 1)(flatten)

model = tf.keras.Model(inputs=[user_input, movie_input], outputs=squash)

model.compile(loss="mse", metrics=[tf.keras.metrics.MeanAbsoluteError()])
```

如果你喜欢 Keras 的`plot_model`输出：

![](img/b175aebb262f0e3808e06a4612b3af5c.png)

图片由作者提供。

![](img/84b0b3e30c35ade68fc1704bc7f4560a.png)

图片由作者提供，创建于[`netron.app/`](https://netron.app/)。

如已指出，模型性能再次改善。

+   MSE ≈ 0.89

+   MAE ≈ 0.746

+   *r*² ≈ 0.245

太好了！我们在这个版本中得到了最低的 MAE 和 MSE（因此*r*²最高）。

## 进行预测

为了回答像“用户 1 会给电影 2 和电影 3 多少分？”这样的问题，你可以做一个简单的

```py
model.predict({"user": tf.constant([[1], [1]]), "movie": tf.constant([[2], [3]])})

# Output:
# array([[3.0344076],
#        [2.9140234]], dtype=float32)
```

为了获取用户 1 的所有评分，你可以做

```py
model.predict({
    "user": tf.ones_like(all_movies.reshape(-1, 1)), # fill user 1 in many times
    "movie": all_movies.reshape(-1, 1)
})
```

# 结论

在这篇文章中，我们已经看到推荐系统对业务有很大的影响，这也是它们被广泛使用的原因。构建一个好的推荐系统不像其他模型那样简单，因为你通常需要处理高基数的分类特征，使得简单的技巧如独热编码变得无效。

我们学习了如何通过在神经网络架构中使用嵌入来规避这个问题。我们添加了一些简单的技巧，最终得到了一个不算差的矩阵分解模型，即使没有调整任何超参数。我们还可以通过

+   优化嵌入维度（我们目前设定为 32）

+   对嵌入应用正则化

+   构建一个适当的时间划分验证集，而不是像我们所做的那样随机生成的

+   在我们知道最佳超参数之后，在完整的训练数据集（包括验证集）上重新训练模型

最大的优势之一是我们可以在大多数情况下应用这个模型，因为我们只需要用户和电影的交互（评分）数据。我们不需要了解更多关于用户和电影的信息，例如年龄、性别、类型等等，因此通常我们可以立即开始。

我们为此付出的代价是我们无法为未知的用户或电影输出有意义的嵌入——**冷启动问题**。模型会输出*某些东西*，但质量会很差。

但是，如果我们恰好拥有用户和电影数据，我们可以做得更聪明，并且可以以直接的方式将这些特征融入进来。这可以缓解冷启动问题，甚至可能改善已知用户和电影的模型。你可以在这里阅读相关内容：

[](/a-performant-recommender-system-without-cold-start-problem-69bf2f0f0b9b?source=post_page-----956faceb1919--------------------------------) ## 一个高效的推荐系统，解决冷启动问题

### 当协作和基于内容的推荐系统合并时

towardsdatascience.com

# 参考文献

[1] D. Jannach 和 M. Jugovac, [推荐系统的商业价值测量](https://arxiv.org/abs/1908.08328) (2019), ACM Transactions on Management Information Systems (TMIS) 10.4 (2019): 1–23

希望你今天学到了新的、有趣的、有用的东西。感谢阅读！

**最后一点，如果你**

1.  **想要支持我写更多关于机器学习的内容**

1.  **无论如何，计划获取一个 Medium 订阅，**

**为什么不通过这个链接** [**支持我**](https://dr-robert-kuebler.medium.com/membership)**呢？这对我帮助很大！😊**

*为了透明，您的价格不会改变，但大约一半的订阅费用会直接给我。*

**非常感谢，如果你考虑支持我！**

> *如果你有任何问题，请在* [*LinkedIn*](https://www.linkedin.com/in/dr-robert-k%C3%BCbler-983859150/)*上联系我！*
