# Tableau 中的 6 种高级可视化

> 原文：[https://towardsdatascience.com/6-advanced-visualizations-in-tableau-71ca97fb10a9?source=collection_archive---------1-----------------------#2023-12-06](https://towardsdatascience.com/6-advanced-visualizations-in-tableau-71ca97fb10a9?source=collection_archive---------1-----------------------#2023-12-06)

## Tableau 中高级可视化的概述，包括逐步示例

[](https://medium.com/@payal-patel?source=post_page-----71ca97fb10a9--------------------------------)[![Payal Patel](../Images/2fc555726b3865db375ce7973f4c1cec.png)](https://medium.com/@payal-patel?source=post_page-----71ca97fb10a9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----71ca97fb10a9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----71ca97fb10a9--------------------------------) [Payal Patel](https://medium.com/@payal-patel?source=post_page-----71ca97fb10a9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F609389be1ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F6-advanced-visualizations-in-tableau-71ca97fb10a9&user=Payal+Patel&userId=609389be1ac2&source=post_page-609389be1ac2----71ca97fb10a9---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----71ca97fb10a9--------------------------------) ·17分钟阅读·2023年12月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F71ca97fb10a9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F6-advanced-visualizations-in-tableau-71ca97fb10a9&user=Payal+Patel&userId=609389be1ac2&source=-----71ca97fb10a9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F71ca97fb10a9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F6-advanced-visualizations-in-tableau-71ca97fb10a9&source=-----71ca97fb10a9---------------------bookmark_footer-----------)![](../Images/b2e268d5edef3705df453c0226c77e72.png)

作者提供的图片：Tableau 中的高级可视化

Tableau 是一个数据可视化工具，用于创建数据可视化、仪表板和故事。当我第一次开始使用这个工具时，我经常使用*Show Me*功能来创建数据可视化。这个功能允许用户创建常见的可视化类型，例如柱状图和折线图。

![](../Images/0c220591b9b8c830ac38e8c83d1e92a9.png)

作者提供的图片：Tableau 的 Show Me 功能

尽管标准可视化效果很好，但有时需要高级技术。最近，我创建了一个仪表盘，用于可视化《鲨鱼坦克》中的交易。以下是该仪表盘中使用的一些高级可视化技术，并附有逐步指导，教你如何重新创建这些可视化——无论是使用相同的数据集还是不同的数据集！这个列表也是学习不同可视化类型和脑力激荡如何将它们应用于其他工具的好方法！

本文介绍了以下6种数据可视化技术。

1.  [圆环图](#cd98)

1.  [桑基图](#b64a)

1.  [词云](#8e9d)

1.  [棒棒糖图](#6c43)

1.  [径向图](#aedf)

1.  [嵌套条形图](#8bb4)

**最终仪表盘可以在** [**此处**](https://public.tableau.com/views/SharkTankDeals-WhatdotheSharkslove/SharkTankDeals-WhatdotheSharkslove?%3Alanguage=en-US&%3Adisplay_count=n&%3Aorigin=viz_share_link)**查看。有关数据集的更多信息，请访问** [**此处**](https://github.com/payalnpatel/Tableau/tree/main)**。**

*注意：一些可视化使用了一个字段，* ***记录数***。在创建任何可视化之前，创建一个新的字段“记录数”，并将其值设置为1。要查看包含所有计算字段的数据集，可以从上述仪表盘下载数据集，或[点击此处下载数据集](https://github.com/payalnpatel/Tableau/blob/main/Shark-Tank/Shark%20Tank%20US%20dataset%20(Shark%20Tank%20US%20dataset)_Shark%20Tank%20US%20dataset.csv)*。*

# 圆环图

圆环图与饼图一样，显示了部分与整体的关系。它们的不同之处在于圆的中心有一个孔，就像一个甜甜圈。

[鲨鱼坦克仪表盘](https://public.tableau.com/app/profile/payal.patel/viz/SharkTankDeals-WhatdotheSharkslove/SharkTankDeals-WhatdotheSharkslove)中的圆环图显示了总的提案数，按成交或未成交进行分段。

要创建这个圆环图，首先在新的工作表中创建一个饼图。

从*数据面板*中，将**记录数**（即提案数）添加到*列*架上，将**成交情况**添加到*行*架上。

*注意：确保将“成交情况”度量转换为维度，如下图所示。*

![](../Images/2d86dc83102f5087369cd48b2268f407.png)

作者提供的图像：*“记录数”*度量在列架上，**成交情况**维度在行架上

从*显示*选项卡中选择饼图可视化类型。这将生成圆环图的基本可视化。

![](../Images/abe0af08fab482a4898fa141f50b12b3.png)

作者提供的图像：用于开发圆环图的基本饼图

在*行*架上添加两个新度量。将**AVG(0)**在*行*架上输入两次。（即，***AVG(0) AVG(0)***）。添加到*行*架后，你将看到两个饼图。

![](../Images/e60e11709ff5e9d1fe10ee9da60020e0.png)

作者提供的图像：两个饼图，是**AVG(0) AVG(0)**添加到行架后的结果

在*行*架上的第二个度量值中选择*双轴*以重叠两个饼图。

![](../Images/a2794306656f22eed642c11b469bd73f.png)

作者提供的图片：选择双轴

两个饼图现在会重叠在一起。调整每个饼图的大小以创建甜甜圈形状。在*标记卡*下，选择*大小*，通过将滑块向右移动来增加饼图的大小。

![](../Images/49694d185b310957e57142d755d19fd5.png)

作者提供的图片：调整饼图大小

要创建内环甜甜圈，请调整第二个，即底部的饼图。在*标记卡*上，导航到第二个度量值的卡片，并移除所有字段。在此情况下，有4个字段需要移除。

![](../Images/911447c4271b2f5052223d005a64b0ac.png)

作者提供的图片：从标记卡中移除额外的字段

移除这些字段将导致中心出现灰色圆圈。这是甜甜圈图的中心。通过在*标记卡*上调整大小来调整甜甜圈的大小。

![](../Images/ace5895af6c3c15775ca01edaf544a4f.png)

作者提供的图片：调整甜甜圈图内圆的大小

使用*标记卡*来调整甜甜圈图中心的颜色。

![](../Images/781d9d2226b910daa63bd8a8faae2476.png)

作者提供的图片：将甜甜圈图中心的颜色调整为白色，以匹配图表背景

使用第一个饼图，即*标记卡*下的第二个选项，来调整甜甜圈图的标签和颜色。

![](../Images/06e52c8fd6e22f04553cd9b94042582e.png)

作者提供的图片：格式化甜甜圈图上的标签和颜色

通过将一个度量移动到*标签*，在甜甜圈图的中心添加文本到*所有标记卡*上。

![](../Images/1db3365badda5e8cd68c72167dcf7045.png)

作者提供的图片：在甜甜圈图中心添加文本

右键单击标题，取消选中*显示标题*以移除轴标题。

![](../Images/f7184571219b86e3acf95a28a6daa900.png)

作者提供的图片：移除图表上的标题

右键单击工作表上的任意位置，选择*格式*以打开*格式窗格*。使用*格式窗格*隐藏网格线、调整边框、改变背景颜色等。

![](../Images/a297694e40c48ccc33d08de7b8cc0334.png)

作者提供的图片：打开格式窗格以获取更多格式化功能

以下图片显示了经过*标记卡*和*格式窗格*格式化后的最终甜甜圈图。

![](../Images/7b0c9700bf63e9c4fb03659c2c205cc6.png)

作者提供的图片：展示按交易或非交易分类的总投球次数的甜甜圈图

# 桑基图

桑基图显示了从一个实体到另一个实体的流动。[Shark Tank仪表板](https://public.tableau.com/app/profile/payal.patel/viz/SharkTankDeals-WhatdotheSharkslove/SharkTankDeals-WhatdotheSharkslove)中的桑基图展示了从投球者性别（即团队组成）到行业的交易流动。

## 步骤 1：自联合

要创建桑基图，上传数据集，然后对数据源进行自联合。以下图片显示已加载的数据集，共1274条记录和50个字段。

![](../Images/b6dc1a2765d3aa32ea7c5c12d71209ad.png)

作者提供的图片：初始 Shark Tank US 数据集加载到 Tableau 中

要创建自联合，将**Shark Tank US 数据集**从*工作表*移动到*画布*上的**Shark Tank US 数据集**。注意行数从 1274 翻倍至 2548。

![](../Images/1cfce24a0c411234873c331dd55c4c75.png)

作者提供的图片：自联合 Shark Tank 数据集

自联合创建了两个字段——**表格名称**字段将在本练习中稍后使用。

![](../Images/80a7282a7cd62371820d6c4fdcbe27d6.png)

作者提供的图片：自联合后创建的表格名称列

## 步骤 2：创建新字段和 Bins

要开发此 Sankey 图，请创建七个计算字段和一个 bin。打开一个新工作表并创建以下计算。*注意：下面列出的粗体字段名称，后面跟有相应的计算。*

1.  **ToPad**: 如果 [表格名称] = ‘Shark Tank US 数据集’则为 1，否则为 49 结束

1.  **填充：** 右键单击**ToPad**字段并选择*创建* → *Bins* 来创建一个新的名为 Padded 的 Bin。将*Bin 的大小*设置为 1。

1.  **t:** (INDEX() — 25) / 4

1.  **Rank 1:** RUNNING_SUM(SUM([记录数]))/TOTAL(SUM([记录数]))

1.  **Rank 2:** RUNNING_SUM(SUM([记录数]))/TOTAL(SUM([记录数]))

1.  **Sigmoid:** 1 / (1 + exp(1)^-[t])

1.  **曲线：** [Rank 1] + (([Rank 2]-[Rank 1])*[Sigmoid])

1.  **交易大小:** WINDOW_AVG(SUM([记录数]))

*注意：Rank 2 是 Rank 1 的副本*

以下图片展示了如何在 Tableau 中创建计算字段**ToPad**的示例。

![](../Images/66938cc72a7f859e84f44aa50bf146db.png)

作者提供的图片：为 Sankey 图创建计算字段**ToPad**的示例

以下图片展示了如何在**ToPad**上创建一个名为**填充**的 bin，bin 大小为 1。

![](../Images/3eea7e134373151d0d367064fbef6e5e.png)

作者提供的图片：为 Sankey 图创建 bin 的示例，**填充**

## 步骤 3：开发 Sankey 可视化

将**t**移动到*列*架上，将**曲线**移动到*行*架上，并将**填充**移动到*标记卡*上的*详细信息*。

![](../Images/b32019660d189cd115c4f1da65837443.png)

作者提供的图片：将初始字段添加到 Sankey 图的工作表中

将**行业**、**投手性别**和**表格名称**移到*标记卡*上的*详细信息*。

![](../Images/54eb678e9f7426978894d71810cbad17.png)

作者提供的图片：Sankey 图工作表上的标记卡

在**曲线**度量上，选择*编辑表格计算*，位于*行*架上。

![](../Images/7e94e67e99be45c7ec7d29fa79602766.png)

作者提供的图片：在**曲线**上选择编辑表格计算

对于**Rank 1**，更新表格计算如下：

+   选择*使用特定维度计算*

+   勾选以下字段：**投手性别**、**行业**、**填充**、**表格名称**

+   确保**投手性别**在顶部，因为它将位于 Sankey 图的左侧

![](../Images/8b385066d10c676f32e5ffbd939f2c9d.png)

作者提供的图片：**Rank 1** 表格计算

对于**Rank 2**，更新表格计算如下：

+   选择*使用特定维度计算*

+   检查以下字段：**行业**、**投球者性别**、**填充**、**表名**

+   确保**行业**在顶部，因为这将位于Sankey图的右侧

![](../Images/0ea67f9837d16589623ad19b2da8ff22.png)

作者提供的图片：**排名2**表计算

对于**t**，更新表计算如下：

+   选择*使用特定维度计算*

+   检查以下字段：**填充**

![](../Images/bccb1df6eb11c9dc056335b01363d2b4.png)

作者提供的图片：**t**表计算

现在，从*Columns*货架上的**t**度量的下拉选项中选择*编辑表计算*。对于**t**，更新表计算如下：

+   选择*使用特定维度计算*

+   检查以下字段：**填充**

![](../Images/5008f88249040b665ef83ce4a3e988b4.png)

作者提供的图片：**t**表计算

Sankey图将在工作表上开始成型。

![](../Images/077327b6e52a4a507fa591b65c451ad1.png)

作者提供的图片：Sankey图初步标记

通过在*Marks卡片*上更改**行业**和**投球者性别**字段的*标记类型*来为Sankey图添加颜色，如下图所示。将**交易规模**和**投球者性别**添加到*过滤器卡片*中，以过滤数据集以显示交易数量。

![](../Images/2a57b3ed4c318b079e68f9ff74a25f83.png)

作者提供的图片：格式化Sankey图

编辑X和Y轴范围。要编辑轴，请右键单击轴并选择*编辑轴*。

对于*Y轴*（**曲线**），将范围更改为从0开始。

![](../Images/dcf783552055dbcbbd353376a18be4a4.png)

作者提供的图片：更新Y轴范围

对于*X轴*（**t**），将范围更改为-5到5。

![](../Images/d58adce99f0ab9110814d18699cc9208.png)

作者提供的图片：更新X轴范围

隐藏每个轴上的标题，以获得更清晰的视觉效果。（注意：要做到这一点，请右键单击标题，然后取消选中*显示标题*）

将**交易规模**移动到*Marks卡片*上的*大小*，并增加大小以增加图中的线条厚度。这些线条的厚度反映了从**投球者性别**到**行业**的交易数量。在**交易规模**上，选择*使用* **填充**，如以下图片所示。

![](../Images/311d354c9ef28802942e04d941329603.png)

作者提供的图片：将**交易规模**添加到可视化中

## 步骤4：创建Sankey图的端点

Sankey图需要两个条形图，一个用于每个端点（**投球者性别**和**行业**）。打开一个新的工作表，创建一个条形图。下面的图片显示了**投球者性别**的条形图。*注意：用于* ***投球者性别*** *的颜色方案在Sankey图的主要部分中使用。*

![](../Images/98c653bf772974429bc10fc41fd7053c.png)

作者提供的图片：按**投球者性别**显示的交易总数条形图

对**行业**字段重复操作，但这次选择*灰色*调色板。

![](../Images/4a7a6621ef270d6bfd163a69696336b7.png)

作者提供的图片：显示按 **行业** 划分的总交易数的条形图

现在是时候将所有内容整合在一起了！创建一个新的仪表板，并将三个工作表添加到一起，如下所示。使用 *格式窗格* 调整背景颜色和边框。

![](../Images/ae4c30d2fa2addda2166f583098029d1.png)

作者提供的图片：组合三个视觉效果的仪表板用于 Sankey 图

## 第五步：添加仪表板操作

添加以下仪表板操作以启用 Sankey 图的悬停功能。

要添加仪表板操作，从主菜单中选择 *仪表板* → *操作* → *添加操作* → *高亮*。

![](../Images/a9fc03b4b30e84c53c8ca93ab3f4fb04.png)

作者提供的图片：添加仪表板操作

对于 **投标者性别**，添加以下 *高亮操作*。

![](../Images/b37b45486f321d24b2cff7e1f0a58715.png)

作者提供的图片：为 **投标者性别** 创建高亮操作

这将允许用户在左侧图表上悬停，并查看交易在右侧的去向。

![](../Images/58e82b83beb20c342019f748f3085919.png)

作者提供的图片：**投标者性别** 高亮操作

为 **行业** 创建另一个高亮操作。再次选择 *仪表板* → *操作* → *添加操作* → *高亮*。

输入以下信息以创建 **行业** 的 *悬停高亮操作*。

![](../Images/225e7f60f75521daf55ba36c1bf7e26e.png)

作者提供的图片：为行业创建高亮操作

用户现在可以悬停在 **行业** 上，查看 **投标者性别** 的交易比例。

![](../Images/780fab277b74be602c3761db24eb6b17.png)

作者提供的图片：**行业** 高亮操作

# 词云

词云显示文本中最常见的单词，出现频率最高的单词会显示得最大。单词以“云”的形状排列，因此得名词云。词云在某些情况下很有用，例如可视化文本列中的单词频率。

要创建词云，需要一个维度（分类变量）和一个度量（数值变量）。本节展示如何重新创建 [Shark Tank Deals 仪表板](https://public.tableau.com/app/profile/payal.patel/viz/SharkTankDeals-WhatdotheSharkslove/SharkTankDeals-WhatdotheSharkslove) 的词云。这个词云显示了每个行业的交易数量。

首先，打开一个新的工作表。将 **行业** 移到 *标记卡* 上的 *文本*，将 **记录数量**（即 投标数量）移到 *大小*。此外，将 **行业** 移到 *颜色*。

![](../Images/af4b647301f5c5618ea938987b0b5dd5.png)

作者提供的图片：标记卡上的初始维度和度量

这将产生如下所示的热图。要将此可视化转换为词云，在 *标记卡* 上，将可视化类型更改为 *文本*。

![](../Images/8cd289d7a87de70cd861211be7cb92df.png)

作者提供的图片：在 Tableau 工作表上更改标记类型

这将产生一个词云。使用 *标记卡* 对格式进行更改。

![](../Images/acb0a3f637c7e57050fec96b93feb124.png)

作者提供的图像：按行业显示交易数量的词云

# 棒棒糖图表

棒棒糖图表类似于条形图。它们显示不同类别的频率。棒棒糖图表与条形图的不同之处在于视觉展示。虽然条形图使用矩形显示每个类别的值，但棒棒糖图表在每根条形的顶部添加了一个圆圈。这种可视化类型在创建信息图或仪表板时非常有用。

[鲨鱼坦克仪表板](https://public.tableau.com/app/profile/payal.patel/viz/SharkTankDeals-WhatdotheSharkslove/SharkTankDeals-WhatdotheSharkslove)中的棒棒糖图表显示了每个鲨鱼的交易数量。

首先，将**已达成交易**和**度量名称**添加到*列*架。通过选择字段上的选项，将**已达成交易**转换为离散维度。

![](../Images/19ec943b4762cb3f3328029040266c8f.png)

作者提供的图像：将**已达成交易**和**度量名称**添加到列架

将**度量值**添加到*行*架上两次。别担心，再经过几步，棒棒糖图表就会开始成型！

![](../Images/de972ba56d7a2e702a0810d36e33e986.png)

作者提供的图像：将**度量值**添加到行架

在第二个**度量值**字段上，选择*双轴*。

![](../Images/55c80112d902d501dc55e687bf47eeee.png)

作者提供的图像：在度量值字段上选择双轴

右击其中一个 y 轴，选择*同步轴*。

![](../Images/413963e2f5628c5ea345f04c7db9fb68.png)

作者提供的图像：同步图表轴

在*度量值卡片*上，添加以下字段：**巴巴拉投资**、**戴蒙德投资**、**凯文投资**、**洛瑞投资**、**马克投资**、**罗伯特投资**。将这些字段的类型更改为*度量（总和）*，如下面的图像所示。从*度量值卡片*中移除不在此列表中的其他字段。

![](../Images/d5eaa64865802af658c4e0e2bedac32b.png)

作者提供的图像：**度量值**最终列表

将**已达成交易**添加到*筛选卡片*。筛选条件为***已达成交易 = 1***，即“已达成交易”。

![](../Images/014b0ba74e1d8eed40daa96a6a25254d.png)

作者提供的图像：将**已达成交易**添加到筛选卡片

在*标记卡片*上，将**度量值**卡片的图表类型更改为*条形图*，并调整大小。

![](../Images/c393de5da160c67fa13be84fe88aeff2.png)

作者提供的图像：在度量值标记卡片上更改图表类型

在第二个*度量值卡片*上，将类型更改为*圆圈*，并使用*大小*区块调整大小。

![](../Images/33b91d90b7a4a50eec89d0be3c155d87.png)

作者提供的图像：在标记卡片上调整大小

将**度量值**药丸移动到*文本*区块，在*度量值标记卡片*上，以便向图表中的圆圈添加标签。

![](../Images/45e429b6ec49cd7a5d48a82e05d83d00.png)

作者提供的图像：向图表添加标签

右键单击 y 轴，取消选中*显示标题*，以去除额外的标题。使用*格式窗格*更改背景颜色、格式化轴，并修改文本标签。要打开*格式窗格*，右键单击工作表并选择*格式*。

![](../Images/b9f7852273f5996aace1b82aa9581791.png)

图片作者：打开格式窗格

下图展示了最终的棒棒糖图表。

![](../Images/8ff54f2f8c05f9a88a94dbecb3dc1e9a.png)

图片作者：显示每个鲨鱼交易数量的棒棒糖图表

# 径向图表

径向图表是显示分类信息的另一种方式。径向图表可以是条形图的一个很好的替代选择。

在[Shark Tank 仪表板](https://public.tableau.com/app/profile/payal.patel/viz/SharkTankDeals-WhatdotheSharkslove/SharkTankDeals-WhatdotheSharkslove)中，径向图表按交易中的鲨鱼数量显示交易。要在 Tableau 中创建径向图表，需要一个维度和一个度量。此外，还会在过程中创建 9 个计算字段和 1 个箱子。以下步骤概述了如何重新创建此图表。

## 第 1 步：自连接

要创建径向图表，上传数据集，然后创建数据源的自连接。下图展示了加载的数据集，总共有 1274 条记录和 50 个字段。

![](../Images/b6dc1a2765d3aa32ea7c5c12d71209ad.png)

图片作者：初始 Shark Tank US 数据集加载到 Tableau 中

要创建自连接，将**Shark Tank US dataset**从*工作表*中拖动到*画布*上的**Shark Tank US dataset**上。注意行数从 1274 翻倍到 2548。

![](../Images/1cfce24a0c411234873c331dd55c4c75.png)

图片作者：自连接 Shark Tank 数据集

自连接创建了两个字段 — **Table Name**字段将在此练习中稍后使用。

![](../Images/80a7282a7cd62371820d6c4fdcbe27d6.png)

图片作者：通过自连接过程创建的表名字段

## 第 2 步：创建新字段和箱子

创建此径向图表需要 9 个计算字段和 1 个箱子。打开一个新的工作表，并按顺序创建以下计算。*注意：下面列出的字段名称为粗体字，后跟相应的计算。*

1.  **Path:** IIF([Table Name] = ‘Shark Tank US dataset’, 0, 270)

1.  **Path (Bin)**: — 右键单击**Path**字段，选择*创建* → *箱子*，以创建一个名为**Path (Bin)**的新箱子。将*箱子大小*设置为 1。

1.  **Index-1:** INDEX()-1

1.  **Total Cat. Deals:** WINDOW_SUM(SUM([Number of Records]))/2

1.  **Total Deals:** WINDOW_SUM(SUM([Number of Records]))/2

1.  **Percent Calc:** [Total Cat. Deals]/[Total Deals]

1.  **Rank Calc:** RANK_UNIQUE([Total Cat. Deals], ‘asc’)

1.  **Size Calc:** [Percent calc]/WINDOW_MAX([Percent calc])

1.  **X:** SIN(RADIANS([Index-1])*[Size calc]) * [Rank calc]

1.  **Y:** COS(RADIANS([Index-1])* [Size calc]) * [Rank calc]

下图展示了如何在 Tableau 中创建计算字段**PATH**的示例。

![](../Images/ebad8764d24f0a67c1053761410c6fc3.png)

作者插图：为 sankey 图创建计算字段的示例，**PATH**

以下图像展示了如何在**PATH**上创建一个名为**Path (bin)** 的 bin，bin 大小为 1。

![](../Images/b12cba1fd09bc68683621172faec5b11.png)

作者插图：为 sankey 图创建 bin 的示例，**路径 (bin)**

## 步骤 3：创建可视化

将**交易中的鲨鱼数量** 移动到*Marks card*上的*Colors tile*，将**Path(bin)** 移动到*Rows*架。检查**Path(bin)** 字段上的*Show Missing Value*。

![](../Images/a1d5dd0738eee6989ee801621e792a52.png)

作者插图：检查显示缺失值

在*Marks card*上，将标记类型更改为*Line*。将**Path(Bin)** 从*Rows*架移到*Marks Card*上的*Path*。确保**Number of Sharks** 是一个维度而不是显示总和。

![](../Images/17c05fd1811481571216d36fb254969b.png)

作者插图：Marks card 上的 PATH(bin) 和交易中的鲨鱼数量

将**X** 移动到*Columns*架，将**Y** 移动到*Rows*架。

![](../Images/6aa6914d09a617425f44b970a9f21b1c.png)

作者插图：将**X**和**Y**添加到工作表中

在**Y**上选择*编辑表计算*。

![](../Images/b8ae0603fe592c30361e703c869a2a7c.png)

作者插图：在**Y**上选择编辑表计算

对于**Index-1**，更新表计算如下：

+   选择*使用特定维度计算*

+   检查以下字段：**PATH(bin)**

![](../Images/d94f53f0e7919f2a3ebced3b3280c2e5.png)

作者插图：**Index-1** 表计算

对于**Size calc**，更新表计算如下：

+   选择*使用特定维度计算*

+   检查以下字段：**交易中的鲨鱼数量**，**PATH(bin)**

+   确保**交易中的鲨鱼数量**在顶部

![](../Images/8a8ddda65713f2357c11e5ce1dbbe9da.png)

作者插图：**Size calc** 表计算

对于**Total Cat. Deals**，更新表计算如下：

+   选择*使用特定维度计算*

+   检查以下字段：**PATH(bin)**

![](../Images/ff46a47ac98fe72a4f5738b1c3c7740e.png)

作者插图：**Total Cat. Deals** 表计算

对于**Total Deals**，更新表计算如下：

+   选择*使用特定维度计算*

+   检查以下字段：**交易中的鲨鱼数量**，**PATH(bin)**

+   确保**交易中的鲨鱼数量**在顶部

![](../Images/5dea2ec6f37af25845f291e62e0b9a30.png)

作者插图：**Total Deals** 表计算

对于**Rank calc**，更新表计算如下：

+   选择*使用特定维度计算*

+   检查以下字段：**交易中的鲨鱼数量**

![](../Images/7bf2850a97f44e87befb49639bd5c4c8.png)

作者插图：**Rank calc** 表计算

在**X**上选择*编辑表计算*。

![](../Images/c8db3b4287d20c055e99bffd1158a75c.png)

作者插图：在**X**上选择编辑表计算

对于**Index-1**，更新表计算如下：

+   选择*使用特定维度计算*

+   检查以下字段：**PATH(bin)**

![](../Images/10736453fc68985f55ebf396e6af7c54.png)

作者图片：**索引-1** 表计算

对于 **大小计算**，请按如下方式更新表计算：

+   选择 *按特定维度计算*

+   勾选以下字段：**交易中的鲨鱼数量**，**路径（bin）**

+   确保 **交易中的鲨鱼数量** 位于顶部

![](../Images/430ef5918fc8c44df1fc6d2c3b27362e.png)

作者图片：**大小计算** 表计算

对于 **总类别交易**，请按如下方式更新表计算：

+   选择 *按特定维度计算*

+   勾选以下字段：**路径（bin）**

![](../Images/6770418da7efc80ede0f5a3a728f7691.png)

作者图片：**总类别交易** 表计算

对于 **总交易**，请按如下方式更新表计算：

+   选择 *按特定维度计算*

+   勾选以下字段：**交易中的鲨鱼数量**，**路径（bin）**

+   确保**交易中的鲨鱼数量**位于顶部

![](../Images/f52c69db0d8d838b629b17fa41b062e4.png)

作者图片：**总交易** 表计算

对于 **排名计算**，请按如下方式更新表计算：

+   选择 *按特定维度计算*

+   勾选以下字段：**交易中的鲨鱼数量**

![](../Images/0cf9955050b3b1f7f0be73bedee19646.png)

作者图片：**排名计算** 表计算

一旦表计算更新，放射状图的基础部分开始成形。

![](../Images/0be1d2cbdaeeb5b68c45814dbf3c48b0.png)

作者图片：放射状图基础

## 第四步：附加格式设置

使用 *标记卡* 来格式化视觉效果——调整线条厚度、颜色和标签。下面的图片显示了每条线的开头添加了标签，并分配了新的颜色调色板。还可以使用左侧的 *格式面板* 来去除边框并调整背景。

![](../Images/6ff155d78453b3c32c710dd767bfb99a.png)

作者图片：格式化放射状图

以下图片显示了最终的放射状图。

![](../Images/dc76547be27d1f71aeb92b2a274d9783.png)

作者图片：最终的放射状图

# 嵌套条形图

Tableau 中的另一种高级可视化技术是嵌套条形图。嵌套条形图是一种堆叠条形图的方法，可以让你比较某一类别的两个数值。虽然这种可视化类型没有进入最终仪表板，但以下示例展示了如何使用这些数据创建嵌套条形图。下面的嵌套条形图显示了按行业分类的交易数量和演讲数量。

从创建两个条形图的工作表开始。将 **行业** 移动到 *行* 货架，将 **记录数量**（即总演讲数）和 **获得交易** 移动到 *列* 货架。

![](../Images/96cd9e46db5c839d55b2221e67a2e4c5.png)

作者图片：显示按行业分类的交易和演讲的两个条形图

从 **获得交易** 的下拉选项中，选择 *双轴*。

![](../Images/d1e38c82ff6630f1767022f85e7b08e5.png)

作者图片：选择双轴

右键点击其中一个 x 轴，选择 *同步轴*。

![](../Images/d553f49c34b9c88b7625474d5634bed2.png)

作者提供的图像：同步图表轴

在 *所有标记卡* 上，将类型更改为 *条形图*。

![](../Images/a6bea393043564fa1d5eb2d75e04fec4.png)

作者提供的图像：将图表类型更改为条形图

使用 **记录数量** 和 **成交情况** *标记卡* 调整条形的大小。将 **成交情况** 条形（海军蓝）设为比 **记录数量** 条形（浅蓝）小，如下图所示。

![](../Images/0a2b9e89615f72f8426e55b343389c22.png)

作者提供的图像：调整条形大小

使用 *标记卡* 添加标签并根据需要格式化视觉效果。以下图像展示了最终的嵌套条形图。

![](../Images/5ee617171df79c5ed4780c2abd372894.png)

作者提供的图像：按行业显示总交易和推介的嵌套条形图

这涵盖了 Tableau 中的 6 种高级可视化！虽然这些示例是在 Tableau 中，但请记住，这些可视化也可以在其他工具中创建——所以在开发下一个数据可视化时，使用它们作为灵感吧！

最终的仪表板可以在[这里](https://public.tableau.com/views/SharkTankDeals-WhatdotheSharkslove/SharkTankDeals-WhatdotheSharkslove?%3Alanguage=en-US&%3Adisplay_count=n&%3Aorigin=viz_share_link)查看或下载源材料。

*Payal 是一位数据与人工智能专家。在她的闲暇时间，她喜欢阅读、旅行和在 Medium 上写作。如果你喜欢她的作品，* [*关注或订阅*](https://medium.com/@payal-patel) *她的列表，绝不要错过任何故事！*

*以上文章为个人观点，不一定代表 IBM 的立场、策略或意见。*

**参考资料**

[1]: Thirumani, Satya. “🦈 Shark Tank Us Dataset 🇺🇸。” *Kaggle*, 2023年8月28日, [www.kaggle.com/datasets/thirumani/shark-tank-us-dataset.](http://www.kaggle.com/datasets/thirumani/shark-tank-us-dataset.) (CC0: 公共领域许可证)
