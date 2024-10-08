# 7 种最常用的特征工程技术

> 原文：[`towardsdatascience.com/7-of-the-most-used-feature-engineering-techniques-bcc50f48474d`](https://towardsdatascience.com/7-of-the-most-used-feature-engineering-techniques-bcc50f48474d)

## 使用 Scikit-Learn、Tensorflow、Pandas 和 Scipy 进行实用特征工程

[](https://dmnkplzr.medium.com/?source=post_page-----bcc50f48474d--------------------------------)![多米尼克·波尔策](https://dmnkplzr.medium.com/?source=post_page-----bcc50f48474d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bcc50f48474d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bcc50f48474d--------------------------------) [多米尼克·波尔策](https://dmnkplzr.medium.com/?source=post_page-----bcc50f48474d--------------------------------)

·发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----bcc50f48474d--------------------------------) ·37 分钟阅读·2023 年 1 月 9 日

--

![](img/38d0b2c91b74d55357c4d376ef78449e.png)

7 种最常用的特征工程技术——图像由作者提供

# 目录

```py
Introduction 1\. Encoding
  1.1 Label Encoding using Scikit-learn
  1.2 One-Hot Encoding using Scikit-learn, Pandas and Tensorflow
2\. Feature Hashing
  2.1 Feature Hashing using Scikit-learn
3\. Binning / Bucketizing
  3.1 Bucketizing using Pandas
  3.2 Bucketizing using Tensorflow
  3.3 Bucketizing using Scikit-learn
4\. Transformer
  4.1 Log-Transformer using Numpy
  4.2 Box-Cox Function using Scipy
5\. Normalize / Standardize
  5.1 Normalize and Standardize using Scikit-learn
6\. Feature Crossing
  6.1 Feature Crossing in Polynomial Regression
  6.2 Feature Crossing and the Kernel-Trick
7\. Principal Component Analysis (PCA)
  7.1 PCA using Scikit-learn
Summary
References
```

# 介绍

特征工程描述了制定相关特征的过程，这些特征尽可能准确地描述了基础的数据科学问题，并使算法能够理解和学习模式。换句话说：

> 你提供的特征作为一种方式，将你对世界的理解和知识传达给你的模型

每个特征描述了一种信息“片段”。这些片段的总和使算法能够对目标变量得出结论——至少当你的数据集实际包含有关目标变量的信息时。

根据[福布斯杂志](https://www.forbes.com/sites/gilpress/2016/03/23/data-preparation-most-time-consuming-least-enjoyable-data-science-task-survey-says/?sh=6d63250f6f63)，数据科学家将约 80%的时间花在收集和准备相关数据上，其中数据清理和数据组织单独占据了约 60%的时间。

> 但这段时间是花得值得的。

我相信数据的质量以及数据集特征的恰当准备，对机器学习模型的成功的影响大于机器学习管道中的任何其他部分：

![](img/f7d3fc33cbf40e45965906c0e267892f.png)

*标准机器学习管道——受[*Sarkar et al., 2018*]的启发*

福布斯杂志认为的“清理和组织”通常在机器学习管道中分解为两个到三个子类别（我在上面的图像中用黄色背景突出显示了它们）：

**(1) 数据（预处理）：** 数据的初步准备——例如，平滑信号、处理异常值等。

(2) **特征工程：** 定义模型的输入特征——例如，通过使用快速傅里叶变换（FFT）将（声学）信号转换为频域，允许我们从原始信号中提取关键的信息。

(3) **特征选择：** 选择对目标变量有显著影响的特征。通过选择重要特征并减少维度，我们可以显著降低建模成本，提高模型的鲁棒性和性能。

> 我们为什么需要特征工程？

Andrew Ng 经常提倡所谓的 ***数据中心方法***，强调选择和策划数据的重要性，而不仅仅是试图收集越来越多的数据。目标是确保数据的高质量和与所解决问题的相关性，并通过数据清洗、特征工程和数据增强不断改进数据集。

> 为什么我们在拥有深度学习的情况下还需要特征工程和数据中心方法？

这种方法特别适用于收集大量数据成本高昂或其他困难的使用场景。[Brown, 2022] 例如，当数据难以访问，或存在严格的法规或其他障碍来收集和存储大量数据时：

+   ***在制造业中***，为生产设施配备全面的传感器技术并将其连接到数据库是昂贵的。因此，许多工厂尚未收集数据。即使机器能够收集和存储数据，它们也很少与中央数据湖连接。

+   ***在供应链背景下***，每家公司每天处理的订单数量有限。因此，如果我们想预测需求非常不规律的产品的需求，有时我们只有少量的数据点可用。

在这些领域，特征工程可能对提高模型性能具有最大影响。这里需要工程师和数据科学家的创造力来将数据集的质量提升到足够的水平。这个过程很少是直接的，而是实验性的和迭代的。

当人类分析数据时，他们通常会利用过去的知识和经验来帮助理解模式并做出预测。例如，如果有人尝试估算不同国家的温度，他们可能会考虑国家相对于赤道的位置，因为他们知道温度往往在靠近赤道的地方较高。

然而，机器学习模型并不像人类一样具有内在的概念和关系理解能力。它只能从提供给它的数据中学习。因此，任何人类用于解决问题的背景信息或上下文必须以数值形式显式地包含在数据集中。

![](img/62082a93409235493b84f426ecb9e547.png)

平均年温度按国家划分 [[Wikimedia](https://commons.wikimedia.org/wiki/File:Average_yearly_temperature_per_country.png)]

> 那么我们需要什么数据才能使模型比人类更聪明？

我们可以使用 Google Maps API 查找每个国家的地理坐标（经度和纬度）。此外，我们还可以收集每个国家的海拔信息和距离最近水体的距离。通过收集这些额外数据，我们希望识别并考虑可能影响每个国家温度的因素。

> 假设我们收集了一些可能影响温度的数据，接下来该怎么办？

一旦我们拥有足够描述问题特征的数据，我们仍然需要确保计算机能够理解这些数据。类别数据、日期等必须转换为数值。

在这篇文章中，我将描述一些常用的原始数据准备技术。某些技术用于将类别数据转换为机器学习模型可以理解的数值，如**编码**和**向量化**。其他技术用于处理数据分布，如**变换器**和**分箱**，这些技术可以帮助以某种方式归一化或标准化数据。还有其他技术用于通过生成新特征来减少数据集的维度，如**哈希**和**主成分分析 (PCA)**。

> 自己试试吧……

如果您想跟随本文并尝试这些方法，可以使用下面的代码库，其中包含 Jupyter Notebook 中的代码片段和使用的数据集：

[](https://github.com/polzerdo55862/7-feature-engineering-techniques?source=post_page-----bcc50f48474d--------------------------------) [## GitHub - polzerdo55862/7-feature-engineering-techniques

### 目前无法执行该操作。您在其他标签页或窗口中登录了。您已在另一个标签页或窗口中退出登录……

github.com](https://github.com/polzerdo55862/7-feature-engineering-techniques?source=post_page-----bcc50f48474d--------------------------------)

# 1\. 编码

特征编码是将类别数据转换为 ML 算法可以理解的数值的过程。有几种类型的编码，包括标签编码和独热编码。

![](img/362d8f5273c387cbfdfb59a5c97c44de.png)

标签编码和热编码 — 作者提供的图片

**标签编码**涉及为每个类别值分配一个数字值。如果类别值之间存在固有顺序，例如从 A 到 F 的等级，可以将其编码为从 1 到 5（或 6）的数字值。然而，如果类别值之间没有固有的顺序，则标签编码可能不是最佳方法。

另外，你也可以使用**一次性编码**将类别值转换为数值值。在一次性编码中，类别值的列被拆分成几列，每列对应一个独特的类别值。

例如，如果类别值是从 A 到 F 的等级：

+   我们将新增五列，每列对应一个等级。

+   数据集中每一行在与其等级对应的列中将有一个值为 1，在所有其他列中则为 0。

+   这会导致所谓的稀疏矩阵，其中大多数值为 0。

一次性编码的缺点是它可能显著增加数据集的大小，如果要编码的列包含数百或数千个独特的类别值，这可能会成为问题。

接下来，我将使用 Pandas、Scikit-learn 或 Tensorflow 对数据集应用标签和一次性编码。以下示例中，我们使用的是***人口普查收入数据集：***

> **数据集：人口普查收入** [许可证： [CC0: 公共领域](https://creativecommons.org/publicdomain/zero/1.0/)]
> 
> [`archive.ics.uci.edu/ml/datasets/census+income`](https://archive.ics.uci.edu/ml/datasets/census+income)
> 
> [`www.kaggle.com/datasets/uciml/adult-census-income`](https://www.kaggle.com/datasets/uciml/adult-census-income)
> 
> 人口普查收入数据集描述了美国个人的收入情况。它包括他们的年龄、性别、婚姻状况和其他人口统计信息，以及他们的年收入，分为两类：超过 50,000 美元或低于 50,000 美元。

对于标记示例，我们使用的是人口普查数据集中描述个人最高学历的“教育”列。它包含的信息例如个人是否完成了高中、大学、获得了研究生学位或其他形式的教育。

首先，让我们加载数据集：

```py
import pandas as pd

# load dataset - census income
census_income = pd.read_csv(r'../input/income/train.csv')
```

## 1.1 使用 Scikit-learn 进行标签编码

标签编码是将类别值转换为数值值的最简单方法。这是一个简单的过程，将每个类别分配一个数值。

![](img/225efd7f24c011459e009239f752cf0a.png)

标签编码——图片来源于作者

你可以在 Pandas、Scikit-Learn 和 Tensorflow 中找到合适的库。我使用的是 Scikit-Learn 的标签编码函数。它随机将整数分配给独特的类别值，这是最简单的编码方式：

```py
from sklearn import preprocessing

# define and fit LabelEncoder
le = preprocessing.LabelEncoder()
le.fit(census_income["education"])

# Use the trained LabelEncoder to label the education column
census_income["education_labeled"] = le.transform(census_income["education"])

display(census_income[["education", "education_labeled"]])
```

![](img/64fe09474dff60fba9a6c2906d87bf08.png)

这种编码方式可能会给一些算法带来问题，因为分配的整数不一定反映类别之间的任何固有顺序或关系。例如，在上述情况中，算法可能会假设类别**博士**（10）和**高中毕业**（11）彼此更为相似，而不是**博士**（10）和**学士**（9），并且**高中毕业**（11）比**博士**（10）“高”。

![](img/50630a5229193aaf7522bd4887c793c4.png)

标签编码结果解释 — 图像由作者提供

如果我们对数据集的特定领域或主题有一些了解，我们可以利用它确保标签编码过程反映任何固有的顺序。以我们的例子为例，我们可以尝试考虑获得不同学位的顺序来标记教育水平，例如：

博士学位是高于硕士和本科的更高学术学位。硕士学位高于本科。

![](img/ec219c83f45cca3dc8740d7fce662877.png)

要将这种手动映射应用于数据集，我们可以使用*pandas.map*函数：

```py
education_labels = {'Doctorate':5, 'Masters':4, 'Bachelors':3, 'HS-grad':2, '12th':1, '11th':0}

census_income['education_labeled_pandas']=census_income['education'].map(education_labels)

census_income[["education", "education_labeled_pandas"]]
```

![](img/0c06e86b3f7444f28c658bf97d102863.png)

> 但这如何影响模型构建过程？

**让我们用标记编码数据构建一个简单的线性回归模型：**

对于下面的图，目标变量定义为**个人收入超过 $50,000 的概率**。

+   **在左图中**，分类值（如“本科”和“博士”）被随机分配给数值。

+   **在右图中**，分配给分类值的数值反映了通常获得学位的顺序，较高的教育学位分配较高的数值。

→ **右图**显示了教育水平与收入之间的明显相关性，这可以通过一个简单的线性模型来表示。

→ **与之相反，左图**展示了教育属性与目标变量之间的关系，这需要一个更复杂的模型才能准确表示。这可能会影响结果的可解释性，增加过拟合的风险，并增加拟合模型所需的计算工作量。

![](img/3963001cfa9989a8450abc7b2c845a1a.png)

标签编码：训练有序和无序“教育”特征的线性回归模型 — 图像由作者提供

> 当值没有固有的顺序或者我们没有足够的信息进行映射时，我们该怎么办？

如果分类值完全没有固有的顺序，使用将类别转换为一组数值变量***而不引入任何偏见***的编码方法可能更好。独热编码就是一种合适的方法。

## 1.2 使用 Scikit-learn、Pandas 和 Tensorflow 进行独热编码

独热编码是一种将分类数据转换为数值数据的技术。它通过为数据集中的每个唯一类别创建一个新的二进制列，并将属于该类别的行赋值为 1，将不属于该类别的行赋值为 0 来实现这一点。

→ 这个过程通过不假设类别之间存在任何固有顺序，帮助避免对数据引入偏见。

![](img/cccce5dc70f46f366bab6dd905058035.png)

One-Hot Encoding — 图像由作者提供

One-Hot 编码的过程相当简单。我们可以自己实现它，也可以使用现有的函数之一。Scikit-learn 有**.preprocessing.OneHotEncoder()**函数，Tensorflow 有**.one_hot()**函数，Pandas 有**.get_dummies()**函数。

+   **Pandas.get_dummies()**

```py
import pandas as pd

education_one_hot_pandas = pd.get_dummies(census_income["education"], prefix='education')
education_one_hot_pandas.head(2)
```

+   **Sklearn.preprocessing.LabelBinarizer()**

```py
from sklearn import preprocessing

lb = preprocessing.LabelBinarizer()
lb.fit(census_income["education"])

education_one_hot_sklearn_binar = pd.DataFrame(lb.transform(census_income["education"]), columns=lb.classes_)
education_one_hot_sklearn_binar.head(2)
```

+   **Sklearn.preprocessing.OneHotEncoder()**

```py
from sklearn.preprocessing import OneHotEncoder

# define and fit the OneHotEncoder
ohe = OneHotEncoder()
ohe.fit(census_income[['education']])

# transform the data
education_one_hot_sklearn = pd.DataFrame(ohe.transform(census_income[["education"]]).toarray(), columns=ohe.categories_[0])
education_one_hot_sklearn.head(3)
```

> One-hot 编码的问题是它可能导致具有高维度的大型稀疏数据集。

使用 one-hot 编码将具有 10,000 个唯一值的分类特征转换为数值数据，会导致在数据集中创建 10,000 列，每列表示一个不同的类别。这在处理大型数据集时可能会成为问题，因为它会迅速消耗大量内存和计算资源。

*如果内存和计算能力有限，可能* ***需要减少数据集中的特征数量*** *以避免遇到内存或性能问题。*

> 我们如何减少维度以节省内存？

在执行此操作时，重要的是尽可能减少信息的丢失。这可以通过仔细选择保留或删除哪些特征、使用如***特征选择或维度减少***等技术来识别和删除冗余或不相关的特征来实现。[Sklearn.org][Wikipedia, 2022]

在本文中，我将描述两种可能的方式来减少数据集的维度：

1.  **特征哈希** — 见第二部分。特征哈希

1.  **主成分分析（PCA）** — 见第七部分。PCA

> 信息丢失 vs. 速度 vs. 内存

可能没有一种“完美”的方法来减少数据集的维度。一种方法可能更快，但可能导致大量信息丢失，而另一种方法则保留更多信息，但需要大量计算资源（这也可能导致内存问题）。

# 2\. 特征哈希

特征哈希主要是一种维度减少技术，通常用于自然语言处理。然而，**当我们需要对具有几百或几千个唯一类别的分类特征进行向量化时，哈希也可以很有用**。通过哈希，我们可以通过将多个唯一值分配到相同的哈希值来限制维度的增加。

***→ 因此，哈希是一种低内存的替代方法，用于 OneHot 编码和其他特征向量化方法。***

哈希通过将哈希函数应用于特征并直接使用哈希值作为索引，而不是构建哈希表并单独查找索引，来进行工作。Sklearn 中的实现基于 Weinberger [Weinberger et al., 2009]。

我们可以将其分解为 2 个步骤：

+   **第 1 步：** 首先使用哈希函数将分类值转换为哈希值。Scikit-learn 中的实现使用了 32 位变体的 MurmurHash3。

![](img/04b7b94232f8b2dc4dab62900efcbad2.png)

哈希技巧 — 作者提供的图像

```py
import sklearn 
import pandas as pd

# load data set
census_income = pd.read_csv(r'../input/income/train.csv')
education_feature = census_income.groupby(by=["education"]).count().reset_index()["education"].to_frame()

############################################################################################################
# Apply the hash function, here MurmurHash3 
############################################################################################################
def hash_function(row):
    return(sklearn.utils.murmurhash3_32(row.education))

education_feature["education_hash"] = education_feature.apply(hash_function, axis=1)
education_feature
```

![](img/f4a61827e67a6d665eb7d96acfd54f1c.png)

+   **第 2 步：** 接下来，我们通过对特征值应用***mod 函数***来降低维度。使用 mod 函数，我们计算哈希值除以***n_features***（输出向量的特征数量）后的余数。

*建议使用 2 的幂作为* ***n_features*** *参数；否则，特征将无法均匀映射到列中。*

**示例：人口普查收入数据集 → 对教育列进行编码**

人口普查收入数据集的“教育”列包含 16 个唯一值：

+   使用独热编码，将向数据集中添加 16 列，每列代表 16 个唯一值中的一个。

→ 为了降低维度，我们将***n_features***设置为 8。

这不可避免地导致“冲突”，即不同的类别值被映射到相同的哈希列。换句话说，我们将不同的值分配到同一个桶中。这类似于我们在***分箱/桶化***（参见第三部分 分箱/桶化）中所做的。在特征哈希中，我们通过将分配到同一桶的值串联起来并存储在一个列表中（称为“分离链”）来处理冲突。

![](img/bd71d9f753687bd02e1655966d778d1d.png)

特征哈希：通过计算余数作为新列索引来降低维度 — 作者提供的图像

```py
############################################################################################################
# Apply mod function
############################################################################################################
n_features = 8

def mod_function(row):
    return(abs(row.education_hash) % n_features)

education_feature["education_hash_mod"] = education_feature.apply(mod_function, axis=1)
education_feature.head(5)
```

![](img/dcafdc55149fa7ce89065cd03e81d4c8.png)

为我们执行上述步骤的函数是来自***Scikit-learn***的***HashingVectorizer***函数。

## 2.1 使用 Scikit-learn 进行特征哈希

> sklearn.feature_extraction.text.HashingVectorizer

```py
from sklearn.feature_extraction.text import HashingVectorizer

# define Feature Hashing Vectorizer
vectorizer = HashingVectorizer(n_features=8, norm=None, alternate_sign=False, ngram_range=(1,1), binary=True)

# fit the hashing vectorizer and transform the education column
X = vectorizer.fit_transform(education_feature["education"])

# transformed and raw column to data frame
df = pd.DataFrame(X.toarray()).assign(education = education_feature["education"])
display(df)
```

![](img/4b4d06da441cbc3657b00e7a18f9efc9.png)

# 3. 分箱 / 桶化

分箱用于分类数据和数值数据。顾名思义，目标是将特征的值映射到“分箱”中，并用表示该分箱的值替换原始值。

**例如**，如果我们有一个值范围从 0 到 100 的数据集，并且我们想将这些值分组为大小为 10 的分箱，我们可以为值 0-9、10-19、20-29 等创建分箱。

→ 在这种情况下，原始值将被替换为表示它们所属的分箱的值，例如 10、20、30 等。这有助于可视化和分析数据。

由于我们减少了数据集中唯一值的数量，这有助于：

+   防止过拟合

+   提高模型的鲁棒性并减轻异常值的影响

+   降低模型复杂性和训练模型所需的资源

系统化的分箱可以帮助算法更容易高效地检测潜在模式。如果我们在定义分箱之前能形成一个假设，这会特别有帮助。

分箱可用于数值和分类值，例如：

![](img/e5207be783db10ba4c34fbb715886797.png)

对类别和数值特征进行分桶 — 图片由作者提供

在以下内容中，我描述了如何使用三个示例来处理数值型和类别型属性：

1.  **数值型 —** 在构建流媒体推荐系统时如何使用分桶——这是我在[Zheng, Alice, and Amanda Casari. 2018]的书***Feature Engineering for Machine Learning***中找到的用例

1.  **数值型 —** 人口普查收入数据集：对“年龄”列应用分桶

1.  **类别型** — 供应链中的分桶：根据目标变量将国家分配到桶中

> **示例 1：流媒体推荐系统——一首歌或视频有多受欢迎？**
> 
> 如果你想开发一个推荐系统，将歌曲的相对受欢迎程度分配数值是很重要的。最显著的属性之一是点击次数，即用户听这首歌的频率。
> 
> **然而**，一个用户听一首歌 1000 次并不一定比听 50 次的用户喜欢它多 20 倍。分桶有助于防止过拟合。因此，将歌曲的点击次数分为类别可能是有益的。： [Zheng, Alice and Amanda Casari. 2018]
> 
> 点击次数 >=10: 非常受欢迎
> 
> 点击次数 >=2: 流行
> 
> 点击次数 <2: 中性，无可能的声明
> 
> **示例 2：年龄与收入的关系**

对于第二个示例，我再次使用人口普查收入数据集。目标是将个体分组到年龄桶中。

***假设 —*** 这个观点是，个人的具体年龄可能对他们的收入影响不大，而是他们所处的生活阶段。例如，一个 20 岁还在上学的人可能与一个 30 岁的年轻专业人士的收入不同。类似地，一个 60 岁仍在全职工作的人可能与一个 70 岁退休的人收入不同，而如果一个人是 40 岁或 50 岁，收入差异可能不大。

## 3.1 使用 Pandas 进行分桶

为了按年龄对数据进行分桶，我定义了三个“桶”：

+   **年轻** — 28 岁及以下

+   **中年** — 29 至 59 岁

+   **老年** — 60 岁及以上

```py
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns

# creating a dictionary
sns.set_style("whitegrid")
plt.rc('font', size=16) #controls default text size
plt.rc('axes', titlesize=16) #fontsize of the title
plt.rc('axes', labelsize=16) #fontsize of the x and y labels
plt.rc('xtick', labelsize=16) #fontsize of the x tick labels
plt.rc('ytick', labelsize=16) #fontsize of the y tick labels
plt.rc('legend', fontsize=16) #fontsize of the legend

# load dataset - census income
census_income = pd.read_csv(r'../input/income/train.csv')

# define figure
fig, (ax1, ax2) = plt.subplots(2)
fig.set_size_inches(18.5, 10.5)

# plot age histogram
age_count = census_income.groupby(by=["age"])["age"].count()
ax1.bar(age_count.index, age_count, color='black')
ax1.set_ylabel("Counts")
ax1.set_xlabel("Age")

# binning age
def age_bins(age):
    if age < 29:
        return "1 - young"
    if age < 60 and age >= 29:
        return "2 - middle-aged"
    else:
        return "3 - old-aged"

# apply trans. function
census_income["age_bins"] = census_income["age"].apply(age_bins)

# group and count all entries in the same bin
age_bins_df = census_income.groupby(by=["age_bins"])["age_bins"].count()

ax2.bar(age_bins_df.index, age_bins_df, color='grey')
ax2.set_ylabel("Counts")
ax2.set_xlabel("Age")
```

![](img/6c081c192794d26340a886fb0f72bb6a.png)

## 3.2 使用 Tensorflow 进行分桶

Tensorflow 提供了一个名为***feature columns***的模块，包含了一系列用于帮助处理原始数据的函数。特征列是组织和解释原始数据的函数，以便机器学习算法可以解读和利用这些数据进行学习。[[Google Developers, 2017](https://www.youtube.com/watch?v=d12ra3b_M-0)]

TensorFlow 的 ***feature_column.bucketized_column*** API 提供了一种对数值数据进行分桶的方法。这个 API 接受一个数值列，并根据指定的边界创建一个桶化列。输入的数值列可以是连续的或离散的。

```py
import numpy as np
import pandas as pd
import tensorflow as tf
from tensorflow import feature_column
from tensorflow.keras import layers

# load dataset - census income
census_income = pd.read_csv(r'../input/income/train.csv')
data = census_income.to_dict('list')

# A utility method to show the transformation from feature column
def demo(feature_column):
    feature_layer = layers.DenseFeatures(feature_column)
    return feature_layer(data).numpy()

age = feature_column.numeric_column("age")
age_buckets = feature_column.bucketized_column(age, boundaries=[30,50])
buckets = demo(age_buckets)

# add buckets to the data set
buckets_tensorflow = pd.DataFrame(buckets)

# define boundaries for buckets
boundary1 = 30
boundary2 = 50

# define column names for buckets
bucket1=f"age<{boundary1}"
bucket2=f"{boundary1}<age<{boundary2}"
bucket3=f"age>{boundary2}"

buckets_tensorflow_renamed = buckets_tensorflow.rename(columns = {0:bucket1,
                                                                    1:bucket2,
                                                                    2:bucket3})

buckets_tensorflow_renamed.assign(age=census_income["age"]).head(7)
```

![](img/9602f985e76ce74320fea36746c0e35e.png)

我们可以通过使用***KBinsDiscretizer***来自***Scikit-learn.***做到这一点。

## 3.3 使用 Scikit-learn 进行分桶化

```py
from sklearn.preprocessing import KBinsDiscretizer

# define bucketizer
est = KBinsDiscretizer(n_bins=3, encode='ordinal', strategy='uniform')
est.fit(census_income[["age"]])

# transform data
buckets = est.transform(census_income[["age"]])

# add buckets column to data frame
census_income_bucketized = census_income.assign(buckets=buckets)[["age", "buckets"]]
census_income_bucketized
```

![](img/dfc3604aa62472fed9992e138200887e.png)

最后一个关于分类值的示例 …

没有一种普遍适用的最佳方式来以数字形式表示分类变量。使用哪种方法，取决于具体的使用案例和你希望传达给模型的信息。不同的编码方法可以突出数据的不同方面，因此选择最适合你特定使用案例的方法非常重要。

> **示例 3: 分类值分桶化 — 例如 国家**

***供应链*** — 在评估供应商/分包商的可靠性时，地区、气候和运输类型等因素可以影响交货时间的准确性。在我们的资源规划系统中，我们通常存储供应商的国家和城市。由于一些国家在某些方面相似而在其他方面完全不同，突出与使用案例相关的方面可能是有意义的。

+   如果我们想突出国家之间的距离并将同一地区的国家分组，我们可以为大陆定义区间

+   如果我们更关心价格或可能的收入，人均 GDP 可能更为重要 → 在这里我们可以将“高成本”国家如美国、德国和日本归入一个区间

+   如果我们想突出一般的天气条件，我们可以将北方国家如加拿大和挪威放在同一个区间

![](img/c08c19c1cdaed647e03a0f1fead8c953.png)

示例: 使用不同属性对国家进行分桶化 — 图片由作者提供

# 4\. Transformer

转换技术是用来改变数据集形式或分布的方法。一些常见的转换技术，如**对数**或**Box-Cox 函数**，用于将不符合正态分布的数据转换为更对称、更接近钟形曲线的数据。这些技术在数据需要满足特定统计模型或技术要求的假设时可能会很有用。使用的具体转换方法可能取决于期望的结果和数据的特性。

![](img/d1b5984dfc4621d1777a4a90f640fef6.png)

Transformers: 转换特征值分布 — 图片由作者提供

## 4.1 使用 Numpy 的 Log-Transformer

对数转换器用于通过对每个值应用对数函数来改变数据的尺度。这种转换通常用于将高度偏斜的数据转换为更接近正态分布的数据。

数据偏斜绝不是不寻常的现象，在许多情况下，数据自然或人为地呈现偏斜，例如：

+   词汇在语言中的使用频率遵循一种被称为***齐普夫定律***的模式

+   人类感知不同刺激的方式遵循一种由***史蒂文斯幂函数***描述的模式

数据的分布不对称，可能存在大量比大多数数据值更大或更小的长尾值。

![](img/e825b6f5da69b99e13d63c58fe9b0ce5.png)

正常分布与幂律分布 — 图片由作者提供（灵感来自 [[Geeksforgeeks, 2022]](https://www.geeksforgeeks.org/box-cox-transformation-using-python)）

某些类型的算法可能难以处理这种类型的数据，可能会产生较少的准确或可靠结果。应用幂变换，如 Box-Cox 变换或对数变换，可以帮助调整数据的尺度，使其更适合这些算法处理。[[Geeksforgeeks, 2020]](https://www.geeksforgeeks.org/box-cox-transformation-using-python)

让我们看看变换器如何影响实际数据。因此，我使用了世界人口、在线新闻流行度和波士顿住房数据集：

> **数据集 1: 世界人口** [许可证: [CC BY 3.0 IGO](https://population.un.org/wpp/Download/Standard/CSV/)]
> 
> [`population.un.org/wpp/Download/Standard/CSV/`](https://population.un.org/wpp/Download/Standard/CSV/)
> 
> 数据集包含了全球每个国家的人口数据。[联合国, 2022]
> 
> **数据集 2: 在线新闻流行度** [许可证: [CC0: 公共领域](https://creativecommons.org/publicdomain/zero/1.0/)]
> 
> [`archive.ics.uci.edu/ml/datasets/online+news+popularity`](https://archive.ics.uci.edu/ml/datasets/online+news+popularity)
> 
> 该数据集总结了 Mashable 在两年内发布的文章的异质特征。目标是预测社交网络中的分享数量（流行度）。
> 
> **数据集 3: 波士顿住房数据集** [许可证: [CC0: 公共领域](https://creativecommons.org/publicdomain/zero/1.0/)]
> 
> [`www.cs.toronto.edu/~delve/data/boston/bostonDetail.html`](https://www.cs.toronto.edu/~delve/data/boston/bostonDetail.html)
> 
> 波士顿住房数据集由美国人口普查局在 1970 年代末和 1980 年代初收集。它包含波士顿（马萨诸塞州）地区房屋的中位数价值的信息。该数据集包括 13 个属性，例如每栋房屋的平均房间数、到就业中心的平均距离和财产税率，以及该地区房屋的中位数价值。

以下函数帮助绘制数据集变换前后的分布图：

```py
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
from scipy import stats
import seaborn as sns

def plot_transformer(chosen_dataset, chosen_transformation, chosen_feature = None, box_cox_lambda = 0):
    plt.rcParams['font.size'] = '16'
    sns.set_style("darkgrid", {"axes.facecolor": ".9"})

    ##################################################################################################
    # choose dataset
    ##################################################################################################
    if chosen_dataset == "Online News Popularity":
        df = pd.read_csv("../input/uci-online-news-popularity-data-set/OnlineNewsPopularity.csv")
        X_feature = " n_tokens_content"
    elif chosen_dataset == "World Population":
        df = pd.read_csv("../input/world-population-dataset/WPP2022_TotalPopulationBySex.csv")
        df = df[df["Time"]==2020]
        df["Area"] = df["PopTotal"] / df["PopDensity"]
        X_feature = "Area"
    elif chosen_dataset == "Housing Data":
        df = pd.read_csv("../input/housing-data-set/HousingData.csv")
        X_feature = "AGE"

    # in case you want to plot the histogram for another feature
    if chosen_feature != None:
        X_feature = chosen_feature

    ##################################################################################################
    # choose type of transformation
    ##################################################################################################
    #chosen_transformation = "box-cox" #"log", "box-cox"

    if chosen_transformation == "log":
        def transform_feature(df, X_feature):
            # We add 1 to number_of_words to make sure we don't have a null value in the column to be transformed (0-> -inf) 
            return (np.log10(1+ df[[X_feature]]))

    elif chosen_transformation == "box-cox":
        def transform_feature(df, X_feature):
            return stats.boxcox(df[[X_feature]]+1, lmbda=box_cox_lambda)
            #return stats.boxcox(df[X_feature]+1)

    ##################################################################################################
    # plot histogram to chosen dataset and X_feature
    ##################################################################################################
    # figure settings
    fig, (ax1, ax2) = plt.subplots(2)
    fig.set_size_inches(18.5, 10.5)

    ax1.set_title(chosen_dataset)
    ax1.hist(df[[X_feature]], 100, facecolor='black', ec="white")
    ax1.set_ylabel(f"Count {X_feature}")
    ax1.set_xlabel(f"{X_feature}")

    ax2.hist(transform_feature(df, X_feature), 100, facecolor='black', ec="white")
    ax2.set_ylabel(f"Count {X_feature}")
    ax2.set_xlabel(f"{chosen_transformation} transformed {X_feature}")

    fig.show()
```

在接下来的内容中，我不仅测试数据变换后的效果，还对这种变换如何影响模型构建过程感兴趣。我使用多项式回归来建立简单的回归模型，并比较模型的性能。模型的输入数据仅为 2 维：

+   对于***Only News Popularity*** ***数据集***，我们尝试建立一个模型，该模型根据在线文章的“n_tokens”预测“分享数”——也就是说，我们试图建立一个模型，根据文章的标记数量/长度预测文章的受欢迎程度。

+   对于***World Population data set***，我们建立了一个简单的模型，该模型仅根据一个输入特征——国家的面积——来预测人口。

> 示例 1: 世界人口数据集 — 国家面积

```py
plot_transformer(chosen_dataset = "World Population", chosen_transformation = "log")
```

![](img/e33c839444d0c3ba79435b1c5f4a2ca3.png)

在图表中，你可以看到原始分布，虽然偏向左侧，但已经被转化为一个更对称的分布。

我们也试试在线新闻受欢迎度数据集：

> 示例 2: 在线新闻受欢迎度数据集 — 根据文章长度的文章受欢迎程度

```py
plot_transformer(chosen_dataset = "Online News Popularity", chosen_transformation = "log")
```

![](img/2608171d4f1356e0d2d2b4155e01f4ce.png)

这个例子更好地展示了效果。偏斜的数据被转化为几乎“完美”的正态分布数据。

但是这会对模型构建过程产生积极影响吗？为此，我为原始数据和转化数据创建了多项式模型，并比较它们的表现：

```py
import numpy as np
import matplotlib.pyplot as plt
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import PolynomialFeatures
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import cross_val_score
import plotly.graph_objects as go
import pandas as pd
from scipy import stats
import plotly.express as px
import seaborn as sns

def plot_cross_val_score_comparison(chosen_dataset):
    scores_raw = []
    scores_transformed = []
    degrees = []

    if chosen_dataset == "Online News Popularity":
        df = pd.read_csv("../input/uci-online-news-popularity-data-set/OnlineNewsPopularity.csv")
        X = df[[" n_tokens_content"]]
        y = df[" shares"]
    elif chosen_dataset == "World Population":
        df = pd.read_csv("../input/world-population-dataset/WPP2022_TotalPopulationBySex.csv")
        df = df[df["Time"]==2020]
        df["Area"] = df["PopTotal"] / df["PopDensity"]
        X = df[["Area"]]
        y = df["PopTotal"]

    for i in range(1,10):
        degree = i
        polynomial_features = PolynomialFeatures(degree=degree, include_bias=False)

        linear_regression = LinearRegression()
        pipeline = Pipeline(
            [
                ("polynomial_features", polynomial_features),
                ("linear_regression", linear_regression),
            ]
        )

        # Evaluate the models using crossvalidation
        scores = cross_val_score(
            pipeline, X, y, scoring="neg_mean_absolute_error", cv=5
        )

        scores_raw.append(scores.mean())
        degrees.append(i)

        #########################################################################
        # Fit model with transformed data
        #########################################################################
        def transform_feature(X):
            # We add 1 to number_of_words to make sure we don't have a null value in the column to be transformed (0-> -inf) 
            #return (np.log10(1+ X))
            return stats.boxcox(X+1, lmbda=0)

        X_trans = transform_feature(X)

        pipeline.fit(X_trans, y)

        # Evaluate the models using crossvalidation
        scores = cross_val_score(
            pipeline, X_trans, y, scoring="neg_mean_absolute_error", cv=5
        )

        scores_transformed.append(scores.mean())

    plot_df = pd.DataFrame(degrees, columns=["degrees"])
    plot_df = plot_df.assign(scores_raw = np.abs(scores_raw))
    plot_df = plot_df.assign(scores_transformed = np.abs(scores_transformed))

    fig = go.Figure()
    fig.add_scatter(x=plot_df["degrees"], y=plot_df["scores_transformed"], name="Scores Transformed", line=dict(color="#0000ff"))

    # Only thing I figured is - I could do this 
    #fig.add_scatter(x=plot_df['degrees'], y=plot_df['scores_transformed'], title='Scores Raw')  
    fig.add_scatter(x=plot_df['degrees'], y=plot_df['scores_raw'], name='Scores Raw')

    # write scores for raw and transformed data in one data frame and find degree that shows the mininmal error score
    scores_df = pd.DataFrame(np.array([degrees,scores_raw, scores_transformed]).transpose(), columns=["degrees", "scores_raw", "scores_transformed"]).abs()
    scores_df_merged = scores_df[["degrees","scores_raw"]].rename(columns={"scores_raw":"scores"}).append(scores_df[["degrees", "scores_transformed"]].rename(columns={"scores_transformed":"scores"}))
    degree_best_performance = scores_df_merged[scores_df_merged["scores"]==scores_df_merged["scores"].min()]["degrees"]

    # plot a vertical line
    fig.add_vline(x=int(degree_best_performance), line_width=3, line_dash="dash", line_color="red", name="Best Performance")

    # update plot layout
    fig.update_layout(
        font=dict(
            family="Arial",
            size=18,  # Set the font size here
            color="Black"
        ),
        xaxis_title="Degree",
        yaxis_title="Mean Absolute Error",
        showlegend=True,
        width=1200,
        height=400,
    )

    fig['data'][0]['line']['color']="grey"
    fig['data'][1]['line']['color']="black"

    fig.show()
    return degree_best_performance
```

我意识到这个简单的二维示例并不是一个具有代表性的例子，因为仅凭一个属性无法准确预测目标变量。然而，我们来看看转化是否有任何改变。

首先，让我们尝试使用在线新闻受欢迎度数据集：

```py
degree_best_performance_online_news = plot_cross_val_score_comparison(chosen_dataset="Online News Popularity")
```

![](img/471a56e4397fe0306a9a499eec7319af.png)

在原始和转化数据上训练的多项式回归模型的表现 — 作者提供的图片

在转化数据上训练的模型表现稍好一些。尽管从 3213 到 3202 的绝对误差差异不是特别大，但这确实表明数据转化对训练过程有积极的影响。

通过绘制转化后的数据并建立模型，我们可以看到，数据被向右移动了。这给了模型更多的“空间”来拟合数据：

```py
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import cross_val_score
import matplotlib.pyplot as plt
import pandas as pd
from sklearn.preprocessing import PolynomialFeatures
from sklearn.linear_model import LinearRegression
from sklearn.pipeline import Pipeline
import numpy as np

def plot_polynomial_regression_model(chosen_dataset, chosen_transformation, degree_best_performance):
    # fig settings
    plt.rcParams.update({'font.size': 16})
    fig, ax = plt.subplots(nrows=1, ncols=2, figsize=(15, 6))

    if chosen_dataset == "Online News Popularity":
        df = pd.read_csv("../input/uci-online-news-popularity-data-set/OnlineNewsPopularity.csv")
        y_column = " shares"
        X_column = " n_tokens_content"
        X = df[[X_column]]
        y = df[y_column]
    elif chosen_dataset == "World Population":
        df = pd.read_csv("../input/world-population-dataset/WPP2022_TotalPopulationBySex.csv")
        df = df[df["Time"]==2020]
        df["Area"] = df["PopTotal"] / df["PopDensity"]
        y_column = "PopTotal"
        X_column = "Area"
        X = df[[X_column]]
        y = df[y_column]

    #########################################################################
    # Define model
    #########################################################################
    # degree_best_performance was calculated in the cell above
    degree = int(degree_best_performance)
    polynomial_features = PolynomialFeatures(degree=degree, include_bias=False)

    linear_regression = LinearRegression()
    pipeline = Pipeline(
        [
            ("polynomial_features", polynomial_features),
            ("linear_regression", linear_regression),
        ]
    )

    #########################################################################
    # Fit model
    #########################################################################
    pipeline.fit(X, y)

    ######################################################################################
    # Fit model and plot raw features
    ######################################################################################
    reg = pipeline.fit(X, y)
    X_pred = np.linspace(min(X.iloc[:,0]), max(X.iloc[:,0]),1000).reshape(-1,1)
    X_pred = pd.DataFrame(X_pred)
    X_pred.columns = [X.columns[0]]
    y_pred_1 = reg.predict(X_pred)

    # plot model and transformed data    
    ax[0].scatter(X, y, color='#f3d23aff')
    ax[0].plot(X_pred, y_pred_1, color='black')
    ax[0].set_xlabel(f"{X_column}")
    ax[0].set_ylabel(f"{y_column}")
    ax[0].set_title(f"Raw features / Poly. Regression (degree={degree})", pad=20)

    #########################################################################
    # Fit model with transformed data
    #########################################################################
    def transform_feature(X, chosen_transformation):
        # We add 1 to number_of_words to make sure we don't have a null value in the column to be transformed (0-> -inf) 
        if chosen_transformation == "log":
            return (np.log10(1+ X))
        if chosen_transformation == "box-cox":
            return stats.boxcox(X+1, lmbda=0)

    X_trans = transform_feature(X, chosen_transformation)

    # fit model with transformed data
    reg = pipeline.fit(X_trans, y)

    # define X_pred
    X_pred = np.linspace(min(X_trans.iloc[:,0]), max(X_trans.iloc[:,0]),1000).reshape(-1,1)
    X_pred = pd.DataFrame(X_pred)
    X_pred.columns = [X.columns[0]]

    # predict
    y_pred_2 = reg.predict(X_pred)

    # plot model and transformed data    
    ax[1].scatter(X_trans, y, color='#f3d23aff')
    ax[1].plot(X_pred, y_pred_2, color='black')
    ax[1].set_xlabel(f"{chosen_transformation} Transformed {X_column}")
    ax[1].set_ylabel(f"{y_column}")
    # calc cross val score and add to title
    ax[1].set_title(f"Transformed features  / Poly. Regression (degree={degree})", pad=20)

    fig.suptitle(chosen_dataset)
    fig.tight_layout()
```

我们使用刚刚定义的函数来绘制表现最佳的多项式模型：

```py
plot_polynomial_regression_model(chosen_dataset = "Online News Popularity", chosen_transformation = "log", degree_best_performance=degree_best_performance_online_news)
```

![](img/852001150fc00eacc49a30643730de5d.png)

多项式回归模型: 在线新闻的分享数 — 作者提供的图片

我们来尝试一下世界人口数据集：

```py
degree_best_performance_world_pop = plot_cross_val_score_comparison(chosen_dataset="World Population")
```

![](img/e265431b970a725198c0f03cfeb3d858.png)

在原始和转化数据上训练的多项式回归模型的表现 — 作者提供的图片

我们可以看到，模型在原始数据集和转化数据集之间的表现差异很大。虽然基于原始数据的模型在低多项式度数时表现显著更好，但在构建高多项式度数的模型时，训练在转化数据上的模型表现更佳。

![](img/a83d1f0022920430427c62acdcf8c410.png)

多项式回归模型: 国家人口 — 作者提供的图片

仅供比较，这是针对原始数据表现最佳的线性模型：

![](img/3c447df2c60debdc6908c69847bbde25.png)

多项式回归模型：国家人口 — 作者图片

## 4.2 使用 Scipy 的 Box-Cox 函数

另一个非常流行的变换函数是 Box-Cox 函数，它属于幂变换的范畴。

幂变换器是一类参数化变换，旨在将任何数据分布转换为正态分布的数据集，并最小化方差和偏斜。 [[Sklearn.org](https://scikit-learn/stable/modules/feature_extraction.html)]

为了实现这种灵活的变换，Box-Cox 函数定义如下：

![](img/32b9f01572d327e74be6f3a372322a38.png)

要使用 Box-Cox 函数，我们必须确定变换参数 lambda。如果未手动指定 lambda，变换器将通过最大化似然函数来寻找最佳 lambda。 [[Rdocumentation.org]](https://www.rdocumentation.org/packages/EnvStats/versions/2.7.0/topics/boxcox) 可以通过函数[***scipy.stats.boxcox***](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.boxcox.html)找到合适的实现。

> 注意：Box-Cox 函数必须用于大于零的数据。

Box-Cox 变换比对数变换更灵活，因为它可以产生各种变换形状，包括线性、二次和指数，这些可能更好地适应数据。此外，Box-Cox 变换可以用于变换既正偏又负偏的数据，而对数变换只能用于正偏数据。

> lambda 参数的用途是什么？

参数 lambda（λ）可以调整，以根据被分析数据的特征来定制变换。lambda 值为 0 会产生对数变换，而 0 到 1 之间的值会产生越来越“*强*”的变换。如果 lambda 设置为 1，则函数不会对数据进行任何变换。

为了展示 Box-Cox 函数可以转换不仅仅是右偏的数据，我还使用了来自波士顿住房数据集的***AGE***（"建于 1940 年前的自有住房比例"）属性。

```py
import seaborn as sns

plot_transformer(chosen_dataset = "Housing Data", chosen_transformation = "box-cox", chosen_feature = None, box_cox_lambda = 2.5)
```

![](img/a63ad55536aced3c7f841022309b7a15.png)

# 5. 归一化 / 标准化

归一化和标准化是机器学习中的重要预处理步骤。它们可以帮助算法更快地收敛，甚至提高模型的准确性。

## **5.1 使用 Scikit-learn 进行归一化和标准化**

+   ***Scikit-learn 的 MinMaxScaler***将特征缩放到给定范围内。它通过将每个特征缩放到 0 到 1 之间的给定范围来变换特征。

+   ***Scikit-learn 的 StandardScaler***将数据转换为均值为 0 和标准差为 1。

```py
# data normalization with sklearn
from sklearn.preprocessing import MinMaxScaler, StandardScaler
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns

plt.rcParams['font.size'] = '16'
sns.set_style("darkgrid", {"axes.facecolor": ".9"})

# load data set
census_income = pd.read_csv(r'../input/income/train.csv')

X = census_income[["age"]]

# fit scaler and transform data
X_norm = MinMaxScaler().fit_transform(X)
X_scaled = StandardScaler().fit_transform(X)

# plots
fig, (ax1, ax2, ax3) = plt.subplots(3)
fig.suptitle('Normalizing')
fig.set_size_inches(18.5, 10.5)

# subplot 1 - raw data
ax1.hist(X, 25, facecolor='black', ec="white")
ax1.set_xlabel("Age")
ax1.set_ylabel("Frequency")

# subplot 2 - normalizer
ax2.hist(X_norm, 25, facecolor='black', ec="white")
ax2.set_xlabel("Normalized Age")
ax2.set_ylabel("Frequency")

# subplot 3 - standard scaler
ax3.hist(X_scaled, 25, facecolor='black', ec="white")
ax3.set_xlabel("Normalized Age")
ax3.set_ylabel("Frequency")

fig.tight_layout()
```

![](img/3f45f8cfcf055c799166626b0d8b0cb3.png)

## 6. 特征交叉

特征交叉是将数据集中的多个特征连接起来以创建新特征的过程。这可以包括结合其他来源的数据，以强调现有的相关性。

创造力没有限制。首先，通过正确组合属性使已知相关性显现是有意义的，例如：

> ***示例 1：预测公寓的价格***
> 
> 假设我们有一系列要出售的公寓，并且有技术蓝图，从而可以获得这些公寓的尺寸。
> 
> 为了确定公寓的合理价格，具体的尺寸可能不像总面积那样重要：一个房间是 6 米 * 7 米还是 5.25 米 * 8 米，并不像房间有 42 平方米那样重要。如果我只知道尺寸 a 和 b，那么将面积作为一个特征添加进去是有意义的。

![](img/6e69dbf95fa81c057d24d85a30aa5dd3.png)

预测公寓的价格 — 图片来自作者

> ***示例 2：利用技术知识 — 预测铣床的能耗***
> 
> 几年前，我在研究一个回归模型，该模型可以预测铣床的能耗。
> 
> 在为特定领域实施解决方案时，查看现有文献总是值得的。由于人们对计算切割机功率需求的兴趣已有 50 年，我们可以利用这 50 年的研究成果。
> 
> 即使现有的公式仅适用于粗略计算，我们仍可以利用关于属性和目标变量（能耗）之间已识别的相关性的知识。在下图中，你可以看到一些功率需求与变量之间的已知关系。
> 
> → 通过将这些已知关系作为特征突出显示，我们可以使模型更容易处理。例如，我们可以交叉**切割宽度 b**和**切割深度 h**（以计算**截面积 A**），并将其定义为一个新特征。这可能有助于训练过程，特别是当我们使用较简单的模型时。

![](img/f6cd19012611bdb5c0db0aba575b8f7a.png)

铣床功率需求的计算 — 图片来自作者

但我们不仅仅用它来准备数据集，一些算法将特征交叉作为其工作原理的基本部分。

> 一些机器学习方法和算法默认已经使用特征交叉

两种著名的机器学习技术使用特征交叉是**多项式回归**和**核技巧（例如，与支持向量回归结合使用）**。

## 6.1 多项式回归中的特征交叉

Scikit-learn 不包含多项式回归的功能，我们从以下步骤创建一个管道：

1.  特征变换步骤和

1.  一个线性回归模型构建步骤。

通过组合和指数化特征，我们生成了若干新的特征，这使得可以表示输出变量之间的非线性关系。

假设我们要构建一个具有二维输入矩阵 X 和目标变量 y 的回归模型。除非特别定义，否则特征转换函数（*sklearn.PolynomialFeatures*）将矩阵转换如下：

![](img/ffcb080c2e0517e1830e59c81a95e586.png)

你可以看到，新矩阵包含四个新列。属性不仅被增强，还相互交叉：

```py
import numpy as np
from sklearn.preprocessing import PolynomialFeatures

# Raw input features
X = np.arange(6).reshape(3, 2)
print("Raw input matrix:")
display(X)

# Crossed features
poly = PolynomialFeatures(2)
print("Transformed feature matrix:")
poly.fit_transform(X)
```

![](img/01f404e97b6412c878d93dcacc03bca7.png)

这样我们可以建模任何可想象的关系，我们只需选择正确的多项式度数。

以下示例展示了一个多项式回归模型（多项式度数=5），它基于属性 LSTAT（*低社会经济状态的人口百分比*）预测波士顿自有住房的中位数价值。

```py
from sklearn.preprocessing import PolynomialFeatures
from sklearn.linear_model import LinearRegression
from sklearn.pipeline import Pipeline

import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd

plt.rcParams['font.size'] = '16'
sns.set_style("darkgrid", {"axes.facecolor": ".9"})

# load data set: Boston Housing
boston_housing_df = pd.read_csv("../input/housing-data-set/HousingData.csv")
boston_housing_df = boston_housing_df.dropna()
boston_housing_df = boston_housing_df.sort_values(by=["LSTAT"])

# define x and target variable y
X = boston_housing_df[["LSTAT"]]
y = boston_housing_df["MEDV"]

# fit model and create predictions
degree = 5
polynomial_features = PolynomialFeatures(degree=degree)
linear_regression = LinearRegression()
pipeline = Pipeline(
    [
        ("polynomial_features", polynomial_features),
        ("linear_regression", linear_regression),
    ]
)
pipeline.fit(X, y)

y_pred = pipeline.predict(X)

# train linear model
regr = LinearRegression()
regr.fit(X,y)

# figure settings
fig, (ax1) = plt.subplots(1)
fig.set_size_inches(18.5, 6)

ax1.set_title("Boston Housing: Median value of owner-occupied homes")
ax1.scatter(X, y, c="black")
ax1.plot(X, y_pred, c="red", linewidth=4, label=f"Polynomial Model (degree={degree})")
ax1.set_ylabel(f"MEDV")
ax1.set_xlabel(f"LSTAT")
ax1.legend()

fig.show()
```

![](img/b0eea2dbbc4d0ac2867891fd248326c3.png)

## 6.2 特征交叉和核技巧

另一个特征交叉已经被固有使用的例子是核技巧，例如在支持向量回归中。通过核函数，我们将数据集转换到更高维空间，使得可以轻松分离不同类别。

对于以下示例，我生成了一个二维数据集，这在二维空间中相当难以分离，至少对于线性模型而言。

```py
from sklearn.datasets import make_circles
from sklearn import preprocessing
import plotly_express as px
import matplotlib
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd

sns.set_style("darkgrid", {"axes.facecolor": ".9"})
plt.rcParams['font.size'] = '30'

# generate a data set
X, y = make_circles(n_samples=1_000, factor=0.3, noise=0.05, random_state=0)
X = preprocessing.scale(X)

X=X[500:]
y=y[500:]

# define target value, here: binary classification, class 1 or class 2
y=np.where(y==0,"class 1","class 2")

# define x1 and x2 of a 2-dimensional data set
x1 = X[:,0]
x2 = X[:,1]

import plotly_express as px
# define the kernel function
kernel = x1*x2 + x1**2 + x2**2

circle_df = pd.DataFrame(X).rename(columns={0:"x1", 1:"x2"})
circle_df = circle_df.assign(y=y)

color_discrete_map = {circle_df.y.unique()[0]: "black", circle_df.y.unique()[1]: "#f3d23a"}
px.scatter(circle_df, x="x1", y="x2", color="y", color_discrete_map = color_discrete_map, width=1000, height=800)

# plot the data set together with the kernel value in a 3-dimensional space
color_discrete_map = {circle_df.y.unique()[0]: "black", circle_df.y.unique()[1]: "grey"}
fig = px.scatter(circle_df, x="x1", y="x2", color="y", color_discrete_map = color_discrete_map, width=1000, height=600)
fig.update_layout(
    font=dict(
        family="Arial",
        size=24,  # Set the font size here
        color="Black"
    ),
    showlegend=True,
    width=1200,
    height=800,
)
```

![](img/72c71499c8e7e1c959185d9c066becc8.png)

通过使用核技巧，我们生成了第三维度。在这个例子中，核函数定义为：

![](img/59851e3599b47311b9334454a60aa526.png)

```py
import plotly_express as px

# define the kernel function
kernel = x1*x2 + x1**2 + x2**2

kernel_df = pd.DataFrame(X).rename(columns={0:"x1", 1:"x2"})
kernel_df = kernel_df.assign(kernel=kernel)
kernel_df = kernel_df.assign(y=y)

# plot the data set together with the kernel value in a 3-dimensional space
color_discrete_map = {kernel_df.y.unique()[0]: "black", kernel_df.y.unique()[1]: "grey"}  
px.scatter_3d(kernel_df, x="x1", y="x2", z="kernel", color="y", width=1000, height=600)
```

![](img/c03994234d41a1deb44a510445014f3a.png)

核技巧：二维数据集转移到三维空间 — 图片由作者提供

三维空间允许使用简单的线性分类器分离数据。

## 7\. 主成分分析（PCA）

主成分分析，或 PCA，通过创建一组从原始特征中衍生的新特征来减少数据集的维度。因此，类似于哈希，我们减少了数据集的复杂性，从而减少了计算工作量。

此外，它还可以帮助我们可视化数据集。具有多个维度的数据集可以在较低维度的表示中进行可视化，同时尽可能保留原始变异。

我将省略如何在本节中深入解释，因为已经有一些很好的资料，例如：

+   [Josh Starmer: StatQuest: 主成分分析（PCA），逐步讲解](https://www.youtube.com/watch?v=FgakZw6K1QQ)

+   [Sebastian Raschka: 主成分分析的 3 个简单步骤](https://sebastianraschka.com/Articles/2015_pca_in_3_steps.html#pca-and-dimensionality-reduction)

简要总结，以下是四个最重要的步骤：

1.  **标准化数据：** 从每个特征中减去均值，并将特征缩放到单位方差。

1.  **计算标准化数据的协方差矩阵：** 该矩阵将包含所有特征之间的成对协方差。

1.  **计算协方差矩阵的特征向量和特征值：** 特征向量确定新特征空间的方向，并表示主成分，特征值确定它们的大小。

1.  **选择主成分：** 选择前几个主成分（通常是特征值最高的那些），并用它们将数据投影到低维空间。你可以根据想要保留的方差量或希望将数据减少到的维度数来选择保留的成分数。

![](img/498c6f04306488412362e4af1c101fd3.png)

PCA 如何工作 — 图片由作者提供（灵感来源于 [Zheng, Alice, and Amanda Casari, 2018]）

我将向你展示如何使用 PCA 创建新的特征，使用***鸢尾花数据集***。

> **数据集：鸢尾花数据集** [许可证：[CC0: 公共领域](https://creativecommons.org/publicdomain/zero/1.0/)]
> 
> [`archive.ics.uci.edu/ml/datasets/iris`](https://archive.ics.uci.edu/ml/datasets/iris)
> 
> [`www.kaggle.com/datasets/uciml/iris`](https://www.kaggle.com/datasets/uciml/iris)
> 
> 鸢尾花数据集包含关于三种鸢尾花（Iris setosa、Iris virginica 和 Iris versicolor）的信息。它包括花的萼片和花瓣的长度和宽度（以厘米为单位），并包含 150 个观察值。

所以首先，让我们加载数据集，查看四个维度（萼片长度、萼片宽度、花瓣长度、花瓣宽度）中的数据分布。

```py
from matplotlib import pyplot as plt
import numpy as np
import seaborn as sns
import math

import pandas as pd

# Load the Iris dataset
iris_data = pd.read_csv(r'../input/iris-data/Iris.csv')
iris_data.dropna(how="all", inplace=True) # drops the empty line at file-end

sns.set_style("whitegrid")
colors = ["black", "#f3d23aff", "grey"]
plt.figure(figsize=(10, 8))

with sns.axes_style("darkgrid"):
    for cnt, column in enumerate(iris_data.columns[1:5]):
        plt.subplot(2, 2, cnt+1)
        for species_cnt, species in enumerate(iris_data.Species.unique()):
            plt.hist(iris_data[iris_data.Species == species][column], label=species, color=colors[species_cnt])

        plt.xlabel(column)
    plt.legend(loc='upper right', fancybox=True, fontsize=8)

    plt.tight_layout()
    plt.show()
```

![](img/565f40c23d8c80f5f7a3b8bc8ada9a38.png)

从分布中你可以看出，基于花瓣宽度和花瓣长度可以相对较好地区分各个类别。在其他两个维度中，分布强烈重叠。

这表明花瓣宽度和花瓣长度对我们的模型可能比其他两个维度更为重要。因此，如果我只能使用两个维度作为模型特征，我会选择这两个。如果我们绘制这两个维度，我们得到如下图：

![](img/03dfabd4f17e8b30358855f01ecb3207.png)

还不错，我们看到通过选择“最重要”的属性，我们可以减少维度并尽可能少地丢失信息。

PCA 更进一步，它对数据进行中心化和缩放，并将数据投影到新生成的维度上。这样我们不再局限于现有维度。类似于我们在空间中旋转数据集的 3D 表示，直到找到一种使我们尽可能容易分离类别的方向：

```py
import plotly_express as px

color_discrete_map = {iris_data.Species.unique()[0]: "black", iris_data.Species.unique()[1]: "#f3d23a", iris_data.Species.unique()[2]:"grey"}

px.scatter_3d(iris_data, x="PetalLengthCm", y="PetalWidthCm", z="SepalLengthCm", size="SepalWidthCm", color="Species", color_discrete_map = color_discrete_map, width=1000, height=800)
```

![](img/309a65a0181f700207d68818f23459f4.png)

鸢尾花数据集在 3D 图中的可视化 — 图片由作者提供

总的来说，PCA 会变换、缩放和旋转数据，直到找到一个新的维度集（主成分），该维度集捕捉到原始数据中最重要的信息。

适用于数据集的 PCA 函数可以在 Scikit-learn 中找到，***sklearn.decomposition.PCA***。

## 7.1 使用 Scikit-learn 的 PCA

```py
import numpy as np
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA
import matplotlib
import plotly.graph_objects as go

# Load the Iris dataset
iris_data = pd.read_csv(r'../input/iris-data/Iris.csv')
iris_data.dropna(how="all", inplace=True) # drops the empty lines

# define X
X = iris_data[["PetalLengthCm", "PetalWidthCm", "SepalLengthCm", "SepalWidthCm"]]

# fit PCA and transform X
pca = PCA(n_components=2).fit(X)
X_transform = pca.transform(X)
iris_data_trans = pd.DataFrame(X_transform).assign(Species = iris_data.Species).rename(columns={0:"PCA1", 1:"PCA2"})

# plot 2d plot
color_discrete_map = {iris_data.Species.unique()[0]: "black", iris_data.Species.unique()[1]: "#f3d23a", iris_data.Species.unique()[2]:"grey"}
fig = px.scatter(iris_data_trans, x="PCA1", y="PCA2", color="Species", color_discrete_map = color_discrete_map)

fig.update_layout(
    font=dict(
        family="Arial",
        size=18,  # Set the font size here
        color="Black"
    ),
    showlegend=True,
    width=1200,
    height=800,
)
```

如果你将前两个主成分（PC1 和 PC2）的图与花瓣宽度和花瓣长度的二维图进行比较，你会发现数据集中最重要的信息得到了保留。此外，数据经过了中心化和标准化。

![](img/96848acddd816403c43292e542b8bd07.png)

转换后的鸢尾花数据的 PCA1 和 PCA2 — 作者提供的图像

## 总结

文章中描述的技术可以应用于任何类型的数据，但它们不能替代特定领域的专用方法。

一个很好的例子是声学领域。假设你对声学和信号处理一无所知…。

+   你会如何处理空气中的声音信号？你会提取哪些特征？

你是否曾考虑将信号分解为单独的频谱成分以了解信号的组成？可能不会。幸运的是，早在你之前，就有人提出了同样的问题，并定义了一些合适的变换方法，比如快速傅里叶变换（FFT）。

> 站在巨人的肩膀上

即使技术已经很先进，这也不意味着我们不能利用几十年前的洞见。我们可以从这些过去的经验中学习，并用它们来提高模型构建过程的效率。因此，如果你想创建有效解决实际问题的模型，花些时间了解数据来源领域总是一个好主意。

> **继续阅读…**

如果你喜欢阅读，并想继续了解机器学习的概念、算法和应用，你可以在这里找到我相关的文章列表：

![多米尼克·波尔泽](img/7c3581b08252e49c58af78556cd5f714.png)

[多米尼克·波尔泽](https://dmnkplzr.medium.com/?source=post_page-----bcc50f48474d--------------------------------)

## 机器学习：概念、技术和应用

[查看列表](https://dmnkplzr.medium.com/list/machine-learning-concepts-techniques-and-applications-e83997daeb7a?source=post_page-----bcc50f48474d--------------------------------)5 个故事![](img/ede57ccab552f553442f43dc51c077c9.png)![](img/71c8b7eee050a29f4fa563afb35f5da5.png)![](img/2ec6d4edef1c2c0c8d5a301825a6b833.png)

如果你对注册 Medium 以获取所有故事的无限访问权限感兴趣，可以通过使用我的 [推荐链接](https://dmnkplzr.medium.com/membership)来支持我。这将为我带来一小部分佣金，而不会给你带来额外费用。

感谢阅读！

# 参考文献

Box, G E P 和 D R Cox. “变换分析。” : 43.

Brown, Sara. 2022 年。 “为什么是‘数据中心人工智能’的时机。” *MIT Sloan*。 [`mitsloan.mit.edu/ideas-made-to-matter/why-its-time-data-centric-artificial-intelligence`](https://mitsloan.mit.edu/ideas-made-to-matter/why-its-time-data-centric-artificial-intelligence)（2022 年 10 月 30 日）。

[educative.io](http://educative.io)。 “特征选择和特征工程——机器学习系统设计。” *Educative: Interactive Courses for Software Developers*。 [`www.educative.io/courses/machine-learning-system-design/q2AwDN4nZ73`](https://www.educative.io/courses/machine-learning-system-design/q2AwDN4nZ73)（2022 年 11 月 25 日）。

*H2O 的特征工程——Dmitry Larko，高级数据科学家，* [*H2O.Ai*](http://H2O.Ai)。 2017 年。 [`www.youtube.com/watch?v=irkV4sYExX4`](https://www.youtube.com/watch?v=irkV4sYExX4)（2022 年 9 月 5 日）。

[featureranking.com](http://featureranking.com)。 “案例研究：预测收入状况。” *[*[*www.featureranking.com*](http://www.featureranking.com)*]([`www.featureranking.com`](http://www.featureranking.com)/) [`www.featureranking.com/tutorials/machine-learning-tutorials/case-study-predicting-income-status/`](https://www.featureranking.com/tutorials/machine-learning-tutorials/case-study-predicting-income-status/)（2022 年 11 月 26 日）。

Geeksforgeeks. 2020 年。 “Python | Box-Cox 变换。” *GeeksforGeeks*。 [`www.geeksforgeeks.org/box-cox-transformation-using-python/`](https://www.geeksforgeeks.org/box-cox-transformation-using-python/)（2022 年 12 月 25 日）。

Google Developers. 2017 年。 “使用 TensorFlow 的特征工程入门——机器学习配方第 9 期。” [`www.youtube.com/watch?v=d12ra3b_M-0`](https://www.youtube.com/watch?v=d12ra3b_M-0)（2022 年 9 月 5 日）。

Google Developers. “介绍 TensorFlow 特征列。” [`developers.googleblog.com/2017/11/introducing-tensorflow-feature-columns.html`](https://developers.googleblog.com/2017/11/introducing-tensorflow-feature-columns.html)（2022 年 9 月 21 日）。

[Heavy.ai](http://Heavy.ai)。 2022 年。 “什么是特征工程？定义和常见问题 | [HEAVY.AI](http://HEAVY.AI)。” [`www.heavy.ai/technical-glossary/feature-engineering`](https://www.heavy.ai/technical-glossary/feature-engineering)（2022 年 9 月 19 日）。

[heavy.ai](http://heavy.ai)。 “特征工程。” [`www.heavy.ai/technical-glossary/feature-engineering`](https://www.heavy.ai/technical-glossary/feature-engineering)。

Koehrsten, Will. “手动特征工程入门。” [`kaggle.com/code/willkoehrsen/introduction-to-manual-feature-engineering`](https://kaggle.com/code/willkoehrsen/introduction-to-manual-feature-engineering)（2022 年 9 月 5 日）。

Kousar, Summer. “网络安全薪资的 EDA。” [`kaggle.com/code/summerakousar/eda-on-cyber-security-salary`](https://kaggle.com/code/summerakousar/eda-on-cyber-security-salary)（2022 年 9 月 7 日）。

Moody, John. 1988\. “在多分辨率层次中的快速学习。” 收录于 *神经信息处理系统的进展*，Morgan-Kaufmann. [`proceedings.neurips.cc/paper/1988/hash/82161242827b703e6acf9c726942a1e4-Abstract.html`](https://proceedings.neurips.cc/paper/1988/hash/82161242827b703e6acf9c726942a1e4-Abstract.html)（2022 年 11 月 28 日）。

Poon, Wing. 2022\. “机器学习中的特征工程（1/3）。” *Medium*. `towardsdatascience.com/feature-engineering-for-machine-learning-a80d3cdfede6`（2022 年 9 月 19 日）。

Poon, Wing. “机器学习中的特征工程（2/3）第二部分：特征生成。”：10。

[pydata.org](http://pydata.org). “Pandas.Get_dummies — Pandas 1.5.2 文档。” [`pandas.pydata.org/docs/reference/api/pandas.get_dummies.html`](https://pandas.pydata.org/docs/reference/api/pandas.get_dummies.html)（2022 年 11 月 25 日）。

Raschka, Sebastian. “主成分分析。” *Sebastian Raschka, PhD*. [`sebastianraschka.com/Articles/2015_pca_in_3_steps.html`](https://sebastianraschka.com/Articles/2015_pca_in_3_steps.html)（2022 年 12 月 17 日）。

[Rdocumentation.org](http://Rdocumentation.org). “Boxcox 函数 — RDocumentation。” [`www.rdocumentation.org/packages/EnvStats/versions/2.7.0/topics/boxcox`](https://www.rdocumentation.org/packages/EnvStats/versions/2.7.0/topics/boxcox)（2022 年 12 月 25 日）。

Sarkar, Dipanjan. 2019a. “分类数据。” *Medium*. `towardsdatascience.com/understanding-feature-engineering-part-2-categorical-data-f54324193e63`（2022 年 9 月 19 日）。

Sarkar, Dipanjan. 2019b. “连续数值数据。” *Medium*. `towardsdatascience.com/understanding-feature-engineering-part-1-continuous-numeric-data-da4e47099a7b`（2022 年 9 月 19 日）。

Sarkar, Dipanjan, Raghav Bali, 和 Tushar Sharma. 2018\. *用 Python 实践机器学习*. 加州伯克利：Apress. [`link.springer.com/10.1007/978-1-4842-3207-1`](http://link.springer.com/10.1007/978-1-4842-3207-1)（2022 年 11 月 25 日）。

Siddhartha. 2020\. “TensorFlow 特征列（Tf.Feature_column）演示。” *ML Book*. [`medium.com/ml-book/demonstration-of-tensorflow-feature-columns-tf-feature-column-3bfcca4ca5c4`](https://medium.com/ml-book/demonstration-of-tensorflow-feature-columns-tf-feature-column-3bfcca4ca5c4)（2022 年 9 月 21 日）。

[Sklearn.org](http://Sklearn.org). “6.2\. 特征提取。” *scikit-learn*. [`scikit-learn/stable/modules/feature_extraction.html`](https://scikit-learn/stable/modules/feature_extraction.html)（2022 年 11 月 28 日）。

[spark.apache.org](http://spark.apache.org). “提取、转换和选择特征 — Spark 3.3.1 文档。” [`spark.apache.org/docs/latest/ml-features`](https://spark.apache.org/docs/latest/ml-features)（2022 年 12 月 1 日）。

[Tensorflow.org](http://Tensorflow.org). “Tf.One_hot | TensorFlow v2.11.0。” *TensorFlow*. [`www.tensorflow.org/api_docs/python/tf/one_hot`](https://www.tensorflow.org/api_docs/python/tf/one_hot)（2022 年 11 月 25 日）。

联合国经济社会事务部，人口司（2022）。*2022 年世界人口展望，在线版*。

[Valohai.com](http://Valohai.com). 2022\. “什么是机器学习管道？” [`valohai.com/machine-learning-pipeline/`](https://valohai.com/machine-learning-pipeline/)（2022 年 9 月 19 日）。

沃塔瓦，亚当。2022\. “跟上数据 #105。” *Medium*. [`adamvotava.medium.com/keeping-up-with-data-105-6a2a8a41f4b6`](https://adamvotava.medium.com/keeping-up-with-data-105-6a2a8a41f4b6)（2022 年 10 月 21 日）。

温伯格，基利安等。2009\. “大规模多任务学习的特征哈希。” 收录于 *第 26 届国际机器学习大会论文集 — ICML ’09*，蒙特利尔，魁北克，加拿大：ACM 出版社，1–8\. [`portal.acm.org/citation.cfm?doid=1553374.1553516`](http://portal.acm.org/citation.cfm?doid=1553374.1553516)（2022 年 12 月 25 日）。

*什么使特征变得优秀？ — 机器学习食谱 #3*。2016\. [`www.youtube.com/watch?v=N9fDIAflCMY`](https://www.youtube.com/watch?v=N9fDIAflCMY)（2022 年 9 月 5 日）。

维基媒体。“各国年均温度.png。” [`commons.wikimedia.org/wiki/File:Average_yearly_temperature_per_country.png`](https://commons.wikimedia.org/wiki/File:Average_yearly_temperature_per_country.png)（2022 年 12 月 25 日）。

维基百科。2022\. “特征哈希。” *维基百科*. [`en.wikipedia.org/w/index.php?title=Feature_hashing&oldid=1114513799`](https://en.wikipedia.org/w/index.php?title=Feature_hashing&oldid=1114513799)（2022 年 11 月 28 日）。

郑阿丽斯和阿曼达·卡萨里。2018\. *机器学习特征工程*。
