# Pandas: 处理你的数据类型！

> 原文：[`towardsdatascience.com/pandas-work-on-your-dtypes-20d9d32d2e42`](https://towardsdatascience.com/pandas-work-on-your-dtypes-20d9d32d2e42)

## 在 pandas 中拥有正确的数据类型是进行干净数据分析的必需条件。下面是原因和方法。

[](https://mocquin.medium.com/?source=post_page-----20d9d32d2e42--------------------------------)![Yoann Mocquin](https://mocquin.medium.com/?source=post_page-----20d9d32d2e42--------------------------------)[](https://towardsdatascience.com/?source=post_page-----20d9d32d2e42--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----20d9d32d2e42--------------------------------) [Yoann Mocquin](https://mocquin.medium.com/?source=post_page-----20d9d32d2e42--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----20d9d32d2e42--------------------------------) ·阅读时间 8 分钟·2023 年 9 月 26 日

--

**为你的 Series 和 DataFrame 选择合适的数据类型非常重要，原因有很多：**

+   **内存管理**：为特定的*系列*选择正确的数据类型可以显著减少内存使用，进一步地，这也适用于*数据框*

+   **解释**：其他任何人（无论是人还是计算机）都会根据数据类型对你的数据做出假设：如果一列全是整数却被存储为字符串，它们会将其视为字符串，而不是整数

+   **它强制要求你拥有干净的数据**，例如处理缺失值或记录错误的值。这将大大简化后续的数据处理

可能还有很多其他原因，你能列举一些吗？如果可以，请在评论中写出来。

**在我系列 pandas 的第一篇文章中，我想回顾 pandas 数据类型的基础知识。**

![](img/30c0a25974ae9b6e906206459f18ef8f.png)

照片由 [Chris Curry](https://unsplash.com/@chriscurry92?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我们首先会回顾 pandas 提供的可用数据类型，然后我将重点介绍 4 种有用的数据类型，它们能满足你 95% 的需求，即**数值数据类型、布尔数据类型、字符串数据类型和分类数据类型**。

**这篇文章的最终目标是让你对 pandas 中各种数据类型及其差异更加熟悉。**

**如果你对 pandas 和时间序列感兴趣，确保查看我关于时间序列傅里叶变换的文章：**

+   **回顾卷积如何与傅里叶变换相关以及它的速度**：

[](/fourier-transform-for-time-series-fast-convolution-explained-with-numpy-5a16834a2b99?source=post_page-----20d9d32d2e42--------------------------------) ## 时间序列的傅里叶变换：使用 numpy 解释快速卷积

### 使用傅里叶变换实现 10000 倍更快的卷积

[towardsdatascience.com

+   **通过图像示例深化你对卷积的理解：**

[](/fourier-transform-for-time-series-about-image-convolution-and-scipy-5e8fa1279603?source=post_page-----20d9d32d2e42--------------------------------) [## 傅里叶变换用于时间序列：关于图像卷积和 SciPy]

### 傅里叶变换卷积同样适用于图像

towardsdatascience.com

+   **了解傅里叶变换如何通过向量可视化方法进行直观理解：**

[](/fourier-transform-for-time-series-plotting-complex-numbers-9743ffe8a8bb?source=post_page-----20d9d32d2e42--------------------------------) [## 傅里叶变换用于时间序列：绘制复数]

### 绘制傅里叶变换算法以理解它。

towardsdatascience.com

+   **查看去趋势技术如何显著改善傅里叶变换的输出：**

[](/fourier-transform-for-time-series-detrending-f0f470f4bf14?source=post_page-----20d9d32d2e42--------------------------------) [## 傅里叶变换用于时间序列：去趋势]

### 去趋势你的时间序列可能是一个改变游戏规则的因素。

towardsdatascience.com

# **回顾可用的数据类型**

让我们花一分钟回顾一下 pandas 提供的数据类型。由于 *pandas* 基于 *numpy*，因此它们可以分为两类：

+   **基于 numpy 的数据类型**

+   **pandas 特定的数据类型**

在底层，*pandas* 将你的数据存储在 *numpy* 数组中，因此使用 numpy 的数据类型。我们可以将这些视为终极低级数据类型。但为了方便起见，*pandas* 还暴露了由 *pandas* 团队特别创建的高级数据类型。

想法是，从你的角度来看，所有数据类型（无论是基于 numpy 还是 pandas 特定的）都被视为对你的 *series* 和 *dataframe* 有效的数据类型。

**基于 *numpy* 的数据类型如下：**

+   `**float**`：用于存储浮点数（0.0245）

+   `**int**`：用于存储整数（1，-6）

+   `**bool**`：用于存储布尔值（True/False）

+   `**datetime64[ns]**`：用于存储时间线上的一个时刻（日期和时间）

+   `**timedelta64[ns]**`：用于存储相对持续时间（这补充了 datettime 数据类型）

+   `**object**`：可以存储任何 Python 对象

[实际的 numpy 数据类型完整列表](https://numpy.org/doc/stable/user/basics.types.html) 比这更大，但我们将坚持上述内容。

**然后 *pandas* 进入并暴露了许多新数据类型，包括：**

+   `**string**`：另一种存储字符串的方式

+   `**Nullable-int**` 和 `**Nullable-float**` 比 *numpy* 的 `**int**` 和 `**float**` 更适合处理缺失值

+   `**boolean**`：比 *numpy* 的 `**bool**` 更适合处理缺失值的布尔类型

+   `**categorical**`：适用于只能有特定值并能处理缺失条目的数据类型

再次，[pandas 中定义了其他数据类型，](https://pandas.pydata.org/docs/user_guide/basics.html#dtypes) 但我想专注于这四种，因为我发现它们最有用。

# **数据类型的设置方式**

实际上，你的数据类型可以有两种来源：

+   **要么你没有指定数据类型**，而 *pandas* 在创建系列/数据框时做了假设（无论是从加载 csv 还是创建类似 *`s = pd.Series([1, 2, 3])`* 的对象）。这种情况通常会有 50% 到 80% 的成功率，具体取决于输入数据的格式。

+   **要么你指定数据类型**，通过明确告知 pandas 每个列/系列使用什么数据类型

对于第一种情况：

- **优点**：更简单更快速

- **缺点**：除非你在之后审查每个数据类型，否则你不知道发生了什么，可能没有最合适的数据类型（例如 `object` 因为 pandas 没有确定列的内容），甚至可能为某些列选择了非常不合适的数据类型

对于第二种情况：

- **优点**：你准确知道数据发生了什么，确切知道列的数据类型，知道你是如何处理缺失和不良条件值的。换句话说，你的数据已经准备好进行实际处理

- **缺点**：需要更多时间和更多代码

**最后，我建议以下做法：首先，让 pandas 推断数据类型并尽力而为。然后审查每一列，并手动设置你认为应该更改的数据类型。**

**我们将在下一篇文章中讨论如何做所有这些，但现在我想回顾一下可用的数据类型及其存在的原因。**

**由于大多数情况下我最终只使用所有可用数据类型的一个子集，所以我会专注于这些：**

+   **数值数据类型，主要是 float 和 int**

+   **布尔数据类型**

+   **类似字符串的（包括字符串和对象）数据类型**

+   **类别数据类型**

# **类似数值的数据类型**

关于数值数据类型，*numpy* 提供了一个坚实的基础：整数和浮点数数据类型，也能处理 `*np.nan*`。还要注意，默认使用的数值数据类型是 `int64` 和 `float64`。这可能不适合你的数据，考虑到内存使用和给定系列的显式允许值：

```py
pd.Series([1, 2, 3])                     # dtype: int64
pd.Series([1, 2, 3], dtype="int")        # dtype: int64
pd.Series([1, 2, 3], dtype='float')      # dtype: float64

pd.Series([1, 2, np.nan])                # dtype: float64
pd.Series([1, 2, np.nan], dtype="int")   # dtype: float64
pd.Series([1, 2, np.nan], dtype='float') # dtype: float64

pd.Series([1, 2, pd.NA])                 # dtype: object
# fails since pd.NA cannot be converted to an int
# pd.Series([1, 2, pd.NA], dtype="int"))
# fails since pd.NA cannot be converted to an int
# pd.Series([1, 2, pd.NA], dtype='float'))
```

注意，一旦使用 `***np.nan***`，数据类型会被转换为 float。因此，*pandas* 还提供了能够处理缺失值并保持显式底层数值数据类型的数据类型，如 `***Int64Dtype***`（[具体理由见这里](https://pandas.pydata.org/docs/user_guide/integer_na.html#nullable-integer-data-type)）。

```py
pd.Series([1, 2, 3, np.nan], dtype="Int64")   # dtype: Int64
pd.Series([1, 2, 3, pd.NA], dtype="Int64")    # dtype: Int64
pd.Series([1, 2, 3, np.nan], dtype="Float32") # dtype: Float32
pd.Series([1, 2, 3, pd.NA], dtype="Float32")  # dtype: Float32
```

这样，我们可以兼得两全：一个明确的底层数值数据类型（int64，float32 等），并使用 `***pd.NA***` 处理缺失值。

# **布尔类数据类型**

基本上，有 2 种数据类型类似于布尔值，即‘***bool***’ 和 ‘***boolean***’。

‘*bool*’ 对应于标准的 *numpy* 布尔类型，因此不能包含‘***NA***’，因为布尔 numpy 数组只能存储 ***True*** 和 ***False***。

为了处理“不可用”条目——或“NA”——pandas 提供了数据类型‘***boolean***’，它可以包含‘NA’。见下面的示例：

```py
pd.Series([True, False], dtype='bool')              # numpy-boolean
pd.Series([True, False, pd.NA], dtype='boolean')    # pandas-boolean
pd.Series([True, False, np.nan], dtype='boolean')   # pandas-boolean

# this cannot work, since numpy does not handle pd.NA
# pd.Series([True, False, pd.NA], dtype='bool') 

# this works, but np.nan is converted to True...
# pd.Series([True, False, np.nan], dtype="bool") # --> dtype: object, not boolean...

# if no dtype is passed, pandas tries to infer an appropriate one
pd.Series([True, False])         # --> dtype: bool
pd.Series([True, False, pd.NA])  # --> dtype: object, not boolean...
pd.Series([True, False, np.nan]) # --> dtype: object, not boolean...
```

# **字符串类数据类型**

本质上，字符串可以存储在 *numpy* 数组中，因为我们可以使用 ***`object`*** 数据类型，该数据类型用于处理“任何 Python 对象”。

字符串与大多数其他数据类型不同，因为不能提前知道字符串的长度，因此也就无法预估所需的内存。这同样适用于任何自定义的 Python 对象，不论其简单还是复杂，都需要不同数量的内存。

因此，我们可以在 pandas 中使用‘***object***’ 数据类型，因为它在 *numpy* 中是可用的：

```py
pd.Series(['toto', 'titi', 'tata'])                 # --> dtype: object
pd.Series(['toto', 'titi', 'tata'], dtype="object") # --> dtype: object
pd.Series(['toto', 'titi', 'tata'], dtype="str")    # --> dtype: object
pd.Series(['toto', 'titi', np.nan], dtype="str")    # --> dtype: object
pd.Series(['toto', 'titi', pd.NA], dtype="str")     # --> dtype: object
```

此外，*pandas* 创建了一种数据类型，使得数据是字符串这一点变得明确：`***StringDtype***`，可以指定为 `***string***`，这更好，因为“显式优于隐式”，并且与 pandas 的其他生态系统接口更好（[理由在这里](https://pandas.pydata.org/docs/user_guide/text.html#text-data-types)）：

```py
pd.Series(['toto', 'titi', 'tata'], dtype="string") # --> dtype: string
pd.Series(['toto', 'titi', np.nan], dtype="string") # --> dtype: string
pd.Series(['toto', 'titi', pd.NA], dtype="string")  # --> dtype: string
```

请注意，大多数情况下，你需要挑战自己，问自己：我真的需要将这个系列存储为字符串吗？这些字符串是否只是其他数据的表示，比如数值数据或分类数据？如果是这样，你可能需要转换这些系列的数据类型。关注我的下一篇文章，了解如何做到这一点！

# **万能数据类型：分类数据类型**

我建议你试试这个方法：在 pandas 中打开你的一个数据集，一一检查列，并问自己：*这个特征是否可以存储为分类数据类型*？

我敢打赌，你会比预期的更容易接受这个问题的答案。

这种数据类型特别适用于通常存储为类似整数的数字（0，1，2 等）和/或字符串（*‘Male’/’Female’，‘Dog’/’Cat’/’Other’*）的内容。

这些数据类型在某些情况下可以显著提高速度和内存使用效率。它还再次明确告知他人（包括人类和计算机）这一特定数据表示一个类别，并应被如此处理。

```py
pd.Series([1, 2, 3, 2], dtype="category")           # dtype: category, with 3 int64 possible values [1, 2, 3]
pd.Series([1, 2, 'a', 1], dtype="category")         # dtype: category, with 4 object-like possible values [1, 2, 'a']
pd.Series(["a", "b", "c", "a"], dtype="category")   # dtype: category, with 3 object-like possible values ['a', 'b', 'c']
pd.Series(["M", "F", "M", "F"], dtype="category")   # dtype: category, with 2 object-like possible values ['M', 'F']
```

# **总结**

其他数据类型在 *pandas* 中也有实现，但我发现它们的使用频率不如上述数据类型。

**所以请记住：**

+   **好的数据类型对于你的处理至关重要，尽早审查并设置合适的数据类型会使你未来的工作变得更加轻松。同时，它也可能节省大量内存和处理复杂性**

+   **在使用 `object` 或 `string` 数据类型之前，考虑使用 `categorical` 数据类型**

+   **如果处理缺失值或 NaN，请考虑使用 pandas 数据类型如 `boolean`，而不是 numpy 的 `bool`**

+   **仅在数据复杂和/或不适合任何其他数据类型时使用 `object` 数据类型**

在下一篇文章中，我们将查看如何检查现有 Series/DataFrame 的数据类型，以及如何将它们转换为其他数据类型。

> 如果**你正在考虑加入 Medium 并获得无限访问我和其他人的文章，请使用此链接快速订阅并成为我推荐的会员：**

[](https://medium.com/@mocquin/membership?source=post_page-----20d9d32d2e42--------------------------------) [## 使用我的推荐链接加入 Medium - Yoann Mocquin

### 作为 Medium 会员，你的会员费用的一部分将用于你阅读的作者，而你将获得对每篇故事的完全访问…

[medium.com](https://medium.com/@mocquin/membership?source=post_page-----20d9d32d2e42--------------------------------)

**然后订阅以获取未来的文章通知：**

[](https://mocquin.medium.com/subscribe?source=post_page-----20d9d32d2e42--------------------------------) [## 订阅以获取我的新文章！

### 订阅以获取我发布的电子邮件！新发布将包括数据转换、先进的绘图和模拟…

[mocquin.medium.com](https://mocquin.medium.com/subscribe?source=post_page-----20d9d32d2e42--------------------------------)

**最后，查看我其他的一些文章：**

[](/300-times-faster-resolution-of-finite-difference-method-using-numpy-de28cdade4e1?source=post_page-----20d9d32d2e42--------------------------------) ## 使用 numpy 提高 300 倍的有限差分法分辨率

### 有限差分法是一种强大的技术来解决复杂问题，而 numpy 使其变得快速！

[towardsdatascience.com ## PCA/LDA/ICA：组件分析算法比较

### 回顾这些著名算法的概念和差异。

[towardsdatascience.com [## 一样本 t 检验，直观解释

### 介绍其中一种最著名的统计检验

[mocquin.medium.com](https://mocquin.medium.com/one-sample-t-test-visually-explained-415c31744e14?source=post_page-----20d9d32d2e42--------------------------------) ## 包装 numpy 数组

### 容器方法。

[towardsdatascience.com
