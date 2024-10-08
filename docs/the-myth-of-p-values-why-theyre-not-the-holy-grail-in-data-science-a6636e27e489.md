# p 值的神话：为什么它们不是数据科学中的圣杯

> 原文：[`towardsdatascience.com/the-myth-of-p-values-why-theyre-not-the-holy-grail-in-data-science-a6636e27e489`](https://towardsdatascience.com/the-myth-of-p-values-why-theyre-not-the-holy-grail-in-data-science-a6636e27e489)

## p 值谬论：我们为什么需要停止把它们当作真理

[](https://federicotrotta.medium.com/?source=post_page-----a6636e27e489--------------------------------)![Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----a6636e27e489--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a6636e27e489--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a6636e27e489--------------------------------) [费德里科·特罗塔](https://federicotrotta.medium.com/?source=post_page-----a6636e27e489--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----a6636e27e489--------------------------------) ·15 分钟阅读·2023 年 4 月 27 日

--

![](img/2a838e36bf1a6ea412b22d9f2d369324.png)

图片由[Gerd Altmann](https://pixabay.com/it/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2620923)提供，来自[Pixabay](https://pixabay.com/it//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2620923)

在[近期文章](https://medium.com/towards-data-science/mastering-linear-regression-the-definitive-guide-for-aspiring-data-scientists-7abd37fcb9ed)中，我提到我们在数据科学中并不总是需要计算`p-values`，展示了我们在不计算它们的情况下找到了解决 ML 问题的好模型。

现在是时候写一篇澄清这一声明的文章了。我知道：这将是一篇有争议的文章，我非常希望在评论中听到你的意见。

如果你是一个有志成为数据科学家的读者，这里你将找到`p-values`的定义以及对其使用的有趣讨论。而如果你是专家，我知道你了解`p-values`的定义，希望你会喜欢对其使用的讨论。

我的观点并不是为了说服任何人：这些讨论在科学界非常严肃，正如你将在文章中看到的。我只是想对`p-values`这一话题进行一些探讨，建议我们作为数据科学家是否必须使用`p-value`，或者是否可以跳过它们，使用其他方式测试我们的 ML 模型（我们将看到具体方法）。

```py
**Table of Contents:**

Definingthe p-values
Why we could avoid p-values as Data Scientists
What to use instead of p-values as Data Scientists
An example in Python
```

# 定义 p 值

引用自[维基百科](https://en.wikipedia.org/wiki/P-value#:~:text=In%20null%2Dhypothesis%20significance%20testing,the%20null%20hypothesis%20is%20correct.):

> 在零假设显著性检验中，`p-value`是在假设零假设正确的情况下，获得至少和实际观察结果一样极端的测试结果的概率。

现在，我不认识你，但我的数学学习方式一直是：将其转化为实践。 [作为工程师，我喜欢实用数学](https://medium.com/towards-data-science/please-no-more-flipping-coins-in-data-science-f21e893d4fbd)因为它帮助我更好地理解它。但我必须告诉你真相：我从未从根本上理解过这个定义。

对我来说好消息是……我不是世界上唯一一个这样的人！实际上，我鼓励你阅读这篇（非常简短的）文章 [在这里](https://fivethirtyeight.com/features/not-even-scientists-can-easily-explain-p-values/)，在那篇文章中我们可以读到：

> 明确一点，我在 METRICS 谈话过的每个人都能告诉我`p-value`的技术定义[…]但几乎没有人能够将其翻译成容易理解的东西。
> 
> 这不是他们的错，METRICS 的共同主任斯蒂文·古德曼说。即使在“整个职业生涯”中一直在思考`p-values`，他说他可以告诉我定义，“但我不能告诉你它的意义，几乎没有人能。”

好吧，让我告诉你，当我读到这些话时，我既感到惊讶又感到高兴。惊讶是因为这意味着“科学世界”似乎在处理某种未知的东西。高兴是因为……我不是错的！

如你所知，我们认为 0.05 的`p-value`是可以接受的。还有另一个问题：为什么是这个数字？乔治·科布，霍利奥克学院数学与统计学名誉教授，在美国统计学会（ASA）讨论论坛上提出了这些问题（[这是完整的文章](https://www.tandfonline.com/doi/full/10.1080/00031305.2016.1154108)）：

> 问：为什么这么多大学和研究生院教 *p* = 0.05？
> 
> 答：因为这仍然是科学界和期刊编辑使用的标准。
> 
> 问：为什么这么多人仍然使用 *p* = 0.05？
> 
> 答：因为这是他们在大学或研究生院时学到的。

这就像是：鸡和蛋哪个先出现？

这种方法的问题在于，p 值不会告诉我们关于结果的任何信息。这导致了[ASA 声明](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5187603/#:~:text=The%20threshold%20of%20statistical%20significance,a%20P%2Dvalue%20of%200.03.)：

> 原则 5：`p-value`，或统计显著性，并不衡量效应的大小或结果的重要性。
> 
> 常用的统计显著性阈值是 0.05 的`p-value`。这是惯例和随意的。它并不传达效应大小的任何有意义的证据。`p-value`为 0.01 并不意味着效应大小比`p-value`为 0.03 时更大。

这是 ASA 在`p-values`方面提出的六个原则中的第五个。我们来阅读所有这些原则（[这是 ASA 的完整文章](https://www.amstat.org/asa/files/pdfs/p-valuestatement.pdf)）：

> `p-values` 可以指示数据与指定统计模型的不兼容程度。
> 
> `p-values` 并不衡量所研究的假设为真的概率，或者数据是仅由随机机会产生的概率。
> 
> 科学结论和商业或政策决策不应仅仅基于`p-value`是否超过特定阈值。
> 
> 适当的推断需要完全的报告和透明度。
> 
> `p-value` 或统计显著性，并不衡量效应的大小或结果的重要性。
> 
> `p-value` 本身并不能提供关于模型或假设的良好证据度量。

因此，根据我们到目前为止阅读的领域专家的观点，我们可以说`p-values`——尤其是它们的使用——在某种程度上是一个有争议的话题。因此，我将继续讨论为什么在我看来，作为数据科学家我们可以避免使用`p-values`，除非在某些情况下。然后，我们将用 Python 实现一个示例。

# 为什么我们作为数据科学家可以避免使用 p 值

我将告诉你一些可能看起来不好的事情，但请让我解释。数据科学是一个非常庞大的领域，许多专业人士在不同的行业中担任数据科学家的角色，他们来自不同的背景。因此，在我看来，根据我的经验，有三种类型的数据科学家：

+   “统计学家”。

+   “软件工程师”。

+   “其他所有人”。

“统计学家”是那些毕业于统计学、数学、物理学或相关领域的人，他们拥有大量的高级统计学知识，并在应用大量高级统计学。例如，研究病毒等生物学领域的人。这些数据科学家通常是研究人员。

“软件工程师”通常是那些学习过计算机工程（或在该领域精通）的人，因为他们喜欢数据，所以转而成为数据科学家。他们特别专注于编程，并学习统计学以成为数据科学家。当然，他们也可以从事研究领域的工作，但我发现他们通常在某些特定领域工作，为业务提供价值（而不是专注于研究）。

“所有其他人”指的是那些通过不同背景成为数据科学家的其他人。这些人拥有丰富的领域知识，并学习编程和统计学，以将数据科学应用于他们的领域。例如，作为一个兼职数据科学家，我毕业于机械工程专业，并学习了统计学和编程，将数据科学应用于我的知识领域（主要是工业领域）。所以，我可以归类为“所有其他人”类别。当然，这些人也可以从事研究，但我发现大多数人从事的是为企业创造价值的工作。

这就是说，我们都知道统计学在数据科学中很重要，但根据我的经验，如果我们在为企业创造价值，我们通常只需掌握基础知识（在某些情况下多了解一点）即可，因为我们不是研究人员。

请不要误解我的意思：我并不是贬低研究的价值与商业的价值。即便我也通过一些合作在某种程度上贡献于研究：所以我可以看到贡献于研究和商业之间的区别。这只是我的观点。

所以，这是一个有争议的观点：`p-values` 在我们进行高级统计分析时，尤其是使用科学方法时，更具意义。特别是在贡献于研究时。我意思是，科学方法——正如其名称所示——是一种由一些简单步骤组成的方法：

+   确定要解决的问题。

+   提出解决问题的假设。

+   解决实际问题。

很简单，不是吗？其实并非如此：这主要取决于领域和问题。例如，我们都知道科学中存在一些未解的问题或仅部分解决的问题。例如，Navier-Stokes 方程目前只能在一些精确且特定的假设下求解。一般情况目前尚未解决。实际上，准确地说：目前没有人证明 Navier-Stokes 方程的解存在，并且如果存在，它是否唯一（如果我的记忆没有错误的话：如果我错了，请告诉我）。

但关键在于：在科学中，我们总是提出假设。记住我们的代数和分析课程：当我们的教授教我们定理时，他们总是从一个问题开始，展示假设，并在假设的基础上解决问题。

现在，关于 `p-values` 的问题，如上所述，是（引用）：

> 通常使用的统计显著性阈值是 `p-value` 为 0.05。这个值是传统的，也是任意的。它没有传达任何关于效应大小的有意义的证据。

所以，根据这一声明，我的经验告诉我，这种方法更适用于与研究和高级数学相关的数据科学应用。例如，在生物学、医学、粒子物理学等领域。

在这些领域，事实上，科学方法对于得出结论非常重要，因为我们需要遵循一些标准化的方法，以便我们的工作可以与他人的工作进行比较。通常因为结论被发表在科学论文中，我们需要严格的方法来展示我们的结果。

但让我们说实话：今天的数据科学远不止于研究。这就是为什么我告诉你，在所有需要将数学和统计应用于“日常”案例的情况中，我们通常需要一些基础统计知识。而且，当我们得出结果时，重要的是结果本身，因为它能满足一些业务需求，这些需求不需要科学验证。

所以，这里是我们在这些情况下可以做的事情，而不是使用`p 值`。

# 数据科学家可以使用什么来代替 p 值

数据科学是一个今天甚至被用来通过数据为企业提供一些见解的领域，通常得益于机器学习进行预测。

为此，我们可以利用“点查”方法，例如，这是一种非常简单的方法，其工作方式如下：

+   我们需要通过机器学习进行预测来解决业务问题。

+   我们获取数据，清理数据，并将其划分为训练集和测试集。

+   我们在训练集上测试 3-5 个机器学习模型，并从中选择表现最好的 2-3 个模型。

+   我们调整在训练集上表现良好的 2-3 个机器学习模型的超参数，并在测试集上测试它们。

+   我们决定哪一个 2-3 个模型表现最好。

因此，根据这种方法，我们可以通过选择一组机器学习模型来解决业务问题。这样，如果我们使用`p 值`，我们应该计算一个机器学习模型的`p 值`，测试它，找到结果，查看不满意的结果，然后更换机器学习模型。

因此，考虑到`p 值`不能说明结果的任何信息，为什么要在“非严格科学环境”中使用它们呢？对我来说，这没有太大意义……

此外，我们通常在数据上使用多个机器学习模型的事实与所谓的**“无免费午餐定理”**（NFL）有关。该定理声明（引自[维基百科](https://en.wikipedia.org/wiki/No_free_lunch_theorem)）：

> 任何两个优化算法在其性能平均到所有可能问题时都是等效的

这意味着一个算法在特定的机器学习问题上表现非常好，但这并不意味着它在另一个问题上表现也会同样好，因为相同的假设可能不适用。

换句话说，一些算法在某些类型的问题上可能表现优于其他算法，但每个算法由于其先验假设都有优缺点。

所以，这里有一个基本概念要记住：我们不能仅仅因为在先前类似问题上取得了良好表现，就将一个机器学习模型应用到一个新问题上，因为这意味着会对解决方案产生偏见。

这就是为什么我们在选择最佳模型之前，最好在同一数据集上测试不同的 ML 模型。我们不能仅仅因为它在类似问题上有效就使用一个模型，因为这样会偏向我们的解决方案。

# Python 中的一个示例

所以，让我们在 Python 中做一个示例。为了学习的目的，我们创建了一个由三次多项式生成的简单数据集（多么大的剧透！）。然后，我们对数据集进行缩放并将其拆分为训练集和测试集：

```py
import numpy as np
import pandas as pd

import matplotlib.pyplot as plt
import seaborn as sns

from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LinearRegression
from sklearn.metrics import r2_score
from sklearn.preprocessing import PolynomialFeatures

# Set random seed for reproducibility
np.random.seed(42)

# Generate some random data
n_samples = 1000
X = np.random.uniform(-10, 10, size=(n_samples, 5))
y = 3*X[:,0]**3 + 2*X[:,1]**2 + 5*X[:,2] + np.random.normal(0, 20, n_samples)

# Create a pandas DataFrame
df = pd.DataFrame(X, columns=['x1', 'x2', 'x3', 'x4', 'x5'])
df['y'] = y

# Fit a 3-degree polynomial regression model
X = df[['x1', 'x2', 'x3', 'x4', 'x5']]
y = df['y']
poly_reg = LinearRegression()
poly_reg.fit(X ** 3 + X ** 2 + X, y)

# Calculate the R-squared value
y_pred = poly_reg.predict(X ** 3 + X ** 2 + X)
r2 = r2_score(y, y_pred)

# Split into train and test sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

现在，我们将把线性回归模型拟合到`X_train`数据集，并使用`statsmodels.api`包计算统计摘要，其中包括`p-values`：

```py
import statsmodels.api as sm

# Add a constant term to X_train
X_train_with_const = sm.add_constant(X_train)

# Fit linear regression model with statsmodels
lin_reg_sm = sm.OLS(y_train, X_train_with_const).fit()

# Print summary of the model
print(lin_reg_sm.summary())
```

![](img/6ed9294774b49d432093bcceeee9c0a2.png)

线性回归模型的统计摘要（注意：由于

ML 的随机性。图像由 Federico Trotta 提供。

使用普通最小二乘法，我们可以获得关于模型的大量统计信息，包括 R²和调整后的 R²。如果你不知道我们所说的是什么，可以阅读我的文章：

[](/mastering-the-art-of-regression-analysis-5-key-metrics-every-data-scientist-should-know-1e2a8a2936f5?source=post_page-----a6636e27e489--------------------------------) ## 掌握回归分析的艺术：每个数据科学家应该知道的 5 个关键指标

### 这是关于回归分析中使用的指标的所有知识的权威指南

[towardsdatascience.com

此外，上述摘要给出了在某个置信区间（0.025, 0.975，即 2.5%、97.5%）下计算的`p-values`。特别是，我们对“t”和“P>|t|”列感兴趣。

让我解释一下：

+   首先，我们可以看到这些值是针对每个特征（`x1, x2, x3, x4, x5`）计算的，包括常数值（`const`）。

+   “t”列衡量估计系数在零假设下距离其假设值多少个标准差。t 统计量的绝对值越大，相应系数在预测目标变量时就越显著。

+   “P>|t|”列表示每个系数的`p-value`。我们说低`p-value`表示相应的系数在统计上显著，而高`p-value`则表明其不显著。理想情况下，我们知道统计显著性的标准是：`p-values` < 0.05。

所以，在这种线性回归模型的情况下，许多特征看起来很显著。很好！所以，让我们看看一个二次和一个三次多项式，看看会发生什么。

为此，我们需要将我们的领域转换为二次和三次模型，并拟合线性回归模型。然后，我们这样打印摘要：

```py
#creating quadratic and cubic features
quadratic = PolynomialFeatures(degree=2)
cubic = PolynomialFeatures(degree=3)

X_quad = quadratic.fit_transform(X_train) #quadratical transformation
X_cubic = cubic.fit_transform(X_train) #cubical tranformation

# Add a constant term to X_train
X_train_with_const_quad = sm.add_constant(X_quad)
X_train_with_const_cub = sm.add_constant(X_cubic)

# Fit polynomial regression models with statsmodels
quad_reg_sm = sm.OLS(y_train, X_train_with_const_quad).fit()
cub_reg_sm = sm.OLS(y_train, X_train_with_const_cub).fit()

# Print summary of the model
print(quad_reg_sm.summary())
print(cub_reg_sm.summary())
```

![](img/b9dd805e90ce27745bcbb840dc760eba.png)

二次回归模型的统计摘要（注意：由于

机器学习的随机性质）。图像由费德里科·特罗塔提供。

![](img/e417fac734bc222fa28e1c1aeebf73d1.png)

三次回归模型的统计摘要（注意：由于

机器学习的随机性质）。图像由费德里科·特罗塔提供。

首先，多项式模型比线性模型有更多的特征，因为多项式模型的公式是：

多项式模型的公式。作者撰写，嵌入-dot-fun 提供技术支持。

所以这个公式在线性模型的公式中添加了多项式项。

所以，对我们的结果进行评论，我们在 R² 和调整后的 R² 上有所改进，但根据 `p-values`，许多特征似乎并不具有统计显著性。特别是，三次多项式的 R²=1（当然！我们用三次多项式创建了数据集，所以……一切都符合！），但许多特征似乎并不具有统计显著性。

现在，让我问你一个问题：根据这些统计数据，你会选择哪个模型？线性模型看起来很不错，不是吗？

所以，这里是问题：正如我们所说，使用 `p-values` 并不能保证结果。

此外，考虑到我们已经用 3 次多项式构建了这个数据集，几乎所有特征都没有统计显著性，这怎么可能呢？嗯，很难说。我们只知道模型的一个或多个假设没有得到尊重（多项式模型的假设与线性模型相同；你可以在[这篇文章中找到](https://medium.com/towards-data-science/mastering-linear-regression-the-definitive-guide-for-aspiring-data-scientists-7abd37fcb9ed)）。但在这些情况下，多重共线性通常是最可能的原因。

现在，让我们使用抽样检查方法来解决这个 ML 模型。记住，线性回归模型没有超参数，所以我们可以直接将其（以及多项式模型）拟合到测试集上：

```py
**NOTE:**

for the sake of clarity, we are using again the 2-degree and 3-degree
polynomial transformation, even if we shouldn't if we were using
a Jupyter Notebook (we've already done the transformation above).
```

```py
# Fit linear regression model to the train set
lin_reg = LinearRegression().fit(X_train, y_train)

# Create quadratic and cubic features
quadratic = PolynomialFeatures(degree=2)
cubic = PolynomialFeatures(degree=3)

X_quad = quadratic.fit_transform(X_train) #quadratical transformation
X_cubic = cubic.fit_transform(X_train) #cubical tranformation

# Fit quadratic and cubic transformed set
quad_reg = LinearRegression().fit(X_quad, y_train)
cubic_reg = LinearRegression().fit(X_cubic, y_train)

# Calculate R² on test set
print(f'Coeff. of determination linear reg test set:{lin_reg.score(X_test, y_test): .2f}')
print(f'Coeff. of determination quadratic reg test set:{quad_reg.score(quadratic.transform(X_test), y_test): .2f}') 
print(f'Coeff. of determination cubic reg test set:{cubic_reg.score(cubic.transform(X_test), y_test): .2f}')

>>>

  Coeff. of determination linear reg test set: 0.81
  Coeff. of determination quadratic reg test set: 0.81
  Coeff. of determination cubic reg test set: 1.00
```

所以，我们为所有模型获得了高 R² 值。特别是，三次多项式在训练集和测试集上都有 R²=1。这应该告诉我们一些事情……

现在，让我们在测试集上验证我们的模型，即使使用 KDE 图。如果你不知道 KDE 模型是什么以及如何使用它，你可以阅读我在这里的文章：

[](/mastering-linear-regression-the-definitive-guide-for-aspiring-data-scientists-7abd37fcb9ed?source=post_page-----a6636e27e489--------------------------------) ## 精通线性回归：有志数据科学家的权威指南

### 你需要知道的线性回归的所有信息都在这里（包括 Python 中的应用）

[towardsdatascience.com

我们可以这样使用它们：

```py
#Kernel Density Estimation plot
ax = sns.kdeplot(y_test, color='r', label='Actual Values') #actual values
sns.kdeplot(y_test_pred_lin, color='b', label='Predicted Values', ax=ax) #predicted values

#showing title
plt.title('Actual vs Predicted values for linear model')
#showing legend
plt.legend()
#showing plot
plt.show()
```

![](img/bcfb547547c94c94de460f4b226862cb.png)

线性模型的 KDE 图。图片来源于**费德里科·特罗塔**。

```py
#Kernel Density Estimation plot
ax = sns.kdeplot(y_test, color='r', label='Actual Values') #actual values
sns.kdeplot(y_test_pred_quad, color='b', label='Predicted Values', ax=ax) #predicted values

#showing title
plt.title('Actual vs Predicted values for quadratic model')
#showing legend
plt.legend()
#showing plot
plt.show()
```

![](img/79e8aaf6db92a7cfa4a0c93c625dac08.png)

二次方模型的 KDE 图。图片来源于**费德里科·特罗塔**。

```py
#Kernel Density Estimation plot
ax = sns.kdeplot(y_test, color='r', label='Actual Values') #actual values
sns.kdeplot(y_test_pred_cub, color='b', label='Predicted Values', ax=ax) #predicted values

#showing title
plt.title('Actual vs Predicted values for the cubic model')
#showing legend
plt.legend()
#showing plot
plt.show()
```

![](img/d3c81a3cbc2e0d17720532a61a73c863.png)

三次方模型的 KDE 图。图片来源于**费德里科·特罗塔**。

所以，通过观察 KDE，我们可以说三次方模型显然过拟合了数据（但即使因为训练集和测试集的 R²都为 1，我们也可以这样判断。不过，再次提醒：我们在这里发现了一些明显的东西。发现明显的事情是好的，因为我们知道自己做得很好！）。

此外，即使我们在线性和二次方模型的两个数据集上得到了非常好的 R²值，KDE 也清楚地告诉我们这些模型并不适合解决这个机器学习问题。因此，我们应该研究其他模型。

最后，给一个最终建议，如果我们真的想计算`p-values`以确保我们的模型符合假设，我们可以在最后进行计算（所以我们只会为我们认为是最佳的模型计算它们）。

但请记住：基于`p-values`，线性模型可能是解决这个机器学习问题的一个好选择，而 KDE 却告诉我们相反的结果！

# 结论

感谢阅读这篇文章：我非常希望在评论中听到你的意见。

让我总结一下这些观点：

+   `p-values`的使用在某种程度上是一个有争议的话题。它有助于测试我们的假设，但正如 ASA 所说，它并不能保证结果。

+   我个人认为`p-values`更适合科学研究，在那里重要的是展示我们在得出某些结果时使用了严格的方法（记住，正如 ASA 所说，结果不能仅仅依赖于`p-values`）。

+   考虑到我们需要在给定的数据集上测试不同的机器学习模型，以验证无免费午餐定理，当提供业务价值时，实际上不需要使用`p-values`来为我们的结果提供科学依据。无论如何，如果我们想使用它们，可以在最后测试假设，以展示我们选择的模型符合假设。

+   *订阅* [*我的新闻通讯*](https://federicotrotta.substack.com/?r=1ep1nf&utm_campaign=pub-share-checklist) *以获取更多关于 Python 和数据科学的内容。*

+   *觉得有用吗？* ***请给我买一杯*** [***Ko-fi***](https://ko-fi.com/federicotrotta)***。***

+   *喜欢这篇文章？通过* [***我的推荐链接***](https://federicotrotta.medium.com/membership)*加入 Medium*: 每月 5 美元（无额外费用）解锁 Medium 上的所有内容。

+   *想联系我？请在* [*这里*](https://bio.link/federicotrotta)*。*
