# 数据科学中的 Python 学习 实战第二部分：练习

> 原文：[`towardsdatascience.com/learning-python-for-data-science-on-the-job-part-2-practice-b4ece80488da`](https://towardsdatascience.com/learning-python-for-data-science-on-the-job-part-2-practice-b4ece80488da)

## Excel 和 Python 中线性回归的并行案例研究

[](https://nrlewis929.medium.com/?source=post_page-----b4ece80488da--------------------------------)![Nicholas Lewis](https://nrlewis929.medium.com/?source=post_page-----b4ece80488da--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b4ece80488da--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b4ece80488da--------------------------------) [Nicholas Lewis](https://nrlewis929.medium.com/?source=post_page-----b4ece80488da--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b4ece80488da--------------------------------) ·10 分钟阅读·2023 年 1 月 15 日

--

![](img/7fe25f9ef78a62d00f5ba0f71a4fd54b.png)

图片由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

欢迎阅读这系列关于在工作中或没有正式教育背景下学习 Python 和数据科学的文章的第二部分。[第一部分](https://medium.com/towards-data-science/learning-python-for-data-science-on-the-job-part-1-philosophy-6e2aedc4e041)讲述了我过去 10 年在工作中和正式教育环境中学习的经历。如果你对学习的哲学和一些激励自己开始学习的想法感兴趣，可以看看这篇文章。或者，如果你像我一样通过实际操作具体例子来学习，继续阅读吧！

## 问题定义

所有相关文件可以在我的 [Github](https://github.com/nrlewis929/excel_python_side-by-side) 上找到。不过，我建议你完全从头开始，按照这里提供的代码块和截图进行操作。

在这个案例研究中，我们将进行一个简单的线性回归。我们有两类输入数据，基于这些输入，我们希望训练一个线性模型来预测一个输出，基于实际观察到的数据。在 `data.csv` 文件中，这些输入被称为 `x1` 和 `x2`，观察到的数据被称为 `y`。模型的形式为 Ax1 + Bx2 + C = y。你可能会注意到 x2 = x1²。这是有意为之的，随着你在数据科学中的进步，你可能会想要记住这个小技巧：你可以通过简单地平方或取已有输入的对数来创建额外的输入（在数据科学中，输入通常被称为特征）。

## 设置

首先，打开 Excel 电子表格和 Jupyter notebook。通常，你可能会开始将原始数据直接复制并粘贴到 Excel 文件中，但对于这个特定的问题，我们首先要做一些类似于你在 Python 中常做的事情。你需要求解器加载项来解决这个问题。如果你从未使用过求解器加载项，请按照[这里](https://support.microsoft.com/en-us/office/load-the-solver-add-in-in-excel-612926fc-d53b-46b4-872c-e24772f078ca)的说明进行操作。启用加载项为 Excel 提供了额外的功能，而这些功能不是标准配置的一部分。

在 Excel 中你很少这样做，但在 Python 中你几乎总会做类似的事情。启用额外功能是通过导入库或幕后代码来实现的，这些代码允许你在 Python 中执行更强大和高效的命令。你可以通过输入 `import [library_name]` 来实现。这告诉 Python 你将使用指定的库。你可以选择为库起一个缩短的名字。例如，你可以说 `import pandas as pd`。每次使用 `pandas` 库的某些功能时，你可以直接输入 `pd` 代替 `pandas`。虽然你可以给库起任何名字，但你会很快发现大多数包都有常见的缩写。

许多库在你下载 Python 时已经预装，就像 Excel 已经有按钮允许你制作图表或执行数学函数一样。你可能不会在导入 `pandas`（用于数据处理）和 `matplotlib`（用于绘图）时遇到问题。不过，你可能需要[使用 pip 安装](https://scikit-learn.org/stable/install.html) `scikit-learn`（或 `sklearn`）库，就像你需要做一些特别的工作来获取 Excel 求解器加载项一样（巧合的是，`sklearn` 在这个练习中将以类似于 Excel 求解器加载项的方式使用）。你的第一段代码应如下所示：

```py
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression
```

来自 `sklearn` 的这一行代码看起来有些不同。这是因为 `sklearn` 是一个庞大的库（可以查看他们的网站和文档），而我们只会使用其中的一小部分。因此，我们在这一行代码中告诉 Python 只从 scikit-learn 中导入那部分特殊功能，而不是全部。可能会是一个无尽的坑，但请注意，以下代码块做了相同的事情：

```py
import sklearn
lr = sklearn.linear_model.LinearRegression()
```

```py
from sklearn.linear_model import LinearRegression
lr = LinearRegression()
```

## 加载数据

相比于 Excel，设置这些内容需要做很多工作，但这正是让 Python 更加多才多艺的因素之一。现在，你将数据加载到你的程序中。在 Excel 中，你可以直接从 `data.csv` 文件中复制并粘贴。在 Python 中，你可以将其加载为数据框（想象成一个超级增强的 Excel 表格）。你的下一行代码应如下所示：

```py
df = pd.read_csv('data.csv')
```

这一行代码告诉 pandas 读取 `data.csv` 文件中的值，并将其存储在 `df`（数据框的缩写）变量中。你应该确保 `data.csv` 文件在与你的 Jupyter notebook 相同的目录中，否则你需要指定文件的路径。

你可能会对无法实际看到每行代码的运行情况感到沮丧。作为视觉生物，这也许是编程的一个缺陷。然而，当你编写代码时，你可以方便地显示输出。例如，在一个新的代码块中输入 `df` 并执行（按 ctrl+enter），看看会发生什么。然后尝试 `df.head()`。最后，尝试 `df.head(3)`。你注意到每个的区别了吗？这就是编码相对于使用电子表格的灵活性和效率的体现。代码简洁而强大，一旦你克服了初始可视化的障碍，你可能会发现编码更为可取。顺便提一句，当你开始处理包含数百万条记录的数据集时，你会更加感激它；在 Excel 中对这些数据集进行操作真的会让它变得非常缓慢，而编码则能保持流畅（直到你处理非常大的数据集）。

## 模型设置

到目前为止，我们的进展相当缓慢，但希望接下来的部分能真正突出编码相对于电子表格的优势、灵活性和速度。让我们先通过 Excel 进行问题设置，然后看看如何用仅几行 Python 代码做到同样的事情。

在 Excel 中，我们将通过设置平方和系统来找到模型的系数。创建一个新的单元格框来跟踪系数，并对系数做一个猜测值。你可以将所有猜测值初始设置为 1，但有时你的猜测确实对结果有很大影响（Python 更方便，不需要提供初始猜测，虽然它提供了这个选项）。然后，按照截图中的模型编程新列单元格，以进行模型预测。

![](img/3b5218b66b67c220da09622d94153a52.png)

预测列编程的截图。图片由作者提供

最后，生成一个新列，称为“平方误差”列，按照下面所示进行计算。在打开 Solver 之前的最后一步是求出所有平方误差的总和——因此叫做“平方和”目标，也称为 l2-范数。（你可以通过转到 E23 单元格并输入公式 =SUM(E2:E22) 来完成此操作。）

![](img/255c737980819bd5abb6ed8fd1fdd68c.png)

计算平方误差的公式截图。图片由作者提供

我们终于完成了问题设置。我不知道你在 Excel 中的熟练程度或你是否使用过 Solver，但以这种方式进行练习的目的是展示在 Python 中这个过程有多么简单。这不是关于 Excel Solver、线性回归或平方和为何有效的教程（尽管我可以讨论这些！），所以我不会在这里深入细节。我们可以用仅 3 行代码在 Python 中完成所有设置：

```py
X = df[['x1','x2']].values
y = df['y'].values
model = LinearRegression()
```

## 模型解决方案

让我们退一步，记住我们要做什么（我们已经很接近了！）。我们想开发一个模型，允许我们基于两个输入特征 `x1` 和 `x2` 预测一个值 `y`。我们假设模型是线性回归，形式为 Ax1 + Bx2 + C = y。看起来我们走了一条绕路的路，但我们只差一步。在 Excel 中，打开 Solver 对话框，并按如下方式填写（特别注意确保未选中限制为正值的复选框）。运行程序，你会看到屏幕上的所有变化。你会在对应的单元格中看到你的 A、B 和 C 值。

![](img/0f40ac58af1bb9abe28ecff2d2c260b2.png)

填写完表格后再按“解决”的截图。图片由作者提供。

我们将回到那个黄色框，展示类似的内容在 Python 中。但为了在 Python 中设置问题，你只需写一行代码即可完成所有工作：

```py
model.fit(X,y)
```

再次，最明显的区别是你在 Python 中看不到任何不同的内容。但你实际上已经有了解决方案。如果你深入文档，你会发现你实际上可以输出这些值。对于线性回归，你可以通过几个打印语句找到这些值，如下所示：

```py
print('Coefficient A:',model.coef_[0])
print('Coefficient B:',model.coef_[1])
print('Coefficient C:',model.intercept_)
```

你的值应该匹配！所以迅速回到那个黄色框。它基本上是在问你的 A、B 和 C 系数是否都应该是正数。有时这很重要，特别是当你建模的系统有真实物理意义且系数受到自然现象限制为正数时。如果你在 scikit-learn 的 `LinearRegression` 文档中找找，你会发现你可以在初始化模型时传递一个参数来做同样的事情。它看起来是这样的：

```py
model = LinearRegression(positive = True)
```

绕道的重点是展示编程中最不直观的事情之一：选项是存在的，你只需要找到它们！没有像 Excel 中那样简单的可视化复选框，但它们确实存在！文档中告诉你的默认值类似于在打开 Solver 时复选框是否被选中的情况。

## 模型预测

很好，现在我们有了一个有效的模型。我们怎么进行预测呢？假设我们想知道 x1 = 0.65 和 x2 = 0.65² = 0.4225 的预测值。在 Excel 中，你需要把这些值放在一些新的单元格中，然后将方程编程到另一个单元格中以获得答案，就像下面的截图一样。

![](img/1657b56de4cc78ce36ae34db660ca83c.png)

编程新的单元格以生成新数据的预测。注意，系数值的单元格已被赋予唯一的名称（稍后讨论）。图片由作者提供。

在 Python 中，你可以通过输入以下代码来做完全相同的事情：

```py
x1_predict = 0.65
x2_predict = x1_predict ** 2 # Careful to not use the ^ symbol to square values!
X_predict = [[x1_predict, x2_predict]]
y_predict = model.predict(X_predict)
```

这可能看起来有点繁琐，因为我们必须输入变量名。但这里有一个有趣的小事实：你知道你可以给 Excel 单元格唯一的变量名吗？这与定义 Python 变量并在未来的公式中使用它是一样的。谷歌搜索“excel 给单元格命名变量”或类似内容，然后你可以像截图中看到的那样重写你的公式。我几乎在实践中从不这样做，但由于这篇文章是关于将 Excel 与 Python 进行比较的，希望这能给你一个更好的理解。

## 绘图结果

在这一领域，Excel 可能看起来比 Python 要好很多，但那只是因为它有一个用户界面可以互动。要在 Python 中进行自定义，你必须输入一行代码。

我不会讲解如何在 Excel 中绘图——你可能对此已经很熟练了。在 Python 中，我们将使用 matplotlib，但要注意，还有许多其他选项可以探索，如 plotly、seaborn 和 altair。我想是时候让你自己动手了，所以我不会逐行讲解这段代码。相反，把它当作一个练习，试着理解每一行的作用。然后，查看文档，看看是否可以更改一些输入，使图形成为你自己的！

```py
plt.plot(df['x1'], df['y'], '.', label = 'raw data')
plt.plot(df['x1'], yp, label = 'model prediction')
plt.xlabel('x1', size = 14)
plt.ylabel('y', size = 14)
plt.legend(fontsize = 12, loc = 'lower right')
plt.show()
```

## 摘要

你做到了！你完成了其中一个完整的代码，从零开始并最终得到可用的结果。希望你从中获得了很多关于如何学习编程的见解，而不是浪费时间在一个 4 小时的教程上，那种教程通常只是听过就忘。很酷的一点是，这项活动，虽然可能比你期望的时间要长，但在一两个月内你可以在 5 分钟内轻松完成。开发整个过程在 Excel 和 Python 中都花的时间比阅读这篇文章要少。

如果我可以总结一下我预期的最大挑战，那就是：我们是视觉动物，而编程却不是视觉的。Excel 很简单，因为有按钮和图形用户界面可供操作。你必须通过编程即时创建可视化图形。在学习过程中，额外的`print`语句、数据图表、数据表格等都不会出错。即使是更高级的程序员，你可能仍然会发现自己在 Excel 中浏览新数据以快速了解它。这是完全正常的！然而，我希望你像我一样，最终更倾向于使用 Python——不仅因为它更强大和多功能，还因为它变得*更简单*！

一如既往，你可以通过[LinkedIn](https://www.linkedin.com/in/nicholas-lewis-0366146b/)与我联系，也可以在[Towards Data Science](https://nrlewis929.medium.com/)上关注我，查看我关于数据科学案例研究的定期帖子。如果有些帖子类型比其他帖子更有用或更有趣，我很乐意听到。下次见！
