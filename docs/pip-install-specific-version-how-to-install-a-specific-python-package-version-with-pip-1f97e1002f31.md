# Pip 安装特定版本 — 如何使用 Pip 安装特定 Python 包版本

> 原文：[`towardsdatascience.com/pip-install-specific-version-how-to-install-a-specific-python-package-version-with-pip-1f97e1002f31`](https://towardsdatascience.com/pip-install-specific-version-how-to-install-a-specific-python-package-version-with-pip-1f97e1002f31)

## 想要使用 Pip 安装特定的 Python 包版本？本文将通过实际示例和指南展示如何操作。

[](https://medium.com/@radecicdario?source=post_page-----1f97e1002f31--------------------------------)![Dario Radečić](https://medium.com/@radecicdario?source=post_page-----1f97e1002f31--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1f97e1002f31--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1f97e1002f31--------------------------------) [Dario Radečić](https://medium.com/@radecicdario?source=post_page-----1f97e1002f31--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1f97e1002f31--------------------------------) ·阅读时间 6 分钟·2023 年 4 月 5 日

--

![](img/0184279087e9fb03099c4581fbd2fa99.png)

图片由[Mario Gogh](https://unsplash.com/@mariogogh?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**简而言之：** 你可以通过运行`pip install <package_name>==<version>`命令来安装 Python 包的特定版本。例如，要安装[Pandas](https://practicalpandas.com/how-to-install-pandas-specific-version/)的版本 1.3.4，请从终端执行`pip install pandas==1.3.4`命令。

这只是一个简短的版本，可能是你所需要的所有信息，但有时你可能需要更好地控制包的安装。这就是本文其余部分的作用。

# 为什么要安装特定（较旧的）版本的 Python 包

那么，为什么要在意旧版本的 Python 包呢？也许你有一个庞大的代码库，它与最新的包更新不兼容。也许你几年前写的代码仍在生产中工作，但更新包可能会破坏它。或者即使是最新的包版本也不兼容你的 Python 版本。

**换句话说**——无论情况如何，安装旧版本 Python 包是有正当理由的。

现在让我们探讨一些使用 Pip 安装特定包版本的实际示例。

# 如何检查已安装的 Python 包的版本

在继续安装特定的 Python 包版本之前，你必须了解两个有用的命令。这些是：

+   `pip show <packagename>` - 显示当前安装的包版本、摘要、作者、许可证、依赖关系等。

+   `pip index versions <packagename>` - 列出你可以安装的所有可用包版本。

让我们检查一下 `pandas` Python 包。以下 shell 命令打印出当前安装的版本：

```py
pip show pandas
```

![](img/1a7c32d5b55eda55ee8bab8796f5fecd.png)

*图像 1 — Pandas 包的当前版本（图像作者提供）*

看起来 Pandas 版本 1.5.3 已安装，并且需要 `numpy`、`python-dateutil` 和 `pytz` 包才能运行。

**但是版本 1.5.3 是最新的吗？我们还可以安装其他哪些版本？** 这是下一个 shell 命令的答案：

```py
pip index versions pandas
```

![](img/7ef6ea6c4c946f6d65332ad27dd59357.png)

*图像 2 — 可用的 Pandas 包版本（图像作者提供）*

版本 2.0.0 是最新的，但根据你在系统或虚拟环境中安装的 Python 版本，你可以回退到很久以前的版本。

你现在知道如何检查当前安装的包版本，并列出所有可用版本，接下来，让我们看看如何使用 pip 安装特定版本的包。

# 如何安装特定版本的 Python 包

安装特定版本的 Python 包有 2 种必知的方法。第一种方法需要两个命令——第一个用于卸载当前版本，第二个用于安装你想要的版本。第二种方法将所有这些功能打包在一个 shell 命令中，因此你在实际操作中应该使用第二种方法。

## 方法 1 — 卸载并安装

现在你知道了你安装了哪个版本的 Pandas，以及根据你的 Python 版本你可以潜在安装哪些版本。

在我们的案例中，我们有版本 1.5.3，但我们想回退到版本 1.3.4。为此，我们首先必须卸载当前版本：

```py
pip uninstall pandas
```

![](img/34caf32ec92fc269f99d08ffc59700d1.png)

*图像 3 — 使用 pip 卸载 Python 包（图像作者提供）*

然后，运行以下命令使用 pip 安装 Pandas 的特定版本：

```py
pip install pandas==1.3.4
```

![](img/3a3b67850b7a2fa1764263102d384964.png)

*图像 4 — 使用 Pip 安装特定版本的 Python 包（图像作者提供）*

看起来一切顺利，但你如何确认版本 1.3.4 已经安装了呢？**剧透：你已经知道命令了**：

```py
pip show pandas
```

![](img/3a4a0375bd700d927b5480cc24a52a17.png)

*图像 5 — 当前安装的 Pandas 版本（图像作者提供）*

Pandas 1.3.4 已成功安装在我们的系统中，所以我们可以认为任务完成了。

## 方法 2 — 覆盖已安装的包

比前一节提到的方法更优的是通过一个简单的 shell 命令来安装特定版本的 Python 包。你可以通过在安装特定版本包时附加 `--ignore-installed` 标志来实现。

这是一个示例——它安装 Pandas 版本 2.0.0，并覆盖了刚刚安装的版本 1.3.4：

```py
pip install pandas==2.0.0 --ignore-installed
```

![](img/471a795ad92096735df8afa4511f09b5.png)

*图像 6 — 覆盖当前包版本（图像作者提供）*

你可以使用`pip show`命令来进行验证：

```py
pip show pandas
```

![](img/08ac37df2a38fdfb7ef70fa14e84c0d1.png)

*图 7 — 当前安装的 Pandas 版本（图片来源：作者）*

这就是两种 pip 安装 Pandas 包特定版本的方法。**这会一直这么简单吗？** 可能不会，所以继续阅读以获取更适合生产环境的用例。

# 如何安装多个具有特定版本的 Python 包

一次一个地安装 Python 包在本地环境中没问题，但在生产环境中不具备扩展性。通常在生产环境中，你会有一个 `requirements.txt` 文件，其中包含项目所有的 Python 依赖项及其特定版本。

> *等一下，什么是 requirements.txt 文件？* [*如果你想像正常人一样管理 Python 依赖项，请阅读这篇文章*](https://betterdatascience.com/python-pipreqs/)*

`requirements.txt` 文件通常结构如下：

```py
package_1==version_1
package_2==version_2
...
```

总之，有一个包含各自版本的 Python 依赖项列表。现在的问题是，**你能用一个命令安装所有这些依赖项吗？** 答案是——可以。

这是我们端上的文件内容：

![](img/b9704d91d9be3ea4251c545511400435.png)

*图 8 — requirements.txt 文件的内容（图片来源：作者）*

如果 `requirements.txt` 文件与您的 shell 在同一目录中，则应运行以下命令：

```py
pip install -r requirements.txt
```

![](img/be7aa07cf3223e0bb7d1304806b5a3c6.png)

*图 9 — 使用一个命令安装多个 Python 依赖项（图片来源：作者）*

如果你的 `requirements.txt` 文件位于其他位置，只需在 `-r` 后提供绝对路径或相对路径。就是这样！

现在，让我们验证是否安装了正确的包版本：

![](img/bff29664d2807c2806bf777dbb35239c.png)

*图 10 — 检查 Python 包版本（图片来源：作者）*

这就是如何一次性安装多个 Python 包。接下来让我们做个简短的回顾。

# 总结 Pip 安装特定版本

项目依赖管理不必复杂。你可以使用 Python 的 [pipreqs](https://betterdatascience.com/python-pipreqs/) 模块来跟踪项目中使用的包并创建 `requirements.txt` 文件，然后应用本文中学到的内容，将相同的依赖项转移到另一台机器或生产环境中。

你也可以一个一个地 pip 安装 Python 包的特定版本，这对于本地开发环境来说没问题，本文展示了几种方法来实现这一点。更好的方法是为依赖项创建一个专门的文件，并一次性安装所有包。

*你如何跟踪项目依赖项？你会安装 Python 包的特定版本，还是坚持使用最新版本？* 请在下面的评论区告诉我。

*喜欢这篇文章吗？成为* *Medium 会员* *以继续无限制地学习。如果你使用以下链接，我将获得你会员费的一部分，而不会额外增加你的费用。*

[](https://medium.com/@radecicdario/membership?source=post_page-----1f97e1002f31--------------------------------) [## 使用我的推荐链接加入 Medium - Dario Radečić

### 阅读 Dario Radečić的每一个故事（以及 Medium 上的其他成千上万位作者的故事）。你的会员费直接支持…

medium.com](https://medium.com/@radecicdario/membership?source=post_page-----1f97e1002f31--------------------------------)

*最初发表于* [*https://betterdatascience.com*](https://betterdatascience.com/pip-install-specific-version/) *于 2023 年 4 月 5 日。*
