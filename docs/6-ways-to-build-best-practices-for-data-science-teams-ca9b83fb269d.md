# 为数据科学团队建立最佳实践的 6 种方法

> 原文：[`towardsdatascience.com/6-ways-to-build-best-practices-for-data-science-teams-ca9b83fb269d`](https://towardsdatascience.com/6-ways-to-build-best-practices-for-data-science-teams-ca9b83fb269d)

## 为高绩效数据科学团队设定标准

[](https://rebeccalvickery.medium.com/?source=post_page-----ca9b83fb269d--------------------------------)![Rebecca Vickery](https://rebeccalvickery.medium.com/?source=post_page-----ca9b83fb269d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ca9b83fb269d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ca9b83fb269d--------------------------------) [Rebecca Vickery](https://rebeccalvickery.medium.com/?source=post_page-----ca9b83fb269d--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ca9b83fb269d--------------------------------) ·阅读时间 7 分钟·2023 年 3 月 20 日

--

![](img/be348ed520b4d053aaa9e0c6d1e8b9d1.png)

图片由 [Marvin Meyer](https://unsplash.com/@marvelous?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/team?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

数据科学是一个将传统数学和统计学与大规模计算能力以及更现代的机器学习和深度学习技术相结合的领域，从数据中生成见解。

数据科学学科复杂，具有实验性，并且存在很大的不确定性。与软件工程等相关领域类似，数据科学团队需要以受控的方式处理代码开发。然而，除了这些，数据科学家还需要处理不断变化的数据，并执行可重复的实验，例如试验机器学习模型的新特性。

因此，无论你是一个单独工作的数据科学家，还是一个大团队共同工作，制定一套最佳实践标准都非常重要。这些标准将确保：

+   **一个数据科学家可以在稍后的时间重复他们自己的实验、模型或见解。**

+   **团队中的其他数据科学家可以重复以上所有内容——如果你在维护生产中的模型，这一点尤其重要。**

+   **单个数据科学家和其他团队成员都能在现有工作基础上进行构建，而不是重复相同的实验、代码、模型或分析。**

在以下文章中，我将给出一些颇具个人观点的建议，说明如何以及使用什么来设定数据科学团队的最佳实践。然而，每个团队都是不同的，因此你可能需要调整这些建议以满足自己团队的具体需求。

**这是我关于数据科学领导力系列文章中的第二篇。第一篇文章链接如下。**

**学习成为数据科学领导者**

## 1\. 代码标准

高质量的代码确保你的团队编写的代码易于他人阅读和理解。这有助于提高团队工作成果的可重现性和可扩展性。

代码应该保持清晰、结构良好，并尽可能模块化。选择共享的编码标准是一个好主意。如果你的团队使用 Python，那么 [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html) 可以作为一个好的标准。编码风格可以通过使用如 [Pylint](https://readthedocs.org/projects/pylint/) 或 [Black](https://github.com/psf/black) 的工具，通过 linting 轻松地强制执行，作为开发过程的标准部分。

一个好的代码标准应该包括标准化的命名约定和用于代码文档的 doc 字符串。制定这些标准的目标应始终是使代码尽可能可读和易于理解，并减少团队的心理负担。一个好的编码标准会提高团队的效率，例如，如果代码始终遵循预期的风格，进行同行代码审查的速度会更快。

代码标准可以通过 [持续集成](https://www.atlassian.com/continuous-delivery/continuous-integration) (CI) 检查自动化，作为你的 GitHub 工作流的一部分。

## 2\. 虚拟环境

代码可重现性的一个重要部分是嵌入能够使团队成员编写的代码在任何其他计算机或云环境中运行的过程。

虚拟环境是实现这一点的常用技术。虚拟环境记录了与运行特定项目所需的依赖项（工具）及其版本相关的详细信息。

创建虚拟环境的常用工具包括 Poetry、[Pipenv](https://pipenv.pypa.io/en/latest/) 和 [Conda](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)。虽然它们的工作方式略有不同，但从高层次来看，虚拟环境通过创建和维护一个存储项目的 Python 版本和所有依赖项的文件来工作，然后使用该文件在任何机器上生成一个隔离的环境。

如果团队不能在不同的机器上精确重现相同的环境，那么在缺少原项目创建者的情况下，项目代码很可能无法在没有一些努力的情况下运行。

通常，每个项目有自己的虚拟环境是一个好做法。我建议团队内决定使用一个工具以保持一致性，并方便团队成员重现环境。例如，我自己的团队目前使用 Poetry 来管理我们所有项目的环境。

## 3\. 版本控制

所有数据科学团队应使用版本控制工具，如 Github，以确保所有工作的副本安全地存储在你的笔记本电脑之外，并且可以以受控的方式对现有代码进行更改。

创建拉取请求（PR）流程以促进协作、确保输出质量和分享知识通常是一个好主意。在我看来，PR 应在项目生命周期中定期提出，并且应尽量避免非常大的 PR。尽可能多地获取代码和项目方向的第二意见，以避免大规模的重写或错误，是良好的做法。

一般来说，以下标准应适用于 Github 的使用：

+   为分支设置标准化的命名约定。

+   鼓励使用清晰和描述性的提交信息。

+   为良好的[PR 礼仪](https://betterprogramming.pub/pull-request-etiquettes-for-reviewer-and-author-f4e80360f92c)制定标准。

+   每天提交代码。

+   每两周至少提一次 PR，或者如果你的团队使用冲刺周期，则在每个冲刺结束时提 PR。

## 4\. 组织良好的代码

团队中所有项目的一致文件夹结构将使其他团队成员更容易快速阅读和理解任何项目的代码。

网上有许多建议的数据科学文件夹结构的示例。我找到的可能最好的例子是[Cookiecutter](https://drivendata.github.io/cookiecutter-data-science/)模板。然而，根据我的经验，文件夹结构在数据科学团队中差异很大，因为使用的具体工具、技术和任务会大大影响标准结构的要求。因此，我的建议是使用类似 Cookiecutter 的工具，但根据团队的需求进行调整。

我非常信奉 KISS（“保持简单，愚蠢”）设计原则，并且会选择最基本的文件夹结构以满足团队的需求。下方展示了我团队用于开发工作的文件夹结构。你可以看到我们选择了一个简单的结构，满足了我们非常特定的需求，但这不一定适用于其他团队。

![](img/f98f59c53c537a42c5aa0eecf58ec6bd.png)

我团队的文件夹结构。图片来源：作者。

## 5\. 文档

良好的文档实践是确保团队工作可重复性和可复现性的另一种方式。文档应以标准方式记录，并且要详尽到足以确保对项目不熟悉的人能够理解运行任何代码，并复现和理解任何结果。

一般来说，我会在数据科学团队的最佳实践中包含以下类型的文档。

**代码文档：** 所有代码应附带理解和运行所需的最少文档。这包括为函数添加文档字符串，在项目文件夹或仓库中包含 README.md 文件，以及为存储在 Jupyter Notebooks 中的代码添加注释。

**产品文档：** 产品文档涉及与项目的开发、设计和结果相关的信息。通常，这些文档可能采用 Google 文档、Confluence 页面或 Miro 白板的形式。这类文档的最终用户通常是非技术性利益相关者和产品经理。

## 6. 组织良好的 notebooks

Jupyter Notebooks 的特性可能导致代码混乱、难以阅读和不可重现。因此，在使用 notebooks 时，遵循本文中已经概述的最佳实践，如版本控制和良好的代码风格，是至关重要的。然而，也需要添加一些与 notebook 使用特别相关的额外最佳实践。

以下是包含在你的 notebook 最佳实践中的一些良好标准列表。

+   利用 notebook 中丰富的代码注释能力，包含标题、章节标题、注释、图像和图表，以使代码和结果尽可能易于理解。

+   将所有导入语句放在 notebook 顶部的单个单元格中。

+   确保数据源明确。这可能意味着在 notebook 中包含指向云存储桶或 SQL 查询的链接。

+   保持 notebook 简单，尽量减少代码行数。在合适的地方将复杂的代码抽象为函数和模块，并确保删除任何冗余的代码。

由于其复杂和实验的特性，数据科学是一个在标准化方法和团队最佳实践中获益匪浅的领域。上述列出的所有最佳实践旨在确保团队项目的可重复性、可扩展性和可复现性。

此外，尽管最佳实践需要一些时间和精力来嵌入和遵守，但从长远来看，它们将节省大量时间和精力，避免重新运行和理解项目或重复已经完成的工作。

我上面列出的最佳实践并非详尽无遗，根据团队使用的工作类型和工具，你的最佳实践可能会有所不同。本文旨在作为最佳实践的入门指南，并应提供一些灵感，以帮助你开始为团队定义和设定自己的标准。

如果你想深入了解数据科学团队的最佳实践，我在本文底部列出了有用的进一步阅读资料。

感谢阅读！

## 有用的资源

[*IBM 数据科学最佳实践*](https://ibm.github.io/data-science-best-practices/model_training.html)

[*Google Cloud 的最佳实践：提升任何使用 Jupyter Notebooks 开发者的工作体验*](https://cloud.google.com/blog/products/ai-machine-learning/best-practices-that-can-improve-the-life-of-any-developer-using-jupyter-notebooks)

*用于可重复 Jupyter Notebooks 的 4 个工具*

*组织数据科学项目的方案*
