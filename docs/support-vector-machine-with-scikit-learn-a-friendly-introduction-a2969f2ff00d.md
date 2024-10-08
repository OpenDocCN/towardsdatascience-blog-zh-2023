# 使用 Scikit-Learn 的支持向量机：友好的介绍

> 原文：[`towardsdatascience.com/support-vector-machine-with-scikit-learn-a-friendly-introduction-a2969f2ff00d`](https://towardsdatascience.com/support-vector-machine-with-scikit-learn-a-friendly-introduction-a2969f2ff00d)

## 每个数据科学家都应该在他们的工具箱中拥有 SVM。通过实践介绍学习如何掌握这一多功能模型。

[](https://medium.com/@riccardo.andreoni?source=post_page-----a2969f2ff00d--------------------------------)![Riccardo Andreoni](https://medium.com/@riccardo.andreoni?source=post_page-----a2969f2ff00d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a2969f2ff00d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a2969f2ff00d--------------------------------) [Riccardo Andreoni](https://medium.com/@riccardo.andreoni?source=post_page-----a2969f2ff00d--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a2969f2ff00d--------------------------------) ·阅读时间 9 分钟·2023 年 10 月 11 日

--

![](img/ca089d89ca86dd60ffd60783581574e6.png)

图片来源：[unsplash.com](https://unsplash.com/photos/3DkouQeZjp4)。

在现有的机器学习模型中，有一种模型的多功能性使它成为**每个数据科学家工具箱中的必备工具**：[**支持向量机**](https://en.wikipedia.org/wiki/Support_vector_machine) ([SVM](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html))。

支持向量机（SVM）是一种强大而多功能的算法，其核心能够在高维空间中划分**最优超平面**，有效地**隔离数据集中的不同类别**。但它不仅仅止于此！它的有效性不限于**分类**任务：SVM 也非常适合**回归**和**异常值检测**任务。

有一个特点使得 SVM 方法特别有效。与 [KNN 方法](https://levelup.gitconnected.com/optical-character-recognition-with-knn-classifier-a-step-by-step-guide-b6d7b90e1122) 处理整个数据集不同，SVM 战略性地只关注位于决策边界附近的数据点。这些点被称为[*支持向量*](https://www.analyticsvidhya.com/blog/2021/10/support-vector-machinessvm-a-complete-guide-for-beginners/#:~:text=Support%20Vectors%3A%20These%20are%20the,is%20considered%20a%20good%20margin.)，这种独特想法背后的数学将在接下来的部分中简单解释。

这样，SVM 算法在**计算上是保守的**，非常适合处理中等或甚至中大型数据集的任务。

正如我在所有文章中所做的，我不仅会解释理论概念，还会提供**编码示例**，以帮助你熟悉[**Scikit-Learn**](https://scikit-learn.org/)（sklearn）[**Python**](https://www.python.org/) 库。

让我们分析支持向量机（SVM）算法，并探索机器学习技术、Python 编程和数据科学应用。

# 线性 SVM 分类

从本质上讲，SVM 分类类似于线性代数的优雅简洁。想象一个二维空间中的数据集，其中有两个需要分开的不同类别。线性 SVM 尝试用最佳的直线分隔这两个类别。

![](img/0c3a48ffc42b897621e5195ca0b7126a.png)

图片由作者提供。

在这个背景下，“最佳”是什么意思？SVM 寻找的是最优的分隔线：不仅能分隔类别，而且与每个类别的最近训练实例之间的距离尽可能远。这个距离称为**边际**。位于边际边缘的数据点是线性 SVM 分类器的关键元素，被称为**支持向量**。

![](img/69aaa61db77873bc703ca9c4bf91ea0e.png)

需要优化的决策边界的方程。

需要注意的是，分隔线仅由支持向量定义。因此，添加更多“随机”训练实例对决策边界没有影响。当更多不是支持向量的训练实例被添加到训练集中时，决策边界不会移动，这些实例会被“忽略”。这一特性是 SVM 的一个巨大优势，因为它不需要记住整个训练集。

对于一个 m 维数据集，分隔线变成了**分隔超平面**，但激发的思想依然有效。

由于 SVM 模型基于距离，因此对**特征缩放极为敏感**。因此，通常建议对特征值进行标准化。

在 Scikit-Learn 中，我们可以从 `.svm` 模块中实例化一个 `LinearSVC()` 对象：

```py
import pandas as pd
import sklearn
from sklearn import datasets

# Import the dataset
df_ = datasets.load_iris()
df = pd.DataFrame()
df['petal_length'] = df_['data'][:,2]
df['petal_width'] = df_['data'][:,3]
df['target'] = df_['target'] == 1

# Define train and test sets
X_train, X_test, y_train, y_test = sklearn.model_selection.train_test_split(df[['petal_length', 'petal_width']], 
                                        df['target'], 
                                        test_size=0.2, 
                                        random_state=42)
# Normalize data
scaler = sklearn.preprocessing.StandardScaler()
X_train = scaler.fit_transform(X_train)

# Instantiate the classifier model
svm_classifier = sklearn.svm.LinearSVC()
# Fit the model with the training data
svm_classifier.fit(X_train, y_train)
# Predict new intances classes
y_predicted = svm_classifier.predict(scaler.transform(Xtest))
# Evaluate model's accuracy
accuracy = sklearn.metrics.accuracy_score(y_test, y_predicted)
print(accuracy)
```

得到的准确率仅为 66%：这是一个不令人满意的结果。在下一节中，我们将了解为什么线性支持向量机分类器表现不佳以及如何应对这种情况。

## 软边际分类

正如你所想，拥有一个类别可以通过线性面完美分隔的数据集是一种奢侈。在现实世界中，这种情况并不会发生，数据只需要一个离群点就会**使线性 SVM 无法找到可行的决策边界**。

![](img/1430d7521724daaf61b6e79826ae6359.png)

图片由作者提供。

如上图所示，在这种情况下，没有任何直线能够分开这两个类别。

为了应对实际场景，我们需要引入软边际支持向量机分类。

与传统的线性 SVM（也称为硬间隔 SVM）不同，软间隔 SVM 不要求类别之间有严格的分隔，允许一些**灵活性元素**。通过使用软间隔 SVM，我们承认存在噪声数据点和离群点。

更实际地说，我们可以指定一个超参数 `C`，它作为正则化参数：

+   将 `C` 设置为较大值，我们强制执行更严格的间隔，从而减少误分类。

+   将 `C` 设置为较小的值，我们鼓励更宽的间隔，从而允许更多的误分类。

![](img/3ffe5e0ccf465f762fd7e6a0cbae1f06.png)

增加 C 值减少间隔违规。

有两种方法可以在 Scikit-Learn 中实现软间隔线性 SVM 分类器：

```py
# Method 1
svm_classifier_soft = sklearn.svm.LinearSVC(C=10)
svm_classifier_soft.fit(X_train, y_train)

# Method 2
svm_classifier_soft = sklearn.svm.SVC(kernel='linear', C=10)
svm_classifier_soft.fit(X_train, y_train)
```

对于相同的数据集运行这些代码行，我们获得的准确度略高于我们为硬间隔线性 SVM 获得的准确度。尽管线性 SVM 分类器在许多场景中效率高且表现良好，但大多数数据集的类别并不是线性可分的。在这些情况下，线性决策边界会产生较差的结果。

幸运的是，SVM 分类器不仅限于线性决策边界。多亏了 *核技巧*，它们可以学习甚至最复杂的分隔形状。下一节将集中于此。

# 非线性 SVM 分类

如我所预见的，许多数据集**不是线性可分的**。即使我们考虑一些灵活性，通过线性分隔线获得的结果也不是最佳的。为了解决这个问题，我们可以**添加更多特征**，例如多项式特征。添加新特征会将原始数据集转换为更高维度的数据集，在那里它可能会被一条直线或超平面分开。

考虑这个简单的例子：

![](img/24d97585963430cd93a00ccc2411bb64.png)

非线性可分的数据。图像由作者提供。

数据只有一个特征，且无法通过任何直线分开。如果我们添加一个人工特征，计算方式为

![](img/59799aed154bf5939e96dea66eefd910.png)

我们得到以下数据集，它可以被线性边界分开：

![](img/e62ed4bcb50b84bae69211782aedd6cd.png)

线性可分的数据。图像由作者提供。

然而，添加多项式特征，如我们上面所做的，对**大型和复杂数据集**来说**不可行**。结果特征数量会太高。

幸运的是，存在一种称为**核技巧**的技术，即使在高次多项式下也能实现。核技巧背后的数学并不复杂，但由于我想专注于实际实施，所以我将留给 [这份指南](https://people.eecs.berkeley.edu/~jordan/courses/281B-spring04/lectures/lec3.pdf) 来解释。

要在 Python 中实现带有多项式核的 SVM 分类器，我们只需使用 `SVC()` 类，并指定我们打算使用的核类型及其度数：

```py
# Instantiate the classifier object
SVM_classifier = sklearn.svm.SVC(kernel='poly', degree=5, C=10, coef0=1)
# Instantiate the scaler object
scaler = sklearn.preprocessing.StandardScaler()

# Normalize data
X_train = scaler.fit_transform(X_train)

# Train the classifier
SVM_classifier.fit(X_train, Y_train)
```

`coef0` 超参数作为正则化参数，允许控制模型如何受到高次多项式的影响。

找到 `degree`、`C` 和 `coef0` 超参数之间的正确平衡并不是一项简单的任务。通常建议使用网格搜索方法来找到一些可行的值，然后进行更精细的手动搜索以精确调整它们。

# 相似性特征：高斯径向基函数

多项式核函数通常适用于各种机器学习问题，然而，还有一种显著的技术常常效果更佳：相似性特征。

与其基于原始特征值添加人工多项式特征，不如在我们高维特征空间中放置多个 *标志* 并测量它们到每个数据点的距离。这些距离度量成为 **新模型的特征**。

考虑一个具有两个独立特征的训练集，如下所示：

![](img/396662f8494f869be53ae7034d77d92d.png)

图片由作者提供。

通过在特定位置添加一定数量的标志，我们可以创建额外的特征，计算为数据点到每个标志的距离。

![](img/2fa21b729b109a5ce564920018b6c59c.png)

在这个例子中，我们可以在这些精确的位置放置两个标志。SVM 分类器可以学习将接近标志的实例预测为 A 类，将远离标志的实例预测为 B 类。

可以使用任何距离度量，然而，已证明实现 [**高斯核函数**](https://pages.stat.wisc.edu/~mchung/teaching/MIA/reading/diffusion.gaussian.kernel.pdf.pdf) 是很方便的：

![](img/6d5f0622a2fd929365149e4b74c87e1e.png)

其中：

+   *x* 是包含数据点坐标的向量

+   *l* 是包含标志坐标的向量

+   *gamma* 作为正则化超参数

现在的问题是：“*我应该将标志放在哪里？*”。最常见的方法是在每个训练样本的位置放置一个标志。拥有 *m* 个训练样本意味着创建 *m* 个标志，从而产生 *m* 个新特征。

这种方法的缺点是对于大规模训练集，我们最终会得到同样大量的距离特征。

在 Scikit-Learn 中，带有高斯核的 SVM 分类器实现如下：

```py
import pandas as pd
import sklearn
from sklearn import datasets

# Import the dataset
df_ = datasets.load_iris()
df = pd.DataFrame()
df['petal_length'] = df_['data'][:,2]
df['petal_width'] = df_['data'][:,3]
df['target'] = df_['target'] == 1

# Define train and test sets
X_train, X_test, y_train, y_test = sklearn.model_selection.train_test_split(df[['petal_length', 'petal_width']], 
                                        df['target'], 
                                        test_size=0.2, 
                                        random_state=42)
# Normalize data
scaler = sklearn.preprocessing.StandardScaler()
X_train = scaler.fit_transform(X_train)

# Instantiate the classifier model
svm_gaussian_classifier = sklearn.svm.SVC(kernel='rbf', gamma=6, C=0.001)
# Fit the model with the training data
svm_gaussian_classifier.fit(X_train, y_train)
# Predict new intances classes
y_predicted = svm_gaussian_classifier.predict(scaler.transform(X_test))
# Evaluate model's accuracy
accuracy = sklearn.metrics.accuracy_score(y_test, y_predicted)
```

# 结论

在这篇文章中，我们详细介绍了这一多功能且强大的模型的理论，并了解了通过 Scikit-Learn 库在 Python 中实现它是多么简单。

我通过阐述支持向量机模型的优缺点来总结这篇入门指南。

毫无疑问，SVM 的优势在于其**高维空间中的准确性**，使其非常适合特征众多的数据，如图像或基因数据。此外，我必须强调 SVM 的**多样性**：除了分类，它还可以无缝地适应回归和异常检测任务。最后，我必须指出其**灵活性**。SVM 通过不同的核函数处理非线性关系的能力，赋予了它广泛的应用范围。

众所周知，“*机器学习没有免费的午餐*”这一箴言，我们必须承认 SVM 的缺点。首先，SVM 容易受到**噪声数据的影响**，正如我们在**线性 SVM**部分所见。**可扩展性**也是 SVM 的一个致命弱点：用 SVM 训练大规模数据集可能会计算密集且要求高。

在完成本指南时，我需要指出，尽管已经涉及了 SVM 的所有最相关的方面，但其领域远超本指南所提供的见解。因此，我建议深入研究附在本文后的资源和参考文献。

如果你喜欢这个故事，考虑关注我，以便及时了解我即将发布的项目和文章！

这是我过去的一些项目：

[使用 NetworkX 进行社交网络分析：友好入门](https://towardsdatascience.com/social-network-analysis-with-networkx-a-gentle-introduction-6123eddced3?source=post_page-----a2969f2ff00d--------------------------------)

### 了解像 Facebook 和 LinkedIn 这样的公司如何从网络中提取洞察

[集成学习与 Scikit-Learn：友好入门](https://towardsdatascience.com/ensemble-learning-with-scikit-learn-a-friendly-introduction-5dd64650de6c?source=post_page-----a2969f2ff00d--------------------------------)

### 像 XGBoost 或随机森林这样的集成学习算法是 Kaggle 竞赛中的顶尖模型之一……

[深度学习生成幻想名字：从零构建语言模型](https://towardsdatascience.com/use-deep-learning-to-generate-fantasy-character-names-build-a-language-model-from-scratch-792b13629efa?source=post_page-----a2969f2ff00d--------------------------------)

### 一个语言模型能否创造独特的幻想角色名字？让我们从零开始构建它

[深度学习生成幻想名字：从零构建语言模型](https://towardsdatascience.com/use-deep-learning-to-generate-fantasy-character-names-build-a-language-model-from-scratch-792b13629efa?source=post_page-----a2969f2ff00d--------------------------------)

# 参考文献

+   [使用 Python 进行机器学习](https://www.coursera.org/learn/machine-learning-with-python)

+   [《动手学机器学习：基于 Scikit-Learn、Keras 和 TensorFlow 的实践（第 2 版）》 — 奥雷利安·热龙](https://www.oreilly.com/library/view/hands-on-machine-learning/9781492032632/)

+   [Scikit-Learn SVM 文档](https://scikit-learn.org/stable/modules/svm.html)

+   [机器学习专项课程](https://www.coursera.org/specializations/machine-learning-introduction)
