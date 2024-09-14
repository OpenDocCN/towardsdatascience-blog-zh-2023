# 逆向工程数据库模式和质量检查：GPT 与 Bard

> 原文：[`towardsdatascience.com/retro-engineering-a-database-schema-and-quality-checks-gpt-vs-bard-2e2776e8af86`](https://towardsdatascience.com/retro-engineering-a-database-schema-and-quality-checks-gpt-vs-bard-2e2776e8af86)

## 大型语言模型（LLMs）能否逆向工程一个整合的数据集，以设计原始数据库并建议相应的数据质量检查？

[](https://pl-bescond.medium.com/?source=post_page-----2e2776e8af86--------------------------------)![Pierre-Louis Bescond](https://pl-bescond.medium.com/?source=post_page-----2e2776e8af86--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2e2776e8af86--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2e2776e8af86--------------------------------) [Pierre-Louis Bescond](https://pl-bescond.medium.com/?source=post_page-----2e2776e8af86--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2e2776e8af86--------------------------------) ·阅读时长 9 分钟·2023 年 7 月 25 日

--

![](img/9c745c0ff34a55794be637adf06abedc.png)

图片由 [Jake Trotman](https://unsplash.com/@jake_t?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/wallpapers/design/pattern?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在我之前关于如何利用生成性 AI 进行数据活动的帖子之后，我想探索这样一个用例：一个数据团队从一个职能部门（比如人力资源）接收了一个整合的数据集，并需要在他们的数据平台上重新设计一个适当的数据模型，以处理未来的查询。

我们将比较 GPT-4 和 Bard 的回答，以确定哪个模型提供了更相关的答案。

*(注：笔记本和数据源在文章末尾提供)*

# 初始（也是最终）数据集

有时候，商业解决方案仅允许你以报告的形式从其专有系统中提取信息……如果运气好的话，它们甚至可能通过 API 进行访问。

这是“我的公司”的情况，其中 HRIS 遗留系统只能提供所有员工的一个提取，其中包含许多关于公司的详细信息，其中一些是机密的。

根据数据网格原则，人力资源团队希望公开这些数据，但他们也理解报告不能直接使用，更不用说像“薪水”、“年龄”或“年度评估”这样的列所引发的机密性问题。

![](img/69c949a452847d131ca2e36b47242ebe.png)

来源于遗留 HRIS 系统的原始报告（作者提供的图片）

# 数据模型的逆向工程

在与数据团队互动时，桌子周围的每个人很快明白这个数据集不能广播到所有功能/员工，并且需要拆分成多个表格。

这些表格中的一些可能会被许多人用于其他分析或用例：

+   内部部门名单

+   员工名单，包括他们的电子邮件、部门、国家和位置

+   等等。

其他一些应保持机密（薪资、年龄、评估）

一位数据分析师决定测试 GPT 或 Bard 如何帮助每个人逆向工程原始关系数据库并节省时间🕘。

# 创建新环境并导入数据

在每个新项目中创建一个专用的 Python 环境始终是一个好习惯。

在终端中，我们运行以下两个命令（我们立即请求两个所需的包：Pandas 和 Tabulate）：

```py
conda create -n "hr-data" python=3.10 pandas tabulate -y

-- installation logs --

conda activate hr-data
```

我们切换到 Jupyter Notebook 并导入 CSV 文件：

```py
import pandas as pd

df = pd.read_csv("Employees_Base.csv")
df
```

![](img/07997339a16992c8cba95cf2bc2b2d4b.png)

VS Code 中 df 变量内容（作者提供的图像）

数据集 7688 行 x 11 列被成功加载，我们可以继续😀

# 为未来的提示格式化数据

对于大多数用户而言，GPT 模型仅通过聊天界面访问，而这正是我们的数据分析师的情况。她需要找到一种“传达”数据集的结构化方式给 LLMs。

> Markdown 来救援！🚀

幸运的是，LLM 可以完美处理 Markdown 格式的表格，因此我们可以直接在 Jupyter Notebook 中打印出行的示例（这也是为什么在环境创建过程中需要“tabulate”包😉）：

```py
sample_md = df.sample(50).to_markdown()
print(sample_md)
```

![](img/90c3a8916d190ba26754ada89b073ead.png)

Markdown 格式的数据集示例（作者提供的图像）

*（注：我们选择 50 行的原因是为了保持一个合理但仍具代表性的样本，而不超过 LLM 的当前令牌限制。）*

# 定义我们的提示

我们希望模型处理不同的任务。为了最大化获得相关答案的机会，我们将解释我们期待的顺序：

```py
Could you :
- identify the categorical columns within this dataset as well as confidential ones
- suggest a database schema with different tables (pay attention to creating a separate table for confidential data)
- for the tables with categorical data, please provide the SQL script to create them, including their content (Key and Values)
- for the remaining tables, please provide the script to create their scheme
- for each column of each table, suggest some data quality checks
```

结合 Markdown 表格，这个请求将消耗大约 6K 令牌，剩下 ~2K 令牌用于不同的答案（根据 GPT-4k 的限制；Bard 的令牌限制目前未公开）。

![](img/d840406d62ccdcec91cb318706e8ed0c.png)

# 分析结果（GPT-4k）

## 识别分类和机密数据

GPT-4k（温度为 0）对数据结构理解良好，即使有人可能会争辩：

+   在公司内部，专业电子邮件地址不是机密的，

+   相反，年龄可能在 HR 层面被保密。

```py
Categorical columns: Department, Country, Location, Education
Confidential columns: Email, Salary, Annual_Performance
```

## 提议一个数据库模型

GPT 按指示正确地分离公共和机密表格，保持 Employee_ID 作为主键，并在“Employee_Details”表中创建“Department_ID”、“Country_ID”、“Location_ID”和“Education_ID”作为外键，以连接分类表格：

```py
Database schema:

Employee_Details (Employee_ID, First_Name, Last_Name, Age, Department_ID, Country_ID, Location_ID, Education_ID)
Confidential_Data (Employee_ID, Email, Salary, Annual_Performance)
Departments (Department_ID, Department_Name)
Countries (Country_ID, Country_Name)
Locations (Location_ID, Location_Name)
Education_Levels (Education_ID, Education_Level)
```

## 创建表格的 SQL 脚本

GPT 生成了适用于分类数据的即用 SQL 脚本：

```py
CREATE TABLE Departments (
    Department_ID INT PRIMARY KEY,
    Department_Name VARCHAR(255) NOT NULL
);
INSERT INTO
    Departments (Department_ID, Department_Name)
VALUES
    (1, 'Digital'),
    (2, 'Manufacturing'),
    (3, 'Finance'),
    (4, 'R&D'),
    (5, 'Legal'),
    (6, 'Sales'),
    (7, 'Human Ressources'),
    (8, 'Procurement'),
    (9, 'Supply-Chain');

CREATE TABLE Countries (
        Country_ID INT PRIMARY KEY,
        Country_Name VARCHAR(255) NOT NULL
    );
INSERT INTO
    Countries (Country_ID, Country_Name)
VALUES
    (1, 'USA'),
    (2, 'UK'),
    (3, 'Canada');

CREATE TABLE Locations (
        Location_ID INT PRIMARY KEY,
        Location_Name VARCHAR(255) NOT NULL
    );
INSERT INTO
    Locations (Location_ID, Location_Name)
VALUES
    (1, 'New-York'),
    (2, 'Detroit'),
    (3, 'San Fransisco'),
    (4, 'London'),
    (5, 'Birmingham'),
    (6, 'Montreal'),
    (7, 'Quebec'),
    (8, 'Toronto'),
    (9, 'Brighton');

CREATE TABLE Education_Levels (
        Education_ID INT PRIMARY KEY,
        Education_Level VARCHAR(255) NOT NULL
    );
INSERT INTO
    Education_Levels (Education_ID, Education_Level)
VALUES
    (1, 'Bachelor''s Degree'),
    (2, 'Master''s Degree'),
    (3, 'Doctoral Degree'),
    (4, 'Certificate'),
    (5, 'Associate Degree');
```

剩下的两个表也是如此：

```py
CREATE TABLE Confidential_Data (  
    Employee_ID VARCHAR(255) PRIMARY KEY,  
    Email VARCHAR(255) NOT NULL,  
    Salary INT NOT NULL,  
    Annual_Performance INT NOT NULL  
);  

CREATE TABLE Employee_Details (  
    Employee_ID VARCHAR(255) PRIMARY KEY,  
    First_Name VARCHAR(255) NOT NULL,  
    Last_Name VARCHAR(255) NOT NULL,  
    Age INT NOT NULL,  
    Department_ID INT NOT NULL,  
    Country_ID INT NOT NULL,  
    Location_ID INT NOT NULL,  
    Education_ID INT NOT NULL,  
    FOREIGN KEY (Employee_ID) REFERENCES Confidential_Data(Employee_ID),  
    FOREIGN KEY (Department_ID) REFERENCES Departments(Department_ID),  
    FOREIGN KEY (Country_ID) REFERENCES Countries(Country_ID),  
    FOREIGN KEY (Location_ID) REFERENCES Locations(Location_ID),  
    FOREIGN KEY (Education_ID) REFERENCES Education_Levels(Education_ID)  
); 
```

让我们在 Snowflake 中测试脚本，结果是“tada🎉”，所有表格在几秒钟内创建成功，没有任何失败：

![](img/6e4dff5805d5f1ff0c15c131c691e93d.png)

## 数据质量检查

除了“经典”的 NULL、重复或不存在的键检查外，我们还可以看到一些有趣的建议：

+   邮件格式化

+   “Annual_Performance”（1–5）或“Age”（18–65）的有效范围

+   “Salary”的正值（希望如此 😅）

```py
Data quality checks for each column of each table:

Confidential_Data
Employee_ID: Check for duplicates and NULL values
Email: Check for valid email format, duplicates, and NULL values
Salary: Check for negative values and NULL values
Annual_Performance: Check for values outside the valid range (e.g., 1-5) and NULL values

Employee_Details
Employee_ID: Check for duplicates and NULL values
First_Name: Check for non-alphabetic characters and NULL values
Last_Name: Check for non-alphabetic characters and NULL values
Age: Check for values outside the valid range (e.g., 18-65) and NULL values
Department_ID: Check for invalid department IDs and NULL values
Country_ID: Check for invalid country IDs and NULL values
Location_ID: Check for invalid location IDs and NULL values
Education_ID: Check for invalid education IDs and NULL values

Departments
Department_ID: Check for duplicates and NULL values
Department_Name: Check for duplicates and NULL values
Countries
Country_ID: Check for duplicates and NULL values
Country_Name: Check for duplicates and NULL values
Locations
Location_ID: Check for duplicates and NULL values
Location_Name: Check for duplicates and NULL values
Education_Levels
Education_ID: Check for duplicates and NULL values
Education_Level: Check for duplicates and NULL values
```

## GPT-4k 的结论：

+   识别分类数据和机密数据 ✅

+   建议数据库模型 ✅

+   创建表的 SQL 脚本 ✅

+   数据质量检查 ✅

## 这是一个无可挑剔的教程！🥇

![](img/2bd43dce9e6edca6587fc768ba463828.png)

# 分析结果（Google Bard）

## 识别分类数据和机密数据

*（我们将使用与 GPT-4k 相同的提示以确保一致性）*

令人惊讶的是，Bard 将“Employee_ID”、“First_Name”和“Last_Name”列视为分类数据（见下文），而根据我看来，它们应该被视为简单字符串列。

和 GPT-4K 一样，Bard 也将“Age”分类为公开信息，而我个人认为这是机密信息。

```py
Categorical columns:

Employee_ID: This is a unique identifier for each employee. It is a numeric column and should be unique for each employee.
First_Name: This is the employee's first name. It is a text column and should be alphabetic.
Last_Name: This is the employee's last name. It is a text column and should be alphabetic.

Department: This is the department where the employee works. It is a text column and should be one of the following values:
  Digital
  Manufacturing
  R&D
  Legal
  Human Ressources
  Procurement
  Sales
  Supply-Chain

Country: This is the country where the employee works. It is a text column and should be one of the following values:
  USA
  UK
  Canada

Location: This is the city where the employee works. It is a text column.

Education: This is the highest level of education the employee has completed. It is a text column and should be one of the following values:
  Bachelor's Degree
  Master's Degree
  Doctoral Degree
  Associate Degree
  Certificate

Confidential columns:

Salary: This is the employee's salary. It is a numeric column and should be kept confidential.
Annual_Performance: This is the employee's annual performance rating. It is a numeric column and should be kept confidential.
```

## 建议数据库模型

在这里，Bard 未能设计适当的数据模型，仅建议创建两个表：“employees”和“confidential_data”，这受到其先前回答的强烈影响：

```py
CREATE TABLE employees (
  employee_id INT NOT NULL AUTO_INCREMENT,
  first_name VARCHAR(255) NOT NULL,
  last_name VARCHAR(255) NOT NULL,
  department VARCHAR(255) NOT NULL,
  country VARCHAR(255) NOT NULL,
  location VARCHAR(255),
  education VARCHAR(255) NOT NULL,
  salary INT NOT NULL,
  annual_performance INT NOT NULL,
  PRIMARY KEY (employee_id)
);

CREATE TABLE confidential_data (
  employee_id INT NOT NULL,
  salary INT NOT NULL,
  annual_performance INT NOT NULL,
  CONSTRAINT fk_employees_confidential_data
    FOREIGN KEY (employee_id)
    REFERENCES employees (employee_id)
    ON DELETE CASCADE
);
```

## 创建表的 SQL 脚本

除了在“employees”表创建时使用“AUTO_INCREMENT”（这与 Snowflake 接受的语法不兼容），两个表创建正确。

# 数据质量检查

Bard 的建议更加“有限”，GPT-4k 提出的某些检查（如电子邮件格式、年龄在可接受范围内（18–65））被遗漏：

```py
Data quality checks for each column of each table:

Employee_ID:
The employee ID should be unique for each employee.
The employee ID should be a positive integer.

First_Name:
The first name should be a string of characters.
The first name should not be longer than 255 characters.

Last_Name:
The last name should be a string of characters.
The last name should not be longer than 255 characters.

Department:
The department should be one of the following values:
  Digital
  Manufacturing
  R&D
  Legal
  Human Ressources
  Procurement
  Sales
  Supply-Chain

Country:
The country should be one of the following values:
  USA
  UK
  Canada

Location:
The location can be any string of characters.

Education:
The education should be one of the following values:
  Bachelor's Degree
  Master's Degree
  Doctoral Degree
  Associate Degree
  Certificate

Salary:
The salary should be a positive integer.

Annual_Performance:
The annual performance should be a number between 1 and 5.
```

## Bard 的结论：

+   识别分类数据和机密数据 ❌

+   建议数据库模型 ❌

+   创建表的 SQL 脚本 ✅

+   数据质量检查 🔶

## 仍有改进空间 🤔

# 结论

GPT-4k 在理解数据集、设计适当的数据模型（这里是第 3 正规形态 (3NF)）、创建相应的 SQL 查询和建议数据质量检查方面确实优于 Bard：

![](img/84e5966c1ba93ffdc7baa5e61ed66624.png)

除了这两个模型的比较外，了解 LLM 如何加速数据公民的工作也很重要，正如本文所示，LLM 允许对数据集进行初步快速的深入分析，从而高效建模。

⏩ 对应的 [Jupyter Notebook](https://github.com/pierrelouisbescond/medium_articles/blob/main/medium_llm_db_retro_eng_load_and_format_data.ipynb) 和 [CSV 数据源](https://github.com/pierrelouisbescond/medium_articles/blob/main/Employees_Base.csv) 的链接。

和往常一样，我尽量识别所有必要的步骤，但如果这个教程中有任何遗漏的说明，请随时联系我！

也请随时浏览我在 Medium 上的其他贡献：

[## Pierre-Louis Bescond 在 Medium 上的文章

### 数据科学、机器学习与创新

[Pierre-Louis Bescond 在 Medium 上的文章](https://pl-bescond.medium.com/pierre-louis-besconds-articles-on-medium-f6632a6895ad?source=post_page-----2e2776e8af86--------------------------------)
