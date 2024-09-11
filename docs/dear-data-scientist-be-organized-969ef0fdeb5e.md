# 亲爱的数据科学家，请保持组织有序！

> 原文：[https://towardsdatascience.com/dear-data-scientist-be-organized-969ef0fdeb5e?source=collection_archive---------4-----------------------#2023-02-03](https://towardsdatascience.com/dear-data-scientist-be-organized-969ef0fdeb5e?source=collection_archive---------4-----------------------#2023-02-03)

## 一个快速指南，帮助你提高组织技能，从而提升你的表现。

[](https://alexandrerossetolemos.medium.com/?source=post_page-----969ef0fdeb5e--------------------------------)[![亚历山德雷·罗斯塞托·莱莫斯](../Images/875245dfdc6d10a20a94791a5d88c5ae.png)](https://alexandrerossetolemos.medium.com/?source=post_page-----969ef0fdeb5e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----969ef0fdeb5e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----969ef0fdeb5e--------------------------------) [亚历山德雷·罗斯塞托·莱莫斯](https://alexandrerossetolemos.medium.com/?source=post_page-----969ef0fdeb5e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fde43cc058e79&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdear-data-scientist-be-organized-969ef0fdeb5e&user=Alexandre+Rosseto+Lemos&userId=de43cc058e79&source=post_page-de43cc058e79----969ef0fdeb5e---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----969ef0fdeb5e--------------------------------) ·8 min read·2023年2月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F969ef0fdeb5e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdear-data-scientist-be-organized-969ef0fdeb5e&user=Alexandre+Rosseto+Lemos&userId=de43cc058e79&source=-----969ef0fdeb5e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F969ef0fdeb5e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdear-data-scientist-be-organized-969ef0fdeb5e&source=-----969ef0fdeb5e---------------------bookmark_footer-----------)![](../Images/fcc191b831f2e1bcacc7207d2fbf188e.png)

照片由 [Matthew Kwong](https://unsplash.com/@mattykwong1?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 在我们开始之前

首先，我有几个简单的问题要问你：

+   你是否曾经在自己的笔记本中迷失，不知道需要按什么顺序执行单元，以确保代码顺利运行？

+   在项目开发过程中，你是否曾因试图记起之前进行的分析及其结果而浪费了宝贵的几分钟甚至几小时？

+   你是否曾经遇到过快速、准确、轻松地找到你在项目中使用的数据的困难？

+   你是否在解释你的代码时遇到困难，特别是在长时间未使用它们之后？

+   当你去解释你的代码或向同事展示一些分析结果时，是否需要几分钟来记住你做了什么或分析结果在哪里？

如果你对这些问题中的任何一个回答是“是”，那么很抱歉地告诉你，你很可能有一个组织问题。

> 不要羞愧，这比看起来更普遍！

好消息是，这类问题很容易解决，但它们繁琐且需要投入精力。我曾经因为缺乏组织而遭受过很多问题，因此我最终创建了一个指南，帮助我在工作中更加有条理和方法化。

我喜欢将我的组织分为四个主题：

+   数据组织

+   文件组织（笔记本）

+   笔记本结构组织

+   代码组织

# 数据组织

![](../Images/a59b22c8e1eb6e5e26927670c69496f5.png)

照片由 [Nana Smirnova](https://unsplash.com/@nananadolgo?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数据科学项目涉及使用数据，要么进行一些分析，要么开发机器学习模型，因此数据是这一领域任何项目的基础。

逻辑上，数据访问必须准确，以便在需要时选择正确的数据。然而，当我们处理大型项目或项目持续很长时间时，这项任务可能会变得困难。在开发过程中，可能会生成多个数据库，如果这些数据库没有正确分类，它们可能会导致混淆，从而做出错误的决策。

为了处理这些问题，我喜欢按照以下结构组织我的数据：

![](../Images/7209ea7942e7f3e5981674d23ec277a4.png)

数据组织结构示意图（作者提供的图片）

是的，这是一种非常简单和直观的结构，但人们（包括我）往往会把所有的数据保存在同一个文件夹里。现在我喜欢为我参与的每个项目单独设立一个文件夹，并且对于每个项目，我总是有一个单独的原始数据文件夹，包含初始信息，还有一个处理后数据文件夹，包含经过某种预处理后的数据。

保存处理后的数据非常有用，这样你就不必运行整个预处理流程来生成你将用于构建模型或进行分析的数据集。

根据我的经验，我学到一个好习惯是保留在项目中使用的数据版本，因为这样你可以用来比较获得的不同结果，并且确保以前结果和分析的可重复性。然而，在项目结束后的一段时间，最好清理一些旧文件，以减少使用的存储量。

# 文件组织

![](../Images/6f8e1b655e5391f7210cd841342f712f.png)

图片由[Maksym Kaharlytskyi](https://unsplash.com/@qwitka?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

通常，在数据科学项目中，需要完成几个步骤才能达到结果。例如，如果你正在为特定任务开发机器学习模型，一些步骤是相当常见的，如开发将要使用的数据库、探索性数据分析（EDA）、开发预处理管道、模型调整和结果评估。

每个任务通常需要合理数量的代码行来完成，这使得将所有任务放在同一个笔记本中变得不切实际，因为同一个文件中的信息量巨大。即使应用我将在接下来的主题中讨论的技巧，笔记本也很容易变得混乱和杂乱。

另一个值得提到的点是，笔记本可能变得计算开销如此之大，以至于如果尝试按顺序运行所有单元格，内核可能会变得不稳定，从而导致浪费宝贵的时间，甚至是小时。

因此，我一直使用并且在项目中对我帮助很大的文件组织方式具有以下格式：

![](../Images/2ebee5e3e816b18f4f7a090736386394.png)

文件组织结构示例（图片由作者提供）

基本上，我按照时间顺序整理我的笔记本（使用文件名中的数字进行排序），我给每本笔记本中开发的内容起非常清晰和明确的名字，我创建文件夹来存储生成的文件，以便将来更容易找到它们。除了方便信息组织之外，遵循这种结构还清楚地显示了达到结果的步骤和所使用的推理。

再次使用这种组织结构，或类似的结构，是一个极其简单的任务，只需自律即可完成。

接下来的两个步骤是最具挑战性的，因为它们要求你不仅要编写代码，还需要思考并且要有自律。

# 笔记本结构组织

![](../Images/f536b14b35bf89ca700234854dc9c98a.png)

图片由[Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我所说的笔记本结构组织是指在同一本笔记本的每一部分中明确你的工作内容。对于这个主题，使用 Markdown 将是你最好的朋友。因为你可以使用不同的样式来区分代码的主题和子主题。

对我而言，这部分几乎和写报告一样，你需要明确陈述每个实验的意图、假设、结果和分析。关键是你必须在目标和分析上非常明确，这需要前瞻性思维和客观性。

井然有序的笔记本结构在解释内容时非常有帮助，这在你向同行和上级展示你的分析时尤为重要。

![](../Images/bac5195c24b815c6d353d96b8ff15823.png)

笔记本结构组织示例（图像由作者提供）

正如你所见，这部分的工作量会根据笔记本的目标而有所不同。然而，我发现这项工作通常会带来更清晰的目标感，并帮助我理解每次分析的结果。这对我帮助很大，特别是在同时处理多个项目时。

# 代码组织

![](../Images/5b95086df149d01f5fe74215a1ec07be.png)

[Fahrul Razi](https://unsplash.com/@mfrazi?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上的照片

解释这个主题的最佳方式是：像向别人解释一样编写代码。使用大量注释，尽量做到清晰。刚开始这个习惯可能会有点困难，你可能会觉得注释代码有些懒惰，但随着时间的推移，你会变得非常擅长快速有效地注释，做到几乎自动化。尽量直接，节省文字和理解所需的时间。

另一个非常重要的点是对你开发的函数进行注释。如果没有人知道它的作用，那么即使函数非常有用也没用！在这里，强烈推荐使用文档字符串。再次强调，要详细解释函数的功能，但尽量不要使用过多的文字。

我使用的模板基本上包含三部分信息：函数的目的是什么（在信息部分），函数的输入变量是什么（在输入部分），以及函数的响应是什么（如果有的话，在输出部分）。下面是我为我在 Medium 上的另一篇文章制作的函数的示例：[遗传算法及其在机器学习中的实用性](https://medium.com/towards-data-science/genetic-algorithm-6aefd897f1ac)。

注释和组织代码的实践非常有用，这不仅有助于你明确自己的工作，还能使你的代码对你组织中的其他人有用。可重用、制作良好且有条理的代码在团队中非常有价值，因为它们可以节省多个员工在执行重复或类似任务时的时间。

另一个重要点是，如果你在项目中途去度假或请假，如果你的代码组织得很好，你会比需要记住你几天前写的所有代码或原因要更快地接续工作。

# 总结

![](../Images/e10375bd22631b3f58ccecc008a53230.png)

照片由[Glenn Carstens-Peters](https://unsplash.com/@glenncarstenspeters?utm_source=medium&utm_medium=referral)拍摄，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇文章中，我展示了在工作中更有条理的几个好处。有条理使你更加高效、清晰和客观，同时也锻炼了你的纪律性和思维能力，因为在写下分析结果或代码中的注释之前，你必须先思考你要写什么。如果你发现自己需要重写某些内容，不要惊讶，这通常是你在锻炼批判性思维，并寻找更高效的表达方式，这也是锻炼这一重要技能的过程。

如果你在职业生涯的早期阶段，更加有条理会对你帮助很大。能够清楚地回答上级提出的问题或清晰地解释你在分析或代码中的目标，这些都是能为你的工作加分的因素。

最后，我的建议是逐步开始，并调整你整理事物的方式，以便找到最适合你的方法。我在这里描述的方法是我为自己调整的，它们对我非常有效，但有时你可能会更适合使用不同的整理方式。不管怎样，有条理只会给你带来好处。

非常感谢你阅读这篇文章，希望它对你有所帮助。

任何评论和建议都非常欢迎。

欢迎通过我的 LinkedIn 联系我，查看我的 GitHub，以及阅读我在 Medium 上的其他文章。

[Medium](https://medium.com/@alexandrerossetolemos)

[LinkedIn](https://www.linkedin.com/in/alexandre-rosseto-lemos/)

[GitHub](https://github.com/alerlemos)
