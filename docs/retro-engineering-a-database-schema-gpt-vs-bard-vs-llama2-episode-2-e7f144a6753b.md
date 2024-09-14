# 数据库模式的逆向工程：GPT 与 Bard 与 Llama2（第 2 集）

> 原文：[`towardsdatascience.com/retro-engineering-a-database-schema-gpt-vs-bard-vs-llama2-episode-2-e7f144a6753b`](https://towardsdatascience.com/retro-engineering-a-database-schema-gpt-vs-bard-vs-llama2-episode-2-e7f144a6753b)

## 在我之前的文章中，我对比了 GPT-4 模型和 Bard。现在 Llama-2 进入了竞技场，是时候看看它与竞争对手的表现了！

[](https://pl-bescond.medium.com/?source=post_page-----e7f144a6753b--------------------------------)![皮埃尔-路易斯·贝斯孔](https://pl-bescond.medium.com/?source=post_page-----e7f144a6753b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e7f144a6753b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e7f144a6753b--------------------------------) [皮埃尔-路易斯·贝斯孔](https://pl-bescond.medium.com/?source=post_page-----e7f144a6753b--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e7f144a6753b--------------------------------) ·6 分钟阅读·2023 年 10 月 6 日

--

![](img/2941f397ecb8ea1cf96ab9c325122f57.png)

图片由 [达斯汀·休姆斯](https://unsplash.com/@dustinhumes_photography?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 初始（和最终）数据集

如 [这篇文章](https://medium.com/p/2e2776e8af86) 中所述，我们将从一个包含员工信息的假 AI 生成数据集开始。

[链接](https://retro-engineering-a-database-schema-and-quality-checks-gpt-vs-bard-2e2776e8af86?source=post_page-----e7f144a6753b--------------------------------) [## 数据库模式及质量检查的逆向工程：GPT 与 Bard

### LLM 是否可以逆向工程一个合并的数据集来设计原始数据库，并提出相应的数据……

[链接](https://towardsdatascience.com/retro-engineering-a-database-schema-and-quality-checks-gpt-vs-bard-2e2776e8af86?source=post_page-----e7f144a6753b--------------------------------)

原始表格有 11 列 x 7688 行，但我们将提取 50 行样本，以适应当前 LLM 的令牌限制。

![](img/0c36c37aab2a4cf97caf94e274d64dcd.png)

来源数据样本（图片由作者提供）

*(注意：笔记本和数据源在文章末尾提供)*

# 数据模型的逆向工程

这里的想法是让每个 LLM 分析这个样本数据，并提供有关初始数据方案可能是什么样的见解。

我们将保持与 GPT-4 和 Bard 使用的相同提示：

```py
Could you :
- identify the categorical columns within this dataset as well as confidential ones
- suggest a database schema with different tables (pay attention to creating a separate table for confidential data)
- for the tables with categorical data, please provide the SQL script to create them, including their content (Key and Values)
- for the remaining tables, please provide the script to create their scheme
- for each column of each table, suggest some data quality checks
```

好的！……但现在的问题是“我在哪里可以测试 Llama-2？”

有几种可用的选项：

+   最明显的一个（但也更复杂和昂贵 💸）是将模型托管在你云架构中的专用服务器上。如果你打算服务于重型应用程序，这通常是一个不错的选择。

    请记住，运行 LLM 所需的虚拟机从小模型的 2–3$/小时... 到要妥善托管 LLama2–70b 的 18$/小时不等，根据 Azure 的信息 😨

+   一个中间解决方案——由 [Yann Lecun 本人](https://www.linkedin.com/feed/update/urn:li:activity:7109561666324885504?commentUrn=urn%3Ali%3Acomment%3A%28activity%3A7109561666324885504%2C7111453478513733632%29&dashCommentUrn=urn%3Ali%3Afsd_comment%3A%287111453478513733632%2Curn%3Ali%3Aactivity%3A7109561666324885504%29) 推荐——是使用像 [Anyscale](https://www.anyscale.com/) 这样的平台，例如，它在 Llama-2–70b-chat 上提供 1M 标记 $1（而 Azure 对 GPT-4 的定价在 $30 和 $60 之间）

+   令人惊叹的 Hugging Face 平台还为我们提供了一个测试平台。例如，这是一个保持财务清晰的好方法！

[](https://huggingface.co/spaces/huggingface-projects/llama-2-7b-chat?source=post_page-----e7f144a6753b--------------------------------) [## Llama 2 7B Chat - 由 huggingface-projects 提供的 Hugging Face 空间

### 发现社区制作的令人惊叹的 ML 应用

huggingface.co](https://huggingface.co/spaces/huggingface-projects/llama-2-7b-chat?source=post_page-----e7f144a6753b--------------------------------)

+   在写这第二集时，我还发现了 [`chat.lmsys.org/`](https://chat.lmsys.org/)，它允许你用两个模型并行执行一个单一的提示。非常适合基准测试它们的性能。这是我将在本文中使用的工具。

![](img/d98758be443f6eec0e3aabe30a2c800d.png)

图片来源：[Paz Arando](https://unsplash.com/@pazarando?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 分析结果（LLama-2）

在 ChatGPT 上运行基准测试时，我们选择了最先进且普遍可用的版本：4.0。

出于相同的精神，我们将在“[LLama-2–70b-chat](https://ai.meta.com/llama/)”版本上运行我们的查询，该版本如其名称所示，具有 70 亿个参数和 4096 个标记的上下文窗口。

## 识别分类和机密数据

```py
Certainly! Here's my analysis of the dataset:

Categorical columns:
- Department
- Country
- Location
- Education

Confidential columns:

- Employee_ID
- Salary
- Annual_Performance
```

即使有人可能会质疑“Employee_ID”是否确实属于机密数据（但我越想越觉得有道理），LLama-2 还是成功找出了所有分类和机密列，按出现顺序 ✅

## 提议一个数据库模型

```py
Suggested database schema:

- Employee (Employee_ID, First_Name, Last_Name, Email, Department, Salary, Annual_Performance)
- Department (Department_ID, Department_Name)
- Country (Country_ID, Country_Name)
- Location (Location_ID, Location_Name)
- Education (Education_ID, Education_Name)
```

再次，Llama-2 做得很好，将分类列正确转换为包含键/标签对的子表。 ✅

将机密数据分离到另一个表中，就像 ChatGPT 所做的那样，会是一个加分项，但这已经是一个接近的结果！ 🔶

最后，“年龄”列已从建议的模式中消失。 ❌

## 创建表的 SQL 脚本

我将在这里继续进行“酸性测试”🧪，即简单地在 Snowflake 中运行这些查询并检查结果是什么样的：

```py
-- SQL script to create the tables:
-- Employee:
CREATE TABLE Employee (
    Employee_ID VARCHAR(20) NOT NULL,
    First_Name VARCHAR(50) NOT NULL,
    Last_Name VARCHAR(50) NOT NULL,
    Email VARCHAR(100) NOT NULL,
    Department VARCHAR(50) NOT NULL,
    Salary INTEGER NOT NULL,
    Annual_Performance INTEGER NOT NULL,
    PRIMARY KEY (Employee_ID)
);

-- Department:
CREATE TABLE Department (
    Department_ID INTEGER NOT NULL,
    Department_Name VARCHAR(50) NOT NULL,
    PRIMARY KEY (Department_ID)
);

-- Country:
CREATE TABLE Country (
    Country_ID INTEGER NOT NULL,
    Country_Name VARCHAR(50) NOT NULL,
    PRIMARY KEY (Country_ID)
);

-- Location:
CREATE TABLE Location (
    Location_ID INTEGER NOT NULL,
    Location_Name VARCHAR(50) NOT NULL,
    PRIMARY KEY (Location_ID)
);

-- Education:
CREATE TABLE Education (
    Education_ID INTEGER NOT NULL,
    Education_Name VARCHAR(50) NOT NULL,
    PRIMARY KEY (Education_ID)
);
```

除了“年龄”仍然缺失之外，一切运行得非常顺利，所有表格在 Snowflake 中无缝创建！ ✅

## 数据质量检查

```py
Data quality checks:

Employee_ID:
* Check for duplicate values
* Check for invalid characters (e.g. special characters, spaces)
First_Name:
* Check for duplicate values
* Check for invalid characters (e.g. special characters, spaces)
Last_Name:
* Check for duplicate values
* Check for invalid characters (e.g. special characters, spaces)
Email:
* Check for invalid email addresses (e.g. invalid syntax, missing @ symbol)
* Check for duplicate values
Department:
* Check for duplicate values
* Check for invalid characters (e.g. special characters, spaces)
Salary:
* Check for invalid values (e.g. negative values, non-numeric values)
Annual_Performance:
* Check for invalid values (e.g. negative values, non-numeric values)
Country:
* Check for duplicate values
* Check for invalid characters (e.g. special characters, spaces)
Location:
* Check for duplicate values
* Check for invalid characters (e.g. special characters, spaces)
Education:
* Check for duplicate values
* Check for invalid characters (e.g. special characters, spaces)
Note: These are just some suggestions for data quality checks. The actual checks will depend on the specific requirements and constraints of the dataset.
```

在这里，Llama-2 提出了些常见的见解：

+   检查重复 ✅

+   检查无效字符 ✅

… 还尝试适应某些字段的性质：

+   有效的电子邮件地址格式 ✅

+   “薪资”与“年度评估”的负值或非数字值 ✅

… 但也会产生一些错误：

+   例如，在表中找到相同的名字（甚至是姓氏）并不罕见，因此应接受重复 ❌

回顾 ChatGPT 的表现，识别诸如薪资、年龄等列的范围，甚至定义年度评估的可接受值，Llama-2 提供了一个“平均”的答案。

## Llama-2–70b 的结论：

+   识别分类数据和机密数据 ✅

+   建议一个数据库模型 ✅

+   创建表的 SQL 脚本 ✅

+   数据质量检查 🔶

# 结论（更新了 Llama-2）

GPT-4k 在理解数据集、设计合适的数据模型（此处为第三范式 (3NF)）、编写相应的 SQL 查询以及建议数据质量检查方面明显优于 Bard 和 Llama-2。

对于这个特定的任务（数据库逆向工程），Llama-2 的表现优于 Bard。

看看上面的答案，我认为 Llama-2 提供的见解比 Bard 提供的要有价值得多。

![](img/d6dbaa44d89f44de9848183352e5cb06.png)

备注：

+   在八月底，Meta 发布了一个“Code Llama”模型，我们也可以对其进行基准测试，也许在未来的文章中。

+   我使用了预定义的温度 0.7 进行推理，这意味着执行相同的查询可能会得到不同的结果。我决定仅保留模型提供的第一个结果，以避免偏倚分析。

⏩ 相关的 [Jupyter Notebook](https://github.com/pierrelouisbescond/medium_articles/blob/main/medium_llm_db_retro_eng_load_and_format_data.ipynb) 和 [CSV 数据源](https://github.com/pierrelouisbescond/medium_articles/blob/main/Employees_Base.csv) 链接。

和往常一样，我已经尽力识别出所有必要的步骤，如果你想重复分析，但如果教程中有任何缺失的说明，请随时联系我！

你也可以查看我在 Medium 上的其他文章：

[](https://pl-bescond.medium.com/pierre-louis-besconds-articles-on-medium-f6632a6895ad?source=post_page-----e7f144a6753b--------------------------------) [## Pierre-Louis Bescond 的 Medium 文章

### 数据科学、机器学习与创新

pl-bescond.medium.com](https://pl-bescond.medium.com/pierre-louis-besconds-articles-on-medium-f6632a6895ad?source=post_page-----e7f144a6753b--------------------------------)
