# 在 Python 中构建互动数据可视化：Plotly 入门

> 原文：[`towardsdatascience.com/building-interactive-data-visualizations-in-python-an-introduction-to-plotly-3ffdd920fc63`](https://towardsdatascience.com/building-interactive-data-visualizations-in-python-an-introduction-to-plotly-3ffdd920fc63)

## 探索用于数据分析和机器学习的互动可视化的强大功能

[](https://federicotrotta.medium.com/?source=post_page-----3ffdd920fc63--------------------------------)![Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----3ffdd920fc63--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3ffdd920fc63--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3ffdd920fc63--------------------------------) [Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----3ffdd920fc63--------------------------------)

·发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----3ffdd920fc63--------------------------------) ·阅读时间 6 分钟·2023 年 7 月 19 日

--

![](img/694658ab3db881ef69f4b764e3ef4ea2.png)

图片由[格尔德·阿特曼](https://pixabay.com/it/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1237457)提供，来源于[Pixabay](https://pixabay.com/it//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1237457)

数据可视化是数据专业人员最重要的任务之一。它实际上帮助我们理解数据，并提出更多问题以便进一步调查。

但是数据可视化不仅仅是我们在探索性数据分析阶段必须完成的任务。我们还可能需要展示数据，通常是向观众展示，以帮助他们得出结论。

在 Python 中，我们通常使用`matplotlib`和`seaborn`作为绘图库。

然而，有时我们可能需要一些互动的可视化。在某些情况下，为了更好地理解数据；在其他情况下，为了更好地展示我们的解决方案。

在这篇文章中，我们将讨论`plotly`，这是一个用于创建互动可视化的 Python 库。

# 什么是 Plotly？

正如我们可以在[他们的网站上](https://plotly.com/python/)阅读到的：

> Plotly 的 Python 绘图库可以创建互动的、出版质量的图表。包括如何制作折线图、散点图、面积图、条形图、误差条、箱线图、直方图、热图、子图、多轴图、极坐标图和气泡图的示例。
> 
> Plotly.py 是[免费且开源的](https://plotly.com/python/is-plotly-free)，你可以[查看源代码、报告问题或在 GitHub 上贡献](https://github.com/plotly/plotly.py)。

所以，Plotly 是一个免费的开源 Python 库，用于制作互动可视化。

正如我们在他们的网站上看到的，它为我们提供了创建不同领域图表的可能性：AI/ML、统计、科学、金融等。

由于我们对机器学习和数据科学感兴趣，我们将展示一些与这些领域相关的图表以及如何在 Python 中创建它们。

最后，要安装它，我们需要输入：

```py
$ pip install plotly
```

# 1. 互动气泡图

Plotly 的一个有趣且有用的功能是可以创建互动气泡图。

在气泡图的情况下，气泡有时会相交，使数据难以读取。如果图表是互动的，我们就可以更轻松地读取数据。

让我们看一个例子：

```py
import plotly.express as px
import pandas as pd
import numpy as np

# Generate random data
np.random.seed(42)
n = 50
x = np.random.rand(n)
y = np.random.rand(n)
z = np.random.rand(n) * 100  # Third variable for bubble size

# Create a DataFrame
data = pd.DataFrame({'X': x, 'Y': y, 'Z': z})

# Create the scatter plot with bubble size with Plotly
fig = px.scatter(data, x='X', y='Y', size='Z',
      title='Interactive Scatter Plot with Bubble Plot')

# Add labels to the bubbles
fig.update_traces(textposition='top center', textfont=dict(size=11))

# Update layout properties
fig.update_layout(
    xaxis_title='X-axis',
    yaxis_title='Y-axis',
    showlegend=False
)

# Display the interactive plot
fig.show()
```

然后我们得到：

![](img/8a394fe987961c15c4ff33ec8d745837.png)

我们编写了互动气泡图。图片由**Federico Trotta**提供。

因此，我们使用`NumPy`创建了一些数据，并将它们存储在一个`Pandas`数据框中。然后，我们通过方法`px.scatter()`创建了互动图，方法从数据框中检索数据，并指定标题（与`Matplotlib`不同，后者将标题插入图表创建方法之外）。

# 2. 互动相关矩阵

我有时面临的任务之一是正确可视化相关矩阵。事实上，当我们有大量数据时，这些矩阵有时难以阅读和可视化。

解决问题的一种方法是使用 Plotly 创建互动可视化。

为了这个目的，让我们创建一个具有 10 列的 Pandas 数据框，并使用 Plotly 创建一个互动相关矩阵：

```py
import pandas as pd
import numpy as np
import plotly.figure_factory as ff

# Create random data
np.random.seed(42)
data = np.random.rand(100, 10)

# Create DataFrame
columns = ['Column' + str(i+1) for i in range(10)]
df = pd.DataFrame(data, columns=columns)

# Round values to 2 decimals
correlation_matrix = df.corr().round(2)  

# Create interactive correlation matrix with Plotly
figure = ff.create_annotated_heatmap(
    z=correlation_matrix.values,
    x=list(correlation_matrix.columns),
    y=list(correlation_matrix.index),
    colorscale='Viridis',
    showscale=True
)

# Set axis labels
figure.update_layout(
    title='Correlation Matrix',
    xaxis=dict(title='Columns'),
    yaxis=dict(title='Columns')
)

# Display the interactive correlation matrix
figure.show()
```

然后我们得到：

![](img/0961063ce13fd590df943b73b0ae2506.png)

我们创建的互动相关矩阵。图片由**Federico Trotta**提供。

因此，我们可以通过方法`ff.create_annotated_map()`以非常简单的方式创建互动相关矩阵。

# 3. 互动 ML 图表

在机器学习中，我们有时需要图形化地比较数量。在这些情况下，有时阅读我们的图表会很困难。

典型的情况是 ROC/AUC 曲线，其中我们比较不同 ML 模型的性能。事实上，有时这些曲线会相交，我们无法正确地可视化它们。

为了改进我们的可视化，我们可以使用 Plotly 创建一个 ROC/AUC 曲线，如下所示：

```py
import pandas as pd
import numpy as np
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.neighbors import KNeighborsClassifier
from sklearn.ensemble import RandomForestClassifier
from sklearn.tree import DecisionTreeClassifier
from sklearn.svm import SVC
from sklearn.metrics import roc_curve, auc
import plotly.graph_objects as go

# Create synthetic binary classification data
X, y = make_classification(n_samples=1000, n_features=20, random_state=42)

# Scale the data using StandardScaler
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Split the data into train and test sets
X_train, X_test, y_train, y_test = train_test_split(X_scaled, y, test_size=0.2, random_state=42)

# Initialize the models
knn = KNeighborsClassifier()
rf = RandomForestClassifier()
dt = DecisionTreeClassifier()
svm = SVC(probability=True)

# Fit the models on the train set
knn.fit(X_train, y_train)
rf.fit(X_train, y_train)
dt.fit(X_train, y_train)
svm.fit(X_train, y_train)

# Predict probabilities on the test set
knn_probs = knn.predict_proba(X_test)[:, 1]
rf_probs = rf.predict_proba(X_test)[:, 1]
dt_probs = dt.predict_proba(X_test)[:, 1]
svm_probs = svm.predict_proba(X_test)[:, 1]

# Calculate the false positive rate (FPR) and true positive rate (TPR) for ROC curve
knn_fpr, knn_tpr, _ = roc_curve(y_test, knn_probs)
rf_fpr, rf_tpr, _ = roc_curve(y_test, rf_probs)
dt_fpr, dt_tpr, _ = roc_curve(y_test, dt_probs)
svm_fpr, svm_tpr, _ = roc_curve(y_test, svm_probs)

# Calculate the AUC (Area Under the Curve) for ROC curve
knn_auc = auc(knn_fpr, knn_tpr)
rf_auc = auc(rf_fpr, rf_tpr)
dt_auc = auc(dt_fpr, dt_tpr)
svm_auc = auc(svm_fpr, svm_tpr)

# Create an interactive AUC/ROC curve using Plotly
fig = go.Figure()
fig.add_trace(go.Scatter(x=knn_fpr, y=knn_tpr, name='KNN (AUC = {:.2f})'.format(knn_auc)))
fig.add_trace(go.Scatter(x=rf_fpr, y=rf_tpr, name='Random Forest (AUC = {:.2f})'.format(rf_auc)))
fig.add_trace(go.Scatter(x=dt_fpr, y=dt_tpr, name='Decision Tree (AUC = {:.2f})'.format(dt_auc)))
fig.add_trace(go.Scatter(x=svm_fpr, y=svm_tpr, name='SVM (AUC = {:.2f})'.format(svm_auc)))
fig.update_layout(title='AUC/ROC Curve',
                  xaxis=dict(title='False Positive Rate'),
                  yaxis=dict(title='True Positive Rate'),
                  legend=dict(x=0.7, y=0.2))
# Show plot
fig.show()
```

然后我们得到：

![](img/83e536cb4d086e837bd2f6825892c9aa.png)

我们创建的 AUC/ROC 曲线。图片由**Federico Trotta**提供。

因此，我们通过方法`add.trace(go.Scatter)`为我们使用的每个 ML 模型（KNN、SVM、决策树和随机森林）创建了散点图。

这样，可以轻松显示曲线自交点的详细信息和区域值。

# 结论

在这篇文章中，我们简要介绍了 Plotly，并展示了如何利用它在数据分析和机器学习中实现更好的可视化。

正如我们所看到的，这是一个低代码库，确实帮助我们更好地可视化数据，从而改善结果。

![](img/5079e3af9eda458328cb258c452fb935.png)

**Federico Trotta**

嗨，我是费德里科·特罗塔，一名自由职业技术写作人员。

想和我合作吗？[联系我](https://bio.link/federicotrotta)。
