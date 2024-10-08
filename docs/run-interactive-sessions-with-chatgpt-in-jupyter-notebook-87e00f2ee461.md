# 在 Jupyter Notebook 中与 ChatGPT 运行交互式会话

> 原文：[`towardsdatascience.com/run-interactive-sessions-with-chatgpt-in-jupyter-notebook-87e00f2ee461`](https://towardsdatascience.com/run-interactive-sessions-with-chatgpt-in-jupyter-notebook-87e00f2ee461)

## 使用 LangChain 和 IPyWidgets 在 Jupyter Notebook 中与 ChatGPT 进行关于自定义文档的对话

[](https://konstantin-rink.medium.com/?source=post_page-----87e00f2ee461--------------------------------)![Konstantin Rink](https://konstantin-rink.medium.com/?source=post_page-----87e00f2ee461--------------------------------)[](https://towardsdatascience.com/?source=post_page-----87e00f2ee461--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----87e00f2ee461--------------------------------) [Konstantin Rink](https://konstantin-rink.medium.com/?source=post_page-----87e00f2ee461--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----87e00f2ee461--------------------------------) ·阅读时间 6 分钟·2023 年 5 月 4 日

--

![](img/3e2944ef6748fadb370f0238766a8a0f.png)

原始照片由 [Charles Etoroma](https://unsplash.com/@charlesetoroma?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/de/fotos/ddPTOSMa-MI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。

2023 年 3 月，OpenAI 发布了其 API，供开发者访问 ChatGPT 和 Whisper 模型。从那时起，开发者可以通过 API 将这些服务和模型集成到他们的应用程序和产品中。许多精彩的教程随后发布了如何使用其 API 结合 Streamlit 或 Streamlit Chat 创建自己的 ChatGPT 聊天 web 应用程序。

本文提出了一种 **更轻量级的方法**。无需运行或托管 Streamlit 服务器或使用 Docker 容器，**所有工作都在 Jupyter Notebook 中完成**。

在本文中，你将学习 **如何使用 OpenAI 的大型语言模型（LLM）ChatGPT 在 Jupyter Notebook 中运行关于自定义文档的交互式会话**，方法是使用 **LangChain** 和 **IPyWidgets**。

最终结果将如下所示：

![](img/43e468f126b37e2c69924176c1f375f1.png)

图 1\. 最终结果的演示（图片来自作者）。

以下章节将分别解释代码的每一部分。

💾 [这里](https://github.com/darinkist/MediumArticle_InteractiveChatGPTSessionsInJupyterNotebook) 你可以找到完整的代码 Notebook。

# 先决条件

在我们开始与 ChatGPT 的对话之前，需要先完成一些准备工作。

## OpenAI API 密钥 🔑

由于我们想要使用 ChatGPT，首先需要一个有效的 OpenAI API 密钥。所需的密钥可以在 [此链接](https://platform.openai.com/account/api-keys) 中创建，然后点击

`+ 创建新的密钥` 按钮。

> OpenAI 提供了一个免费的试用期，之后才收费。在我看来，价格非常公平，考虑到在许多情况下，托管你自己的 LLM 更为昂贵。

## 安装 OpenAI 包 📦

一旦我们拥有密钥，我们还需要通过运行以下命令来安装官方 OpenAI 包：

```py
pip install openai
```

## 安装 LangChain 包 🦜️🔗

LangChain 是一个相对较新的框架，建立在 ChatGPT 等 LLMs 之上。它的目标是将不同的组件链式连接在一起，以创建更高级的使用案例，例如特定文档上的问答或聊天机器人。

要安装它，请运行以下命令：

```py
pip install langchain
```

## 安装或更新 Jupyter Widgets 🪐

如果你使用 Jupyter Notebook 或 Jupyter Lab，ipywidgets 应该已经安装。然而，可能你正在使用旧版本的包。本文使用的是（最新）版本 `8.0.5`。

要安装或更新 ipywidgets，请运行下面的命令：

```py
pip install -U ipywidgets
```

安装成功后，重启你的 Jupyter Notebook/Lab。

> **题外话**：…我知道… 木星没有光环 — 这是土星 🪐 :P

## 数据 📑

正如上面承诺的，我们不仅会创建一个与 ChatGPT 的互动会话，还会将自己的文档发送给 ChatGPT，然后询问有关这些文档的问题。我们用作示例的文档是关于 [硅谷银行倒闭](https://en.wikipedia.org/wiki/Collapse_of_Silicon_Valley_Bank) 的 Wikipedia 文章（Wikipedia 贡献者，[CC BY-SA 3.0](https://en.wikipedia.org/wiki/Wikipedia:Text_of_the_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License)）

> **注意**：我使用 [wikipedia package](https://pypi.org/project/wikipedia/) 下载了提到的文章作为文本文件。当然，你也可以使用任何你喜欢的文本文件或 DocumentLoaders。

## 项目结构

我们的项目有以下文件夹结构

```py
documents/
|- Collapse_of_Silicon_Valley_Bank.txt
images/
|- bear_avatar.png
|- cat_avatar.png
|- loading.gif
InteractiveSession.ipynb
```

# 将 ChatGPT 与 LangChain 集成

现在我们已经拥有了所需的所有包和工具，我们可以着手将 ChatGPT 与 LangChain 结合使用。正如上面提到的，LangChain 具有许多有用的功能来加载文档和开始与 ChatGPT 的对话会话。

下面的代码展示了我们稍后将与 Jupyter Widgets 结合的逻辑。

在 `line 10` 中，我们必须设置我们的 OpenAI API 密钥。`Lines 12-13` 从指定的 `/documents` 路径加载所有文本文件（在本例中只有一个）。

[Chroma](https://github.com/chroma-core/chroma) （`lines 15-16`）是一个内存中的嵌入数据库，包含我们以 `OpenAIEmbeddings` 形式的文本文档。

在我们初始化 ChatGPT 的 `line 18` 后，我们创建了一个 `ConversationalRetrievalChain` （`line 21`）。要开始与 ChatGPT 对话，我们需要指定一个问题和聊天记录（`lines 24–29 和 line 31`），以便它记住之前的对话，例如，当我们在后续问题中引用之前的答案时。

> **注意**：如果你在选择 Jupyter Notebook 和 Jupyter Lab 之间犹豫，请选择后者。使用 Jupyter Lab，你有更多的选项来调试代码（即，日志控制台）。

# 将其与交互设计结合起来。

根据上述逻辑，我们已经可以开始对话了。然而，这将不会是互动式的。对于每个新问题，我们都需要创建一个新单元格，包含`lines 24–29 and line 31`中的代码。

为了使我们的对话具有互动性，我们将使用 Jupyter 小部件、CSS，并可以选择使用两个头像图像和一个加载动画 gif。

+   我使用了头像图像（[熊](https://www.flaticon.com/free-icon/bear_3940403?term=animal+bear&page=1&position=21&origin=search&related_id=3940403)，[猫](https://www.flaticon.com/free-icon/cat_4322991?term=animal+cat&page=1&position=60&origin=search&related_id=4322991)）来自[Freepik 创建的动物图标 — Flaticon](https://www.flaticon.com/free-icons/animal)（*Flaticon 许可，个人和商业使用均免费，需署名*）。

+   以及来自 loading.io 的[加载动画](https://loading.io/spinner/bars/-bounce-bar-column-chart-equalizer-histogram-rectangle-block-progress-facebook)（[免费许可](https://loading.io/license/#free-license)）。

+   所有图像都位于`images/`文件夹中。

+   以下 CSS 和 HTML 来自[Bootstrap snippet. chat app](https://www.bootdey.com/snippets/view/chat-app)（[MIT 许可](https://www.bootdey.com/license)）。

下面的代码片段展示了我们需要首先导入的库。

```py
from datetime import datetime
from IPython.display import HTML, display
from ipywidgets import widgets
```

导入后，我们将以下代码添加到一个新单元格中。这是必要的，因为我们使用了`%%html`单元格魔法。

以下代码展示了如何将上述逻辑（将 ChatGPT 与 LangChain 集成）与 HTML 代码结合起来。

为了使我们的会话具有互动性，我们需要创建一个由 Jupyter Text Widget（我们的输入字段）更改触发的方法。

我们必须将`text.continuous_update`设置为`False`。否则，我们定义的方法将在每个字符输入时被触发。

最后但同样重要的是，我们定义了输入字段、输出和加载条的外观或布局。

我们在这里使用`flex_flow="column-reverse"`，以便始终将滚动条置于底部，这样我们就不必为每条新消息向下滚动。

就这些！现在我们可以开始一个互动会话了！

> 完整代码可以在[这里](https://github.com/darinkist/MediumArticle_InteractiveChatGPTSessionsInJupyterNotebook)找到。

# 演示会话

如上所述，我为这个演示使用了一篇关于硅谷银行倒闭的文章。由于此事件发生在 ChatGPT 最近的更新或刷新之后，它无法了解这次倒闭。

让我们通过使用官方控制台（图 2）来找出答案：

![](img/1d08f289be2476ab9c7172aef8dec88b.png)

图 2\. ChatGPT 的（[ChatGPT Mar 23 版本](https://help.openai.com/en/articles/6825453-chatgpt-release-notes)）回答（图片由作者提供）。

我们可以看到当前版本（3 月 23 日）尚未了解倒闭情况。让我们开始我们的互动环节：

![](img/faf1cc78574e8db19129b5f05bcfd2e3.png)

图 3\. 互动输出（作者提供的图片）。

BÄM！通过我们提供的信息，ChatGPT 能够提供关于倒闭的更详细回答。

# 结论

如果你在寻找一个轻量级的替代方案来创建你自己的 ChatGPT 聊天网页应用，那么这个方法可能是一个相当不错的选择。所有需要的工作都可以在 Jupyter Notebook 中通过 LangChain、IPyWidgets 和 HTML/CSS 完成。由于 LangChain 相对较新，我预计不久会有许多更新和可能的代码更改。

# 来源

维基百科贡献者。（2023 年 5 月 1 日）。硅谷银行倒闭。在*维基百科，自由百科全书*中。检索于 2023 年 5 月 1 日 18:56，来自[`en.wikipedia.org/w/index.php?title=Collapse_of_Silicon_Valley_Bank&oldid=1152681730`](https://en.wikipedia.org/w/index.php?title=Collapse_of_Silicon_Valley_Bank&oldid=1152681730)
