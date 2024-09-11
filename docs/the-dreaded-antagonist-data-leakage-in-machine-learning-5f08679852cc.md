# 恐怖的对手：机器学习中的数据泄漏

> 原文：[`towardsdatascience.com/the-dreaded-antagonist-data-leakage-in-machine-learning-5f08679852cc?source=collection_archive---------6-----------------------#2023-05-19`](https://towardsdatascience.com/the-dreaded-antagonist-data-leakage-in-machine-learning-5f08679852cc?source=collection_archive---------6-----------------------#2023-05-19)

## 可能是机器学习中最被低估的概念之一

[](https://medium.com/@andreas030503?source=post_page-----5f08679852cc--------------------------------)![Andreas Lukita](https://medium.com/@andreas030503?source=post_page-----5f08679852cc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5f08679852cc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5f08679852cc--------------------------------) [Andreas Lukita](https://medium.com/@andreas030503?source=post_page-----5f08679852cc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F955ef38ea7b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-dreaded-antagonist-data-leakage-in-machine-learning-5f08679852cc&user=Andreas+Lukita&userId=955ef38ea7b&source=post_page-955ef38ea7b----5f08679852cc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5f08679852cc--------------------------------) · 13 分钟阅读 · 2023 年 5 月 19 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5f08679852cc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-dreaded-antagonist-data-leakage-in-machine-learning-5f08679852cc&user=Andreas+Lukita&userId=955ef38ea7b&source=-----5f08679852cc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5f08679852cc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-dreaded-antagonist-data-leakage-in-machine-learning-5f08679852cc&source=-----5f08679852cc---------------------bookmark_footer-----------)

我参加了超过 5 门商务分析和机器学习课程，包括面对面课程和在线课程。令人惊讶的是，只有一门课程稍微触及了数据泄漏的话题。

![](img/27869387f3796925a3534d992a08e29b.png)

图片由 [Luis Tosta](https://unsplash.com/@luis_tosta?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

讨论数据泄漏时，在没有机器学习背景的情况下，我们通常指的是机密信息在没有适当安全措施或许可的情况下转移到第三方，导致隐私和安全的泄露¹。

尽管这个概念有些类似，但在机器学习的背景下并不完全是这样。这在机器学习领域的含义是：

> 数据泄漏发生在测试数据集的信息错误地被包含在训练数据集中。²

**结果是什么？** 训练过程中性能指标表现得不切实际地好，但当模型真正投入使用时表现很差。

**简单来说，** 模型记住了它不该访问的信息，导致训练过程中性能指标被人为地夸大。

**还是无法理解？** 好吧，想象一下。你正在为即将到来的数学考试复习。你做了很多练习题以便每天进步。然后，你发现考试题目不小心泄露到了网上。你获得了这些关键信息，并决定在这张试卷上练习（你在这个考试前不应知道的数据集上训练自己，因此，你“记住”了题目模式）。结果？你对这张试卷变得过于熟悉，表现指标异常优秀，但当你真的投入到实际情况中时……（我们还是不提了）。

**目录**

+   机器学习中的数据泄漏类型（**目标泄漏**，**训练-测试污染，数据预处理中的泄漏**）

+   **机器学习中数据泄漏的后果**

+   预防机器学习中的数据泄漏（**手动审查**，**数据清理和预处理**，**使用管道**，**适当的验证技术**）

+   **现实世界数据集示例：泰坦尼克号数据集**

+   **数据泄漏第一种示例：将目标存活作为特征包含**

+   **数据泄漏第二种示例：混淆训练和测试数据的记录**

+   **数据泄漏第三种示例：错误的数据预处理步骤**

+   **数据泄漏第四种示例：将特征舱位包含为模型中的一部分**

# 目标泄漏

关于**目标泄漏**，识别可能不会那么简单明了。想象一下：你正在构建一个模型来预测客户是否会取消他们的月度订阅服务（即流失）。乍一看，将**“客户服务电话的数量”**作为特征包含在模型中似乎并没有问题，因为你可能认为更多的客户服务电话与更高的流失概率相关。

然而，经过仔细检查发现，**“客户服务电话数量”** 是客户流失的结果，而不是一个贡献特征。已经决定流失的客户只是打电话解决未结清的问题，然后最终取消订阅。因此，在预测客户是否会流失时，这些信息是不可用的（换句话说，我们只知道已经决定流失的客户的信息）。

> **将目标变量作为特征变量的一部分，或任何直接或间接从目标变量派生的代理变量，可能导致数据泄漏。**

# 训练-测试污染和数据预处理中的泄漏

这些情况指的是对训练集和测试集应用相同的预处理步骤。例如，当我们进行特征缩放、估计缺失值和去除异常值等数据预处理步骤时，我们应该确保不会像下面所示的那样从测试数据集中“学习”³。

```py
scaler = StandardScaler()
scaler.fit(X_train)
scaler.transform(X_train)
scaler.transform(X_test)
```

在这里，我们在预处理步骤之前将数据集分成训练集和测试集，以便我们只能**拟合**训练数据集。请注意，我们**不应该**在整个数据集（包括训练集和测试集）上**拟合**，因为这会导致数据泄漏（我们的模型学会了它不应该学习的东西，换句话说，在预测时对测试数据集的信息是不知道的）。

> **在训练数据上拟合，在训练和测试数据上转换。**

# 机器学习中数据泄漏的后果

在机器学习项目中未能检测到数据泄漏的后果是巨大的——它们带来了虚假的希望。有没有遇到过训练性能极高而测试数据集表现非常差的情况？数据泄漏可能是罪魁祸首。这里的关键字是过拟合和无法泛化。这是因为模型学会了记忆噪声和无关信息，导致在面对真实测试数据集时表现不佳²。

最终结果？

> **你会做出不准确的模型评估和不可靠的预测。真是资源浪费！**

# 防止数据泄漏：手动审查

是的，我们都明白。手动审核效率低下且非常耗时。然而，花时间研究特征与目标变量之间的关系，也许是检测数据泄漏最一致的方式，因此，非常值得。当一个特征与目标变量的相关性非常高时，我们应该怀疑并进一步调查这种关系。有时，进行探索性数据分析（EDA）可能有助于揭示特征与目标之间的相关性。此外，全面的领域知识和专业技能可以帮助确定一个特征是否应该包含在模型中。记住，当有疑问时，总是要问自己这个指导性问题。

> **“这个特征是否包含在预测时不可用的信息？”**

如果上述问题的答案是“是”，那么包括该特征可能会导致数据泄漏。

# 防止数据泄漏：管道是王道

我之前参加的商业分析和机器学习课程中，没有提到构建机器学习预处理管道。最常见的做法是编写到处都是的“意大利面条”代码，而没有任何工作流程标准化。虽然这对很多人来说可能很熟悉，但这并不是最佳实践——其中一个原因是可能会将数据泄漏引入模型。我第一次接触到利用管道的想法是来自于书籍*Data Cleaning and Exploration with Machine Learning*⁴。从这本书中，我学到的关键经验包括通过将每个预处理步骤作为变量参数嵌入到`make_pipeline`方法中，并分别为数值型、分类变量和二进制变量分开处理。

简单来说，管道是一系列线性的数据预处理步骤，依次执行。管道提供了一个清晰有序的链式过程，用于自动化机器学习项目的工作流程。我们可以利用 scikit-learn 的 Pipeline 类⁵，它接受一个元组列表作为输入，每个元组代表管道中的一个步骤。每个元组的第一个元素是表示步骤名称的字符串，第二个元素是 scikit-learn 转换器或估计器对象的实例。当然，还有一个简化的方法，就是`**make_pipeline**`，它不要求我们命名估计器（我们都是懒惰的生物）。记住，估计器需要具有`**fit**`和`**transform**`方法。

**为什么要使用 Pipeline？**

> **管道自动处理所有数据处理过程。它还确保每个步骤仅在训练数据上进行拟合，从而防止数据泄漏，并确保各阶段按正确顺序进行。**

这里有一个示例：我们需要对数据集中不同类型的数据进行数据预处理，即数值型、分类型和二元特征，每种特征都有不同的步骤。我们可以利用`**make_pipeline**`将过程按顺序安排，让 Pipeline 处理所有后台工作。这将返回一个`**Pipeline**`对象，该对象具有多个属性和方法可供调用。例如，我们可以调用`**fit**(X_train, y_train)`和`**score**(X_test, y_test)`来分别拟合和评估模型。

```py
from sklearn.preprocessing import StandardScaler
from sklearn.impute import SimpleImputer
from sklearn.pipeline import make_pipeline
from feature_engine.encoding import OneHotEncoder
from preprocfunc import OutlierTrans #self-created Python class

standardtrans = make_pipeline(OutlierTrans(2), 
                              StandardScaler()
                             )

categoricaltrans = make_pipeline(SimpleImputer(strategy="most_frequent"), 
                                 OneHotEncoder(drop_last=True)
                                )

binarytrans = make_pipeline(SimpleImputer(strategy="most_frequent")
                           )

columntrans = ColumnTransformer(transformers=[
    ("standard", standardtrans, numerical_cols),
    ("categorical", categoricaltrans, ['gender']),
    ("binary", binarytrans, ['completedba'])
])

lr = LinearRegression()
pipe = make_pipeline(columntrans, KNNImputer(n_neighbors=5), lr)
```

# 防止数据泄漏：交叉验证

本节内容受到《*数据清洗与机器学习探索*》⁴一书的启发。我从书中获得的另一个观点是将 Pipeline 和交叉验证的概念结合起来。是的，它们并不是互相排斥的！训练和测试数据集的选择非常关键，如果做得不对可能会导致数据泄漏。当我们不执行交叉验证来评估模型时，我们面临着对训练数据过拟合以及在新的、未见过的数据上表现不佳的风险。我们进行的一次性训练测试拆分可能使模型学习到某个特定特征，这个特征可能对该拆分是独有的，而不具有普遍性。

**为什么要进行交叉验证？**

> **交叉验证使我们能够更精确地预测模型在全新、未测试数据上的表现。通过使用交叉验证，我们可以在多个数据子集上测试模型的有效性。**

我们可以利用**scikit-learn**的 K 折交叉验证来实现这一点。

K 折交叉验证的工作原理是什么？数据首先被分成**k**个大小相等的折叠，然后在**k-1**个折叠上训练模型，再在最后一个折叠上测试模型。在这个过程中，每个折叠都作为一次测试集，这个过程重复进行 k 次。在迭代结束时，通过对 k 次迭代结果的平均来估算模型的性能。当 k 设置为 1 时，这意味着我们回退到通常的训练测试拆分。我们在整个数据集上训练模型，并在一个独立的数据集上进行测试。

好消息是，我们可以从 Pipeline 中未完成的地方继续进行。

```py
from sklearn.model_selection import cross_validate, KFold

ttr = TransformedTargetRegressor(regressor=pipe, transformer=StandardScaler())

kf = KFold(n_splits=5, shuffle=True, random_state=0)
scores = cross_validate(ttr, 
                        X=X_train, 
                        y=y_train, 
                        cv=kf, 
                        scoring=('r2', 'neg_mean_absolute_error'), 
                        n_jobs=1) 
```

# 真实世界数据集示例：泰坦尼克号数据集

泰坦尼克号。经典。泰坦尼克号数据集是经典的机器学习问题，我们为每个乘客提供了一组特征，例如他们的年龄、性别、票务等级、登船地点，以及他们是否有家人在船上⁶。使用这些特征，目标是训练一个机器学习模型来预测乘客是否`**幸存**`。以下是一个简短的预测版本，未涉及超参数调整和特征选择。

***使用以下代码清理原始数据集。***

```py
import numpy as np
import pandas as pd
from sklearn.impute import SimpleImputer

df = pd.read_csv("../dataset/titanic/csv_result-phpMYEkMl.csv")

#Change column names, replace "?" to "NaN", change data types
def tweak_df(df):
    features = ["PassengerId", "Survived", "Pclass", "Name", "Sex", "Age", "SibSp", "Parch", "Ticket", "Fare", "Cabin", "Embarked"]
    return (df
            .rename(columns={"id": "PassengerId", "'pclass'": "Pclass", "'survived'": "Survived", "'name'": "Name", "'sex'": "Sex", "'age'": "Age", "'sibsp'": "SibSp", "'parch'": "Parch", "'ticket'": "Ticket", "'fare'": "Fare", "'cabin'": "Cabin", "'embarked'": "Embarked"})
            [features]
            .replace('?', np.nan)
            .astype({'Age': 'float', 'Fare': 'float16'})
           )

#Splitting dataset into train, validation, test, and unseen
X_train_val_test, X_unseen, y_train_val_test, y_unseen = train_test_split(tweak_df(df).drop(columns=['Survived']), tweak_df(df).Survived, test_size=0.33, random_state=42)
X_train, X_val_test, y_train, y_val_test = train_test_split(X_train_val_test, y_train_val_test, test_size=0.4, random_state=42)
X_val, X_test, y_val, y_test = train_test_split(X_val_test, y_val_test, test_size=0.5, random_state=42)

#Extensive cleanup
def tweak_titanic_cleaned(train_df):

    impute_table = (train_df
                     .assign(SibSp=lambda df_: np.where(df_.SibSp==0, 0, 1),
                             Parch=lambda df_: np.where(df_.Parch==0, 0, 1))
                     .groupby(['SibSp', 'Parch'])
                     ['Age']
                     .agg('mean')
                   )

    train_df_intermediary = (train_df
                             .assign(SibSp=lambda df_: np.where(df_.SibSp==0, 0, 1),
                                     Parch=lambda df_: np.where(df_.Parch==0, 0, 1),)
                            )

    condlist = [((train_df_intermediary.Age.isna()) & (train_df_intermediary.SibSp == 0) & (train_df_intermediary.Parch == 0)),
                ((train_df_intermediary.Age.isna()) & (train_df_intermediary.SibSp == 0) & (train_df_intermediary.Parch == 1)),
                ((train_df_intermediary.Age.isna()) & (train_df_intermediary.SibSp == 1) & (train_df_intermediary.Parch == 0)),
                ((train_df_intermediary.Age.isna()) & (train_df_intermediary.SibSp == 1) & (train_df_intermediary.Parch == 1)),]

    choicelist = [impute_table.iloc[0],
                  impute_table.iloc[1],
                  impute_table.iloc[2],
                  impute_table.iloc[3],]

    bins = [0, 12, 18, 30, 50, 100]
    labels = ['Child', 'Teenager', 'Young Adult', 'Adult', 'Senior']
    features = ["Survived", "Pclass","Sex","Fare","Embarked","AgeGroup","SibSp","Parch","IsAlone","Title"]

    return (train_df
             .assign(Embarked=lambda df_: SimpleImputer(strategy="most_frequent").fit_transform(df_.Embarked.values.reshape(-1,1)),
                     Age=lambda df_: np.select(condlist, choicelist, df_.Age),
                     IsAlone=lambda df_: np.where(df_.SibSp + df_.Parch > 0, 0, 1),
                     Title=lambda df_: df_.Name.str.extract(',(.*?)\.'))
             .assign(AgeGroup=lambda df_: pd.cut(df_.Age, bins=bins, labels=labels),
                     Title=lambda df_: df_.Title.replace(['Dr', 'Rev', 'Major', 'Col', 'Capt', 'Sir', 'Lady', 'Don', 'Jonkheer', 'Countess', 'Mme', 'Ms', 'Mlle','the Countess'], 
                                                         'Other'))
             .set_index("PassengerId")
             [features]
            )
```

让我解释一下我所实现的数据预处理步骤的理念：

1.  使用 scikit-learn 的 `SimpleImputer` 类用最频繁的条目填补 `Embarked` 列中的缺失数据。

1.  使用基于乘客是否有家人随行的条件的均值列表来填补 `Age` 列中的缺失数据。（即，如果乘客没有家人随行，则我们用其他没有家人随行的乘客的均值进行填补）

1.  创建了一个特征 `IsAlone` 用于表示乘客是否有家人随行

1.  创建了一个特征 `Title` 用于表示乘客的头衔

1.  创建了一个特征 `AgeGroup` 用于将乘客划分为 5 个不同的年龄组。

# **数据泄漏第 1 种示例：将目标** `**survived**` **作为特征**

```py
#Intentionally add target variable to list of features
X_train = tweak_titanic_cleaned(pd
 .concat([X_train, pd.DataFrame(y_train)], axis=1))

X_val = tweak_titanic_cleaned(pd
 .concat([X_val, pd.DataFrame(y_val)], axis=1))

X_test = tweak_titanic_cleaned(pd
 .concat([X_test, pd.DataFrame(y_test)], axis=1))

# Prepare the training data
X_train = pd.get_dummies(X_train, columns=["Survived", "Pclass", "Sex", "Embarked", "AgeGroup", "IsAlone", "Title"], drop_first=True)
X_val = pd.get_dummies(X_val, columns=["Survived", "Pclass", "Sex", "Embarked", "AgeGroup", "IsAlone", "Title"], drop_first=True)

# Scale numerical columns
scaler = MinMaxScaler()
num_cols = ["Fare","SibSp","Parch"]
X_train[num_cols] = scaler.fit_transform(X_train[num_cols])
X_val[num_cols] = scaler.transform(X_val[num_cols])

# Fit and evaluate Logistic Regression model
lr_model = LogisticRegression(random_state=0)
lr_model.fit(X_train, y_train)

# Make predictions on validation data
y_pred_val = lr_model.predict(X_val)

# Evaluate model on validation data
acc_val = round(accuracy_score(y_val, y_pred_val) * 100, 2)
print("Logistic Regression Model accuracy on validation data:", acc_val)
```

正如你可能预期的那样，将目标变量 `survived` 作为特征有效地使我们的模型变得毫无用处，因为它在验证数据上的准确率现在是 100.0%。进行预测没有意义。这种错误很容易发现，但不常见。

# 数据泄漏第 2 种示例：混淆训练数据和测试数据的记录

如果测试数据被意外地包含在训练集中，模型可能会在这些泄漏的信息上进行训练，从而在测试集上表现得不切实际。以下是一个***故意编造的例子***，展示了如何将部分测试集包含到训练集中。

```py
X_train = (pd
           .concat([tweak_titanic_cleaned(X_train), 
                    tweak_titanic_cleaned(X_val).iloc[:150, :]])
          )
y_train = (pd
           .concat([y_train, 
                    y_val.iloc[:150]])
          )

X_val = tweak_titanic_cleaned(X_val)

# Prepare the training data
X_train = pd.get_dummies(X_train, columns=["Pclass", "Sex", "Embarked", "AgeGroup", "IsAlone", "Title"], drop_first=True)
X_val = pd.get_dummies(X_val, columns=["Pclass", "Sex", "Embarked", "AgeGroup", "IsAlone", "Title"], drop_first=True)

# Scale numerical columns
scaler = MinMaxScaler()
num_cols = ["Fare","SibSp","Parch"]
X_train[num_cols] = scaler.fit_transform(X_train[num_cols])
X_val[num_cols] = scaler.transform(X_val[num_cols])

# Fit and evaluate Logistic Regression model
lr_model = LogisticRegression(random_state=0)
lr_model.fit(X_train, y_train)

# Make predictions on validation data
y_pred_val = lr_model.predict(X_val)

# Evaluate model on validation data
acc_val = round(accuracy_score(y_val, y_pred_val) * 100, 2)
print("Logistic Regression Model accuracy on validation data:", acc_val)
```

# 数据泄漏第 3 种示例：错误的数据预处理步骤

在这里，我们应该在预处理步骤之前将数据集分成训练集和测试集。如果我们在拆分数据集之前进行预处理步骤，我们可能会意外地从测试数据集中学习，从而导致模型性能被过度夸大。

```py
# Prepare the training data
df_leaked = pd.get_dummies(tweak_titanic_cleaned(X_train_val_test), columns=["Pclass", "Sex", "Embarked", "AgeGroup", "IsAlone", "Title"], drop_first=True)

# Scale numerical columns
scaler = MinMaxScaler()
num_cols = ["Fare","SibSp","Parch"]
df_leaked[num_cols] = scaler.fit_transform(df_leaked[num_cols])

# Split the data into train, validation, and test sets
X_train, X_val_test, y_train, y_val_test = train_test_split(df_leaked, y_train_val_test, test_size=0.4, random_state=42)
X_val, X_test, y_val, y_test = train_test_split(X_val_test, y_val_test, test_size=0.5, random_state=42)
```

正确的步骤是对训练数据集进行 `fit_transform`，对测试数据集进行 `transform`。

# 数据泄漏第 4 种示例：将舱位特征作为模型的一部分

这个数据泄漏问题不容易发现，可能需要一些领域知识来理解。主要问题是**“这个特征是否包含在预测时不可用的信息？”** 如果答案是肯定的，那么很有可能存在数据泄漏。

在这种情况下，“预测”是在乘客已经登船且事件已经发生后进行的。目标是基于事件发生后的现有数据（即乘客等级、年龄等）预测乘客是否会生存。在预测时，舱位号信息不可用，因为它仅在乘客登船后才分配给乘客。

不是每位乘客的舱号在数据集中都有记录，实际上，我们有大量缺失的数据。即使对于那些有记录的乘客，舱号也可能不准确或不完整。因此，我们在开发估算生存模型时不能将舱号作为预测因子，因为它可能并不对所有乘客都准确或可用。

想象一下，我们决定将舱号作为模型中的一个预测因子。在训练模型时，它使用舱号进行预测，并且表现得非常好。但是，当我们尝试在现实世界中使用模型时，我们可能没有所有乘客的舱号，或者我们拥有的舱号可能是错误的。这意味着即使模型在训练过程中非常准确，它在实际应用中可能表现不好。

一个可能的解决方案是删除这个特征，并在模型构建中排除它。

# 后记

防止数据泄露确实是一个具有挑战性的任务。研究特征与目标变量之间的关系是揭示这个问题的关键。下次当你看到模型的性能异常高时，也许更好地学会坐下来观察，因为不是所有事情都需要反应。

感谢阅读，祝建模愉快！

如果你从这篇文章中获得了有用的信息，请考虑在 Medium 上给我一个 [***关注***](https://medium.com/@andreas030503)。简单，每周一篇文章，保持更新，领先一步！

***你可以在 LinkedIn 上与我联系:*** [***https://www.linkedin.com/in/andreaslukita7/***](https://www.linkedin.com/in/andreaslukita7/)

***参考文献:***

1.  Forcepoint. *什么是数据泄露？数据泄露的定义、解释和探索*。 [`www.forcepoint.com/cyber-edu/data-leakage`](https://www.forcepoint.com/cyber-edu/data-leakage)

1.  Analytics Vidhya. *数据泄露及其对机器学习模型性能的影响*。 [`www.analyticsvidhya.com/blog/2021/07/data-leakage-and-its-effect-on-the-performance-of-an-ml-model/`](https://www.analyticsvidhya.com/blog/2021/07/data-leakage-and-its-effect-on-the-performance-of-an-ml-model/)

1.  JFrog. *小心数据泄露——你的机器学习模型中的潜在陷阱*。 [`jfrog.com/community/data-science/be-careful-from-data-leakage-2/`](https://jfrog.com/community/data-science/be-careful-from-data-leakage-2/)

1.  Michael Walker 的《机器学习中的数据清理与探索》：[`www.packtpub.com/product/data-cleaning-and-exploration-with-machine-learning/9781803241678`](https://www.packtpub.com/product/data-cleaning-and-exploration-with-machine-learning/9781803241678)

1.  Scikit-learn Pipeline. [`scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html#sklearn.pipeline.Pipeline`](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html#sklearn.pipeline.Pipeline)

1.  Titanic 数据集。 [`www.openml.org/search?type=data&status=active&id=40945&sort=runs`](https://www.openml.org/search?type=data&status=active&id=40945&sort=runs)

1.  [`github.com/datasciencedojo/datasets/blob/master/titanic.csv`](https://github.com/datasciencedojo/datasets/blob/master/titanic.csv)
