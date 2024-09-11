# 4种编码具有高基数的分类特征的方法——带Python实现

> 原文：[https://towardsdatascience.com/4-ways-to-encode-categorical-features-with-high-cardinality-1bc6d8fd7b13?source=collection_archive---------0-----------------------#2023-06-26](https://towardsdatascience.com/4-ways-to-encode-categorical-features-with-high-cardinality-1bc6d8fd7b13?source=collection_archive---------0-----------------------#2023-06-26)

## 学习如何使用scikit-learn和TensorFlow应用目标编码、计数编码、特征哈希和嵌入

[](https://medium.com/@aichabokbot?source=post_page-----1bc6d8fd7b13--------------------------------)[![Aicha Bokbot](../Images/1aa9ba6ae6296d8be3350b14dba97dd2.png)](https://medium.com/@aichabokbot?source=post_page-----1bc6d8fd7b13--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1bc6d8fd7b13--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1bc6d8fd7b13--------------------------------) [Aicha Bokbot](https://medium.com/@aichabokbot?source=post_page-----1bc6d8fd7b13--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F50566ce7e21&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-ways-to-encode-categorical-features-with-high-cardinality-1bc6d8fd7b13&user=Aicha+Bokbot&userId=50566ce7e21&source=post_page-50566ce7e21----1bc6d8fd7b13---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----1bc6d8fd7b13--------------------------------) · 9分钟阅读·2023年6月26日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1bc6d8fd7b13&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-ways-to-encode-categorical-features-with-high-cardinality-1bc6d8fd7b13&user=Aicha+Bokbot&userId=50566ce7e21&source=-----1bc6d8fd7b13---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1bc6d8fd7b13&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-ways-to-encode-categorical-features-with-high-cardinality-1bc6d8fd7b13&source=-----1bc6d8fd7b13---------------------bookmark_footer-----------)![](../Images/c1d754d660f384bf0e8be0017f196bbb.png)

“点击” — 照片由[Cleo Vermij](https://unsplash.com/@cleovermij?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在本文中，我们将深入探讨4种流行的方法来编码具有高基数的分类变量：**（1）目标编码，（2）计数编码，（3）特征哈希** 和 **（4）嵌入**。

我们将解释每种方法的工作原理，讨论其优缺点，并观察其对分类任务性能的影响。

## **目录**

— [引入类别特征](#8744)

*(1)* [*为什么我们需要编码类别特征？*](#2429) *(2)* [*为什么一热编码不适用于高基数？*](#b13b)

— [在AdTech数据集上的应用](#706a)

— [每种编码方法概述](#a959)

*(1)* [*目标编码*](#ffbc) *(2)* [*计数编码*](#fbd1) *(3)* [*特征哈希*](#e278) *(4)* [*嵌入*](#99d8)

— [预测CTR的性能基准测试](#892c)

— [结论](#033c)

— [进一步阅读](#3bd1)

# **引入类别特征**

**类别特征**是一种描述类别或组的变量（如性别、颜色、国家），而**数值特征**则测量数量（如年龄、身高、温度）。

类别数据有两种类型：**有序特征**，其类别可以排序（如T恤尺码或餐厅评分从1星到5星），和**名义特征**，其类别不具有任何有意义的顺序（如人的名字、城市名）。

## 我们为什么需要编码类别特征？

编码类别变量意味着找到一种映射，将类别转换为数值。

虽然一些算法可以直接处理类别数据（如决策树），**但大多数机器学习模型无法处理类别特征**，它们设计时只处理数值数据。对类别变量进行编码是一个必要的步骤。

此外，一些机器学习库要求所有数据必须是数值型的。例如，scikit-learn 就是这种情况。

## 为什么一热编码不适用于高基数？

编码类别特征的常见方法是应用**一热编码**。这种方法通过为每个唯一类别添加一个二元变量来对类别变量进行编码。

如果一个描述颜色的特征有三个类别[红色、蓝色、绿色]，一热编码器将其转换为三个二元变量，每个类别一个。

如果一个类别特征有数百或数千个类别，应用一热编码将会向特征向量中添加数百或数千个二元变量。模型在面对大规模稀疏数据时会遇到[维度灾难](https://www.quora.com/What-is-the-curse-of-dimensionality)的问题：在更多维度的解空间中搜索更困难，容易过拟合，计算时间增加，以及空间复杂度上升。

那么如何在不增加特征向量维度的情况下编码高基数的类别特征呢？

# 在AdTech数据集上的应用

我们将通过在[**Criteo展示广告挑战赛**](https://www.kaggle.com/competitions/criteo-display-ad-challenge/data)的数据集上应用四种编码技术来预测展示广告的点击率，来回答这个问题。

这是一个著名的Kaggle挑战赛，由**Criteo**（一家专注于程序化广告和实时竞价的法国在线广告公司）于2014年发起。广告的**点击率（CTR）**是广告被点击的次数与广告在页面上展示的次数之比。

**广告技术中的数据集通常包含具有高基数的ID变量**，例如 *site_id*（广告展示的网站ID）、*advertiser_id*（广告背后的品牌ID）、*os_id*（广告展示给用户的操作系统ID）。

**Criteo数据集包含100万行，39个匿名列**：13个数值变量和26个类别变量。它们的基数见下表。我们可以看到**许多特征具有非常高的基数（超过10k）**。

![](../Images/d673f17f1b6da26bcc979fd333875bc1.png)

Criteo数据集中类别特征的基数

数据集总共包含241,338个类别。**应用独热编码意味着将特征空间从39维度转换为241,351维度。** 显然，对一个超过241k列的稀疏矩阵进行计算是非常昂贵且低效的。

我们将数据集拆分为训练集和测试集，并探索编码方法。

```py
from sklearn.model_selection import train_test_split

features = df.columns[1:]
categorical_features = [feature for feature in features if feature[0] == "C" ]
x_train, x_test, y_train, y_test = train_test_split(df[features], df["label"])
```

# 每种编码方法的概述

## (1) 目标编码

我们使用了库 [category_encoder](https://contrib.scikit-learn.org/category_encoders/targetencoder.html) 的目标编码器，其定义如下：

> 特征被替换为给定特定类别值的目标的预期值与所有训练数据上目标的预期值的混合。

```py
from category_encoders.target_encoder import TargetEncoder

enc = TargetEncoder(cols = categorical_features).fit(x_train, y_train)
X_train_encoded = enc.transform(x_train)
X_test_encoded = enc.transform(x_test)
```

请注意，我们只在训练数据集上拟合编码器，然后使用拟合后的编码器转换训练集和测试集。由于在实际生活中我们无法访问y_test，使用它来拟合编码器将是不诚实的。

+   **编码特征空间的维度：** 39列，X_train_encoded和X_test_encoded的形状与x_train和y_train相同。

+   **优点：**

    - **无参数**

    - 特征空间没有增加

+   **缺点：**

    - **目标泄漏的风险**（目标泄漏指使用目标的一些信息来预测目标本身）

    - 当类别样本较少时，目标编码器会将它们替换为非常接近目标的值，这使得模型容易过拟合训练集。

    - 不接受测试集中的新值

## (2) 计数编码

使用计数编码，**也称为频率编码**，类别被替换为它们在数据集中的频率。如果ID *3f4ec687* 在列C7中出现了957次，则我们将 *3f4ec687* 替换为957。

如果两个类别在数据集中出现的次数相同，这种方法会用相同的值对它们进行编码，尽管它们不包含相同的信息。这**会造成所谓的冲突**：两个不同的类别被编码为相同的值。

```py
from category_encoders.count import CountEncoder

enc = CountEncoder(cols = categorical_features).fit(x_train, y_train)
X_train_encoded = enc.transform(x_train)
X_test_encoded = enc.transform(x_test)
```

+   **编码特征空间的维度：** 39 列，X_train_encoded 和 X_test_encoded 的形状与 x_train 和 y_train 相同。

+   **优点：**

    - 易于理解和实现

    - 无参数

    - 特征空间没有增加

+   **缺点：**

    - 当发生碰撞时存在信息丢失的风险

    - 可能过于简化（我们从分类特征中保留的唯一信息是它们的频率）

    - 在测试集中不接受新值

## (3) 特征哈希

特征哈希**将分类特征投影到一个固定维度的特征向量中**，该维度比原始特征空间要小。这个特征向量的维度需要事先定义。这是通过使用**哈希技巧，将哈希函数应用于特征**，并将其哈希值作为特征向量中的索引来完成的。

有**两种实现特征哈希的方法**：

+   或者**我们逐个特征应用哈希**（每个特征一个特征空间，因此我们需要为每个特征选择一个维度参数）

+   或**我们将所有特征一起哈希**（所有特征共享一个特征空间，选择一个参数，但特征之间可能会发生碰撞）。

这种方法不是无参数的，我们需要选择哈希空间的大小。我们遵循伟大文章 [不要被哈希技巧欺骗](https://booking.ai/dont-be-tricked-by-the-hashing-trick-192a6aae3087) 的建议，选择哈希大小 = 20**k*，其中 *k* 是分类特征的数量（在我们的例子中 *k*=26）。

```py
from category_encoders.hashing import HashingEncoder

enc = HashingEncoder(
    cols = categorical_features, 
    n_components=20*len(categorical_features)
).fit(x_train, y_train)
X_train_encoded = enc.transform(x_train)
X_test_encoded = enc.transform(x_test)
```

+   **编码特征空间的维度：** 533 列

+   **优点：**

    - 特征空间的增加有限（与独热编码相比）

    - 由于不维护观察到的类别的字典，因此在推断过程中不会增长大小并接受新值

    - 当特征哈希应用于所有分类特征以创建单个哈希时，捕捉特征之间的交互

+   **缺点：**

    - 需要调整哈希空间维度的参数

    - 当哈希空间的维度不足时存在碰撞风险

## (4) 嵌入

嵌入是一种流行的编码技术，来自深度学习和自然语言处理（NLP）。它的核心是**构建一个可训练的查找表，将类别映射到固定长度的向量表示**。在训练过程中，查找表中的权重会被更新，以更好地描述类别之间的相似性。

我们将遵循这个 [Keras 教程](https://keras.io/examples/structured_data/classification_with_tfdf/#experiment-3-decision-forests-with-trained-embeddings) 来“**构建一个编码器模型，将分类特征编码为嵌入向量**，其中给定分类特征的嵌入大小是其词汇表大小的平方根。我们通过反向传播在一个简单的神经网络模型中训练这些嵌入向量。”

```py
import tensorflow as tf

def build_input_layers(features):
    input_layers = {}
    for feature in features:
        if feature in categorical_features:
            input_layers[feature] = tf.keras.layers.Input(
                shape=(1,),
                name=feature,
                dtype=tf.string
            )
        else:
            input_layers[feature] = tf.keras.layers.Input(
                shape=(1,),
                name=feature,
                dtype=tf.float32
            )
    return input_layers

def build_embeddings(size=None):
    input_layers = build_input_layers(features)
    embedded_layers = []

    for feature in input_layers.keys():
        if feature in categorical_features:
            # Get the vocabulary of the categorical feature
            vocabulary = sorted(
                    [str(value) for value in list(x_train[feature].unique())]
                )
            # convert the string input values into integer indices
            cardinality = x_train[feature].nunique()
            pre_processing_layer = tf.keras.layers.StringLookup(
              vocabulary=vocabulary, 
              num_oov_indices=cardinality,
              name=feature+"_preprocessed"
            )
            pre_processed_input = pre_processing_layer(input_layers[feature])
            # Create an embedding layer with the specified dimensions
            embedding_size = int(math.sqrt(cardinality))
            embedding_layer = tf.keras.layers.Embedding(
                input_dim=2*cardinality+1,
                output_dim=embedding_size,
                name=feature+"_embedded",

            )
            embedded_layers.append(embedding_layer(pre_processed_input))   
        else:
            # return numerical feature as it is
            embedded_layers.append(input_layers[feature])

    # Concatenate all the encoded features.
    encoded_features = tf.keras.layers.Concatenate()([
                tf.keras.layers.Flatten()(layer) for layer in embedded_layers
            ])

    # Apply dropout.
    encoded_features = tf.keras.layers.Dropout(rate=0.25)(encoded_features)

    # Perform non-linearity projection.
    encoded_features = tf.keras.layers.Dense(
        units=size if size else encoded_features.shape[-1], activation="gelu"
    )(encoded_features)
    return tf.keras.Model(inputs=input_layers, outputs=encoded_features)

def build_neural_network_model(embedding_encoder):
    input_layers = build_input_layers(features)
    embeddings = embedding_encoder(input_layers)
    output = tf.keras.layers.Dense(units=1, activation="sigmoid")(embeddings)

    model = keras.Model(inputs=input_layers, 
                        outputs=output)

    model.compile(
        optimizer=tf.keras.optimizers.Adam(),
        loss=tf.keras.losses.BinaryCrossentropy(),
        metrics=[tf.keras.metrics.AUC()]
    )

    return model

embedding_encoder = build_embeddings(64)
neural_network_model = build_neural_network_model(embedding_encoder)

# Training
def build_dataset(x, y):
    dataset = {}
    for feat in features:
        if feat in categorical_features:
            dataset[feat] = np.array(x[feat]).reshape(-1,1).astype(str)
        else:
            dataset[feat] = np.array(x[feat]).reshape(-1,1).astype(float)

    return dataset, np.array(y).reshape(-1,1)

x_train, y_train = build_dataset(x_train, y_train)
x_test, y_test = build_dataset(x_test, y_test)
history = neural_network_model.fit(x_train, y_train, batch_size=1024, epochs=5)

X_train_encoded = embedding_encoder.predict(x_train, batch_size=1024)
X_test_encoded = embedding_encoder.predict(x_test, batch_size=1024)
```

+   **编码特征空间的维度：** 64 列

+   **优点：**

    - 特征空间的增加有限（与独热编码相比）

    - 在推断过程中接受新值

    - 捕捉特征之间的交互并学习类别之间的相似性

+   **缺点：**

    - 需要调整嵌入大小的参数

    - 嵌入和逻辑回归模型不能在一个阶段中协同训练，因为逻辑回归不使用反向传播进行训练。相反，嵌入需要在初始阶段进行训练，然后作为静态输入用于决策森林模型。

# 预测CTR的性能基准

我们拟合了一个简单的逻辑回归模型来预测CTR，并使用每种编码方法生成预测。

```py
from sklearn.linear_model import LogisticRegression
model = LogisticRegression(max_iter=10000)
model.fit(X_train_encoded, y_train)
y_pred = model.predict(X_test_encoded)
```

我们计算了对数损失、AUC、召回率和预测CTR的平均值。结果见下表。**数据集中的平均CTR为22.6%。**

![](../Images/b04ea3a2befc95538b580375fb0ffdc1.png)

我们首先注意到所有编码方法的AUC都相当低，这主要是因为我们使用了一个非常简单的模型，比逻辑回归更复杂的模型更适合这个任务。

前三种方法（目标编码、计数编码和哈希编码）**未能让模型捕捉到足够的信号以预测CTR**：与真实平均值相比，平均预测CTR非常低，召回率也接近于零，AUC接近0.5则表明模型几乎是随机的。**使用嵌入的模型显示出最高的AUC和召回率**，以及接近目标的平均预测CTR。

# 结论

我们探讨了四种编码高基数分类数据的技术：目标编码、计数编码、特征哈希和嵌入。具体来说，我们学到了：

+   高基数分类数据处理的挑战

+   四种技术的优缺点

+   如何将每种技术应用于分类任务中以预测点击率

# 进一步阅读

+   本文代码的笔记本：[编码高基数分类特征](https://www.kaggle.com/aichabokbot/encoding-high-cardinality-categorical-features)

+   如何选择最相关的编码技术的备忘单：[更聪明地编码：如何轻松将分类编码集成到您的机器学习流程中](https://innovation.alteryx.com/encode-smarter/)

+   关于碰撞的更多信息：[不要被哈希技巧欺骗](https://booking.ai/dont-be-tricked-by-the-hashing-trick-192a6aae3087)

+   关于分类嵌入的更多信息：[带有分类嵌入的密集神经网络](https://www.kaggle.com/code/dustyturner/dense-nn-with-categorical-embeddings)
