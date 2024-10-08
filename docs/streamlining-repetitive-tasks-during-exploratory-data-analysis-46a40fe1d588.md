# 在探索性数据分析中简化重复任务

> 原文：[`towardsdatascience.com/streamlining-repetitive-tasks-during-exploratory-data-analysis-46a40fe1d588`](https://towardsdatascience.com/streamlining-repetitive-tasks-during-exploratory-data-analysis-46a40fe1d588)

## 数据科学中的自动化

## 邀请你识别你的重复 EDA 任务，并创建一个自动化工作流，通过一个示例工具进行说明。

[](https://medium.com/@christabellecp?source=post_page-----46a40fe1d588--------------------------------)![Christabelle Pabalan](https://medium.com/@christabellecp?source=post_page-----46a40fe1d588--------------------------------)[](https://towardsdatascience.com/?source=post_page-----46a40fe1d588--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----46a40fe1d588--------------------------------) [Christabelle Pabalan](https://medium.com/@christabellecp?source=post_page-----46a40fe1d588--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----46a40fe1d588--------------------------------) ·7 min read·Oct 24, 2023

--

![](img/e5031188e988019bb8a2ffc2e978e858.png)

图片来源：作者 (DALL-E 生成)

## 编程原则：自动化单调的任务

人们常说懒惰的程序员是最好的程序员。然而，更准确的说法是，那些没有耐心处理重复工作流的程序员会投入前期时间来自动化他们能自动化的所有任务，以避免这些任务。简而言之，最好的程序员不会耐心重复单调的任务——他们会自动化它们。熟练的程序员之所以“懒惰”，是因为他们在前期投入时间创建工具，以便在未来节省精力。这可能意味着学习键盘快捷键、创建自定义模块或寻找聪明的软件来自动化工作流。

在一篇标题为“*为什么优秀的程序员懒惰和愚蠢*”的文章中，Philipp Lenssen 说道：

> “只有懒惰的程序员才会避免编写单调、重复的代码——从而避免冗余，这对软件维护和灵活重构来说是敌人 […] 为了让一个懒惰的程序员成为一个优秀的程序员，他（或她）也必须在学习如何*保持*懒惰时非常*不懒惰*——也就是说，哪些软件工具使他的工作更轻松，哪些方法可以避免冗余，以及他如何使自己的工作能够被轻松维护和重构。”

没有人喜欢枯燥单调的任务，如果有人发现自己在项目中重复相同的功能，这种总体的沮丧感就会开始缠绕他们，并低声耳语，“*将它们打包成模块。*”

![](img/f41e17bd46b6462380c7e6d015f0701d.png)

图片来源：作者

## **EDA 的重复性质**

这些低语在我探索性数据分析阶段确实出现了。

**探索性数据分析（EDA）** 涉及使用统计技术和可视化来研究数据、理解其结构、识别模式，并检测任何不规则性或异常值。通常，对于新的数据集需要相同的分析和可视化，因此 EDA 可以大大受益于自动化。

## **完全自动化的限制**

然而，在之前的尝试中，我每次都被阻碍，因为完全自动化受到了每个数据集独特挑战的限制，例如确定编码策略和确保数据类型正确。数据清洗过程与数据分析之间的相互作用是重复的，因此，很难完全标准化。

## **模块化方法**

为了解决这个限制，我创建了一个工具，假设数据已经经过了最小处理并具有正确的数据类型。它还需要定义数值列、分类列和目标列（假设我们在处理分类任务）。

## 它包含了什么？

+   数值和分类数据的高级统计

+   统计显著性测试

+   相关性热图

+   类别平均值

+   数据分布可视化

该函数还提供了可选参数的灵活性，以启用或禁用上述任何功能。

本文旨在展示创建定制化 EDA 工具的价值。虽然示例侧重于自动化摘要和可视化，但关键是识别您在重复 EDA 工作中的痛点，并将您自己的重复工作流程编码化。我将重点展示工具的关键功能和示例输出，而不是包含完整代码。

## 数据集

数据集已上传到 [Kaggle](https://www.kaggle.com/datasets/teamincribo/stroke-prediction)，目的是研究哪些因素可能预测患者是否会被诊断为中风。

![](img/903f5a05e2b1af50ad8c9254aa8f0788.png)

数据集的五个随机样本观察。作者提供的图片。

## **轻度预处理和特征工程**

我开始了这个过程：

+   从“胆固醇水平”中提取 HDL 和 LDL 胆固醇值

+   为每个症状生成二进制指示符列

+   通过标签编码将分类列和目标列转换为数值代码

```py
# Define a function to extract values from a column and convert to integer
def extract_and_convert(column, prefix):
    return column.str.extract(f'{prefix}(\d+)')[0].astype(int)

# Extract HDL and LDL values and add them as new columns
df['HDL'] = extract_and_convert(df['Cholesterol Levels'], 'HDL:')
df['LDL'] = extract_and_convert(df['Cholesterol Levels'], 'LDL:')
```

```py
# List of unique symptoms
unique_symptoms = ['Difficulty Speaking', 'Headache', 'Loss of Balance', 'Dizziness',
                   'Confusion', 'Seizures', 'Blurred Vision', 'Severe Fatigue',
                   'Numbness', 'Weakness']

# Create binary columns for each unique symptom indicating its presence in 'Symptoms'
df[unique_symptoms] = df['Symptoms'].str.contains('|'.join(unique_symptoms))
```

```py
# Convert categorical columns to numerical codes using label encoding
df[categorical_columns] = df[categorical_columns].apply(lambda x: pd.factorize(x)[0])

# Convert the target variable to numerical codes using label encoding
df[target] = pd.factorize(df[target])[0]
```

**轻度预处理数据的示例**

![](img/e5883ca955fbdb1bffd36896c342fe98.png)

特征工程后数据集的 5 个随机样本观察。作者提供的图片。

从这里，我需要采取两个步骤：

1.  定义数值列、分类列和目标列

1.  运行 `summary()` 并输入我希望看到的函数

# Summary()

## 定义数值列、分类列和目标列

```py
# Define numerical columns 
numerical_columns = ['age', 'bmi', 'glucose', 'stress', 'bp', 'hdl', 'ldl', ]

# Define categorical columns
categorical_columns = ['gender', 'hypertension', 'heart_dis', 'married', 'work', 'residence',
                       'smoker', 'alcohol', 'fitness', 'stroke_history', 'family_stroke_history',
                       'diet', 'speech', 'headache', 'balance', 'dizziness', 'confusion',
                       'seizures', 'vision', 'fatigue', 'numbness', 'weakness']

# Define target column
target = 'diagnosis'
```

在这篇文章中，我包含了更大的工具`summary()`，并排除了辅助函数：`calculate_entropy()`、`statistical_tests()`、`plot_distribution_plots()`、`plot_correlation_heatmap()`、`calculate_categorical_summary`、`calculate_numerical_summary()`。

## **Summary() 实现**

```py
def summary(df: pd.DataFrame, 
            numerical_columns: list, 
            categorical_columns: list, 
            target: str,
            categorical_summary: Optional[bool] = True, 
            numerical_summary: Optional[bool] = True,
            perform_tests: Optional[bool] = True, 
            plot_corr_heatmap: Optional[bool] = True,
            calculate_cat_averages: Optional[bool] = True, 
            plot_distribution: Optional[bool] = True) -> None:
    """
    Generate a summary of data exploration tasks.
    """
    df_numerical = df[numerical_columns]
    df_categorical = df[categorical_columns]

    # Join numerical and categorical columns together
    df_joined = df_numerical.join(df_categorical)
    df_joined[target] = df[target]

    if categorical_summary:
        print('\nCATEGORICAL SUMMARY')
        categorical_summary = calculate_categorical_summary(df_categorical)
        display(categorical_summary.round(2))

    if numerical_summary:
        print('\nNUMERICAL SUMMARY')
        numerical_summary = calculate_numerical_summary(df_numerical)
        display(numerical_summary.round(2))

    if perform_tests:
        print('\nSTATISTICAL TESTS')
        df_summary = statistical_tests(df, categorical_columns, numerical_columns, target)
        display(df_summary.round(2))

    if plot_corr_heatmap:
        plot_correlation_heatmap(df_joined)

    if calculate_cat_averages:
        for col in categorical_columns:
            display(df_joined.groupby(col).mean())

    if plot_distribution:
        plot_distribution_plots(df, categorical_columns + [target], numerical_columns)
```

## **类别和数值汇总**

该工具生成两个统计汇总——一个用于类别变量，一个用于数值变量。

**类别汇总**提供了对每个类别的高层次洞察，包括：

+   唯一值的数量

+   最频繁的值及其频率

+   缺失值的百分比

+   熵——分布的随机性测量

**数值汇总**计算常见的描述性统计数据，如：

+   唯一值的数量

+   缺失值的百分比

+   异常值的数量

+   集中趋势测量（均值，中位数）

+   离散度测量（标准差，最小/最大）

这种分解作为对类别和数值数据分布及完整性的快速评估。这些汇总有效地指出了需要更深入探索的领域，如显著的缺失数据或重要的异常值。总体而言，它们提供了数据集基本特征的全面快照。

例如，下文中可以明显看出血压数据中有四个异常值，其中一半的人群有中风病史，75%的患者表现出高血压。

![](img/a69696285941fc426eadeca2147a192d.png)

图片由作者提供。

## 统计测试

**统计测试汇总**包括统计测试结果，用于评估每个特征与目标变量之间的关系。该工具对类别变量运行**卡方检验**，对数值变量运行**双尾 t 检验**，以评估每个特征与目标之间的关系。

然而，这些测试有其局限性。它们检测线性相关性，但可能忽略非线性关联或变量之间的复杂交互。结果提供了识别潜在预测特征的起点，但需要进一步分析来揭示细致的关系。因此，自动化测试加速了初步特征筛选，但应结合更深入的技术，如多变量建模和集成方法，以获得进一步的见解。

![](img/1abc5234ce86e89fc9ff55a3c5b07b2e.png)

图片由作者提供。

## 相关热图

这种可视化突出了数值变量、序数变量和目标变量之间的斯皮尔曼相关性。选择斯皮尔曼相关性是因为它在捕捉各种类型关系方面更具鲁棒性。与皮尔逊相关性不同，斯皮尔曼的相关性是非参数的，适用于序数、类别或非线性关系。

![](img/88523ee5d5350c3be2eef513faef1ebb.png)

斯皮尔曼相关热图。图片由作者提供。

## 图示

对于分布可视化，`summary()`将返回类别变量的条形图以及数值变量的直方图和箱型图。分布可视化可以揭示数据应如何分离和处理不同，并可能突出质量保证（QA）问题或异常。

![](img/0f0ef7cd80427831da5eaa8790fe3d74.png)

类别数据的条形图。图片作者提供。

![](img/22e8546b42e34f9d287de48917b511f6.png)

直方图、箱型图以及不包含异常值的箱型图用于数值数据。图片作者提供。

## 总结

本文展示了一个以自动生成统计摘要、可视化和基本特征分析为重点的示例 EDA 工具。虽然不全面，但它允许快速探索新数据集，并揭示洞察以指导更有针对性的分析。通过一些定制，这些工具可以适应不同领域或业务背景的典型探索性工作流程。

关键在于识别流程中的冗余，并提前花时间将工作流程编码化。这会随着时间的推移积累，使你能够将认知资源集中在更高价值的领域，如领域知识、特征工程和建模。简而言之——创建你的工具，自动化重复工作，让自动化处理繁重的工作，以便你可以专注于艺术。
