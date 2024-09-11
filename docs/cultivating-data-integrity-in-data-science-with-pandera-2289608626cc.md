# 通过 Pandera 培养数据科学中的数据完整性

> 原文：[https://towardsdatascience.com/cultivating-data-integrity-in-data-science-with-pandera-2289608626cc?source=collection_archive---------9-----------------------#2023-12-22](https://towardsdatascience.com/cultivating-data-integrity-in-data-science-with-pandera-2289608626cc?source=collection_archive---------9-----------------------#2023-12-22)

## 使用 Pandera 的高级验证技术以促进数据质量和可靠性

[](https://medium.com/@le_Tomassini?source=post_page-----2289608626cc--------------------------------)[![Alessandro Tomassini](../Images/e5bf527fafd933239bff6b87005ba457.png)](https://medium.com/@le_Tomassini?source=post_page-----2289608626cc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2289608626cc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2289608626cc--------------------------------) [Alessandro Tomassini](https://medium.com/@le_Tomassini?source=post_page-----2289608626cc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4cb5c2b60ac8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcultivating-data-integrity-in-data-science-with-pandera-2289608626cc&user=Alessandro+Tomassini&userId=4cb5c2b60ac8&source=post_page-4cb5c2b60ac8----2289608626cc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2289608626cc--------------------------------) ·6 分钟阅读·2023年12月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2289608626cc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcultivating-data-integrity-in-data-science-with-pandera-2289608626cc&user=Alessandro+Tomassini&userId=4cb5c2b60ac8&source=-----2289608626cc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2289608626cc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcultivating-data-integrity-in-data-science-with-pandera-2289608626cc&source=-----2289608626cc---------------------bookmark_footer-----------)![](../Images/496ecd9799362bd2389967b06cf79fa1.png)

由 DALL-E 生成的图像

欢迎进入数据验证的探索之旅，通过 Pandera 这个在数据科学家工具箱中较少为人知却强大的工具。本教程旨在为那些希望通过强大的验证技术来强化其数据处理管道的人们照亮道路。

Pandera 是一个 Python 库，为 pandas 数据结构提供灵活而表达力强的数据验证。它旨在为数据处理步骤带来更多的严谨性和可靠性，确保你的数据在进行分析或建模之前符合指定的格式、类型和其他约束。

# **为什么选择 Pandera？**

在数据科学的复杂织锦中，数据是基本的线索，确保其质量和一致性至关重要。Pandera 通过严格的验证促进数据的完整性和质量。它不仅仅是检查数据类型或格式；Pandera 扩展其警觉性到更复杂的统计验证，使其成为你数据科学工作中的不可或缺的盟友。具体来说，Pandera 的亮点包括：

1.  **模式强制**：确保你的 DataFrame 遵守预定义的模式。

1.  **可定制验证**：支持创建复杂的自定义验证规则。

1.  **与 Pandas 的集成**：与现有的 pandas 工作流程无缝对接。

# 创建你的第一个模式

![](../Images/f3f8e4e5353cdb195396a0180b6b5cc3.png)

[charlesdeluvio](https://unsplash.com/@charlesdeluvio?utm_source=medium&utm_medium=referral) 提供的照片，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

让我们开始安装 Pandera。可以使用 pip 完成：

```py
pip install pandera 
```

Pandera 中的模式定义了你的 DataFrame 的预期结构、数据类型和约束。我们将从导入必要的库和定义一个简单的模式开始。

```py
import pandas as pd
from pandas import Timestamp
import pandera as pa
from pandera import Column, DataFrameSchema, Check, Index

schema = DataFrameSchema({
    "name": Column(str),
    "age": Column(int, checks=pa.Check.ge(0)),  # age should be non-negative
    "email": Column(str, checks=pa.Check.str_matches(r'^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$'))  # email format
})
```

这个模式指定了我们的 DataFrame 应该有三列：`name`（字符串）、`age`（整数，非负）和 `email`（字符串，匹配电子邮件的正则表达式）。现在，既然我们已经有了模式，让我们验证一下 DataFrame。

```py
# Sample DataFrame
df = pd.DataFrame({
    "name": ["Alice", "Bob", "Charlie"],
    "age": [25, -5, 30],
    "email": ["alice@example.com", "bob@example", "charlie@example.com"]
})

# Validate
validated_df = schema(df)
```

在这个示例中，Pandera 将引发 `SchemaError`，因为 Bob 的年龄是负数，这违反了我们的模式。

```py
SchemaError: <Schema Column(name=age, type=DataType(int64))> failed element-wise validator 0:
<Check greater_than_or_equal_to: greater_than_or_equal_to(0)>
failure cases:
   index  failure_case
0      1            -5
```

Pandera 的一个优势是能够定义自定义验证函数。

```py
@pa.check_input(schema)
def process_data(df: pd.DataFrame) -> pd.DataFrame:
    # Some code to process the DataFrame
    return df

processed_df = process_data(df)
```

`@pa.check_input` 装饰器确保输入的 DataFrame 在函数处理之前符合模式。

# 使用自定义检查进行高级数据验证

![](../Images/63a5c18f885fb03d8561166b8c92ace5.png)

[Sigmund](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral) 提供的照片，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

现在，让我们深入了解 Pandera 提供的更复杂的验证。基于现有模式，我们可以添加具有各种数据类型和更复杂检查的附加列。我们将引入分类数据、日期时间数据的列，并实施更高级的检查，比如确保唯一值或引用其他列。

```py
# Define the enhanced schema
enhanced_schema = DataFrameSchema(
    columns={
        "name": Column(str),
        "age": Column(int, checks=[Check.ge(0), Check.lt(100)]),
        "email": Column(str, checks=[Check.str_matches(r'^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$')]),
        "salary": Column(float, checks=Check.in_range(30000, 150000)),
        "department": Column(str, checks=Check.isin(["HR", "Tech", "Marketing", "Sales"])),
        "start_date": Column(pd.Timestamp, checks=Check(lambda x: x < pd.Timestamp("today"))),
        "performance_score": Column(float, nullable=True)
    },
    index=Index(int, name="employee_id")
)

# Custom check function
def salary_age_relation_check(df: pd.DataFrame) -> pd.DataFrame:
    if not all(df["salary"] / df["age"] < 3000):
        raise ValueError("Salary to age ratio check failed")
    return df

# Function to process and validate data
def process_data(df: pd.DataFrame) -> pd.DataFrame:
    # Apply custom check
    df = salary_age_relation_check(df)

    # Validate DataFrame with Pandera schema
    return enhanced_schema.validate(df)
```

在这个增强的模式中，我们添加了：

1.  分类数据：`department` 列验证特定类别。

1.  日期时间数据：`start_date` 列确保日期在过去。

1.  可空列：`performance_score` 列可以有缺失值。

1.  索引验证：定义了一个类型为整数的索引`employee_id`。

1.  复杂检查：一个自定义函数`salary_age_relation_check`确保每个部门内薪资与年龄之间的逻辑关系。

1.  在数据处理函数中实现自定义检查：我们将`salary_age_relation_check`逻辑直接集成到我们的数据处理函数中。

1.  使用 Pandera 的`validate`方法：我们没有使用`@pa.check_types`装饰器，而是手动使用 Pandera 提供的`validate`方法来验证 DataFrame。

现在，让我们创建一个示例 DataFrame `df_example`，它符合我们增强模式的结构和约束，并对其进行验证。

```py
df_example = pd.DataFrame({
    "employee_id": [1, 2, 3],  
    "name": ["Alice", "Bob", "Charlie"],  
    "age": [25, 35, 45],  
    "email": ["alice@example.com", "bob@example.com", "charlie@example.com"],  
    "salary": [50000, 80000, 120000], 
    "department": ["HR", "Tech", "Sales"], 
    "start_date": [Timestamp("2022-01-01"), Timestamp("2021-06-15"), Timestamp("2020-12-20")], 
    "performance_score": [4.5, 3.8, 4.2]  
})

# Make sure the employee_id column is the index
df_example.set_index("employee_id", inplace=True)

# Process and validate data
processed_df = process_data(df_example)
```

在这里，Pandera 会因为`enhanced_schema`中的`salary`列预期的数据类型（`float`，对应于 pandas/Numpy 类型中的`float64`）与`df_example`中实际的数据类型（`int`或`int64`，在 pandas/Numpy 类型中）不匹配而引发`SchemaError`。

```py
SchemaError: expected series 'salary' to have type float64, got int64
```

# 具有统计假设检验的高级数据验证

![](../Images/c642d2aab10a659a3daeb1fb89ec761e.png)

由[Daniela Paola Alchapar](https://unsplash.com/@paoalchapar?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Pandera 可以在验证过程中执行统计假设检验。这个功能对于验证数据分布或变量之间关系的假设特别有用。

假设你想确保数据集中的平均薪资接近某个值，例如 £75,000。可以定义一个自定义检查函数来执行单样本 t 检验，以评估样本的均值（例如数据集中薪资的均值）是否与已知均值（在我们的例子中为 £75,000）显著不同。

```py
from scipy.stats import ttest_1samp

# Define the custom check for the salary column
def mean_salary_check(series: pd.Series, expected_mean: float = 75000, alpha: float = 0.05) -> bool:
    stat, p_value = ttest_1samp(series.dropna(), expected_mean)
    return p_value > alpha

salary_check = Check(mean_salary_check, element_wise=False, error="Mean salary check failed")

# Correctly update the checks for the salary column by specifying the column name
enhanced_schema.columns["salary"] = Column(float, checks=[Check.in_range(30000, 150000), salary_check], name="salary")
```

在上述代码中，我们有：

1.  定义了自定义检查函数`mean_salary_check`，它接受一个 pandas Series（我们 DataFrame 中的`salary`列），并对预期均值进行 t 检验。如果 t 检验的 p 值大于显著性水平（alpha = 0.05），则函数返回`True`，表示平均薪资与 £75,000 并无显著差异。

1.  然后，我们将这个函数包装在 Pandera 的`Check`中，指定`element_wise=False`以表示检查应用于整个系列，而不是逐个元素地应用。

1.  最后，我们更新了 Pandera 模式中的`salary`列，以包括这个新的检查以及任何现有的检查。

通过这些步骤，我们的 Pandera 模式现在包括对`salary`列的统计测试。我们故意提高`df_example`中的平均薪资以违反模式的预期，以便 Pandera 引发`SchemaError`。

```py
# Change the salaries to exceede the expected mean of £75,000
df_example["salary"] = df_example["salary"] = [100000.0, 105000.0, 110000.0]
validated_df = enhanced_schema(df_example)
```

```py
SchemaError: <Schema Column(name=salary, type=DataType(float64))> failed series or dataframe validator 1:
<Check mean_salary_check: Mean salary check failed>
```

# 结论

Pandera 将数据验证从一个普通的检查点提升为一个动态过程，涵盖了甚至复杂的统计验证。通过将 Pandera 集成到你的数据处理流程中，你可以早期发现不一致性和错误，节省时间，避免未来的麻烦，并为更可靠和深入的数据分析铺平道路。

# 参考文献和进一步阅读

对于那些希望深入了解 Pandera 及其功能的人来说，以下资源是很好的起点：

1.  Pandera 文档：全面介绍 Pandera 的所有功能和特性 ([Pandera 文档](https://pandera.readthedocs.io/))。

1.  Pandas 文档：由于 Pandera 扩展了 pandas，因此熟悉 pandas 非常重要 ([Pandas 文档](https://pandas.pydata.org/docs/))。

# 免责声明

我与 Pandera 没有任何关联，只是对它非常热情 :)
