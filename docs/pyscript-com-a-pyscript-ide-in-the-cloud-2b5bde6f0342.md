# PyScript.com：云中的 PyScript IDE

> 原文：[`towardsdatascience.com/pyscript-com-a-pyscript-ide-in-the-cloud-2b5bde6f0342`](https://towardsdatascience.com/pyscript-com-a-pyscript-ide-in-the-cloud-2b5bde6f0342)

## PyScript.com 是 Anaconda 推出的一个新在线 IDE，允许你创建、运行和托管 PyScript 应用。

[](https://medium.com/@alan-jones?source=post_page-----2b5bde6f0342--------------------------------)![Alan Jones](https://medium.com/@alan-jones?source=post_page-----2b5bde6f0342--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2b5bde6f0342--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2b5bde6f0342--------------------------------) [Alan Jones](https://medium.com/@alan-jones?source=post_page-----2b5bde6f0342--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2b5bde6f0342--------------------------------) ·12 分钟阅读·2023 年 4 月 13 日

--

![](img/07925010c17760f2045486aee7a5dd6b.png)

哇！他一定是个非常严肃和重要的程序员，如果他需要这么大的屏幕 —— 我很好奇为什么这些屏幕大多是空白的。照片由 [Max Duzij](https://unsplash.com/@max_duz?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

*注意：2023 年末发布了全新重写的 PyScript 版本，这可能使得此处描述的一些语法过时 —— 请参见* [PyScript 正在成长](https://medium.com/codefile/pyscript-is-growing-up-9df5c51fdf51) *以获取更新。*

好消息！目前还不清楚现有的 IDE 或编辑器哪个适合构建 PyScript 应用，但现在有了 PyScript.com，我们有了一个专用的在线 IDE。

它到底有多好？我们将会揭晓。

我们将来看看 Anaconda 的新 PyScript 在线 IDE：我将介绍这个新平台，我们会看看如何开始使用它编写 PyScript 应用，最后我们将完成一个完全功能且已部署的 PyScript 应用。

## PyScript.com

Anaconda Inc. 对其新产品的看法毫不隐晦。

> *“这个革命性的平台使得 99% 的人能够进行编程，推动了 Anaconda 的使命，旨在使数据科学和 Python 开发民主化。” —— Anaconda Inc.*

你无疑知道 Anaconda 是一家基于他们自己 Python 发行版的数据科学平台供应商。你可能还知道他们是 PyScript 的发明者（如果你一直关注我在 Medium 上的文章，你肯定知道 —— 见 [你好 PyScript](https://medium.com/towards-data-science/hello-pyscript-goodbye-javascript-c8d8fb83a93a)，[2023 年 PyScript 有何新变化](https://medium.com/codefile/whats-new-in-pyscript-dfdf25538281) 和 [其他](http://alajones2.github.io)）。

Anaconda 通过[PyScript.net](http://pyscript.net)网站发布了 PyScript；这是一个开源项目，致力于将 Python 应用程序创建为网页，并托管在 GitHub 上。

PyScript 基于 Pyodide，这是一个被移植到 WebAssembly 的 Python 解释器。WebAssembly 是一种将在浏览器中运行的低级语言，这意味着你现在可以在浏览器中本地运行 Python 程序。

当你考虑到这一点时，这确实是件大事。

使用 PyScript，你可以编写与 Javascript 和 DOM 通信的 Python 应用程序，从而创建无需服务器的以 Python 为中心的 Web 应用程序——上传到 Web 主机，它们就能正常工作！

## PyScript.net 和 PyScript.com

PyScript.com 不应与 PyScript.net 混淆。新网站不是开源产品的一部分，而是一个新的在线编程环境。根据 Anaconda 的说法：

> “一个自由且灵活的编码平台，世界上任何人都可以使用 Python 驱动的数据交互和计算创建下一代 Web 应用程序”

他们还继续说

> “该平台现在作为软件服务免费提供。”

然而，这不会永远完全免费。通过创始人订阅的优惠，你可以用$150 获得一年的即将推出的付费功能的免费访问权（我目前不清楚这些功能是什么，但可以推测 Anaconda 认为它们值得付费。我猜测——希望——当前功能会保持免费）。

那么，究竟怎么回事？我们从 PyScript.com 中能得到什么？

## 入门

首先，你需要一个账户。没问题，前往[网站](http://pyscript.com)，注册并登录。

你将看到仪表板，那里会有一个新的项目等待你。屏幕看起来大致是这样的：

![](img/781c5425683a11162adb4fba668c9f87.png)

PyScript.com 仪表板——作者的截图

“Weathered Moon”是为我创建的默认项目的可爱名称——你的项目会有不同的名字。（如果你觉得这个名字不够可爱——或者太可爱——你可以稍后更改名字。）

正如你所见，有选项可以*查看*或*编辑*网站，而点状菜单提供了额外的选项，如*删除*或*复制*项目。

点击“编辑”，项目将会打开。界面会有三个面板：左侧是文件管理器和编辑器，右侧是显示正在运行的项目的面板。（如果你使用的是手机或小浏览器窗口，配置可能会有所不同。）

默认项目包含三个文件，

+   index.html：定义了 HTML 中的网页

+   main.py：包含 Python 代码

+   pyscript.toml：这将是空的——稍后我们将看到它如何使用。

这是打开项目的截图——你不会对它的功能感到惊讶！

![](img/c53e32daca978a83c3401550244dd8ca.png)

作者的默认项目截图

这个 Python 程序简单地打印了“Hello World！”，这也是应用程序所做的（嗯，你期待什么呢！）。但这个应用程序还有一点更多的内容。

正如我所说的，`pyscript.toml` 是空的，但让我们看看 HTML。点击 FILES 下的 `index.html`，它将（当然）在编辑器中弹出。这是你将看到的内容：

```py
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Weathered Moon</title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <link rel="stylesheet" href="https://pyscript.net/releases/2023.03.1/pyscript.css" />
    <script defer src="https://pyscript.net/releases/2023.03.1/pyscript.js"></script>
</head>
<body>
    <py-config src="./pyscript.toml"></py-config>
    <py-script src="./main.py"></py-script>
</body>
</html>
```

这个 HTML 文件可以作为许多 PyScript 应用的基础。它的格式是 HTML，但有一些 PyScript 特有的部分。前两个部分是 `<link>` 和 `<script>` 标签，它们加载 `pyscript.css` 和 `pyscript.js`。这些是任何应用程序所必需的，它们加载使 Python 在浏览器中运行的组件。

接下来的 PyScript 特有部分在主体中；`<py-config src="./pyscript.toml">` 加载配置。这可以直接包含在标签中，但将其隐藏在 `pyscript.toml` 文件中更为整洁。正如我之前所说，这个文件目前是空的，但我们很快会用到它。

然后，我们在 `<py-script src="./main.py">` 标签中包含了来自 `main.py` 的 Python 代码。

一切都很漂亮、整洁且直接。

让我们回到 `main.py`。

```py
print("Hello, World!")
```

然后进行更改。

```py
print("Hello, Moon!")
```

现在点击“运行”按钮，你将看到右侧窗格中的新输出。

![](img/113ee78268e1775d17e40d80bc45cb1a.png)

作者截图

这里到底发生了什么？在 Python 中，`print` 语句将内容写入标准输出设备（通常是屏幕），在 PyScript 中，它将内容写入名为 `<py-terminal>` 的标签中，除非在 HTML 文件中包含这个标签，否则这个标签会在第一次使用 `print` 时自动创建。因此，当执行 `print("Hello Moon!")` 时，网页中会创建 `<py-terminal>` 标签，并将 `print` 语句的输出写入其中。

坦白说，当我们在构建应用程序时，这并不是特别有用。它适合用于调试目的，但在构建应用程序时，我们生成的输出应该成为网页的一部分——例如在 `<div>` 中——要输出文本到任意 HTML 标签中，我们不会使用 `print`，而是使用 `display`。不过稍后会详细说明。

不过，首先，我们可以通过从右侧菜单中选择“查看站点”来查看浏览器中的应用程序。

![](img/588fd7b0b0c22a68218814327cfaadcf.png)

作者截图

我这里不打算提供截图，它与预览窗格完全一样。值得稍微惊讶的是，应用程序会在浏览器的新标签页中出现，并且从网站实时提供。它有一个独特的公共 URL，你可以与任何人分享。为了证明这一点，网址如下：

[`26efd18d-1c15-4b46-b574-58731b341c76.pyscriptapps.com/5b49c512-f88f-493c-9d62-f0d745a298ed/latest/`](https://26efd18d-1c15-4b46-b574-58731b341c76.pyscriptapps.com/5b49c512-f88f-493c-9d62-f0d745a298ed/latest/)

现在把它输入到你的浏览器中——开玩笑的，当然这是一个链接。

## 让我们绘制一个图表

我们将对‘Hello World’应用进行一些扩展，制作一个展示 Python 代码和 HTML 如何良好协作的应用。

首先，作为一个好的起点，使用右侧菜单复制你现有的项目。

![](img/588fd7b0b0c22a68218814327cfaadcf.png)

作者截图

然后我们会得到一个新项目

![](img/5b7de43a07860f44b2c59b95cebc1080.png)

作者截图

要更改标题，请点击编辑按钮，如图所示，然后保存。

我们不会做任何特别复杂的事情，只是创建一个包含我们在 Python 中绘制的图表的网页。因此，我们需要调整 HTML 代码以适应这个需求。

```py
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Let's Plot a Graph</title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <link rel="stylesheet" href="https://pyscript.net/releases/2023.03.1/pyscript.css" />
    <script defer src="https://pyscript.net/releases/2023.03.1/pyscript.js"></script>
</head>
<body>
    <py-config src="./pyscript.toml"></py-config>
    <py-script src="./main.py"></py-script>

    <h1>Let's Plot a Graph</h1>
    <div id="graph"></div>
</body>
</html>
```

你可以看到这是与我们开始时的 `index.html` 一样的，但做了一些更改。我更改了标题，并在正文末尾添加了两个标签，首先是一个 `<h1>` 标题，然后是一个 id 为“graph”的 `<div>` — 这就是图表将被绘制的位置。

先不要运行它（它仍然会显示‘Hello Moon’），我们需要将 Python 代码添加到 `main.py` 中。

```py
import matplotlib.pyplot as plt
import pandas as pd

df = pd.DataFrame()
df['x'] = [1,2,3,4,5,6,7,8,9]
df['y'] = [1,2,3,4,5,6,7,8,9]

fig, ax = plt.subplots()
df.plot("x", "y", ax=ax)

display(fig, target="graph")
```

在导入模块后，我们创建一个 Pandas 数据框，包含两个列用于 x 和 y 轴。它们各自包含整数 1 到 9。接着，我们创建一个 `mathplotlib` 图形（当然，它将是一个直线图），然后使用 PyScript 命令 `display` 将图形显示在 id 为“graph”的 HTML 标签中。

现在我们可以运行它了吗？不，请稍微耐心一点。

我们在 Python 代码中导入了 `pandas` 和 `mathplotlib` 库，但这些库并不包含在 PyScript 包中。因此，我们需要在 `<py-config>` 中指定这些库，并在 `<pyscript.toml>` 中添加以下内容。

```py
packages = ["pandas", "matplotlib"]
```

你可以使用一大堆 Python 包，但它们需要在配置中指定，并且需要在 Python 代码中导入。配置部分可以用于将外部文件，如数据文件，加载到应用程序中。

好，现在你可以运行它了。

![](img/b27d40c4560828ea721b416fff2c3bb8.png)

作者截图

你可能之前已经注意到了，但如果你在其自身网页上查看项目，你会看到底部有一个链接。

![](img/b6b5b547463e8ae6f75a59ef709e0e0b.png)

作者截图

任何能够看到链接的人也可以访问你的代码。点击‘查看代码’链接将打开一个不可编辑（但可能可复制）的项目版本。

所以这就是一个简单的 PyScript 应用，有几点值得注意：

1.  给你的默认文件 `pyscript.toml, main.py, & index.html` 是创建新项目的良好起点。所以，创建新项目的第一步可能是复制默认应用并重命名。

1.  如果你想使用某个库，你必须在配置中指定它，并在 Python 代码中导入它。

1.  PyScript 命令 `display` 用于将 Python 代码的输出写入具有特定 id 的 HTML 标签中。

1.  你的代码从 PyScript.com 服务器提供，可以从其 URL 查看。任何拥有该 URL 的人都可以运行应用程序，并查看你的代码（所以不要在里面放秘密）。

有一件事我没有提到，就是如何返回到你的项目仪表板视图。

![](img/125db9529ecf256d81ada0331e360fd2.png)

作者截图

实际上，点击 PyScript 徽标的任何位置都会带你到这个视图。

兔子标志怎么回事？如果有机会的话，Python 不会吃兔子吗？我不确定这是我们想要的 PyScript 形象。

你还可以通过右上角的菜单进入仪表板：

![](img/b5e952ec60b0cd27e74fa60fa136bd38.png)

作者截图

## 一个完整的应用

我答应你一个应用程序，它相当简单，但这里就是。它基于文章 [PyScript, Pandas 和 Plotly：一个互动网页应用](https://medium.com/@alan-jones/pyscript-pandas-and-plotly-an-interactive-web-app-a65a01b04ae6)，但为了简单起见，它使用 `matplotlib` 代替 Plotly。它还使用 Bootstrap JavaScript 库来美化 UI。

它看起来是这样的：

![](img/9fd051d210b64f066e5c856923217336.png)

作者截图

这个应用下载一些天气数据¹，并允许你从下拉菜单中选择一个图表进行显示。（如果你读过我的其他文章，你可能见过无数个使用不同技术的版本。）

我不会详细讲解应用的工作原理，因为大部分内容在上述文章中已有解释，或者在我们已经覆盖的内容中有说明。Python 代码中也有注释解释正在发生什么。

这是 HTML：

```py
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Weathered App</title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <link rel="stylesheet" href="https://pyscript.net/releases/2023.03.1/pyscript.css" />
    <script defer src="https://pyscript.net/releases/2023.03.1/pyscript.js"></script>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">
</head>
<body>
    <py-config src="./pyscript.toml"></py-config>
    <py-script src="./main.py"></py-script>

    <div class="jumbotron p-2">
        <h1>Weather Data</h1>
        <p class="lead">
          Some graphs about the weather in London in 2020
        </p>
    </div>   

    <div class="row">
        <div class="col-sm-2 m-2">
           <b>Select chart from list:</b>
        </div>
        <div class="col-sm-4 m-2">
            <select class="form-control" id="select" py-change="selectChange()">
                <option value="Tmax">Maximum Temperature</option>
                <option value="Tmin">Minimum Temperature</option>
                <option value="Sun">Sun</option> 
                <option value="Rain">Rain</option>
            </select>
        </div>

        <div class="row">
           <div class="col-sm-6 m-2"> 
               <div id="chart1"></div>
           </div>
        </div>
    </div>
</body>
</html>
```

这是 Python：

```py
# Import libraries
import pandas as pd
import matplotlib.pyplot as plt
import js

# Get the data
# Note you can't use the 'requests' package or similar
# so we import the date using a built-in pyodide function
from pyodide.http import open_url

url = 'https://raw.githubusercontent.com/alanjones2/uk-historical-weather/main/data/Heathrow.csv'
url_content = open_url(url)

# Create dataframe for the year 2020
df = pd.read_csv(url_content)
df = df[df['Year']==2020]

# Create a matplotlib chart and display it in "chart1"
# Note append is false so that old charts are overwritten
def plot(chart):
    fig, ax = plt.subplots()
    df.plot("Month", chart, ax=ax)

    display(fig, target="chart1", append=False)

# The is the call back form the dropdown menu
# it gets the value selected and calls plot
def selectChange():
    choice = js.document.getElementById("select").value
    plot(choice)

# Call plot on startup
plot('Tmax')
```

还有配置：

```py
packages = ["pandas", "matplotlib"]
```

还有一个链接到最终应用的 [这里](https://26efd18d-1c15-4b46-b574-58731b341c76.pyscriptapps.com/fde16857-a0d3-4fd3-b2a0-0920f1b4b973/latest/)，你可以在这里看到它的全部，查看代码并复制它，如果你愿意的话。

## 我们怎么认为？

`pyscript.com` 当然有优缺点。以下是一些我认为值得注意的：

**优点：**

+   这是一个简单易用的 IDE。

+   所有需要的东西都在一页上。

+   你创建的内容默认是公开的——太好了，我们可以分享！

+   默认应用是新项目的良好起点。

+   复制功能很好，你可以用它从旧项目中创建新项目，或者创建现有项目的新版本。

+   自动部署和免费托管！（不过 URL 有点麻烦）

**缺点：**

+   你创建的内容默认是公开的——哦，不，没有专有代码！但我一点也不会感到惊讶，如果付费功能在未来能满足这一点。

+   你不能下载一个项目并将其复制到不同的开发环境中。

+   启动有点慢，但这主要是 PyScript 的问题——也许 IDE 会稍微增加启动时间，我不太确定。

我认为第一个优点是最重要的。它非常易于使用，编辑器与 HTML 和 Python 的兼容性很好，并且你对代码所做的任何更改几乎可以即时看到结果。自动部署和托管也很棒。

总的来说，这是一个受欢迎的包。显然，它并不针对商业项目，但却是分享想法和尝试 PyScript 的绝佳场所。

尝试一下吧！

一如既往，感谢你的阅读，希望你觉得有用。我必须说，我很享受写这篇文章和使用 pyscript.com 的过程。你可以在我的[GitHub 网页](http://alanjones2.github.io)找到我其他的作品，包括更多关于 PyScript 的内容。

如果你不是 Medium 会员，你可以使用我的[推荐链接](https://medium.com/@alan-jones/membership)注册，每月只需 $5 即可阅读任何 Medium 内容。

## 备注

1.  天气数据来自我自己的 GitHub 仓库（见代码中的链接），并且属于公共领域。数据来源于[英国气象局历史气象站数据](https://www.metoffice.gov.uk/research/climate/maps-and-data/historic-station-data)，该数据也可以在[英国公共部门信息开放政府许可 v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/)下免费使用。
