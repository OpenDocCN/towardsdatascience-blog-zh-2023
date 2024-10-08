# 使用 Black 和 GitHub Actions 维护干净的 Python 代码

> 原文：[`towardsdatascience.com/black-with-git-hub-actions-4ffc5c61b5fe`](https://towardsdatascience.com/black-with-git-hub-actions-4ffc5c61b5fe)

## 没有人希望代码库凌乱不堪；也很少有人有耐心去清理它

[](https://thuwarakesh.medium.com/?source=post_page-----4ffc5c61b5fe--------------------------------)![Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----4ffc5c61b5fe--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4ffc5c61b5fe--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4ffc5c61b5fe--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----4ffc5c61b5fe--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4ffc5c61b5fe--------------------------------) ·阅读时间 8 分钟·2023 年 1 月 16 日

--

![](img/5b6f032c9c15cbfb6fd4451172f5f685.png)

就像这个清洁机器人一样，我们可以构建一个自动化系统，使用 Black 和 GitHub Actions 来清理我们的 Python 代码库。——照片来自 [Onur Binay](https://unsplash.com/@onurbinay?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

编写代码已经足够困难，但确保它的格式良好并且易于阅读则可能更具挑战性。

我的编码技能在十年中有了很大的提升。但大多数并不是一些花哨的 API 使用或其他内容，而是代码的格式化方式。

在我最早的几年里，我编写代码是为了结果。当然，这也是每个程序员的终极目标。但随着我的进步，我意识到良好的编码远不只是完成任务那么简单。

与他人分享我的代码并避免一大堆问题并不容易。其他程序员发现理解我的代码非常具有挑战性，即使有支持文档和内联注释。

# 程序员的工作并不会因为代码运行正常而结束。

除了文档，还有一些因素使得优秀的代码变得更加出色。而编程社区也没有辜负这个发现。

这就是我们格式化代码的方式。

Python，我最喜欢的编程语言，具有最简单的语法。但这并不意味着每个人都能理解你的 Python 代码。

因此，我们有一个称为 PEP 8 的风格指南。这个标准使每个程序员都采用相同的编码风格。如果做到正确，新程序员可以减少花费在搞清楚每一行是什么的时间。

这是一个示例代码。第一个是我早期职业生涯中的编写方式。它完成了工作。它读取一个文件并训练一个模型。

![](img/2c014c2f9a9ce335aeaf0e5bd84b8665.png)

未格式化的 Python 代码——作者提供的图片。

在上面的图像中，你可以看到只有一半的代码适合屏幕。你必须左右滚动才能阅读和理解代码。到处都是不必要的空行和空白，而在需要空行的地方却没有。

实施风格指南后的样子就是这样。这看起来更容易理解，不是吗？

![](img/fd588f3d289822f81eb58f43f32c3f3d.png)

格式化的 Python 代码 — 作者提供的图像。

但有一个问题。记住风格指南并强迫自己以这种方式编码需要很多工作。我还是希望花更多的时间来完成工作。

如果其他人处理代码格式化，那将会很有帮助。这就是[Black](https://black.readthedocs.io/)的用武之地。Black 是一个 Python 包，可以用一个命令将代码格式化为预定义的风格指南。这个指南可以是 PEP 8，或者你可以调整为你们组织的版本。

现代代码编辑器通常支持文档格式化。例如，在 VSCode 中，你可以右键点击编辑器并点击代码格式化。如果安装了 Black，它将立即格式化你的代码。

所有这些改进并没有止步于此。你可以通过预提交钩子自动格式化代码，而不必在编码时担心。当你提交更改时，它将格式化所有的 Python 文件。我在之前的帖子中讨论过这个问题。

[](/python-code-formatting-made-simple-with-git-pre-commit-hooks-9233268cdf64?source=post_page-----4ffc5c61b5fe--------------------------------) ## 使用 Git 预提交钩子简化 Python 代码格式化

### 编写每个人都会喜欢的代码。

[towardsdatascience.com

最后一个缺失的拼图就是这个。即使预提交钩子在开发者的电脑上运行，当作为团队工作时，你仍然依赖于它们进行格式化。

如果你可以在中心化处理，无论开发者是谁以及他们是如何做的，那会怎么样？

这就是这篇文章的重点。我们利用 [GitHub Actions](https://docs.github.com/en/actions) 来做到这一点。

当开发者提交到中央仓库时，他们的代码将自动符合组织标准。每当我们推送更改到仓库时，GitHub Actions 将触发一个工作流。我们可以配置它以运行 Black 格式化命令。

# 在 GitHub Actions 上设置 Black 以自动格式化 Python 代码

要在提交更改时使用 `black` 格式化你的 Python 代码，可以按照以下步骤操作：

首先，确保你的项目中安装了 `black`。你可以通过将 `black` 添加到项目的 `requirements.txt` 文件中，或在项目的虚拟环境中运行 `pip install black` 来完成这一步。

接下来，在你的项目的 `.github/workflows` 目录中创建一个名为 `format.yml` 的新文件。这个文件将包含你 GitHub Actions 工作流的配置。

在 `format.yml` 文件中，你可以定义一个工作流，每当你将更改提交到你的仓库时，它就会在你的代码上运行 `black`。以下是一个实现这一点的工作流示例：

```py
name: Format code

on:
  push:
    branches: [ master ]

jobs:
  format:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Format code with black
        run: |
          pip install black
          black .
      - name: Commit changes
        uses: EndBug/add-and-commit@v4
        with:
          author_name: ${{ github.actor }}
          author_email: ${{ github.actor }}@users.noreply.github.com
          message: "Format code with black"
          add: "."
          branch: ${{ github.ref }}
```

这个工作流将在你向仓库的 `master` 分支推送更改时运行。

它会安装 `black`，然后在整个项目上运行 `black` 命令（`black` 命令末尾的 `.` 指定了当前目录）。最后，它会将新更改提交到相同的分支。

一旦你定义了工作流，你可以将更改提交到你的仓库并推送到 GitHub。当你这样做时，工作流会自动运行，用 `black` 格式化你的代码。

[](/python-project-structure-best-practices-d9d0b174ad5d?source=post_page-----4ffc5c61b5fe--------------------------------) ## 7 种方法使你的 Python 项目结构更优雅

### 这里是可管理、可扩展和易于理解的 Python 项目结构的最佳实践。

towardsdatascience.com

# 排除和包含用于格式化的文件和文件夹

上述配置存在一个问题。它会遍历我们仓库中的所有文件和文件夹，并尝试对它们进行格式化。有时，我们希望避免对特定文件进行格式化。

`black` 有选项来配置其格式化行为。

在以下示例中，我们已经配置了 `black` 以忽略 `ref` 文件夹中的任何文件。

```py
- name: Format code with black
  run: |
    pip install black
    black . --exclude="env/*"
```

这将对当前目录下所有文件运行 `black` 命令，但不包括 `env` 文件夹中的文件。

默认情况下，`black` 将格式化所有 `.py`、`.pyi` 和 `.ipynb` 文件。或者，你可以使用 `--include` 标志直接指定要包含的文件列表，如下所示：

```py
- name: Format code with black
  run: |
    pip install black
    black --include="\.py" .
```

这将仅在当前目录下具有 `.py` 扩展名的文件上运行 `black` 命令。

是的，你可以同时使用 `--include` 和 `--exclude` 标志来指定更复杂的文件包含或排除模式。

```py
- name: Format code with black
  run: |
    pip install black
    black --include="\.py" --exclude="env/*" .
```

这将对当前目录下所有具有 `.py` 扩展名的文件运行 `black` 命令，但不包括 `env` 文件夹中的文件。

> 请注意，`--include` 标志优先于 `--exclude` 标志，因此如果一个文件同时匹配这两个模式，它将被包含在内。因此，在包含模式时要格外小心。像上述示例那样盲目地包含模式，会导致 `black` 也格式化你 `env` 文件夹中的文件。

你可以通过用逗号分隔来指定多个 `--include` 和 `--exclude` 模式，如下所示：

```py
- name: Format code with black
  run: |
    pip install black
    black --include="\.py,\.pyi" --exclude="env/*,tests/*" .
```

这将对当前目录下所有具有 `.py` 或 `.pyi` 扩展名的文件运行 `black` 命令，但不包括 `env` 或 `tests` 文件夹中的文件。

管理要排除的文件的另一个有用方法是你的.gitignore 文件。你的项目中可能已经有一个。如果文件匹配.gitignore 中的任何模式，这些文件将在格式化时被忽略。

如果你需要同时使用.gitignore 和排除行为，根据官方文档，你需要使用`--extend-exclude`而不是`--exclude`。

[](/github-automated-testing-python-fdfe5aec9446?source=post_page-----4ffc5c61b5fe--------------------------------) ## 如何在每次提交时使用 GitHub Actions 运行 Python 测试？

### 自动化无聊的工作，并通过 CI 管道确保代码质量。

[towardsdatascience.com

# 扩展工作流以包括更多清洁代码工具

Black 是格式化 Python 代码的重要工具之一。它处理了很多事情。但我们还有其他重要的工具。

清洁编码的一个方面是逻辑排序导入。但再次强调，你不必担心这个。你可以使用 isort 包与 GitHub 工作流来自动处理这个问题。

移除冗余或未使用的变量是一个优秀代码库的另一个特质。现在许多 IDE 会自动突出显示这些新变量。但 autoflake8 也可以自动移除它们。

这是完成的 GitHub Actions 配置：

```py
name: Format code

on:
  push:
    branches: [ master ]

jobs:
  format:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Format code with black
        run: |
          pip install black
          black .
      - name: Sort imports with isort
        run: |
          pip install isort
          isort .
      - name: Remove unused imports with autoflake
        run: |
          pip install autoflake
          autoflake --in-place --remove-all-unused-imports --remove-unused-variables --recursive .
      - name: Commit changes
        uses: EndBug/add-and-commit@v4
        with:
          author_name: ${{ github.actor }}
          author_email: ${{ github.actor }}@users.noreply.github.com
          message: "Format code with black"
          add: "."
          branch: ${{ github.ref }}
```

# 当事情没有按照我们希望的方式进行时

尽管我们付出了所有努力，但这些自动化代码重构有时仍可能失败。

在一个好的开发环境中，这应该避免。大多数开发者在将代码推送到 Git 之前会在本地测试代码，因为这样可以在部署之前识别代码中的任何错误或问题。

本地测试也允许开发者进行必要的更改，确保代码在推送上线之前正常运行，从而节省时间和资源。

因此，诸如拼写错误和其他小问题等问题，通常是代码重构失败的原因，将不再是问题。但当它们发生时，我们需要做好准备。

以下配置将在我们的代码重构失败时发送电子邮件通知。

```py
- name: Notify errors
  if: failure()
  uses: dawidd6/action-send-mail@v2
  with:
    server_address: smtp.gmail.com
    server_port: 465
    username: ${{ secrets.EMAIL_USERNAME }}
    password: ${{ secrets.EMAIL_PASSWORD }}
    subject: "Format code with black"
    body: "Format code with black failed"
    to: ${{ secrets.EMAIL_TO }}
    from: ${{ secrets.EMAIL_FROM }}
    content_type: text/plain
```

上述配置扩展使用了由[Dawid Dziurla](https://github.com/dawidd6)创建的[自定义 GitHub 动作](https://github.com/dawidd6/action-send-mail)。

# 结论

没有人希望代码混乱，但只有少数人有耐心去清理它。

每个编程社区都有一个风格指南，以确保代码可读。但这只是解决问题的第一个障碍。我们需要使代码格式化对每个开发者而言都是毫不费力的。像 Black 这样的包可以做到这一点。

使其毫不费力并不意味着每个人都会使用它。预提交钩子可以自动化过程，但仍然依赖于开发者。本文展示了一种集中自动格式化的方法。

使用 GitHub Actions 的 Black 可以确保代码始终符合格式。你可以集中管理风格指南，可以是 PEP 8 或你自定义的风格。而且当格式化失败时，你也会收到通知。

希望这对你有帮助。

> 谢谢阅读，朋友！在 [**LinkedIn**](https://www.linkedin.com/in/thuwarakesh/)、[**Twitter**](https://twitter.com/Thuwarakesh) 和 [**Medium**](https://thuwarakesh.medium.com/) 上跟我打招呼吧。
> 
> 还不是 Medium 会员？请使用这个链接 [**成为会员**](https://thuwarakesh.medium.com/membership)，因为在没有额外费用的情况下，我会因为推荐你而获得少量佣金。
