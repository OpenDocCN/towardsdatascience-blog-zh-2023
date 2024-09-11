# 如何准备你的数据以进行可视化

> 原文：[https://towardsdatascience.com/how-to-prepare-your-data-for-visualizations-94e33473f70b?source=collection_archive---------2-----------------------#2023-06-26](https://towardsdatascience.com/how-to-prepare-your-data-for-visualizations-94e33473f70b?source=collection_archive---------2-----------------------#2023-06-26)

## Python

## 无需使用 Tableau Prep 或 Alteryx

[](https://medium.com/@stephanie_lo?source=post_page-----94e33473f70b--------------------------------)[![Stephanie Lo](../Images/2b7787607352815394cf8b734a9f0f02.png)](https://medium.com/@stephanie_lo?source=post_page-----94e33473f70b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----94e33473f70b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----94e33473f70b--------------------------------) [Stephanie Lo](https://medium.com/@stephanie_lo?source=post_page-----94e33473f70b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff4309a31ceee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-prepare-your-data-for-visualizations-94e33473f70b&user=Stephanie+Lo&userId=f4309a31ceee&source=post_page-f4309a31ceee----94e33473f70b---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----94e33473f70b--------------------------------) ·8 分钟阅读·2023年6月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F94e33473f70b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-prepare-your-data-for-visualizations-94e33473f70b&user=Stephanie+Lo&userId=f4309a31ceee&source=-----94e33473f70b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F94e33473f70b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-prepare-your-data-for-visualizations-94e33473f70b&source=-----94e33473f70b---------------------bookmark_footer-----------)![](../Images/3adf0a964e9289b69e1d09c3fa24aad7.png)

图片由 [Robert Katzki](https://unsplash.com/@ro_ka?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

想开始你的下一个数据可视化项目吗？首先从熟悉数据清洗开始。数据清洗是任何数据管道中至关重要的一步，将原始的“脏”数据输入转变为更可靠、更相关、更简洁的数据。诸如Tableau Prep或Alteryx之类的数据准备工具是为此目的而创建的，但为什么要花钱使用这些服务，而你可以通过像Python这样的开源编程语言完成这项任务呢？本文将指导你如何使用Python脚本为可视化准备数据，提供一种比数据准备工具更具成本效益的替代方案。

> 注：在本文中，我们将重点关注使数据为Tableau的数据可视化做好准备，但主要概念同样适用于其他商业智能工具。

我明白了。数据清洗似乎只是将可视化或仪表板变为现实这一漫长过程中的**另一**个步骤。但它至关重要，并且可以是令人愉快的。这是你通过深入了解你拥有和没有的数据，以及你必须做出的相应决策，来使自己对数据集感到舒适的过程。

尽管Tableau是一个多功能的数据可视化工具，但有时获得答案的路线并不明确。这时，在将数据加载到Tableau之前处理数据集可能是你最大的秘密帮手。让我们探讨一些在将数据集成到Tableau之前，数据清洗为何有益的关键原因。

+   **消除无关信息：** 原始数据通常包含不必要或重复的信息，这可能会使你的分析变得杂乱。通过清洗数据，你可以去除这些垃圾，并将你的可视化集中在最相关的数据特征上。

+   **简化数据转换：** 如果你对打算生成的可视化有清晰的愿景，在将数据加载到Tableau之前进行这些预转换可以简化过程。

+   **团队内部的更容易转移：** 当数据源定期更新时，新增加的内容可能会引入不一致性，并可能破坏Tableau。通过Python脚本和代码描述（更正式地称为Markdown文档），你可以有效地分享并帮助他人理解你的代码，以及排除可能出现的任何编程问题。

+   **节省数据刷新时间：** 需要定期刷新的数据可以从利用[Hyper API](https://tableau.github.io/hyper-db/docs/)中受益——这是一个生成特定于Tableau的Hyper文件格式的应用程序，并允许自动数据提取上传，同时使数据刷新过程更高效。

现在我们已经介绍了一些准备数据的优势，让我们通过创建一个简单的数据管道将其付诸实践。我们将探讨数据清洗和处理如何集成到工作流程中，并帮助使你的可视化更易于管理。

## **使用 Python 脚本创建数据管道**

![](../Images/af05fd654844ba80dc4efc209b3ddb1a.png)

图片由作者提供

我们的数据旅程相当简单：数据清洗、数据处理以生成可视化，并将其转换为 Tableau 准备好的 Hyper 文件，以实现无缝集成。

在深入我们的工作示例之前，有一点需要注意的是，对于 Hyper 文件转换，你需要下载 `pantab` 库。这个库简化了 Pandas 数据框到 Tableau .hyper 提取文件的转换。你可以通过在你选择的环境的终端中使用以下代码轻松完成这个任务（对于那些对环境不太熟悉的人，[这是一个很好的入门文章](/a-data-scientists-guide-to-python-virtual-environments-858841922f14)，介绍了环境是什么以及如何安装某些库）：

```py
#run the following line of code to install the pantab library in your environment
pip install pantab
```

## **教程：使用 Python 探索加拿大电动汽车许可证的数据准备**

我们将致力于制作的数据可视化专注于根据来自加拿大统计局的政府数据，展示不同电动汽车制造商和车型的受欢迎程度。

> 需要注意的是，这建立在我之前文章中探讨的数据集基础上：[使用 R 进行电动汽车分析](https://medium.com/towards-data-science/the-starter-guide-for-transitioning-your-python-projects-to-r-8de4122b04ad)。如果你对数据集的初步探索和决策背后的理由感兴趣，请参考该文章以获取更多细节。本教程专注于构建 Python 脚本，在每一步执行初始输入后，我们将把每个 Python 脚本的输出保存到其相应的文件夹中，如下所示：

![](../Images/8d3dd5b6a797fd46c82484c68f7176b6.png)

图片由作者提供

文件夹流程确保管道组织良好，并且我们能够保留项目中每个输出的记录。让我们开始构建第一个 Python 脚本吧！

## **数据清洗**

我们管道中的初始脚本遵循数据清洗的基本步骤，对于这个数据集包括：保留/重命名相关列，删除空值和/或重复项，以及使数据值一致。

我们可以通过指定输入文件的位置和输出文件的目的地来开始。这一步很重要，因为它使我们能够在相同位置组织文件的不同版本，在这种情况下，我们每月修改文件输出，因此每个文件输出按月份分隔，如文件名末尾所示 `2023_04`：

以下代码读取原始 .csv 输入文件，并定义我们希望保留的列。在这种情况下，我们感兴趣的是保留与购买车型相关的信息，并忽略与汽车经销商或其他无关列相关的信息。

现在我们可以缩短列名，去除前导或尾随的空白，并添加下划线以便更易于理解。

接下来，在检查数据集中只有少量空值之后，我们将使用`.dropna`函数删除这些空值。此时，你还可以删除重复项，但对于这个特定的数据集，我们将不这样做。这是因为存在大量重复信息，并且在缺乏行标识符的情况下，删除重复项会导致数据丢失。

最后一步是将我们的数据保存为 .csv 文件到适当的文件夹位置，该位置将放在我们共享目录的`clean_data`文件夹中。

注意我们如何使用`__file__`引用文件，并通过使用 bash 命令指定文件目录，其中`../`表示上一级文件夹。这结束了我们的数据清理脚本。现在让我们进入数据处理阶段吧！

> 完整的工作代码和汇总脚本可以在我的 GitHub 仓库 [这里](https://github.com/stephrlo/Pipeline_for_Data_Visualization) 找到。

**可视化的数据处理**

让我们重新审视一下我们试图实现的可视化目标，这些目标旨在突出电动车注册受欢迎程度的变化。为了有效展示这一点，我们希望最终的 Tableau 准备数据集包含以下功能，我们将对此进行编码：

+   按年份的车辆绝对计数

+   按年份的车辆比例计数

+   注册车辆的最大增加和减少

+   注册车辆的排名

+   车辆注册的先前排名以便进行比较

> 根据你希望生成的可视化，创建理想列可能是一个迭代过程。在我的情况下，在构建了可视化之后我才包含了最后一列，因为我知道我想提供一个排名差异的视觉对比，因此 Python 脚本做了相应的调整。

对于以下代码，我们将重点关注模型聚合数据集，因为品牌的其他数据集非常相似。让我们首先定义我们的 `inputfile` 和 `outputfile`：

注意我们如何引用来自 `clean_data` 文件夹的 `inputfile`，这是我们数据清理脚本的输出。

以下代码读取数据，并创建一个按 `Vehicle_Make_and_Model` 和 `Calendar_Year` 聚合计数的数据框：

`pivot`函数的功能类似于 Excel 中的数据透视表功能，它将 `Calendar_Year` 中的每个值作为列输入。

然后，脚本使用 For Loop 创建 `per_1K` 输入。这计算每个模型的比例，以便在相同的尺度上比较每个模型，并为每一年创建一列：

通过按年份计算比例，我们可以从数据集开始的 2019 年到最后一个完整数据年的 2022 年，计算每个模型的最大增加和减少。

在这里，`melt`函数用于将按年份分离的 `per_1K` 列重新透视为行，以便我们只保留一个 `per_1K` 列及其相关值。

以下代码允许我们将绝对计数和刚刚创建的其他计算进行连接。

我们现在可以使用许可计数创建`rank`列，并按`Vehicle_Make_and_Model`和`Calendar_Year`对这些值进行排序。

最后一列是通过使用`shift`函数创建的`previous_rank`列。

最后，我们可以将输出保存到管道中的`clean_model`文件夹路径，提供一个准备好可视化的数据集。

> 作为友好的提醒，包括`clean_brand`处理数据集的完整Python脚本代码可以在我的GitHub仓库[这里](https://github.com/stephrlo/Pipeline_for_Data_Visualization)找到。

## **将最终数据文件转换为.hyper文件格式**

我们管道中的最后一步相对简单，因为我们只需将处理后的.csv文件转换为.hyper文件格式。只要你已经下载了前面提到的`pantab`库，这应该是相对容易的。

值得一提的是，在Tableau中，连接的数据可以是实时连接或提取的。实时连接确保数据有持续流动，源的更新几乎立即在Tableau中反映出来。提取的数据涉及Tableau创建一个带有.hyper文件扩展名的本地文件，其中包含数据的副本（数据源的详细描述可以在[这里](https://www.flerlagetwins.com/2022/01/data-sources-1.html)找到）。它的主要优点是加载速度快，Tableau可以更高效地访问和呈现信息，这对于大型数据集特别有益。

hyper文件转换脚本的代码从加载`pandas`和`pantab`包开始，然后读取你需要的`cleaned_model`数据集以供Tableau使用。

最后一行代码使用`frame_to_hyper`函数生成.hyper文件并将其保存到`hyper`文件夹中。

最后一步，我们可以通过打开一个新工作簿将.hyper文件格式轻松加载到Tableau中，在`select a file`部分你可以选择要加载的文件，选择`more`。当我们加载`ev_vehicle_models.hyper`文件时，它应显示为Tableau提取，如下图所示，你的数据已准备好进行可视化构建！

![](../Images/bc18e6c9cf681746035b80323cb5483e.png)

## 结束语

通过将深思熟虑的规划融入到你的可视化中，你可以通过创建简单的数据管道来简化仪表板的维护。如果你缺乏资源也不要担心；像Python这样的开源编程程序提供了强大的功能。作为最终的友好提醒，要获取Python脚本，请查看我的GitHub仓库[这里](https://github.com/stephrlo/Pipeline_for_Data_Visualization)。

除非另有说明，所有图片均由作者提供。

**参考文献**

1.  Salesforce, [Tableau Hyper API](https://tableau.github.io/hyper-db/docs/), 2023

1.  R.Vickery, [数据科学家 Python 虚拟环境指南](/a-data-scientists-guide-to-python-virtual-environments-858841922f14)，2021年1月

1.  K.Flerlage, [Tableau 数据源第1部分：数据源类型](https://www.flerlagetwins.com/2022/01/data-sources-1.html)，2022年7月
