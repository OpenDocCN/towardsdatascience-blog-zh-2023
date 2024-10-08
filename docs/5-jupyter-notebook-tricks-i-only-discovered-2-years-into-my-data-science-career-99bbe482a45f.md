# 5 个我在数据科学生涯中仅发现了 2 年的 Jupyter Notebook 技巧

> 原文：[`towardsdatascience.com/5-jupyter-notebook-tricks-i-only-discovered-2-years-into-my-data-science-career-99bbe482a45f`](https://towardsdatascience.com/5-jupyter-notebook-tricks-i-only-discovered-2-years-into-my-data-science-career-99bbe482a45f)

## 自定义键盘快捷键、高亮文本等

[](https://medium.com/@mattchapmanmsc?source=post_page-----99bbe482a45f--------------------------------)![Matt Chapman](https://medium.com/@mattchapmanmsc?source=post_page-----99bbe482a45f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----99bbe482a45f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----99bbe482a45f--------------------------------) [Matt Chapman](https://medium.com/@mattchapmanmsc?source=post_page-----99bbe482a45f--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----99bbe482a45f--------------------------------) ·阅读时间 7 分钟·2023 年 7 月 17 日

--

![](img/11a5e5cc7fc3b7013b0a0b06b1b1f927.png)

图片来源于 [Lukas Bato](https://unsplash.com/@lks_bt) 在 [Unsplash](https://unsplash.com/photos/BAhBnhkbVuU)

尽管 R、Python 和 Julia 的用户都很喜欢使用它们，Jupyter Notebook 的潜力却很少被充分利用。

大多数用户知道基本命令（执行代码、注释、保存等），但很少有人利用 Jupyter 的隐藏技巧 — **尽管这些技巧可以节省大量时间和精力**。

自 2019 年（我开始使用 Jupyter）以来，我发现了很多节省时间的技巧和窍门，这些是大多数初学者不知晓的。在本文中，我将展示我最喜欢的 5 个技巧：

1.  **键盘快捷键** — “现成的”命令，例如插入/删除/移动单元格和文本

1.  ***自定义* 键盘快捷键** — 可以从键盘直接添加自己的高级命令，例如上下移动单元格、重启内核以及运行到当前单元格

1.  **Markdown 格式** — 创建表格、格式化文本和创建复选框

1.  **HTML 格式** — 突出显示文本并使评论更引人注目

1.  **“启用输出滚动”** — 抑制冗长的单元格结果（在调整超参数时，这是一笔巨大的财富）

# 提示 1：键盘快捷键

键盘快捷键提供了一种方便的方法来导航 Jupyter Notebook 并执行命令。以下是我常用的主要快捷键：

## 运行单元格

+   `Shift + Enter` — 运行当前单元格并选择下一个单元格

+   `Ctrl/Cmd + Enter` — 运行当前单元格

+   `Option/Alt + Enter` — 运行当前单元格并在下方插入另一个单元格

## 保存进度

+   `Ctrl/Cmd + s` — 保存笔记本

## 插入/删除单元格

首先，点击单元格内，然后确保你在`command`模式下，按`Esc`键。如果不按`Esc`，你将处于`edit`模式，只能对单元格内容进行操作（而不是单元格本身）。一旦你进入`command`模式，单元格中的光标将停止闪烁。然后，按以下任意一个键：

+   `a` — 在当前单元格上方插入一个新单元格

+   `dd`（按`d`键两次） — 删除当前单元格

+   `b` — 在当前单元格下方插入一个新单元格

## 更改单元格类型

Jupyter Notebooks 的一个乐趣在于它们允许你将评论和代码并排放置。要设置单元格的类型并确定其应将文本视为“评论”还是“代码”，首先通过选择一个单元格并按`Esc`键进入`command`模式。然后，按以下任意一个键：

+   `m` — markdown 模式（用于编写评论和标题）

+   `y` — 代码模式（用于编写代码）

## 选择多个单元格

再次确保你在`command`模式下，然后按住`Shift`键，使用`Up`或`Down`箭头扩展选择范围，选择任意数量的单元格。

![](img/4c5337e6becdbdd43bddaf7ae81784fb.png)

作者提供的图片

选定所有想要操作的单元格后，你可以一次性删除或移动它们，而不是逐个进行。

# 提示 2：自定义键盘快捷键

这对我来说是一个改变游戏规则的功能。

除了安装 Jupyter 时为你预定义的标准键盘快捷键外，你还可以创建你自己的*自定义*快捷键来执行高级命令。

例如，假设我想定义两个新的键盘快捷键：一个用于将单元格向上移动，另一个用于将单元格向下移动。默认情况下，可以通过使用鼠标“拖放”单元格来完成，但也可以创建一个自定义键盘快捷键，一键即可完成这个操作。

首先，点击 JupyterLab 窗口中的‘设置’，然后点击‘高级设置编辑器’，你将能够看到所有现有的键盘快捷键。

![](img/29f4284794f8d1f81755facc5bb64572.png)

作者提供的图片

要添加自定义键盘快捷键，你需要点击高级设置编辑器窗口右上角的‘JSON 设置编辑器’（如上图所示）。

然后，在右侧的‘用户首选项’标签页中，向下滚动到键盘快捷键列表，并添加所需的快捷键。我添加了这两个，是 [David](https://stackoverflow.com/a/66310163) 在 StackOverflow 上建议的。

```py
{
    "command": "notebook:move-cell-up",
    "keys": [
        "Ctrl Shift ArrowUp"
    ],
    "selector": ".jp-Notebook:focus"
},
{
    "command": "notebook:move-cell-down",
    "keys": [
        "Ctrl Shift ArrowDown"
    ],
    "selector": ".jp-Notebook:focus"
},
```

![](img/97979b07b242cf904e4ac08333eb7d92.png)

作者提供的图片

完成快捷键添加后，点击‘保存’（在右上角），然后你就可以开始使用了！

很酷吧？

*如果你喜欢这个故事，点击我的‘关注’按钮对我来说意义重大——只有 1% 的读者会这样做！感谢阅读。*

# 提示 3：Markdown 格式

当你在所谓的“注释”单元格中写文本时，你实际上是在使用 markdown，所以，惊喜惊喜，你可以使用 markdown 格式进行书写。

这可能看起来显而易见或微不足道，但根据我的经验，很少有 Jupyter 用户充分利用他们笔记本中的 markdown 功能。

使用 markdown，你可以书写文本、标题、列表、表格，甚至代码片段。这是为你的笔记本带来更多动态和结构化的好方法。

例如，这里是我最常使用的一些格式。我特别喜欢‘表格’格式（因为这是展示某些预处理代码结果的好方法），并且我推荐使用这个[Markdown Tables Generator](https://www.tablesgenerator.com/markdown_tables)来快速生成正确格式的表格。

![](img/d2b3b1719a77f38b0357c1b0f7d49644.png)

使用 markdown 格式你可以做的一些事情。作者提供的图片

# 提示 4: HTML 格式化

这个提示有点像上面的扩展，但使用频率更低。

当你在 markdown 单元格中写文本时，你可以使用 HTML 标签来格式化文本，实现*非常*自定义的格式，比如高亮文本和在切换中隐藏文本。

![](img/85e8cd228e432e1c2e0f0af3eb52d209.png)

作者提供的图片

就个人而言，我发现高亮功能在我审查他人笔记本时非常有用，尤其是当我想做评论或标记一些稍后需要回顾的内容时。这是一种很好的方式来突出我留下的注释，而不至于让笔记本显得杂乱。

# 提示 5: “启用输出的滚动”

默认情况下，Jupyter Notebooks 中的‘output’单元格会根据输出的高度进行调整——即当你运行一些代码时，Jupyter 会打印你代码指示的所有内容。我是说*所有*内容。

通常这非常有用，但在你运行打印大量输出的代码时，笔记本单元格可能会迅速增长到荒谬的长度，使得在笔记本中导航变得困难。

例如，下图展示了一个简单的代码片段，它将打印从 1 到 1001 的每一个整数。正如你所见，输出是很长的。

为了在不丢失有价值的打印信息的情况下压缩输出，右键点击输出单元格并选择‘启用输出的滚动’。这将输出保留在一个可滚动的输出单元格中，但避免了笔记本的杂乱。

![](img/47398cb17b8fc5c0bc5b697cdd6a7936.png)

作者提供的图片

在实际应用中，我发现这非常有用，比如（a）打印出表中所有列的名称，或者（b）使用`optuna`这样的库来调整模型的超参数。在这两种情况下，看到所有输出（所有列名或每轮训练的结果）都很有价值，我很少想要完全*抑制*这些输出，因为它们对调试非常有帮助。通过选择‘启用输出滚动’，我可以保留输出但将其最小化，使其不会占据整个笔记本。

# 还有一件事 —

我开设了一个名为[AI in Five](https://aiinfive.substack.com/)的免费新闻通讯，每周分享 5 个要点，涵盖最新的 AI 新闻、编码技巧以及数据科学家/分析师的职业故事。没有炒作，没有“**数据是新的石油**”的废话，也没有埃隆的推文——只有实用的技巧和见解，帮助你在职业上有所发展。

[在这里订阅](https://aiinfive.substack.com/) 如果你对此感兴趣！

[](https://aiinfive.substack.com/?source=post_page-----99bbe482a45f--------------------------------) [## AI in Five | Matt Chapman | Substack

### 最新的新闻、职业故事和来自数据科学与 AI 领域的编码技巧，概括为 5 个要点…

aiinfive.substack.com](https://aiinfive.substack.com/?source=post_page-----99bbe482a45f--------------------------------)
