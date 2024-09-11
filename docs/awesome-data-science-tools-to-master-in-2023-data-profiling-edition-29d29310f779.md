# 2023年值得掌握的精彩数据科学工具：数据分析版

> 原文：[https://towardsdatascience.com/awesome-data-science-tools-to-master-in-2023-data-profiling-edition-29d29310f779?source=collection_archive---------0-----------------------#2023-02-22](https://towardsdatascience.com/awesome-data-science-tools-to-master-in-2023-data-profiling-edition-29d29310f779?source=collection_archive---------0-----------------------#2023-02-22)

## *数据工具回顾*

## 5个开源Python包用于EDA和可视化

[](https://medium.com/@miriam.santos?source=post_page-----29d29310f779--------------------------------)[![Miriam Santos](../Images/decbc6528a641e7b02934a03e136284a.png)](https://medium.com/@miriam.santos?source=post_page-----29d29310f779--------------------------------)[](https://towardsdatascience.com/?source=post_page-----29d29310f779--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----29d29310f779--------------------------------) [Miriam Santos](https://medium.com/@miriam.santos?source=post_page-----29d29310f779--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F243289394aaa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fawesome-data-science-tools-to-master-in-2023-data-profiling-edition-29d29310f779&user=Miriam+Santos&userId=243289394aaa&source=post_page-243289394aaa----29d29310f779---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----29d29310f779--------------------------------) ·15分钟阅读·2023年2月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F29d29310f779&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fawesome-data-science-tools-to-master-in-2023-data-profiling-edition-29d29310f779&user=Miriam+Santos&userId=243289394aaa&source=-----29d29310f779---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F29d29310f779&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fawesome-data-science-tools-to-master-in-2023-data-profiling-edition-29d29310f779&source=-----29d29310f779---------------------bookmark_footer-----------)

*这是一个专注于数据科学开源工具的系列专栏：每篇文章都关注一个特定主题，并向读者介绍一系列不同的工具，展示它们在真实世界数据集上的特性。* ***这一部分专注于数据分析******并回顾了***`*ydata-profiling*`*、* `*dataprep*`*、* `*sweetviz*`*、* `*autoviz*`* 和* `*lux*`*。***鼓励读者跟随教程：*** *我会参考所有项目的单独GitHub仓库，但工具的整理列表以及本文章中使用的Google Colab笔记本* ***可以在*** [***awesome-data-centric-ai***](https://github.com/Data-Centric-AI-Community/awesome-data-centric-ai) ***仓库中找到。***

在***数据不完善的世界中，精心设计的数据理解工具就是哲学家的石头。***

![](../Images/4b29e5607c38a5b9f16bd511633cb65c.png)

“多维”的哲学家石头：数据立方体。分析数据通常涉及旋转和扭曲数据集，以找到最有洞察力的视角。照片来自 [aaron boris](https://unsplash.com/@aaron_boris?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

数据质量在我们的机器学习模型结果中发挥着至关重要的作用。一些 [数据缺陷](https://medium.com/towards-data-science/data-quality-issues-that-kill-your-machine-learning-models-961591340b40) 可能严重影响模型的内部工作，使其无法应用（例如，数据缺失）。其他缺陷可能在模型开发阶段未被发现，并在模型部署到生产环境后带来不良后果（例如，类别不平衡、数据不足、数据集漂移）。

在经过长期艰苦的努力以产生更强健的机器学习模型之后，**学术界和工程师们现在都在转向以数据为中心的人工智能范式**。我们共同意识到数据可以成就或毁掉我们的模型，并且很多时候，如果数据*聪明*，问题可能通过“更简单”的模型得到解决。

*但我们如何将数据从不完善变为聪明呢？*

# 数据分析是数据理解的精髓

*由于模型由数据驱动，而数据由人们整理，因此人们需要理解他们让模型处理的数据的特性。*

数据分析与探索性数据分析的概念密切相关。然而，谈到*分析*数据时，我们倾向于将其与**自动化报告——即“获取*分析*”——数据特性**联系在一起，理想情况下，这能立即提醒我们潜在问题或进一步的问题。

**更重要的是，数据分析是数据团队中所有角色必须掌握的一项重要技能，包括数据科学家、机器学习工程师、数据工程师和数据分析师。**

*“我们的填补方法返回了高偏差值，输入发生了变化吗？模型输出了无效预测，我们需要快速调整，它发生了什么？今天这个数据流完全崩溃了，出了什么问题？我需要这些结果在仪表板上，以便在下次会议中讨论，有人能给我一个当前数据状态的快照吗？”*

这些只是数据团队在实际环境中面临的许多“数据难题”中的一部分。

**幸运的是，可以借助数据分析工具将这些问题最小化。**

*让我们通过探索一个真实世界的用例来看看？准备好打开你的Google Colabs吧！*

# HCC数据集……稍作修改！

在本文中，我将使用HCC数据集，该数据集是我在硕士论文期间亲自整理的。你可以在[Kaggle](https://www.kaggle.com/datasets/mrsantos/hcc-dataset)和[UCI Repository](https://archive.ics.uci.edu/ml/datasets/HCC+Survival)上找到它。随意使用它，该数据集已授权，我们只希望适当引用。

**为了此次评审的目的，我从原始数据集中选择了一个特征子集，并通过人工引入一些问题进一步修改了数据。目的是查看不同的数据分析工具如何识别和表征这些问题。**

这里是我们将使用的数据的详细信息：

+   下面考虑了原始特征的子集：`Gender`、`Age`、`Alcohol`、`Hallmark`、`PS`、`Encephalopathy`、`Hemoglobin`、`MCV`、`Total_Bil`、`Dir_Bil`、`Ferritin`、`HBeAg`和`Outcome`。本质上，我选择了一组具有和不具有缺失值的数值和分类（名义和二元）特征，一些类别有欠代表性，还有一些高度相关；

+   原始的`MCV`包含一些缺失值，这些值在特征中被其他值替代（只是为了拥有另一个完整的数值特征，除了`Age`）；

+   `O2`特征是人工引入的：它仅包含“999”值，代表数据采集或收集中的错误。想象一下，一个传感器发生故障，开始输出荒谬的值，或者数据采集人员决定用“999”编码他们的缺席；

+   `Hallmark`被修改了：我将其值与“A、B、C、D、（…）”编码连接，这可能代表了患者ID与实际标志结果的连接。这代表了数据处理或存储中的错误（如果你愿意，可以称之为“ETL故障”）；

+   `HBeAg`也被修改了。在这种情况下，我只是删除了所有“是”值。这可能模拟了[缺失非随机机制](https://ieeexplore.ieee.org/document/8605316)，如果观察到，所有缺失值都会是“是”。

由于数据包含一些修改，我也[已将其添加到存储库](https://github.com/Data-Centric-AI-Community/awesome-data-centric-ai/tree/master/medium/data-profiling-tools/data)中。请记住，这纯粹是为了学术讨论：您可以在Kaggle或UCI存储库中访问完整且未修改的数据，如前所述。

*现在，事不宜迟，让我们开始实际的代码吧！*

# 1\. ydata-profiling

你可能知道它为`pandas-profiling`，因为新名称仍然很新：简而言之，版本4.0.0现在还支持Spark DataFrames，除了Pandas DataFrames，因此它更名为[YData Profiling](https://github.com/ydataai/ydata-profiling)。

该包目前是探索性数据分析的热门选择。它的高效性和简单性似乎赢得了技术和非技术观众的喜爱，因为它能快速而直接地对数据进行可视化理解。

让我来展示如何让它迅速上手！首先，安装该包：

```py
pip install ydata-profiling=4.0.0
```

然后，生成数据概况报告是直接的：

```py
# Import libraries
import pandas as pd
from ydata_profiling import ProfileReport

# Load the data
df = pd.read_csv("hcc.csv")

# Produce and save the profiling report
profile = ProfileReport(df,title="HCC Profile Report")
profile.to_file("report.html")
```

报告迅速生成，内容包括数据集属性的一般概述、每个特征的汇总统计、交互和相关性图，以及缺失值和重复记录的有见地可视化：

![](../Images/6483c24ed9cd908b648aa3e5cdfbae54.png)

YData Profiling报告。作者录屏。

然而，`ydata-profiling`最受赞誉的功能可能是**自动检测潜在的数据质量问题**。

在处理我们没有任何先前洞察的新数据集时，这一点尤为重要。它节省了我们大量时间，因为它会立即突出数据不一致性和其他复杂的数据特征，这些特征我们可能希望在模型开发之前进行分析。

关于我们的用例，请注意以下警报的生成方式：

+   `常量`：HBeAg和02；

+   `高相关性`：总胆红素和直接胆红素，以及脑病和总胆红素之间的相关性；

+   `不平衡`：脑病；

+   `缺失`：血红蛋白、HBeAg、总胆红素、直接胆红素和铁蛋白的缺失数据百分比最高（接近50%）；

+   `均匀`、`唯一`和`高基数`关于Hallmark。

由于对数据质量问题的全面支持，`ydata-profiling`能够检测所有引入的不一致性，特别是它为同一特征生成多个不同的警报。

例如，*Hallmark如何同时引发高基数、唯一和均匀的警报？*

这立即引发了红色警报，使数据科学家怀疑可能与某种ID有关。进一步检查该特征时，这种“数据气味”会变得相当明显：

![](../Images/63c37486a9e0167c7b053fd1163479ef.png)

YData Profiling报告：检查Hallmark。作者图片。

HBeAg也是如此：它引发了`CONSTANT`和`MISSING`警报，这可能会在经验丰富的数据科学家脑海中响起警钟：更复杂的缺失机制可能在发挥作用。

`ydata-profiling`还具有其他几个有用的功能，如**支持时间序列数据和数据集并排比较**，我们可以利用这些功能来增强数据集上的一些数据质量转换，例如缺失数据插补。

为了进一步调查这个可能性，我对`Ferritin`进行了简单的插补（例如，均值插补），并生成了原始数据和插补数据之间的对比报告。结果如下：

![](../Images/2d0a2deb73ae6cf9d7e6c3ddab1bf752.png)

YData Profiling: 对比报告。作者录制的视频。

`ydata-profiling`的源代码、文档和几个示例可以在这个[GitHub仓库](https://github.com/ydataai/ydata-profiling)中找到。你可以使用这个[Google Colab笔记本](https://colab.research.google.com/github/Data-Centric-AI-Community/awesome-data-centric-ai/blob/master/medium/data-profiling-tools/notebooks/ydata_profiling_demo.ipynb)复制上面的示例，并进一步探索[包的附加功能](https://medium.com/ydata-ai/auditing-data-quality-with-pandas-profiling-b1bf1919f856)。

# 2\. DataPrep

[DataPrep](https://github.com/sfu-db/dataprep)还可以通过一行代码创建交互式数据分析报告。安装相当简单，模块导入后，通过调用`create_report`来生成报告：

```py
pip install -U dataprep

import pandas as pd
from dataprep.eda import create_report

df = pd.read_csv("hcc.csv")
create_report(df)
```

就整体外观和感觉而言，`dataprep`似乎在之前的包基础上进行了广泛构建，包括类似的汇总统计和可视化（报告的相似性令人惊讶！）：

![](../Images/fa08f854e278c25982a33e8b2d687d4b.png)

DataPrep Profiling Report。作者录制的视频。

然而，这个包的一个有用特性是**更深入探索特征互动的能力**。

我们可以使用几种类型的图表探索特征之间的有趣互动（包括数值型和分类特征）。

这是探索几种特征类型之间关系的方法：

```py
from dataprep.eda import plot

plot(df, "Age", "Gender") # Numeric - Categorical
plot(df, "Age", "MCV") # Numeric - Numeric
plot(df, "Gender", "Outcome") # Categorical - Categorical
```

![](../Images/c43180db193cf11307c0bf13a4812907.png)

DataPrep Profiling Report: 探索特征互动。作者录制的视频。

`dataprep`还允许调查缺失值的影响（比较删除缺失值前后的数据）在所有特征上的影响。

这是`Ferritin`缺失值处理影响的可视化：

![](../Images/fc9ce10f7595d1908b684e53b1748545.png)

DataPrep Profiling Report: 缺失的铁蛋白影响。作者录制的视频。

你可以在相应的 [Colab 笔记本](https://colab.research.google.com/github/Data-Centric-AI-Community/awesome-data-centric-ai/blob/master/medium/data-profiling-tools/notebooks/dataprep_demo.ipynb) 中探索所有生成的可视化。`dataprep` 在 [GitHub](https://github.com/sfu-db/dataprep) 上有很好的文档，也有一个 [专门的网站](https://dataprep.ai)，如果你想查看更多资源和细节。

# 3\. SweetViz

类似于之前的工具，[SweetViz](https://github.com/fbdesignpro/sweetviz) 也通过生成简单的可视化和总结重要特征统计信息来提升 EDA。你可以按照以下步骤开始使用 `sweetviz`：

```py
pip install sweetviz

# Import libraries
import pandas as pd
import sweetviz as sv

# Load the data
df = pd.read_csv("hcc.csv")

# Produce and save the profiling report
report = sv.analyze(df)
report.show_html('report.html')
```

`sweetviz` 有一种更“复古”的外观，汇总统计信息的显示不如我们迄今评审的其他包那样整洁。

关于数据质量，它会提醒缺失值的存在及其对数据的影响（例如，根据缺失数据的百分比使用绿色、黄色和红色高亮显示）。其他不一致之处不会直接高亮显示，可能会被忽视（低相关值则在每个特征的详细信息中标出）。

除了“关联”矩阵，它并不专注于深入探索特征交互或缺失数据行为。

这是报告的外观和感觉：

![](../Images/f9619c1e996ec2a4759624b7bd05e6e2.png)

Sweetviz 分析报告。作者提供的屏幕录像。

然而，它的最大特点是围绕 **可视化目标值** 和 **比较数据集** 构建的。

在这方面，`sweetviz` 的一个有趣用例是同时进行 *目标分析*（研究目标值如何与数据中的其他特征相关）同时比较 *不同的数据集*（例如，训练集与测试集）或 *数据集内特征特征*（例如，比较“男性”与“女性”组）。

这是调整“Outcome”特征以探索类别（“男性”和“女性”）并比较见解的方法：

```py
# Create a 'Survival' feature based on 'Outcome'
df.Outcome = pd.Categorical(Outome)
df['Survival'] = df.Outcome.cat.codes

# Create a comparison report
comparison_report = sv.compare_intra(df, df["Gender"] == 'Male', ["Male", "Female"], 'Survival')
comparison_report.show_notebook()
```

![](../Images/83e9b481e663c3dd7111755e779db73b.png)

Sweetviz 分析报告：比较“男性”和“女性”子组。作者提供的屏幕录像。

`sweetviz` 的所有文档都可以在这个 [GitHub 仓库](https://github.com/fbdesignpro/sweetviz) 中找到。请随时使用 [这个笔记本](https://colab.research.google.com/github/Data-Centric-AI-Community/awesome-data-centric-ai/blob/master/medium/data-profiling-tools/notebooks/sweetviz_demo.ipynb) 对数据进行进一步转换，并借助 [这篇专门的帖子](/powerful-eda-exploratory-data-analysis-in-just-two-lines-of-code-using-sweetviz-6c943d32f34) 探索其他用例。

# 4\. AutoViz

[AutoViz](https://github.com/AutoViML/AutoViz) 是另一个简单直观的 EDA 包，提供广泛的可视化和自定义支持。

与之前的包类似，它的安装也非常简单，如下所示：

```py
pip install autoviz
```

然后你可以导入该包，但`autoviz`不会自动显示图表，因此你需要在尝试对数据进行分析之前运行`%matplotlib inline`：

```py
from autoviz.AutoViz_Class import AutoViz_Class
%matplotlib inline
```

现在你可以开始了！运行以下命令将生成大量可视化图表，你可以在之后进行自定义和调整：

```py
AutoViz_Class().AutoViz('hcc.csv')
```

![](../Images/79c36a51ad02ef2db5b1b248c7a0830c.png)

AutoViz分析报告。作者录屏。

`autoviz`的有趣特点包括**自动数据清理建议**和**执行“监督”可视化**的能力——即通过给定的目标特征对结果进行分类，这与我们在`sweetviz`中所做的类似。

数据清理建议在报告生成期间提供，如下所示：

![](../Images/6f4886c97ac4b13ac1190d9e6c66057e.png)

AutoViz分析报告：清理建议。作者提供的图像。

至于清理建议，`autoviz`做得非常好：

+   它将`Hallmark`与“可能的ID列”关联，建议将其删除；

+   它识别出多个缺失或偏斜的特征，如`Ferritin`、`Hemoglobin`、`Total_Bil`、`Dir_Bil`、`Encephalopathy`和`HBeAg`；

+   它识别出`HBeAg`和`O2`具有不变的值，并建议删除`O2`。

生成的警报不如`ydata-profiling`生成的全面，并且不如`autoviz`直观（例如，`autoviz`展示了唯一值的热图，但其他警报如`HIGH CARDINALITY`、`UNIQUE`或`CONSTANT`可能对指出我们面临的具体问题更有帮助）。

然而，我非常欣赏它在清理建议方面引入的可解释性，这对数据科学领域的新手无疑是一个有用的指南。

如果你想尝试我之前提到的“监督”分析，你首先需要定义一个`depVar`参数作为目标特征。在这种情况下，我将其设置为“结果”，但我也可以将其设置为“性别”，以获取类似于`sweetviz`返回的图表：

```py
import pandas as pd
df = pd.read_csv("hcc.csv")

AV = AutoViz_Class()
dft = AV.AutoViz(
    filename="",
    sep="",
    depVar="Outcome",
    dfte=df,
    header=0,
    verbose=1,
    lowess=False,
    chart_format="png",
    max_rows_analyzed=200,
    max_cols_analyzed=30,
)
```

这是考虑到“结果”生成的报告：

![](../Images/199444916c6af662e096964f5e922125.png)

AutoViz分析报告：比较“结果”类别。作者录屏。

你可以玩一下[这个 Google Colab 笔记本](https://colab.research.google.com/github/Data-Centric-AI-Community/awesome-data-centric-ai/blob/master/medium/data-profiling-tools/notebooks/autoviz_demo.ipynb)，探索`autoviz`的其他功能。你还可以查看[这篇帖子](/autoviz-a-new-tool-for-automated-visualization-ec9c1744a6ad)，其中详细介绍了这些功能，或者参考[GitHub上的文档](https://github.com/AutoViML/AutoViz)。

# 5\. Lux

[Lux](https://github.com/lux-org/lux) 以可能最适合初学者的方式启用快速且简单的 EDA，因为它可以通过简单地创建 Pandas DataFrame 使用：一旦你安装它，只需打印出一个 DataFrame，它将自动推荐一组最适合发现数据中有趣趋势和模式的可视化。

从安装软件包并读取数据开始：

```py
pip install lux

import lux
import pandas as pd
df = pd.read_csv("hcc.csv")
df
```

然后，你可以浏览一个互动小部件，选择最适合你需求的。它的外观如下：

![](../Images/4b0f88df345d4846493e422668e6c094.png)

Lux Profiling Report。作者录制的演示。

该软件包没有分析任何潜在的数据质量问题，分析也不是很全面，但仍涵盖了基础内容。

该小部件在 Pandas 和 Lux 之间切换，关注 3 个主要可视化：**Correlation and Distribution** 用于数值特征，**Occurrence** 用于分类特征。

一个可能被低估的功能是可以直接从小部件中导出或删除可视化，通过选择所需的图表。当生成快速报告时，这种简便性实际上非常有价值。

`lux` 还试图在 EDA 过程中支持我们。假设我们对进一步探索特定特征感兴趣，例如“PS”和“Encephalopathy”。我们可以指定你的“意图”，`lux` 将引导我们向潜在的下一步：

```py
df.intent = ["PS", "Encephalopathy"]
```

新生成的推荐分为 **Enhance**、**Filter** 和 **Generalize** 标签。

**Enhance tab** 为“意图特征”添加了额外的可视化功能，实质上是探索额外维度可能引入的信息类型。

**Filter tab** 是不言而喻的：它保持意图特征不变，并在可视化中引入如 `Gender = Male` 或 `Outcome = Alive` 的过滤器。

最终，**Generalize tab** 显示意图特征，以确定是否可以得出更一般的趋势：

![](../Images/314f0fc7660c7e95a53348f5ebaa6bf0.png)

Lux Profiling Report：探索“意图”功能。作者录制的演示。

随意浏览 [我的 Colab 笔记本](https://colab.research.google.com/github/Data-Centric-AI-Community/awesome-data-centric-ai/blob/master/medium/data-profiling-tools/notebooks/lux_demo.ipynb) 并进行你自己的示例操作。**Lux** 的 [文档页面](https://lux-api.readthedocs.io/en/latest/index.html) 和 [GitHub 仓库](https://github.com/lux-org/lux) 都相当全面，但可以查看 [这篇文章](https://medium.com/analytics-vidhya/lets-discover-the-data-intelligently-using-lux-a40f1c63995d) 来填补任何额外的空白。

# 最终思考：是否有最佳方式？

*公平地说，不能。列出的工具都有相似之处，同时也引入了不同的风格。*

除了 `lux` 外，其他剩余的软件包对于基本的探索性数据分析都非常合适。

根据你的专业水平、特定用例或你希望通过数据实现的目标，你可能会想尝试不同的软件包，甚至将它们的功能结合起来。

总的来说，这就是我的看法：

+   `ydata-profiling` 是专业数据科学家希望掌握新数据集的最佳选择，因为它**广泛支持自动数据质量警报**。由于对 Spark DataFrames 的新支持，该软件包对于需要排查数据流的数据工程师以及需要理解模型为何表现不稳定的机器学习工程师也极其有用。**对于需要了解数据行为的经验丰富的数据专业人员来说，这是一个极好的选择，特别是在数据质量方面**。提供的可视化也是一个很好的资产，但该软件包在互动性方面还可以改进（例如，允许我们“鼠标悬停”在图表上并检查特定特征或相关值）。

+   `dataprep` 似乎是建立在 `ydata-profiling` 之上的，这意味着两个项目的路线图之间可能会存在不匹配（例如，后者在数据质量警报支持方面可能比前者更快地增加更多功能），尤其是在关注支持数据质量警报的方面。或者相反，这取决于我们感兴趣跟进哪些组件！然而，**我确实喜欢那些启用互动的额外功能，但我会说在某些情况下，“少即是多”**。如果你是经验丰富的数据科学家，你会知道更深入地研究哪些内容，哪些可以丢弃，但当你刚进入这个领域时就不一定如此。你可能会花费相当多的时间去理解为什么某些图表如果不具启发性（例如，数字-类别特征的折线图、数字-数字特征的箱线图）还会存在。然而，**缺失值的单独图表（删除前后）确实很有用**。我可以预见未来会使用它们**来诊断一些缺失机制**，特别是“随机缺失”，其中一个特征中的缺失值与另一个特征的观察值相关。

+   `autoviz` 是一个**非常有趣的入门级数据科学工具**。它提供了数据的全面描述，并报告了真正有用的清理建议。对于经验丰富的专业人士来说，这些建议可能不必要，但对于初学者来说，这些建议可能对数据准备过程至关重要。**分析报告也非常全面**，自动列出了特征之间所有可能的组合。虽然冗长，但这对**缺乏经验的人来说是有帮助的**，因为下一步只是浏览生成的图表，看看是否有一些“突出”的内容。代码不是特别友好，但对于初学者来说仍然容易理解，我特别喜欢“监督”分析：**视觉效果干净美观**，我绝对会在自己的研究中使用它们。

+   `sweetviz` 有一个非常有洞察力的功能，尽管它的设计可以更好。总体而言，报告看起来**相当复古**，质量警报也**不是很直观**（例如，为什么突出显示最低的相关性而不是最高的？）。然而，**同时检查目标特征和子组的想法是我希望在其他软件包中看到的！** 对于更复杂的数据集，我们需要同时分析超过 2、3 或 4 个维度的数据，跟踪目标值是极其有用的（在大多数情况下，这就是我们试图绘制的内容，对吧？）。我可以告诉你，HCC 数据集就是这样的情况：它是一种异质性疾病，在类似阶段的患者可以映射到不同的生存结果。我曾经不得不费尽周折才能快速而正确地可视化一些子集，这个工具在寻找这类见解并将其传达给照顾患者的医疗团队时会非常有帮助。

+   最后，`lux` 可以作为**学习工具，用于基本的数据探索**，以及教学生或新手数据领域的统计学和数据处理基础知识：特征类型、图表类型、数据分布、正负相关性、缺失值表示（例如，`null`，`NaN`）。**它是超低代码的完美工具，适合“试水”**。

所以，你看，*似乎没有免费的午餐*。我真诚地希望你喜欢这篇评论！反馈和建议总是受欢迎的：你可以给我留言，[为仓库点赞并贡献](https://github.com/Data-Centric-AI-Community/awesome-data-centric-ai)，或者直接在[数据中心人工智能社区](https://tiny.ydata.ai/dcai-medium)与我联系，讨论其他数据相关的话题。再见，祝你科学研究愉快！

# 关于我

博士，机器学习研究员，教育者，数据倡导者，以及全能型人才。在 Medium 上，我撰写关于**数据驱动的人工智能和数据质量**的文章，旨在教育数据科学与机器学习社区如何从不完美的数据转变为智能数据。

[数据驱动的人工智能社区](https://tiny.ydata.ai/dcai-medium) | [GitHub](https://github.com/Data-Centric-AI-Community) | [Google Scholar](https://scholar.google.com/citations?user=isaI6u8AAAAJ&hl=en) | [LinkedIn](https://www.linkedin.com/in/miriamseoanesantos/)

# 参考文献

1.  M. Santos, P. Abreu, P. J. García-Laencina, A. Simão, A. Carvalho, [一种基于集群的过采样方法以改善肝细胞癌患者的生存预测](https://www.sciencedirect.com/science/article/pii/S1532046415002063) (2015), *生物医学信息学杂志 58*, 49–59。
