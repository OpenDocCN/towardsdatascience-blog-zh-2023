# 使用 TensorFlow 推荐系统的隐式反馈推荐系统

> 原文：[`towardsdatascience.com/recommender-systems-from-implicit-feedback-using-tensorflow-recommenders-8ba36a976c57`](https://towardsdatascience.com/recommender-systems-from-implicit-feedback-using-tensorflow-recommenders-8ba36a976c57)

## [推荐系统](https://medium.com/tag/recommendation-system)

## 当客户没有明确告诉你他们想要什么时

[](https://dr-robert-kuebler.medium.com/?source=post_page-----8ba36a976c57--------------------------------)![Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----8ba36a976c57--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8ba36a976c57--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8ba36a976c57--------------------------------) [Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----8ba36a976c57--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8ba36a976c57--------------------------------) ·11 分钟阅读·2023 年 11 月 5 日

--

![](img/085198f24d7254f94814a1ebdec4619b.png)

图片由[Noom Peerapong](https://unsplash.com/@imnoom?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

提供推荐实际上并没有那么难。你只需要检查你的客户如何评分你的产品，例如使用 1 到 5 颗星，然后在其基础上训练一个**回归**模型，对吗？

![](img/ae0ec03fd0b1941c5147d59abe3a80c0.png)

一个典型的数据集，你会希望拥有的。图片由作者提供。

好吧，如果我们没有任何数值的用户或电影特征，我们可能需要处理嵌入，但我们已经在我之前的文章中见过如何做到这一点。

[](/introduction-to-embedding-based-recommender-systems-956faceb1919?source=post_page-----8ba36a976c57--------------------------------) ## 嵌入式推荐系统简介

### 学习在 TensorFlow 中构建一个简单的矩阵分解推荐系统

towardsdatascience.com

在本文中我们也需要嵌入，因此我建议在继续之前阅读上面的文章。

# 隐式反馈

然而，有时候我们没有**显式**的用户反馈，例如星级、点赞或点踩，或类似的反馈。这在零售中很常见，我们知道哪个客户购买了哪个商品，但不知道他们是否真的喜欢它。我们从客户那里得到的唯一信息是关于他们对该产品**兴趣的隐式信号**。

> 如果他们购买了（观看、消费……）这个产品，他们就表现出兴趣。如果没有，他们**可能**不感兴趣，但也可能只是还不知道。我们无法确定。

这听起来像是我们可以将其视为分类问题。感兴趣 = 1，不感兴趣 = 0。然而，这里有一个小问题，我们不能确定 0（不感兴趣）是否**真的**是零。也可能是客户只是从未有机会购买，但实际上是想要的。

让我们回到电影，假设我们没有任何评分。我们只知道哪个用户观看了哪个电影。

![](img/2987a72fdbf65a3e5923851a43666eb0.png)

比如说，Alice 观看了 Gaußzilla 和 The Markov Chainsaw Massacre。图片由作者提供。

从这里我们可以有至少两种方法继续。

1.  只需将所有缺失值视为零，然后训练一个二分类器。

1.  使用成对损失函数来确保用户与他们观看过的电影之间的相似性高于同一用户与他们没有观看过的电影之间的相似性。

## 将所有缺失值视为零

这是最简单的解决方案。从上面的不完整表格，你可以创建以下数据集：

> ***注意: A*** *= Alice,* ***B*** *= Bob,* ***C*** *= Charlie,* ***G*** *= Gauß,* ***E*** *= Euler,* ***M*** *= Markov*

![](img/dc8285c01783fc90b2c4f4bcc62fd864.png)

图片由作者提供。

你可以将**观看**列解释为用户**是否对电影感兴趣**的标签。从这个表格中，你可以推断出，例如，用户**A**喜欢电影**G**，但不喜欢电影**E**，这在数据上是一个大胆的声明。也许**A**还不知道**E**。或者更糟的是，它实际上在**A**的观看列表中，但没有时间观看。

这个方法在技术方面的问题是，模型学会对几乎任何（用户，电影）输入都返回 0，因为大多数**观看**值通常为零。想象一下你有一个包含 1,000,000 个用户和 100,000 部电影的数据集。平均每个用户观看多少部不同的电影？也许是 1000 部？那你**1%**的观看标签是 1。因此，你有一个严重不平衡的数据集，这本身并不是坏事。然而，由于我们**人工创建这些零值**，可能会导致性能较差。

一个计算问题是这个数据集变得非常庞大。1,000,000 个用户乘以 100,000 部电影意味着你有一个包含 100,000,000,000 行的数据集。并且通常，你的数据库中还有更多的电影和项目。在这种情况下，你不会将所有的零目标行都放入数据集中，而是**进行子采样，也称为负采样**。例如，如果你有 1,000,000,000 行目标为 1 的数据（= 发生的交易），你也可以子采样 1,000,000,000 个负样本（= 从未发生的交易）。这样你就有了一个可以训练的良好数据集。

这种方法有效，但通常不是最优的，因为你把问题变得比实际需要的更复杂。你不需要完美预测观看标签。你只想对每个用户对电影进行排序，即你想能够说“用户**A**更喜欢电影**G**而不是电影**E**”。第二种方法正好满足这个需求。

## 使用成对损失函数

在这种方法中，我们不会告诉模型用户是否喜欢某个特定的电影。我们更谨慎地表述：

> 如果用户 A 观看了电影 G，但没有观看电影 E，我们仅仅说 A 对 G 的兴趣大于对 E 的兴趣。

这使我们能够解决一个更简单的目标。现在，让我们开始一些公式，以便更好地理解这种直觉如何转化为算法。

我们将再次训练一个处理**嵌入**的模型。假设我们有用户**A**、电影**G**和电影**E**的嵌入。如果**A**观看了**G**，但没有观看**E**，我们仅希望得到

![](img/33995c21ff7c4dc7fa4deb3b801e798c.png)

图片由作者提供。

其中的*e*是嵌入，·是点积。这意味着对于用户**A**来说，电影**G**在某种程度上是**更好**的，优于电影**E**。但这比说“**A**喜欢**G**但不喜欢**E**”要温和，因为后者是二元分类的情况。

训练这样的模型听起来比训练二元分类器复杂得多，但有几个库可以帮助我们。我将展示如何使用[**TensorFlow Recommenders**](https://www.tensorflow.org/recommenders)，因为这是我知道的最灵活的库。另一个值得一提的是[implicit](https://benfred.github.io/implicit/)，它易于使用但不够灵活。

# 使用 TensorFlow Recommenders 训练

我们现在将看到将之前描述的逻辑放入代码中是多么简单。为了开诚相见，我遵循了[官方 TFRS 网站的指南](https://www.tensorflow.org/recommenders/examples/basic_retrieval)。我只是试图让它更简洁。

## *准备和数据生成*

首先，让我们做一个

```py
pip install tensorflow tensorflow-recommenders tensorflow-datasets
```

然后我们可以通过以下方式加载一些数据

```py
import tensorflow_datasets as tfds
import tensorflow_recommenders as tfrs
import tensorflow as tf

ratings = (
    tfds.load("movielens/100k-ratings", split="train")
    .map(lambda x: {
        "movie_title": x["movie_title"],
        "user_id": x["user_id"],
    })
    .shuffle(10000)
)

ratings_df = tfds.as_dataframe(ratings)
```

`ratings` 是一个 TensorFlow 数据集，处理起来总是有点繁琐。然而，对于大数据集的内存高效训练，你必须使用它。但对于我们的小示例，我尽量停留在友好的数据框世界中，因此将数据集转换为数据框`ratings_df`。数据如下：

![](img/42a19cafd1bd357457c9ce228fd02f5b.png)

图片由作者提供。

## 模型定义

我们将构建一个有两个部分的模型：

+   一个用户模型

+   一个电影模型

这些模型应分别接收用户或电影，并将其转换为嵌入，即一组浮点数。

```py
embedding_dimension = 32

user_model = tf.keras.Sequential([
  tf.keras.layers.StringLookup(vocabulary=ratings_df["user_id"].unique()),
  tf.keras.layers.Embedding(ratings_df["user_id"].nunique() + 1, embedding_dimension)
])

movie_model = tf.keras.Sequential([
  tf.keras.layers.StringLookup(vocabulary=ratings_df["movie_title"].unique()),
  tf.keras.layers.Embedding(ratings_df["movie_title"].nunique() + 1, embedding_dimension)
])
```

使用这两个组件，我们可以这样定义完整的模型：

```py
class MovielensModel(tfrs.Model):
    def __init__(self, user_model, movie_model, task):
        super().__init__()
        self.movie_model = movie_model
        self.user_model = user_model
        self.task = task

    def compute_loss(self, features, training=False):
        user_embeddings = self.user_model(features["user_id"])
        positive_movie_embeddings = self.movie_model(features["movie_title"])

        return self.task(user_embeddings, positive_movie_embeddings)
```

我们可以看到模型由`user_model`和`movie_model`组成。我们将这个`task`属性设置为**检索任务**，这正是我们所需要的实现。还有另一种任务类型，叫做**排名任务**，当你有明确的反馈如评分时可以使用。本文不会进一步探讨这个。

你还可以看到一些损失值被计算出来。输入是一个名为`features`的字典，它应该看起来像这样：

```py
features = {
    "user_id": ["A", "B", "C"],
    "movie_title": ["G", "E", "M"],
}
```

它包含了一些用户 ID 以及一系列电影标题。在这个例子中，用户**A**观看了**G**，用户**B**观看了**E**，用户**C**观看了**M**。我们这里只有**正例**，即过去发生的电影会话。

用户和电影被转化为嵌入，然后计算一些损失。我稍后会详细说明，但请放心，它正在做我们希望它做的事。

模型的架构如同我在其他文章中描述的那样：

![](img/432cf2b8372b38b95ab77cca25a82110.png)

图片由作者提供。

## 训练模型

最后，我们可以使用一个很好的 TFRS 预测类，但为了使其正常工作，我们需要一个独特的电影列表作为 TensorFlow 数据集。

```py
# a TensorFlow dataset
unique_movies = tf.data.Dataset.from_tensor_slices(ratings_df["movie_title"].unique())
```

使用这个数据集，我们可以定义之前提到的任务：

```py
task = tfrs.tasks.Retrieval()
```

现在，我们可以训练模型了！

```py
model = MovielensModel(user_model, movie_model, task)
model.compile(optimizer=tf.keras.optimizers.Adagrad(learning_rate=0.1))

model.fit(ratings.batch(10000).cache(), epochs=5)
```

你最终会看到类似这样的结果：

![](img/4fa10014f6516a019104171a83351b54.png)

图片由作者提供。

很难给`loss`赋予具体的意义，但越小越好。我会进一步详细说明，但让我们先用我们的模型预测一些电影！

## 预测时间

首先，你需要定义一个叫做**索引**的东西。然后你可以使用这个索引来获取预测结果。

```py
index = tfrs.layers.factorized_top_k.BruteForce(model.user_model)

index.index_from_dataset(
  tf.data.Dataset.zip((unique_movies.batch(100), unique_movies.batch(100).map(model.movie_model)))
)
```

**注意：** 你不再使用`model`了。你只是用它来调整`user_model`和`movie_model`的参数，现在直接使用这两个子模型。此时你基本上可以抛弃 TFRS 模型`model`。

现在，你可以将用户传递给`index`。索引将会

1.  将这个用户 ID 转换为嵌入 —— 这是因为你传递了它给`user_model` —— 然后

1.  计算每部电影的嵌入 —— 这是通过执行`index_from_dataset`函数实现的 —— 然后

1.  输出与用户嵌入最接近的电影标题。

**注意：** 它在后台进行的是精确的最近邻搜索，这可能会很慢。它还支持使用[ScaNN](https://github.com/google-research/google-research/tree/master/scann)进行近似最近邻搜索。你可以通过输入`ScaNN`代替`BruteForce`来使用它。

它的工作原理如下：

```py
_, titles = index(tf.constant(["99"]))
print(f"Recommendations for user 99: {titles[0, :3]}")

# Output:
# Recommendations for user 99: [b'Sunset Park (1996)' b'Happy Gilmore (1996)' b'High School High (1996)']
```

很好！你现在已经准备好使用这个模型了。

## 收尾工作

还有一些我承诺要深入探讨的内容：这个`task`和它输出的损失。内部发生的事情文档（尚未）非常好，但我查看了源代码以了解发生了什么。[你可以在这里找到我提到的源代码](https://github.com/tensorflow/recommenders/blob/7caed557b9d5194202d8323f2d4795231a5d0b1d/tensorflow_recommenders/tasks/retrieval.py#L119)。

我将通过一个小例子批次来解释。

![](img/e99f92bf4583acaeb2821ea5567cd9f4.png)

进入模型的批量。图片由作者提供。

它进入模型，然后使用`user_model`和`movie_model`创建每个用户和电影的嵌入。这些嵌入进入检索任务对象。

在`task`中，所有用户嵌入与所有电影嵌入进行相乘（点积）。这可以通过简单的矩阵乘法完成，其中电影矩阵首先转置。假设我们使用二维嵌入来节省空间。

![](img/e6949c37f37826b9474b23e3b47d341f.png)

图片由作者提供。

矩阵乘积是

![](img/ee3e4eb45f9fa7a327bd476daa3cdc25.png)

图片由作者提供。

现在，推理过程如下：从数据中我们知道

+   爱丽丝观看了高斯，

+   鲍勃观看了欧拉，并且

+   查理观看了马尔可夫。

这就是为什么我们希望在单元格中的对应数字（**A**，**G**），（**B**，**E**），（**C**，**M**）——这就是主对角线——具有最高的数字。在这个小例子中，我们差得很远。

为了量化这一点，他们进行另一步：TFRS 的作者进行逐行 softmax。

![](img/78ec8f81d6c217d9cd20982559a039b2.png)

经过逐行 softmax 处理。注意每行的和是 1。图片由作者提供。

现在，观察到的是：如果前一个矩阵中主对角线上的元素远高于其他数字，那么“softmax 处理过”的矩阵接近于**单位矩阵**。

![](img/604191623a481dc49c87dd0ceca69bce.png)

最优的单位矩阵。图片由作者提供。

这是因为如果你对一个数组进行 softmax 处理，如果一个数字远高于其他数字，这个数字将接近 1。所以其他数字必须接近零。可以尝试一下：

```py
x = np.array([1, 2, 10])
np.exp(x) / np.exp(x).sum() # softmax

# Output:
# array([1.23353201e-04, 3.35308764e-04, 9.99541338e-01])
```

所以，损失来自于将上面的矩阵与单位矩阵进行比较。具体来说，使用的是分类交叉熵损失。但是不是平均值，而是**总和**，[见此处](https://github.com/tensorflow/recommenders/blob/7caed557b9d5194202d8323f2d4795231a5d0b1d/tensorflow_recommenders/tasks/retrieval.py#L84)。这就是为什么损失值总是这么高的原因。批量越大，损失会越大。所以不要对损失突然变得非常低感到困惑，仅仅因为你把批量大小从 10,000 改成 1,000 或类似的数值。

# 结论

在这篇文章中，我们学习了如何利用隐式反馈数据构建推荐系统。为此，我们使用了 TensorFlow Recommenders，因为它扩展性强且非常表达清晰：你可以使用任何子模型——只要它输出一个嵌入——然后将它们组合在一起，使用 `tfrs.Model` 类进行联合训练。

经过训练后，你可以使用一个方便的类来进行实际的预测。如果你使用 ScaNN，这应该会非常快，但如果你需要更强大的搜索功能，你可以使用像 [Qdrant](https://qdrant.tech/) 这样的专用向量数据库。你将训练模型中的用户和电影嵌入提供给它，它会为你进行搜索。

我们还查看了库的内部结构，以了解要最小化的损失来自哪里，因此这个库不再是纯粹的魔法。

如果你想了解如何评估隐式反馈推荐系统的质量，请参考我的另一篇文章：

[](/the-guide-to-recommender-metrics-c5d72193ea2b?source=post_page-----8ba36a976c57--------------------------------) ## 推荐系统度量指南

### 离线评估推荐系统可能会很棘手

towardsdatascience.com

我希望你今天学到了一些新的、有趣的和有价值的东西。感谢你的阅读！

> *如果你有任何问题，可以在* [*LinkedIn*](https://www.linkedin.com/in/dr-robert-k%C3%BCbler-983859150/)*上写信给我*！

如果你想更深入地了解算法的世界，可以尝试我的新出版物**《全面了解算法》**！我还在寻找作者！

[](https://medium.com/all-about-algorithms?source=post_page-----8ba36a976c57--------------------------------) [## 全面了解算法

### 从直观的解释到深入的分析，算法通过示例、代码和精彩的内容变得生动起来…

medium.com](https://medium.com/all-about-algorithms?source=post_page-----8ba36a976c57--------------------------------)
