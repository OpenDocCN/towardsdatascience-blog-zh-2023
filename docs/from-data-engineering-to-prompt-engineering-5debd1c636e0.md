# 从数据工程到提示工程

> 原文：[https://towardsdatascience.com/from-data-engineering-to-prompt-engineering-5debd1c636e0?source=collection_archive---------1-----------------------#2023-05-22](https://towardsdatascience.com/from-data-engineering-to-prompt-engineering-5debd1c636e0?source=collection_archive---------1-----------------------#2023-05-22)

## 使用 ChatGPT 解决数据准备任务

[](https://medium.com/@c4ristian?source=post_page-----5debd1c636e0--------------------------------)[![Christian Koch](../Images/6daf756236838069bf79f1078b03ae6d.png)](https://medium.com/@c4ristian?source=post_page-----5debd1c636e0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5debd1c636e0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5debd1c636e0--------------------------------) [Christian Koch](https://medium.com/@c4ristian?source=post_page-----5debd1c636e0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7633c76cf996&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-data-engineering-to-prompt-engineering-5debd1c636e0&user=Christian+Koch&userId=7633c76cf996&source=post_page-7633c76cf996----5debd1c636e0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5debd1c636e0--------------------------------) · 8 分钟阅读 · 2023 年 5 月 22 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5debd1c636e0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-data-engineering-to-prompt-engineering-5debd1c636e0&user=Christian+Koch&userId=7633c76cf996&source=-----5debd1c636e0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5debd1c636e0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-data-engineering-to-prompt-engineering-5debd1c636e0&source=-----5debd1c636e0---------------------bookmark_footer-----------)![](../Images/9fd676ffe1e9c0711ce34e74831e47e0.png)

照片由 [Ricardo Gomez Angel](https://unsplash.com/@rgaleriacom?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数据工程是数据科学过程中的一个重要部分。在 CRISP-DM 方法中，这个过程阶段被称为“数据准备”。它包括数据摄取、数据转换和数据质量保证等任务。在我们的文章中，我们使用 ChatGPT 和 Python 解决典型的数据工程任务。通过这样做，我们探讨了数据工程与新兴领域提示工程之间的联系。

# 介绍

在2022年5月，Stephen Wolfram和Lex Fridman进行了题为“[编程已死？](https://www.youtube.com/watch?v=uD353DeOM-4)”的深刻讲座。他们讨论了高层语言是否仍会被开发者使用。根据Wolfram的说法，许多编程任务可以用大型语言模型（LLMs）来自动化。在撰写本文时，这种模型的最突出例子是[ChatGPT](https://openai.com/blog/chatgpt)。自2022年末推出以来，它产生了惊人的结果。指定LLM要执行的操作被称为“提示工程”。如果Wolfram是对的，那么软件开发的部分工作将从编写代码转向编写提示。

在数据科学中，数据准备可能是一个耗时且繁琐的任务。那么为什么不尝试用LLM自动化呢？在接下来的部分，我们将用ChatGPT和Python处理不同的数据工程问题。我们没有自己编写Python代码，而是使用提示工程来生成代码。我们的实验于2023年5月19日进行，基于当时最新的免费ChatGPT版本（GPT-3.5）。

# 创建数据框

让我们从一个简单的问题开始，并从样本数据集中创建一个Pandas数据框。表格1包含了由[世界银行](https://data.worldbank.org/indicator)提供的国家指标。

```py
 |   Continent   |  Country |  Year |    Category   |   Variable  |  Value |
|---------------|----------|-------|---------------|-------------|--------|
| Asia          |  China   |  2022 |  Economy      |  GDP        |   18.1 |
| Asia          |  China   |  2022 |  Demographics |  Population |   1412 |
| North America |  USA     |  2022 |  Economy      |  GDP        |   25.5 |
| North America |  USA     |  2022 |  Demographics |  Population |    333 |
| Europe        |  Germany |  2022 |  Economy      |  GDP        |    3.9 |
| Europe        |  Germany |  2022 |  Demographics |  Population |     84 |
```

*表格1：包含国家指标的数据集*

为了从上述样本创建数据框，我们开始了与ChatGPT的新对话，并发布了以下提示：

```py
Create a Pandas table with the following data:
Continent, Country, Year, Category, Variable, Value
Asia, China, 2022, Economy, GDP, 18.1
Asia, China, 2022, Demographics, Population, 1412
North America, USA, 2022, Economy, GDP, 25.5
North America, USA, 2022, Demographics, Population, 333
Europe, Germany, 2022, Economy, GDP, 3.9
Europe, Germany, 2022, Demographics, Population, 84
```

*提示1：创建数据框*

作为回应，模型生成了以下Python代码：

```py
import pandas as pd

data = {
    'Continent': ['Asia', 'Asia', 'North America', 'North America', 'Europe', 'Europe'],
    'Country': ['China', 'China', 'USA', 'USA', 'Germany', 'Germany'],
    'Year': [2022, 2022, 2022, 2022, 2022, 2022],
    'Category': ['Economy', 'Demographics', 'Economy', 'Demographics', 'Economy', 'Demographics'],
    'Variable': ['GDP', 'Population', 'GDP', 'Population', 'GDP', 'Population'],
    'Value': [18.1, 1412, 25.5, 333, 3.9, 84]
}

df = pd.DataFrame(data)
print(df)
```

*响应1：数据框创建代码*

运行脚本时，我们看到输出符合我们的规范：

```py
 Continent  Country  Year      Category    Variable   Value
0           Asia    China  2022       Economy         GDP    18.1
1           Asia    China  2022  Demographics  Population  1412.0
2  North America      USA  2022       Economy         GDP    25.5
3  North America      USA  2022  Demographics  Population   333.0
4         Europe  Germany  2022       Economy         GDP     3.9
5         Europe  Germany  2022  Demographics  Population    84.0
```

即使我们使用了一个样本进行实验，接下来的代码原则上也可以处理更大的数据集。

# 数据框的透视

对于数据科学而言，我们用第一个提示创建的表格结构并不理想。相反，所谓的“平面表”会更方便。在表格1中，指标“GDP”和“Population”由名为“Variable”和“Value”的两列表示。相比之下，平面表将包含每个指标的单独列，如表格2所示。

```py
 |   Continent   |  Country |  Year |  GDP  |  Population |
|---------------|----------|-------|-------|-------------|
| Asia          |  China   |  2022 |  18.1 |        1412 |
| North America |  USA     |  2022 |  25.5 |         333 |
| Europe        |  Germany |  2022 |   3.9 |          84 |
```

*表格2：包含国家指标的平面表*

将表格1的格式转换为表格2的格式称为“透视”。让我们尝试用ChatGPT解决这个任务。为此，我们在对话中发布了第二个提示：

```py
Transform the table into the following format:
Continent, Country, Year, GDP, Population
Asia, China, 2022, 18.1, 1412
North America, USA, 2022, 25.5, 333
Europe, Germany, 2022, 3.9, 84
```

*提示2：数据框透视*

上述信息基于所需数据格式的示例。我们本可以用纯口头方式表述我们的请求。与经典编程一样，提示工程中有替代解决方案来实现目标。找到最佳方式将是未来令人兴奋的任务。作为我们提示的结果，ChatGPT调整了Python代码如下：

```py
import pandas as pd

data = {
    'Continent': ['Asia', 'Asia', 'North America', 'North America', 'Europe', 'Europe'],
    'Country': ['China', 'China', 'USA', 'USA', 'Germany', 'Germany'],
    'Year': [2022, 2022, 2022, 2022, 2022, 2022],
    'Category': ['Economy', 'Demographics', 'Economy', 'Demographics', 'Economy', 'Demographics'],
    'Variable': ['GDP', 'Population', 'GDP', 'Population', 'GDP', 'Population'],
    'Value': [18.1, 1412, 25.5, 333, 3.9, 84]
}

df = pd.DataFrame(data)

# Pivot the table
pivot_df = df.pivot_table(index=['Continent', 'Country', 'Year'], columns='Category', values='Value').reset_index()

# Rename the columns
pivot_df.columns.name = None
pivot_df.columns = [column if column != 'Variable' else None for column in pivot_df.columns]

print(pivot_df)
```

*响应2：透视代码*

然而，运行脚本的第二个版本显示的输出并不是我们所请求的：

```py
 Continent  Country  Year  Demographics  Economy
0           Asia    China  2022        1412.0     18.1
1         Europe  Germany  2022          84.0      3.9
2  North America      USA  2022         333.0     25.5
```

而不是预期的列“GDP”和“人口”，表中包含两列名为“人口统计”和“经济”。为了解决这个问题，我们发布了下一个提示：

```py
The columns are not correct, you should have used 
the variables GDP and population
```

*提示 3：修复枢轴代码*

对此，模型纠正了错误并生成了Python代码的另一版本：

```py
import pandas as pd

data = {
    'Continent': ['Asia', 'Asia', 'North America', 'North America', 'Europe', 'Europe'],
    'Country': ['China', 'China', 'USA', 'USA', 'Germany', 'Germany'],
    'Year': [2022, 2022, 2022, 2022, 2022, 2022],
    'Category': ['Economy', 'Demographics', 'Economy', 'Demographics', 'Economy', 'Demographics'],
    'Variable': ['GDP', 'Population', 'GDP', 'Population', 'GDP', 'Population'],
    'Value': [18.1, 1412, 25.5, 333, 3.9, 84]
}

df = pd.DataFrame(data)

# Pivot the table
pivot_df = df.pivot_table(index=['Continent', 'Country', 'Year'], columns='Variable', values='Value').reset_index()

# Rename the columns
pivot_df.columns.name = None
pivot_df.columns = ['Continent', 'Country', 'Year', 'GDP', 'Population']

print(pivot_df)
```

*响应 3：修正的枢轴代码*

运行脚本显示，这次实施了正确的枢轴：

```py
 Continent  Country  Year   GDP  Population
0           Asia    China  2022  18.1      1412.0
1         Europe  Germany  2022   3.9        84.0
2  North America      USA  2022  25.5       333.0
```

# 丰富数据框

另一个典型的数据工程任务是用额外信息丰富数据集。为此目的，我们要求ChatGPT通过发布以下提示为我们的数据添加[ISO国家代码](https://en.wikipedia.org/wiki/ISO_3166-1)：

```py
For each country, in the table above, add its ISO code
```

*提示 4：添加国家代码*

结果，模型调整了响应并返回了以下代码片段：

```py
import pandas as pd

data = {
    'Continent': ['Asia', 'Asia', 'North America', 'North America', 'Europe', 'Europe'],
    'Country': ['China', 'China', 'USA', 'USA', 'Germany', 'Germany'],
    'Year': [2022, 2022, 2022, 2022, 2022, 2022],
    'Category': ['Economy', 'Demographics', 'Economy', 'Demographics', 'Economy', 'Demographics'],
    'Variable': ['GDP', 'Population', 'GDP', 'Population', 'GDP', 'Population'],
    'Value': [18.1, 1412, 25.5, 333, 3.9, 84]
}

iso_codes = {
    'China': 'CN',
    'USA': 'US',
    'Germany': 'DE'
}

df = pd.DataFrame(data)

# Add ISO code column
df['ISO Code'] = df['Country'].map(iso_codes)

# Pivot the table
pivot_df = df.pivot_table(index=['Continent', 'Country', 'ISO Code', 'Year'], columns='Variable', values='Value').reset_index()

# Rename the columns
pivot_df.columns.name = None
pivot_df.columns = ['Continent', 'Country', 'ISO Code', 'Year', 'GDP', 'Population']

print(pivot_df)
```

*响应 4：带有国家代码的数据框*

运行脚本显示，数据框已添加了ISO代码的新列。

```py
 Continent  Country ISO Code  Year   GDP  Population
0           Asia    China       CN  2022  18.1      1412.0
1         Europe  Germany       DE  2022   3.9        84.0
2  North America      USA       US  2022  25.5       333.0
```

# 派生一个列

让我们接下来通过将一个国家的GDP除以其人口大小来派生一个新列“人均 GDP”。为此，我们发送了这个提示：

```py
Derive a new column "GDP per capita" by dividing the GDP 
of a country by its population size
```

*提示 5：派生一个新列*

对此，模型修改了代码如下：

```py
import pandas as pd

data = {
    'Continent': ['Asia', 'Asia', 'North America', 'North America', 'Europe', 'Europe'],
    'Country': ['China', 'China', 'USA', 'USA', 'Germany', 'Germany'],
    'Year': [2022, 2022, 2022, 2022, 2022, 2022],
    'Category': ['Economy', 'Demographics', 'Economy', 'Demographics', 'Economy', 'Demographics'],
    'Variable': ['GDP', 'Population', 'GDP', 'Population', 'GDP', 'Population'],
    'Value': [18.1, 1412, 25.5, 333, 3.9, 84]
}

iso_codes = {
    'China': 'CN',
    'USA': 'US',
    'Germany': 'DE'
}

df = pd.DataFrame(data)

# Add ISO code column
df['ISO Code'] = df['Country'].map(iso_codes)

# Pivot the table
pivot_df = df.pivot_table(index=['Continent', 'Country', 'ISO Code', 'Year'], columns='Variable', values='Value').reset_index()

# Calculate GDP per capita
pivot_df['GDP per capita'] = pivot_df['GDP'] / pivot_df['Population']

# Rename the columns
pivot_df.columns.name = None
pivot_df.columns = ['Continent', 'Country', 'ISO Code', 'Year', 'GDP', 'Population', 'GDP per capita']

print(pivot_df)
```

*响应 5：带有新列的数据框*

输出显示脚本产生了预期的结果：

```py
 Continent  Country ISO Code  Year   GDP  Population  GDP per capita
0           Asia    China       CN  2022  18.1      1412.0        0.012819
1         Europe  Germany       DE  2022   3.9        84.0        0.046429
2  North America      USA       US  2022  25.5       333.0        0.076577
```

# 分析数据

一旦数据转换完成，实际分析就可以开始了。最后让我们计算国家的GDP和人口之间的相关系数。为此，我们发布了最后一个提示：

```py
Use the table above to calculate the correlation coefficient between 
the GDP and the population of the countries
```

*提示 6：分析相关性*

因此，模型返回了以下代码片段：

```py
import pandas as pd

data = {
    'Continent': ['Asia', 'Asia', 'North America', 'North America', 'Europe', 'Europe'],
    'Country': ['China', 'China', 'USA', 'USA', 'Germany', 'Germany'],
    'Year': [2022, 2022, 2022, 2022, 2022, 2022],
    'Category': ['Economy', 'Demographics', 'Economy', 'Demographics', 'Economy', 'Demographics'],
    'Variable': ['GDP', 'Population', 'GDP', 'Population', 'GDP', 'Population'],
    'Value': [18.1, 1412, 25.5, 333, 3.9, 84]
}

df = pd.DataFrame(data)

# Pivot the table
pivot_df = df.pivot_table(index=['Continent', 'Country', 'Year'], columns='Variable', values='Value').reset_index()

# Calculate correlation coefficient
correlation_coefficient = pivot_df['GDP'].corr(pivot_df['Population'])

print("Correlation Coefficient: {:.3f}".format(correlation_coefficient))
```

*响应 6：分析相关性的代码*

运行脚本时，我们发现这两个变量之间存在正相关，这并不令人意外：

```py
Correlation Coefficient: 0.350
```

# 结论

诚然，我们的实验基于一个简单的数据集。尽管如此，结果令人瞩目。我们完成了多项数据工程任务，而不用写一行代码。ChatGPT不仅在大多数情况下能够正确实现我们的提示，即使在模型犯错时，它也能够反思并修正。正如在软件开发中一样，生成的代码必须经过测试。此外，它可能需要重构和优化。在AI时代，使用pylint仍然是个好主意。总结来说，然而，我们不得不同意沃尔夫勒姆的观点：未来，数据工程的重要部分将从编码转向提示工程。这种新方法不会取代数据工程师，但会让他们变得更高效。

所有图片（除非另有说明）均由作者提供。

# 关于作者

*克里斯蒂安·科赫*是BWI GmbH的企业架构师，同时也是纽伦堡理工大学乔治·西蒙·奥姆学院的讲师。

*马库斯·施塔迪*是Dehn SE的高级云数据工程师，多年来一直从事数据工程、数据科学和数据分析工作。

*卢卡斯·伯勒* 是 TeamBank AG 的数据架构师，专注于设计和实施强大的数据分析架构。
