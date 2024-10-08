# 数据科学成功秘诀：你在大学里没有学到的 4 项关键技能

> 原文：[`towardsdatascience.com/succeeding-in-data-science-4-essential-skills-you-didnt-learn-in-university-1920815acef3`](https://towardsdatascience.com/succeeding-in-data-science-4-essential-skills-you-didnt-learn-in-university-1920815acef3)

## 如何弥合学术界与数据科学领域就业之间的差距

[](https://medium.com/@tomergabay?source=post_page-----1920815acef3--------------------------------)![Tomer Gabay](https://medium.com/@tomergabay?source=post_page-----1920815acef3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1920815acef3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1920815acef3--------------------------------) [Tomer Gabay](https://medium.com/@tomergabay?source=post_page-----1920815acef3--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1920815acef3--------------------------------) ·6 分钟阅读·2023 年 5 月 11 日

--

![](img/d08ac260c10a4ed8700842214ccceada.png)

由 [krakenimages](https://unsplash.com/@krakenimages?utm_source=medium&utm_medium=referral) 提供的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

自从从荷兰的一所大学毕业后，我在不同组织和公司担任了多个数据科学家的职位。让我和一些其他毕业生感到惊讶的是，大学所教的技能与成为一名有价值的数据科学员工所需的技能之间的差距。在这篇文章中，我想强调大学所教的技能和我发现对数据科学员工表现良好所需的四大技能差距。

我在每一节中都包含了有用的资源，以帮助你提升在该领域的技能，若你希望提高你的熟练程度。

# 构建扎实的数据科学项目

在大学里，通常会使用 Jupyter Notebook 进行“笔记本式”的工作。我在大学时设置的最稳固项目结构是从*main.py*文件调用不同的模块，并使用*requirements.txt*。但是使用*setup.py*？或者*pyproject.toml*？没有。

公司对数据科学项目的要求往往大相径庭。你的数据科学项目通常应该被构建成一个可安装的包，例如通过`pip install <package_name>`。有时需要一个*requirements.txt*，但至少应该在*pyproject.toml*中指定包的依赖（现在比*setup.py*更受欢迎，参见[PEP-518](https://peps.python.org/pep-0518/)）。

除了可以安装外，你的数据科学项目通常还应能够在云中作为基于 Docker 的 API 运行。通过使用 REST 请求调用你的 API，可以通过管道处理数据，甚至用机器学习模型进行预测。

如果你发现自己也主要是在以 'Notebook 风格' 进行数据科学项目，或者你不熟悉如何构建一个可安装的 Python 项目，我建议阅读例如：

[](/how-to-convert-your-python-project-into-a-package-installable-through-pip-a2b36e8ace10?source=post_page-----1920815acef3--------------------------------) ## 如何将你的 Python 项目转换为通过 pip 安装的包

### 一个带有现成模板的教程，描述了如何将 Python 项目转换为可以在…中使用的包。

towardsdatascience.com [](https://medium.com/mlearning-ai/how-to-start-any-professional-python-package-project-9f66538ebc2?source=post_page-----1920815acef3--------------------------------) [## 如何启动任何专业的 Python 包项目。

### 包括测试自动化、在 ReadTheDocs 上创建文档以及发布到 PyPi。

medium.com](https://medium.com/mlearning-ai/how-to-start-any-professional-python-package-project-9f66538ebc2?source=post_page-----1920815acef3--------------------------------)

如果你能够构建一个可安装的包，但不确定如何将数据科学项目构建并运行为基于 Docker 的 API，请阅读：

[](/deploy-a-machine-learning-model-as-an-api-on-aws-43e92d08d05b?source=post_page-----1920815acef3--------------------------------) ## 在 AWS 上将机器学习模型部署为 API

### 逐步指南，构建模型 API、使用 Docker 容器化并通过 AWS Elastic 部署到网络。

towardsdatascience.com

# 在基于云的环境中工作

现在，几乎每家公司和组织都使用至少一些基于云的软件。目前，最受欢迎的有 [Microsoft Azure](https://azure.microsoft.com/)、[GCP](https://cloud.google.com/) 和 [AWS](https://aws.amazon.com/)。在我的大学里，我几乎没有使用这些云平台上提供的资源。然而，在工作中，数据科学家对云平台的知识几乎与他们的编程技能一样重要。以下是我作为数据科学家迄今为止需要完成的（云）工程任务的一个例子：

+   在 Docker 容器中部署 Python 应用程序

+   构建 CI/CD 管道

+   在云中托管 Python 包作为工件

+   构建数据管道

如果你对上述某个平台有一定的云知识，这在申请数据科学职位时已经是一个巨大的优势。每个平台都有自己的学习路线和证书来证明你在云计算方面的知识。每个平台都有不同角色的不同路线。以下是每个平台针对数据科学家的相关路线：

+   [Azure DP-100](https://learn.microsoft.com/en-us/certifications/azure-data-scientist/)

+   [GCP 机器学习工程师](https://cloud.google.com/learn/certification/machine-learning-engineer)

+   [AWS 数据分析](https://aws.amazon.com/certification/certified-data-analytics-specialty/)

获得这样的证书无疑会让你作为（潜在的）数据科学员工更具价值。

# 与其他数据科学家合作

大学里的数据科学和现实生活中的数据科学之间最重要的区别之一是，在现实生活中，你必须持续不断地协作，而在大学里，通常只有少数几个团队项目贯穿整个本科或硕士阶段。

到目前为止，我工作过的每家公司和组织都使用了 [*scrum*](https://www.scrum.org/resources/what-scrum-module) 或 [*DevOps*](https://en.wikipedia.org/wiki/DevOps)。我没有遇到过一次面试中对这两种方法的知识和/或经验没有加分的情况。除了使用 *scrum* 或 *DevOps*，代码审查也是数据科学家工作中的关键任务。这意味着你既需要能够编写清晰易读的代码以供他人审查，也需要能够快速且批判性地评估他人的代码。你可以在下面的链接中找到一篇非常有趣的关于如何为代码审查提供建设性反馈的文章：

[## 如何像人类一样进行代码审查（第一部分）](https://mtlynch.io/human-code-reviews-1/?source=post_page-----1920815acef3--------------------------------)

### 最近，我在阅读关于代码审查最佳实践的文章。我注意到这些文章专注于发现...

[mtlynch.io](https://mtlynch.io/human-code-reviews-1/?source=post_page-----1920815acef3--------------------------------)

此外，你不能再用不明确的函数或变量名称，或不遵守 Python 的 [PEP-8 命名规范](https://peps.python.org/pep-0008/) 的对象来应付了。如果你想了解更多关于如何编写高质量代码的内容，可以参考下面的文章：

[## 区分高级开发者和初级开发者的 6 个 Python 最佳实践](https://mtlynch.io/human-code-reviews-1/?source=post_page-----1920815acef3--------------------------------)

### 如何编写被视为来自经验丰富开发者的 Python 代码。

[towardsdatascience.com](https://mtlynch.io/human-code-reviews-1/?source=post_page-----1920815acef3--------------------------------)

# 处理现实生活中的数据

在大学里，学生遇到的大多数数据集已经经过大量预处理。这些数据集的列通常具有有意义的名称，错误的条目已被删除，数据类型也已经正确配置。你在公司或组织中遇到的数据通常远不如大学里遇到的数据标准，除非你在一家真正的数据驱动（技术）公司工作。

我在工作中遇到的一些杂乱的真实数据示例：

+   一些犯罪的数据，其中某些犯罪的实施日期在未来。

+   没有任何命名规范的列名 [ `name`, `Address`, `JobDescription`, `place_of_birth`]

+   重复的列具有不同的值 [ `job_title` `JobTitle` ]

+   不同时区的 DateTime 值而不涉及时区问题。

由于真实数据比为学生准备的数据要复杂得多，作为数据科学家，你要准备好花费大部分时间与业务方沟通，询问列和数值的解释、数据清理以及合并来自不同来源的数据，而不是进行数据分析或构建机器学习模型。

欲了解更多数据清理的信息，例如：

[](/the-ultimate-guide-to-data-cleaning-3969843991d4?source=post_page-----1920815acef3--------------------------------) ## 数据清理的终极指南

### 当数据产生垃圾时

[towardsdatascience.com

如果你想在一些真实（杂乱）数据集上练习，[这里有十个数据集](https://analyticsindiamag.com/10-datasets-for-data-cleaning-practice-for-beginners/)供你练习数据清理技能！

# 总结

大学教授的数据科学技能可能不足以在数据科学员工职位上出色表现。作为在多个组织工作过的数据科学家，我注意到四个主要技能缺口需要解决：

+   建立稳固的数据科学项目。

+   在基于云的环境中工作。

+   与其他数据科学家合作。

+   处理真实数据。

利用本文中提到的资源，你可以帮助缓解在这些高度重视的领域中可能存在的知识或技能缺乏！

> 当然，每所大学和每个专业都有自己的数据科学课程，一些大学可能在为你准备成为数据科学员工方面做得更好或更差。此外，我毕业已经有几年了，因此也许大学的数据科学课程在云领域等方面已经有所改进，例如增加了（更多）与云相关的课程。

# 资源

**稳固的 Python 项目** [`packaging.python.org/en/latest/tutorials/packaging-projects/`](https://packaging.python.org/en/latest/tutorials/packaging-projects/)

`medium.com/r/url=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-convert-your-python-project-into-a-package-installable-through-pip-a2b36e8ace10`

[`medium.com/mlearning-ai/how-to-start-any-professional-python-package-project-9f66538ebc2`](https://medium.com/mlearning-ai/how-to-start-any-professional-python-package-project-9f66538ebc2)

`towardsdatascience.com/deploy-a-machine-learning-model-as-an-api-on-aws-43e92d08d05b`

**基于云的环境** [`learn.microsoft.com/en-us/certifications/azure-data-scientist/`](https://learn.microsoft.com/en-us/certifications/azure-data-scientist/)

[`cloud.google.com/learn/certification/machine-learning-engineer`](https://cloud.google.com/learn/certification/machine-learning-engineer)

[`aws.amazon.com/certification/certified-data-analytics-specialty/`](https://aws.amazon.com/certification/certified-data-analytics-specialty/)

**与其他数据科学家合作** [`www.scrum.org/resources/what-scrum-module`](https://www.scrum.org/resources/what-scrum-module)

[`en.wikipedia.org/wiki/DevOp`](https://en.wikipedia.org/wiki/DevOps)

[`mtlynch.io/human-code-reviews-1/`](https://mtlynch.io/human-code-reviews-1/)

[`peps.python.org/pep-0008/`](https://peps.python.org/pep-0008/)

[`medium.com/towards-data-science/6-python-best-practices-that-distinguish-seniors-from-juniors-84199d4cac3c`](https://medium.com/towards-data-science/6-python-best-practices-that-distinguish-seniors-from-juniors-84199d4cac3c)

**与现实数据打交道** `towardsdatascience.com/the-ultimate-guide-to-data-cleaning-3969843991d4`

[`analyticsindiamag.com/10-datasets-for-data-cleaning-practice-for-beginners/`](https://analyticsindiamag.com/10-datasets-for-data-cleaning-practice-for-beginners/)
