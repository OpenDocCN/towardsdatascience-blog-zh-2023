# 深入了解 Colab 的新更新和增强功能

> 原文：[`towardsdatascience.com/a-close-look-at-colabs-new-updates-and-enhancements-f1225fd5d504`](https://towardsdatascience.com/a-close-look-at-colabs-new-updates-and-enhancements-f1225fd5d504)

## 充分利用 Google Colab 笔记本

[](https://pandeyparul.medium.com/?source=post_page-----f1225fd5d504--------------------------------)![Parul Pandey](https://pandeyparul.medium.com/?source=post_page-----f1225fd5d504--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f1225fd5d504--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f1225fd5d504--------------------------------) [Parul Pandey](https://pandeyparul.medium.com/?source=post_page-----f1225fd5d504--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f1225fd5d504--------------------------------) ·阅读时间 8 分钟·2023 年 10 月 3 日

--

![](img/fdb868990b807acbc8ae4bc0f5c6484a.png)

图片由作者使用 [Excalidraw](https://excalidraw.com/) 创建。

*最后更新于 2023 年 12 月 20 日*

每当我进行编码研讨会或教程时，[Google Colaboratory Notebooks](https://colab.google/)——或称为 Colab——始终是我首选的资源。它消除了讲师和参与者在环境设置上的麻烦，并且提供了对强大计算资源（如 GPUs 和 TPUs）的免费访问。凭借其易于共享的链接，Colab 使整个学习过程更加高效和有效。为了充分利用 Colab 提供的功能，我始终关注其最新发布和更新。

虽然我通常 [通过 LinkedIn 分享这些更新](https://www.linkedin.com/posts/parulpandeyindia_googlecolab-datascience-dataanalysis-activity-7092134378327195649-KRHb?utm_source=share&utm_medium=member_desktop)，但大量的新功能和增强功能值得这样一篇更全面的文章。我之前的 Colab 重要功能汇总是在 2022 年，显然现在需要一个更新的概述。

[](/use-colab-more-efficiently-with-these-hacks-fc89ef1162d8?source=post_page-----f1225fd5d504--------------------------------) ## 使用这些技巧更高效地使用 Colab

towardsdatascience.com

让我们看看 Colab 的一些杰出功能，它们在我的工作中非常宝贵。我希望你也会发现它们同样有用。

# 1\. 来自 Google Sheets 的智能数据粘贴

现在，当用户将数据粘贴到一个空的代码单元格时，Colab 会自动生成代码来创建一个`pd.DataFrame`。这一增强功能消除了传统流程中的额外步骤，使用户体验更加流畅。

![](img/3c7f1248b71b3ea0dec9e9632ee05c87.png)

从 Google Sheets 智能粘贴数据 | 图片由作者提供

而且不仅如此——如果代码单元格中已经有文本，Colab 会贴心地为你添加 CSV 文字。

# 2\. 从 Pandas 数据框自动生成图表

Colab 中的数据可视化现在变得更加便捷，通过其新功能——从 Pandas 数据框自动生成图表。当你执行一个以数据框引用结束的代码单元格时，输出的右下角会出现一个自动绘图按钮。

![](img/713749093f9e00ea1c0e2c8202a809fe.png)

在 Colab 中自动生成 Pandas 数据框图表的图标 | 图片由作者提供

点击此图标会显示针对你的数据框量身定制的各种推荐图表，每一个图表都可以通过单击无缝地添加到你的笔记本中。

![](img/835312b4939c346156796b2cb6ba6e0e.png)

从 Pandas 数据框自动生成图表 | 图片由作者提供

如演示所示，这个功能不仅展示了渲染的图表，还附加了相关代码，并将其整齐地放置在笔记本的后续单元格中。

# 3\. 交互式数据框

Pandas 数据框提供了一种结构化的表格格式，这种格式直观且兼容多种数据操作。使用 Google Colab，用户可以实时与这些数据框进行交互。当数据框在 Colab 中加载并显示时，会出现一个类似计算器的图标，如下所示。

![](img/26f60550f1605e4148edc3e74dd1c871.png)

渲染数据框交互式图标 | 图片由作者提供

点击此图标，数据框会转换为交互式表格：

![](img/5a8616ed00d798ff00715af2b4e43a12.png)

Colab 中的交互式数据框 | 图片由作者提供

如你所见，我们立即展示了良好的表格格式。增强的交互性提供了许多优势，例如：

+   调整页面大小并使用分页功能可以提供更清晰的数据视图。

![](img/042974edb305943cbbc9c726490d3afb.png)

调整页面大小并使用分页功能可以提供更清晰的数据视图 | 图片由作者提供

+   简单点击列标题可以让用户对整个数据集中的列进行排序，更新会立即可见。

![](img/61d7034efca6d796ed527460b893cd1f.png)

在交互式格式中排序列变得容易 | 图片由作者提供

+   可以通过直接字符串匹配或正则表达式匹配轻松筛选特定行。例如，要突出显示美国原产的汽车，用户可以在`**origin**`字段中搜索`USA`。还可以在搜索中设置值的边界。例如，识别 1970-1971 年间所有具有八个气缸且每加仑至少 15 英里燃油效率的美国原产汽车。

![](img/f7fa7a8fcbe6bdffd0b0e4cbabba5305.png)

交互式数据框中的筛选操作 | 图片由作者提供

+   数据可以以各种格式轻松复制，例如 CSV、markdown 或 JSON。

![](img/cb18dbbd26d5a0a34e9fe820025d9e46.png)

数据可以轻松以各种格式复制，例如 CSV、markdown 或 JSON | 图片来源：作者

本质上，Google Colab 的交互式表格支持动态数据搜索和过滤。这消除了不断重新运行单元格的需要，使数据洞察更加迅速，并改善了整体数据探索过程。

# 4\. 管理单元格执行

Colab 现在支持两个新功能，旨在简化用户在笔记本中运行代码单元格的方式。

+   其中第一个功能允许用户直接从笔记本的目录运行一组单元格。这可以通过上下文菜单中的`**Run cells in section**`选项完成，该选项由三个垂直点表示。

![](img/6fb942282b9a6028d2b0ebfca1a6f4c7.png)

从笔记本的目录直接运行一组单元格 | 图片来源：作者

+   第二个功能专注于在笔记本中设置先决条件。**用户现在可以指定特定单元格在打开笔记本时自动运行**。这可以通过位于`**Edit**`菜单下的笔记本设置对话框进行设置，在那里有一个选项可以`Automatically run the first cell or section`。如果笔记本仅以一个单元格开始，则该单元格将在每次访问笔记本时自动运行。类似地，如果笔记本以一个部分开始，则在打开笔记本时，该部分的所有初始单元格将自动执行。

![](img/3507d22a43d5fd9cba3bad2799634300.png)

指定特定单元格在打开笔记本时自动运行 | 图片来源：作者

# 5\. 语言之间更轻松的转换

Google Colab 更新了其界面，以简化不同语言之间的切换过程。用户现在可以通过进入`Edit`然后选择`Notebook settings`轻松切换已安装的内核。**R 编程语言是可用选项之一，**因为它是默认安装的。

![](img/687c0cafed715519a90b84661b8b50aa.png)

在 Colab 中转换 Python 和 R | 图片来源：作者

# 6\. 笔记本 X Colab

通过弥合各种笔记本与强大的 Colab 平台之间的差距，Colab 帮助提升用户体验并扩展 Colab 的多功能性。

## 1\. Hugging Face 🤗 笔记本在 Colab 中

用户现在可以轻松访问、查看和运行[Hugging Face 笔记本](https://huggingface.co/spaces/librarian-bots/notebooks-on-the-hub/blob/main/welcome_notebook_on_the_hub.ipynb)在 Colab 环境中，受益于其先进的执行功能和协作工具。这种集成简化了从 Hugging Face 上的模型托管到 Colab 中的实时演示的过渡。只需点击笔记本右上角的`Open in Colab`即可在 Colab 平台内启动和执行它们。你可以在[这里](https://medium.com/google-colab/hugging-face-notebooks-x-colab-722d91e05e7c)了解更多信息。

![](img/c4df1b830211d707bc38cfdc150c3e16.png)

在 Colab 环境中运行 Hugging Face 笔记本 | 图片来源：作者

## 2\. 移植到 Colab 的流行笔记本

Google Colab 不仅仅是一个编写和执行机器学习模型的平台，它还是一个集成复杂代码的卓越讲故事工具。Colab 拥有一系列有见地的案例研究，例如：

+   斯坦福大学的 Ashwin Rao 教授制作了一个引人注目的 Colab 笔记本，深入探讨了 [***硅谷银行（SVB）失败的高层原因***](https://colab.research.google.com/drive/15uxrAeCCL327kWH9N0X-ogKwf2zErjP5)。

+   Colab 团队还成功移植了流行的 ***Ten Minutes to Pandas*** 教程，为用户提供了一个引人入胜的 pandas 入门介绍。

# 7\. 使用 Colab 的新 URL 快捷方式快速创建笔记本

类似于你可以通过链接如 `[**docs.new**](http://docs.new)` 立即创建新文档，或通过 `[sheets.new](http://sheets.new)` 创建新电子表格，现在也有一个简单的链接用于 Google Colab！如果你访问 `**c**[**olab.new**](http://colab.new)`，你将立即开始一个新的笔记本。

![](img/e25b4c9d781600cff8618860a4458b4a.png)

使用 Colab 的新 URL 快捷方式快速创建笔记本 | 图片由作者提供

# 8\. 程序化终止 Colab 会话

另一个新增功能是能够直接从代码中终止运行时。

```py
from google.colab import runtime
runtime.unassign()
```

`unassign()` 函数指定要移除的活动运行时，并断开与任何笔记本会话的连接。无论是为了节省内存、重置会话还是结束项目，这个功能都确保用户可以直接在代码环境中轻松准确地完成操作。

![](img/e49aaffbcd87f45521d143cf10a86da2.png)

程序化终止 Colab 会话 | 图片由作者提供

# 9\. Colab 链接预览在 Docs 套件中

Colab 链接预览已集成到 Google Docs 套件中。现在，Colab 中的 Drive 笔记本链接可以转换为 Docs 套件中的智能卡片。此外，这一增强功能允许用户直接在 Google 生态系统内请求访问 Colab Drive 笔记本，简化了访问过程。

![](img/a0be6ba0b77273083981c05a47d572d5.png)

Colab 链接预览在 Docs 套件中 | 来源: https://x.com/GoogleColab/status/1701684666972205243?s=20

# 10\. 生成 Colab 徽章

访问 [**openincolab.com**](https://openincolab.com/) 为你的 README、网站、文档等创建徽章。只需输入托管在 GitHub 上的笔记本的 URL，测试徽章，并获取 HTML 代码！

![](img/62336adf143381353b14e8a59490907d.png)

生成 Colab 徽章 | 图片由作者提供

# 11\. 在 Colab 中轻松访问 Transformers

Colab 的托管运行时镜像现在预装了 **transformers** 库，为用户带来了一个小而重要的改进。保持设置的最新状态，定期更新镜像，使用 `!pip install transformers --upgrade` 强制升级以获取最新功能。

![](img/9eaafe5a084ce7f7c62bd7199eab3aef.png)

Transformers 库预装在 Colab 中 | 图片由作者提供

这一改进简化了工作流程，并解锁了令人兴奋的功能，例如轻松将 **Huggingface** 数据集读取到 Pandas 中。

![](img/ac8b396838cc82b662ecf7a1c9fcb763.png)

轻松将 **Huggingface** 数据集读取到 Pandas 中 | 图片由作者提供

# 12\. 使用 Colab 新的秘密功能保护您的 API 密钥

Colab 现在推出了一项新功能，允许您安全地存储私人密钥，如 Huggingface 或 Kaggle API 令牌。通过 `Secrets`，这些值保持私密，仅对您和您选择的笔记本可见。这确保了在进行项目时的安全性和安心感。

![](img/46b3cd8f8ef5c9957c797c0aa723e5bd.png)

使用 Colab 新的秘密功能保护您的 API 密钥 | 图片由作者提供

# 13\. Colab 向所有用户推出限时 AI 代码助手试用

Colab 现在向免费计划的用户提供 [限时试用 **AI 代码助手**](https://blog.google/technology/ai/democratizing-access-to-ai-enabled-coding-with-colab/)。AI 驱动的代码助手支持从自然语言生成代码和代码助手聊天机器人。根据官方博客，Colab 预计此服务将持续到 2024 年，只要容量允许。

![](img/bffaf1b62279825a934478d346402cac.png)

使用 AI 代码助手来调试代码 | 图片由作者提供

# 结论

总结来说，这些最近对 Google Colab 的更新和改进在我使用该平台的日常工作中证明是非常有用的。从与 Google Docs 套件的集成到高级数据过滤功能等，这些更新共同提升了 Colab 作为协作数据科学工具的地位。所有这些更新都已从其官方发布说明和博客中整理出来，相关链接已在参考部分共享。我鼓励大家探索这些功能，如果你发现了额外的功能或有个人见解，请与社区分享。你的见解可能对他人有很大帮助！

# 参考资料

+   [Colaboratory 发布说明](https://colab.research.google.com/notebooks/relnotes.ipynb#scrollTo=tIOYL5w7ONNT)

+   [Colab 的官方 Medium 博客](https://medium.com/google-colab)
