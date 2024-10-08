# 我的数据科学工作流程模板

> 原文：[`towardsdatascience.com/heres-my-data-science-workflow-template-caa83c1e3032`](https://towardsdatascience.com/heres-my-data-science-workflow-template-caa83c1e3032)

## 一份专注于数据科学方面的快速指南

[](https://datascience2.medium.com/?source=post_page-----caa83c1e3032--------------------------------)![Matt Przybyla](https://datascience2.medium.com/?source=post_page-----caa83c1e3032--------------------------------)[](https://towardsdatascience.com/?source=post_page-----caa83c1e3032--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----caa83c1e3032--------------------------------) [Matt Przybyla](https://datascience2.medium.com/?source=post_page-----caa83c1e3032--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----caa83c1e3032--------------------------------) ·阅读时间 6 分钟·2023 年 2 月 22 日

--

![](img/d4d5b55acda5558f032c25d0eb85673f.png)

[Campaign Creators](https://unsplash.com/@campaign_creators?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/--kQ4tBklJI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片 [1]。

# 目录

1.  介绍

1.  数据探索、数据汇总和特征工程

1.  算法比较

1.  摘要

1.  参考文献

# 介绍

虽然我已经写了涵盖整个数据科学过程的文章，包括业务理解和利益相关者协作等方面，但我想专注于数据科学部分。在这篇文章中，我将提供一个供数据科学家使用和构建的模板。我假设更多的中高级数据科学家已经遵循这个通用模式，所以如果你在数据科学方面较为初级、刚刚入门或对数据科学感兴趣，这篇文章适合你。话虽如此，让我们更深入地探讨数据科学过程的主要部分，包括数据探索、数据汇总、特征工程和算法比较的步骤。

# 数据探索、数据汇总和特征工程

![](img/0a88fe82e541f8e5edc8ead6d7abc837.png)

[Jason Briscoe](https://unsplash.com/@jsnbrsc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/amLfrL8LGls?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片 [2]。

我们要记住，不同的公司和角色有所不同，因此这个模板可能需要根据你的情况进行更新。下面描述的步骤是当你已经定义了业务问题并与利益相关者会面后进行的步骤。根据你的情况，你也可能需要负责数据工程和数据分析的其他部分。

> 数据探索

+   **定义你的初始数据集** → 你可能在训练模型之前已经对需要的数据有了假设，因此会从这些特征开始，并进一步迭代。

+   **定义该数据集的来源** → 是来自当前的公司数据库表，还是一次性的 Google Sheet/Microsoft Excel 表格，或是第三方数据平台等？

+   **数据目前是否存在？** → 如果存在，则继续前进。如果不存在，则你可能需要与数据工程/数据分析等合作，以提供模型所需数据的准确来源。

— 你也可能需要向数据工程师提供理由，例如，请求特定数据点。例如：我希望得到按邮政编码分组的人口数据 — 先自行进行分组，并提供这种分组如何影响准确性以及最终业务问题的理由，以验证数据工程的时间。

> 数据汇总与特征工程

+   **你的数据是否已经正确汇总？** → 如果是，则继续前进。如果不是，则需要对数据进行汇总并以该格式存储。例如，如果你想预测县级人口，可能需要将以州（*地理分组*）为基础的训练数据行进行汇总。你可能能够在没有数据工程帮助的情况下进行数据存储，并使用如 Python 编程语言中的 pandas 等工具进行一些分组技术。

+   **你是否想创建新的模型特征？** → 如果是，则继续前进。如果不是，则需要通过相关性、自动特征选择、SHAP 特征重要性等工具，识别和测试更多可以解释你预测变异性的特征。

***反思与可能的惊喜：***

本节的关键点是，你可能在开始数据科学项目时并没有预期的那么多相关数据/特征。话虽如此，开始初步数据探索时，要对这种情况保持警惕。

# 算法比较

![](img/0d74685be1e5ac406f2ecd52a1836590.png)

图片由[Liz Sanchez-Vegas](https://unsplash.com/@whatdaliz?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，发布在[Unsplash](https://unsplash.com/photos/NQTqZLh6CxE?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) [3]。

需要记住的一点是，最后一节的最后一步，*特征工程*，可以与*算法比较*的顺序来回切换，因为你可能会发现不同的特征对于某些算法更有用或更易于使用。

现在你已经有了你的特征（*或至少是一个起点*），你会想选择一个特定的算法来构建最终模型。

> 算法比较

+   **你的目标是一个连续的数字还是一个类别？** → 如果答案是一个连续的数字，那么你会比较回归算法；如果是一个类别，那么你会比较分类算法。

+   **你选择什么算法？** → 开始时最好选择尽可能多的你了解的算法，然后再从中筛选。比较算法有很多方法，例如分别运行每一个，或者创建一个循环比较你选择的所有算法，但我发现最简单的方法是使用一个已经能并排比较算法的库，如[Moez Ali](https://medium.com/u/fba05660b60f?source=post_page-----caa83c1e3032--------------------------------)的 PyCaret 库——以下链接展示了极其有价值的 compare_models()函数：

[](https://nbviewer.org/github/pycaret/pycaret/blob/master/tutorials/Regression%20Tutorial%20Level%20Beginner%20-%20REG101.ipynb?source=post_page-----caa83c1e3032--------------------------------) [## Notebook on nbviewer

### 一旦设置成功执行，它会打印出包含若干重要部分的信息网格……

[nbviewer.org](https://nbviewer.org/github/pycaret/pycaret/blob/master/tutorials/Regression%20Tutorial%20Level%20Beginner%20-%20REG101.ipynb?source=post_page-----caa83c1e3032--------------------------------)

+   **比较不同算法时我应该关注什么？** → 根据你的用例，你可以比较一些因素，但通常你会关注损失函数，比如 MAE 或 RMSE，例如，对于回归目标。你还可以查看训练所需的时间，以及是否需要添加超参数调整等。

***反思与可能让你惊讶的事：***

算法比较曾经是非常繁琐的，随着时间的推移，你可以利用越来越多的工具来轻松比较算法，这样你就可以花更多时间在你最终选择的算法上。你甚至可以在同一时间窗口内训练和比较多个算法，更轻松地针对特定的损失函数进行优化，这样你就不会局限于单一算法，因为你的基础数据和假设可能会发生变化。

# 总结

虽然公司和角色可能会有所不同，但数据科学流程中有一些关键步骤是被广泛实践的。我们讨论了几个涵盖常规项目的数据科学工作流的步骤。

> 总结一下，这里是我们在数据科学工作流模板中讨论的步骤：

+   **定义你的初始数据集**

+   **定义此数据集的来源**

+   **数据是否已经存在？**

+   **您的数据是否已经正确汇总？**

+   **您是否想创建新的模型特征？**

+   **您的目标是连续数字还是类别？**

+   **您选择哪个算法？**

+   **在比较不同算法时，我需要关注什么？**

我希望您发现我的文章既有趣又有用。如果您作为数据科学家的经验与我的相同或不同，请随时在下方评论，或者您对第一次进入数据科学有什么期望？为什么或者为什么不？您认为还应该讨论哪些其他内容，包括更多的步骤？这些内容肯定可以进一步阐明，但我希望能够为您提供一些关于典型数据科学工作流程的预期信息。

***我与这些公司没有任何关联。***

*请随时查看我的个人资料，* [Matt Przybyla](https://medium.com/u/abe5272eafd9?source=post_page-----caa83c1e3032--------------------------------)，*以及其他文章，同时可以通过以下链接订阅以接收我的博客的电子邮件通知，或者* ***点击屏幕顶部关注图标旁的订阅图标***，如果您有任何问题或评论，请在 LinkedIn 上与我联系。*

**订阅链接：** [`datascience2.medium.com/subscribe`](https://datascience2.medium.com/subscribe)

**推荐链接：** [`datascience2.medium.com/membership`](https://datascience2.medium.com/membership)

(*如果您通过 Medium 注册会员，我将获得佣金*)

# 参考文献

[1] 照片由 [Campaign Creators](https://unsplash.com/@campaign_creators?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，拍摄于 [Unsplash](https://unsplash.com/photos/--kQ4tBklJI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)，(2018)

[2] 照片由 [Jason Briscoe](https://unsplash.com/@jsnbrsc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，拍摄于 [Unsplash](https://unsplash.com/photos/amLfrL8LGls?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)，(2019)

[3] 照片由 [Liz Sanchez-Vegas](https://unsplash.com/@whatdaliz?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，拍摄于 [Unsplash](https://unsplash.com/photos/NQTqZLh6CxE?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)，(2019)

[4] [Moez Ali](https://medium.com/u/fba05660b60f?source=post_page-----caa83c1e3032--------------------------------)，PyCaret，[PyCaret 回归教程 (REG101) — 初学者级别](https://nbviewer.org/github/pycaret/pycaret/blob/master/tutorials/Regression%20Tutorial%20Level%20Beginner%20-%20REG101.ipynb)，(2023)
