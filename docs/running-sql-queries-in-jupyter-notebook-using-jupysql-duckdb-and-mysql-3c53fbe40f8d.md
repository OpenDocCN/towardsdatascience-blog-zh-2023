# 在 Jupyter Notebook 中使用 JupySQL、DuckDB 和 MySQL 运行 SQL 查询

> 原文：[`towardsdatascience.com/running-sql-queries-in-jupyter-notebook-using-jupysql-duckdb-and-mysql-3c53fbe40f8d`](https://towardsdatascience.com/running-sql-queries-in-jupyter-notebook-using-jupysql-duckdb-and-mysql-3c53fbe40f8d)

## 学习如何在你的 Jupyter Notebooks 中运行 SQL

[](https://weimenglee.medium.com/?source=post_page-----3c53fbe40f8d--------------------------------)![魏梦龙](https://weimenglee.medium.com/?source=post_page-----3c53fbe40f8d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3c53fbe40f8d--------------------------------)![数据科学的前沿](https://towardsdatascience.com/?source=post_page-----3c53fbe40f8d--------------------------------) [魏梦龙](https://weimenglee.medium.com/?source=post_page-----3c53fbe40f8d--------------------------------)

·发表于 [数据科学的前沿](https://towardsdatascience.com/?source=post_page-----3c53fbe40f8d--------------------------------) ·8 分钟阅读·2023 年 2 月 24 日

--

![](img/243efc1d9e9a6b0829180fe106bd4934.png)

图片由 [Wafer WAN](https://unsplash.com/@waferwan?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

传统上，数据科学家使用 Jupyter Notebook 从数据库服务器或外部数据集（如 CSV、JSON 文件等）中提取数据，并将其存储到 Pandas 数据框中：

![](img/045e46113ac1e7638be1dcd0a1163924.png)

除非另有说明，否则所有图片均由作者提供

然后，他们使用数据框进行可视化。这种方法有几个缺点：

+   查询数据库服务器可能会降低数据库服务器的性能，因为数据库服务器可能未针对分析工作负载进行优化。

+   将数据加载到数据框中会占用宝贵的资源。例如，如果目的是可视化数据集的某些方面，则需要先将整个数据集加载到内存中，然后才能进行可视化操作。

为了提高上述操作的性能，理想情况下，应将数据处理（所有的数据清理和过滤）卸载到能够高效执行数据分析的客户端，并返回结果用于可视化。这就是本文的主题 — **JupySQL**。

**JupySQL 是一个 Jupyter Notebook 的 SQL 客户端，允许你使用 SQL 直接访问数据集。JupySQL 的主要思想是在 Jupyter Notebook 中运行 SQL，因此得名。**

JupySQL 允许你使用 SQL 查询数据集，而无需维护存储数据集的数据框。例如，你可以使用 JupySQL 连接到数据库服务器（如 MySQL 或 PostgreSQL），或通过 DuckDB 引擎访问你的 CSV 文件。查询结果可以直接用于可视化。下图展示了 JupySQL 的工作原理：

![](img/40d2fce894792268bd0d96361347db05.png)

你可以使用以下魔法命令在 Jupyter Notebooks 中使用 JupySQL：

+   `%sql` — 这是一个行魔法命令，用于执行 SQL 语句

+   `%%sql` — 这是一个单元魔法命令，用于执行多行 SQL 语句

+   `%sqlplot` — 这是一个行魔法命令，用于绘制图表

# 我们的数据集

对于本文，我将使用一些数据集：

+   **泰坦尼克号数据集** (*titanic_train.csv*)。 *来源*：[`www.kaggle.com/datasets/tedllh/titanic-train`](https://www.kaggle.com/datasets/tedllh/titanic-train)。 *许可证* — 数据库内容许可证 (DbCL) v1.0

+   **保险数据集** (*insurance.csv*)。 *来源*：[`www.kaggle.com/datasets/teertha/ushealthinsurancedataset`](https://www.kaggle.com/datasets/teertha/ushealthinsurancedataset?resource=download.)。 *许可证* — [CC0: 公开领域](https://creativecommons.org/publicdomain/zero/1.0/)。

+   **2015 年航班延误数据集** (*airports.csv*)。 *来源*：[`www.kaggle.com/datasets/usdot/flight-delays`](https://www.kaggle.com/datasets/usdot/flight-delays)。 *许可证* — [CC0: 公开领域](https://creativecommons.org/publicdomain/zero/1.0/)

+   **波士顿数据集** (*boston.csv*)。 *来源*：[`www.kaggle.com/datasets/altavish/boston-housing-dataset`](https://www.kaggle.com/datasets/altavish/boston-housing-dataset)。 *许可证* — [CC0: 公开领域](https://creativecommons.org/publicdomain/zero/1.0/)

+   **苹果历史数据集** (AAPL.csv)。 *来源*：[`www.kaggle.com/datasets/prasoonkottarathil/apple-lifetime-stocks-dataset`](https://www.kaggle.com/datasets/prasoonkottarathil/apple-lifetime-stocks-dataset)。 *许可证* — [CC0: 公开领域](https://creativecommons.org/publicdomain/zero/1.0/)

# 安装 JupySQL

要安装 JupySQL，你可以使用 `pip` 命令：

```py
!pip install jupysql duckdb-engine --quiet
```

上述语句安装了 `jupysql` 包以及 `duckdb-engine`。

下一步是使用 `%load_ext` 行魔法命令加载 `sql` 扩展：

```py
%load_ext sql
```

# 与 DuckDB 集成

加载 `sql` 扩展后，你需要加载一个数据库引擎来处理数据。对于本节，我将使用 DuckDB。以下语句启动一个 DuckDB 内存数据库：

```py
%sql duckdb://
```

## 执行查询

启动 DuckDB 数据库后，让我们使用 airports.csv 文件执行查询：

```py
%sql SELECT * FROM airports.csv ORDER by STATE
```

你将看到以下输出：

![](img/ac17759472c92a58412055a3420a371a.png)

如果你的 SQL 查询很长，请使用 `%%sql` 单元魔法命令：

```py
%%sql
SELECT 
    count(*) as Count, STATE 
FROM airports.csv 
GROUP BY STATE 
ORDER BY Count 
DESC LIMIT 5
```

上述 SQL 语句生成了以下输出：

![](img/1ab89a50abb6c4134e20ddcad6287dd1.png)

# 保存查询

你还可以使用`--save`选项保存查询，以便后续使用：

```py
%%sql --save boston 
SELECT 
    * 
FROM boston.csv
```

![](img/cbedffeddc2827536c099f9ed5aec256.png)

如果你想保存一个查询但不执行它，请使用`--no-execute`选项：

```py
%%sql --save boston --no-execute
SELECT 
    * 
FROM boston.csv
```

上述语句将查询结果保存为名为`boston`的表。你将看到以下输出：

```py
*  duckdb://
Skipping execution...
```

# 绘图

JupySQL 允许你使用 `%sqlplot` 行魔术命令绘制图表。

## 直方图

使用前面部分保存的查询，你现在可以绘制一个直方图，显示`age`和`medv`字段的分布：

```py
%sqlplot histogram --column age medv --table boston --with boston
```

这是显示`age`和`medv`字段值分布的直方图：

![](img/b5f2e086355ced24376f2ed9b8736814.png)

这是另一个示例。这一次，我们将使用 titanic_train.csv 文件：

```py
%%sql --save titanic 
SELECT 
   *
FROM titanic_train.csv WHERE age NOT NULL AND embarked NOT NULL
```

![](img/ab4da20440e64c643db854959906e851.png)

你现在可以绘制所有乘客的年龄分布：

```py
%sqlplot histogram --column age --bins 10 --table titanic --with titanic
```

> 你可以使用`--bin`选项来指定你想要的箱数。

![](img/eaa05f94a056dad6ec55023085b95136.png)

你还可以通过将绘图分配给一个变量来定制绘图，该变量的类型为`matplotlib.axes._subplots.AxesSubplot`：

```py
ax = %sqlplot histogram --column age --bins 10 --table titanic --with titanic
ax.grid()
ax.set_title("Distribution of Age on Titanic")
_ = ax.set_xlabel("Age")
```

使用`matplotlib.axes._subplots.AxesSubplot`对象，你可以开启网格、设置标题以及为绘图设置 x 轴标签：

![](img/d3bda39fe4bfe149f53d086dfd84332a.png)

## 箱形图

除了直方图，你还可以绘制箱形图：

```py
%sqlplot boxplot --column age fare --table titanic --with titanic
```

结果箱形图显示了`age`和`fare`字段的中位数、最小值和最大值以及异常值：

![](img/ac59b10d99ed183e8576a79db3283f5e.png)

你还可以查看`sibsp`和`parch`字段的箱形图：

```py
%sqlplot boxplot --column sibsp parch --table titanic --with titanic
```

![](img/312243836dd970a1c5e734f5fb8f2da4.png)

## 饼图

你还可以使用 JupySQL 的遗留绘图 API 绘制饼图。对于这个示例，我将使用 airports.csv 文件来找出每个州的机场数量。

首先，我使用 SQL 统计每个州的所有机场并筛选出前五名：

```py
airports_states = %sql SELECT count(*) as Count, STATE FROM airports.csv GROUP BY STATE ORDER BY Count DESC LIMIT 5
print(type(airports_states))
```

`%sql`语句的结果是一个`sql.run.ResultSet`对象。从这个对象中，如果需要，我可以获得数据框：

```py
airports_states.DataFrame()
```

![](img/1a9a19ad4a577d017a5c0e286825ca63.png)

我还可以使用它调用`pie()` API 来绘制饼图：

```py
import seaborn
# https://seaborn.pydata.org/generated/seaborn.color_palette.html
palette_color = seaborn.color_palette('pastel')

total = airports_states.DataFrame()['Count'].sum()
def fmt(x):    
    return '{:.4f}%\n({:.0f} airports)'.format(x, total * x / 100)

airports_states.pie(colors=palette_color, autopct=fmt)
```

![](img/6a6f133061e2ebb47d8586589284a9a3.png)

绘图 API 还支持条形图：

```py
palette_color = seaborn.color_palette('husl')
airports_states.bar(color=palette_color)
```

![](img/39b800c595e1f5ac7c77bda56e444945.png)

并使用`plot()`函数绘制折线图（这里我使用 AAPL.csv 文件）：

```py
apple = %sql SELECT Date, High, Low FROM AAPL.csv

# apple.plot() is of type matplotlib.axes._subplots.AxesSubplot
apple.plot().legend(['High','Low'])
```

![](img/09de289e6c1c3b0ba63a8f76fc5d8297.png)

# 集成 MySQL

迄今为止，前面几个部分的所有示例都是使用 DuckDB。现在让我们尝试连接到数据库服务器。对于我的示例，我将使用 MySQL 服务器，具体信息如下：

+   **数据库** — 保险

+   **表格** — 保险（从 insurance.csv 文件导入）

+   **用户账户** — `user1`

要连接到 MySQL 服务器，请创建一个**SQLAlchemy URL 标准**连接字符串，格式如下：`mysql://*username*:*password*@*host*/*db*`

运行以下代码片段时，将提示你输入 `user1` 账户的密码：

```py
from getpass import getpass

password = getpass()
username = 'user1'
host = 'localhost'
db = 'Insurance'

# Connection strings are SQLAlchemy URL standard
connection_string = f"mysql://{username}:{password}@{host}/{db}"
```

输入 `user1` 账户的密码：

![](img/e49945f5cb9d819394a4e8e166fe5a45.png)

要将 JupySQL 连接到 MySQL 服务器，请使用 `%sql` 行魔法，并附上连接字符串：

```py
%sql $connection_string
```

如果你使用 `%sql` 行魔法而没有任何输入，你将会看到当前的连接（即 DuckDB 和 MySQL）：

```py
%sql
```

![](img/67bbd9cb9035553579e716ca7368af60.png)

让我们选择**保险**表来检查其内容：

```py
%sql SELECT * FROM Insurance
```

![](img/4efc0b067ea097fe466ca98134fb4129.png)

接下来，让我们使用 `bar()` API 绘制一个条形图：

```py
regions_count = %sql SELECT region, count(*) FROM Insurance GROUP BY region
regions_count.bar(color=palette_color)
```

![](img/06940a90ecf6ec929dd7fbc51da667cc.png)

**如果你喜欢阅读我的文章，并且它对你的职业/学习有所帮助，请考虑注册成为 Medium 会员。每月 $5，它可以让你无限访问 Medium 上的所有文章（包括我的文章）。如果你使用以下链接注册，我将获得一小笔佣金（对你没有额外费用）。你的支持意味着我将能投入更多时间写类似的文章。**

[](https://weimenglee.medium.com/membership?source=post_page-----3c53fbe40f8d--------------------------------) [## 使用我的推荐链接加入 Medium - Wei-Meng Lee

### 阅读 Wei-Meng Lee（以及 Medium 上成千上万其他作家的）每一个故事。你的会员费用直接支持……

[weimenglee.medium.com](https://weimenglee.medium.com/membership?source=post_page-----3c53fbe40f8d--------------------------------)

# 概要

我希望这篇文章让你更好地了解如何使用 JupySQL 以及连接到不同数据源（如 MySQL 和 DuckDB）的各种方法。此外，除了连接到我们的数据集，我还展示了如何使用 JupySQL 直接对查询结果进行可视化。像往常一样，请务必尝试一下，并告诉我效果如何！
