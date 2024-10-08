# Flapjax: 使用 Plotly 和 Flask 进行网络数据可视化

> 原文：[`towardsdatascience.com/flapjax-data-visualization-on-the-web-with-plotly-and-flask-465090fa3fba`](https://towardsdatascience.com/flapjax-data-visualization-on-the-web-with-plotly-and-flask-465090fa3fba)

## 使用 Plotly 和 Flask 构建一个数据可视化网页，并用一些 UI 组件使其互动

[](https://medium.com/@alan-jones?source=post_page-----465090fa3fba--------------------------------)![Alan Jones](https://medium.com/@alan-jones?source=post_page-----465090fa3fba--------------------------------)[](https://towardsdatascience.com/?source=post_page-----465090fa3fba--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----465090fa3fba--------------------------------) [Alan Jones](https://medium.com/@alan-jones?source=post_page-----465090fa3fba--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----465090fa3fba--------------------------------) ·17 分钟阅读·2023 年 11 月 17 日

--

![](img/d6e1e3b1af74219738061df64cef799c.png)

图片由 [Mae Mu](https://unsplash.com/@picoftasty?utm_source=medium&utm_medium=referral) 提供，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

构建数据可视化应用程序的最佳框架是什么？是 [Streamlit](https://medium.com/towards-data-science/streamlit-from-scratch-getting-started-f4baa7dd6493) 还是 Dash？或者你可以用 [Mercury](https://medium.com/towards-data-science/build-a-web-app-with-jupyter-and-mercury-9d59661441b7) 或 [Voilá](https://medium.com/towards-data-science/how-to-share-your-jupyter-notebook-with-mercury-or-voil%C3%A0-2177110d2f6e) 将 Jupyter Notebook 转换为网络应用程序？

所有这些都是创建应用程序的好方法，而且相对容易入门。但通常，容易入门的东西会随着你变得更有冒险精神而变得稍微复杂一些。因此，我将试图说服你，回到基础，使用 Python 服务器代码和 HTML 页面作为用户界面，其实并没有看起来那么令人生畏。

我们可以使用相当多的模板代码和模板构建引人入胜的交互式应用程序，这意味着你可以仍然集中精力在你的 Python 代码上，对 HTML 和 Javascript 的接触是最小的。我称这种方法为*Flapjax*——稍后我会解释原因。

创建一个简单的 Python 网络应用程序的一种最简单方法是使用 Flask，这正是我们要做的，我们将创建一个看起来像下面图片中的应用程序。

![](img/bf5e22fe0b4b24eb267590c0eb842f85.png)

一个示例互动应用程序

## Flask 框架

Flask 是一个用于开发 Web 应用的极简框架。在 Flask 应用中，网页通常是由模板和 Python 代码提供的数据构建的——这些数据可以是形成网页内容的文本或图形。结果会被发送给用户以在浏览器中显示。

下图展示了一个交互式应用的基本结构。当应用运行时，Python 部分在服务器上执行，并将数据传递给在浏览器中运行的 HTML。网页上的用户输入会传回 Python 代码，Python 代码可能会发送更多数据以更新 HTML 内容，例如用户选择的新图表。

![](img/44e68bc2cef4a95a663df7c843984269.png)

这是创建 Web 应用程序的最简单方法吗？

在我看来，将用户界面设计与程序逻辑分离确实使生活变得更轻松。但如果你习惯于在 Streamlit 或 Jupyter Notebooks 中构建应用，你可能会发现有一定的学习曲线。然而，一旦你采用了一个基本应用的模式，创建新的应用会容易得多。

因此，我们将使用 Flask 开发一个数据可视化应用，并且我们还将使用 Jinja 模板来定义我们的 HTML 页面——虽然实际出现在这些页面中的数据将由我们的 Python 代码定义。

要制作一个交互式用户界面，我们需要一些 UI 组件和一点 JavaScript，但我们会看到这基本上是可以在未来应用中重用的样板代码。

我们还将使用 Bootstrap 5 UI 组件，因为你为什么要满足于一个看起来像左侧的网页呢，而稍加努力就可以让它像右侧的那个网页一样？

![](img/604849608ee775264c998086fd66c9ee.png)

本教程分为两个部分：首先，我们创建一个静态网页，了解 Flask 和 HTML 如何协同工作；接下来，我们将处理回调，以创建一个交互式页面。

本文的所有代码将存储在我的 GitHub 仓库中。我会在文章发布后的不久提供链接。

## Bootstrap UI

使用 Bootstrap 创建吸引人的网页并不需要太多努力。包含 Bootstrap 5 文件并为 HTML 元素添加一些属性，可以轻松改善基本 HTML。

这不是一个 Bootstrap 教程，但让我快速向你展示上面两个网页中标题的基本 HTML 代码之间的区别。

**基础 HTML**

```py
<header>
   <h1>Title</h1>
   <p>subtitle</p>
</header>
```

**添加了 Bootstrap 属性**

```py
<header class="bg-primary text-white text-center py-2">
    <h1 class="display-4">Title</h1>
    <p class="lead">subtitle</p>
</header>
```

你可以看到，头部由两个元素组成，一个 `<h1>`，即顶级标题，以及一个段落 `<p>`。在 Bootstrap 版本中，这些元素有了额外的属性：头部本身具有 *primary* 背景色和白色文本，文本居中，上下边距设置为 2 像素；标题标签使用 `display-4` 字体，而段落的字体设置为 `lead` —— 这些字体在 Bootstrap 中定义，其中 `display` 字体大而粗体，而 `lead` 字体用于需要突出的普通文本。

这些特性是在 HTML `class` 属性中设置的。我们将在接下来的代码中看到更多这些特性，它们应该相当容易理解。我不会详细描述这些特性，但你可以在 [他们的网站](https://getbootstrap.com/) 上找到 Bootstrap 文档 —— 这将告诉你所需了解的一切。

## 一个 Flask 项目

Flask 框架使得创建基于 web 的应用程序变得简单。一个 Flask 应用通常由至少两个文件组成：一个 Python 应用程序和一个 HTML 模板。

Python 部分包含应用逻辑：例如，在数据可视化应用中，它可能将数据加载到 Pandas 数据框中，进行一些分析，并在 Plotly 中创建图表。HTML 模板定义了网页的布局，并由 Python 程序提供显示的数据。

```py
/project_name
    |--- app.py
    |
    |--- /templates
            |
            |--- index.html
```

一个简单应用的目录结构，如我们将要创建的那样，应该类似于上面的示意图。主 Python 应用程序位于项目文件夹中，模板文件位于名为 *templates* 的子文件夹中。

当然，你还需要安装 Flask：

```py
pip install flask
```

在这样做之前，你可能需要创建一个虚拟环境。

Flask 应用的 Python 部分定义了一个或多个应用将响应的路由。通常其中一个路由是‘/’，即项目的根目录。

下面是一个使用模板的最小 Python 应用。模板名为 *index.html*，必须位于 *templates* 文件夹中，并由 Flask 使用 `render_template()` 库函数呈现为网页。请注意，我们为网页标题创建了一个值并将其传递给函数。

```py
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/')
def index():
    title = "This is the title"
    return render_template('index.html', title=title)
```

下面是 *index.html* 模板，它期望 `title` 值被纳入其中。你可以看到，在 `<h1>` 标签中，标识符 `title` 被包围在双大括号中。

```py
<!DOCTYPE html>
<html>
<body>
   <h1>{{title}}</h1>
</body>
</html>
```

Flask 使用 Jinja 模板引擎来用传递给 `render_template()` 的值替换 HTML 模板中的占位符。

你可以通过在终端中输入 `flask run` 来运行应用程序，你应该会得到类似于下面的响应。（这假设你将应用程序命名为 *app.py* —— 如果你用其他名字命名，需要输入 `flask --app app_name run`。将 `app_name` 更改为你的应用程序名称，不包含 `.py` 扩展名。）

```py
flask run
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://127.0.0.1:5000
Press CTRL+C to quit
```

将浏览器指向 *http://127.0.0.1:5000* 或 *localhost:5000*，你将看到一个简单的网页，展示了在 Python 代码中定义的文本。

想要了解更多关于 Flask 的信息，你可以从查看他们的[快速入门](https://flask.palletsprojects.com/en/3.0.x/quickstart/)教程开始，但我希望我会在这里涵盖你开始所需的所有内容。

# 一个静态数据可视化应用程序

我们的第一个应用程序将基于迄今为止看到的内容，创建一个包含 Plotly 图表和一些辅助文本的网站。稍后，我们将添加一些交互功能。

让我们看看 Python 的部分。下面的代码列表中，请暂时关注以`#### Simple template ####`开头的部分。在这里，你可以看到我们定义了一个名为`/simple`的路由，这意味着我们通过将浏览器指向*localhost:5000/simple*来调用应用程序，而装饰器下方的`simpleindex(),`函数将会被执行。

在这个函数中，我们设置了一些文本和我们希望在网页上显示的图形。我们首先设置了一些变量，然后使用这些变量创建一个 HTML 模板将使用的参数字典。变量的名称清楚地表明了它们的使用方式。

`get_graph()`函数设置了图形参数。首先，它加载了 1881 年至 2022 年的全球温度异常数据，并追踪气候变化如何影响这段时间的温度（有关详细信息，请参见新数据表明 2023 年是有记录以来最热的夏天）。这些数据在下表中显示（在图表中会更清晰！）。

![](img/560ca8eb44fd84bfdd8efac6f29c0470.png)

全球温度异常

对于静态应用程序，我们将使用单列`JJA`，它指的是 6 月、7 月和 8 月的温度。交互式应用程序也会使用其他一些列，因此我们当前应用程序的`period`参数有一个默认值，之后可以被交互式应用程序更改。

数据用于创建一个 Plotly 条形图，生成的图形被转换为 JSON，网页将使用这些数据。因此，这些 JSON 数据被返回以设置字典参数中的`graph`条目。

回到之前的函数，我们现在需要使用我们设置的参数调用`render_template`。为了节省输入时间，我创建了一个名为`template`的辅助函数，它提取模板参数并将所有数据传递到网页上。

```py
from flask import Flask, request, jsonify, render_template
import json
import pandas as pd
import plotly.express as px

app = Flask(__name__)

def get_graph(period = 'JJA'):
    df = pd.read_csv('GlobalTemps1880-2022.csv')
    fig = px.bar(df, x='Year', y = period, 
                 color=period, title = period, 
                 color_continuous_scale='reds', 
                 template='plotly_white', width=1000, height=500)

    graphJSON = fig.to_json()
    return json.dumps(graphJSON)

def template(params):
    return render_template(params['template'], params=params)

#### App ####

@app.route('/')
def index():
    return render_template('index.html')

#### Simple template ####

@app.route('/simple')
def simpleindex():
    header = "Global Temperature"
    subheader = "Global Temperature changes over the last few centuries"
    description = """The graph shows the increase in temperature year on year.
    The data spans the years 1881 to 2022 and includes temperature anomalies 
    for the months June through August.
    """
    params = {
        'template':'simpleindex.html',
        'title'   : header,
        'subtitle': subheader,
        'content' : description,
        'graph'   : get_graph()
    }
    return template(params)

#### Main ####

if __name__ == '__main__':
    app.run(debug=True)
```

现在来看 HTML 模板。下面的代码列表中，好消息是你可以忽略除`<body>...</body>`标签内的代码外的所有内容。其余的都是你需要的 Bootstrap 和 Plotly 网页的样板代码，可以剪切并粘贴到任何类似的网页中。最后的`<script>`标签也是样板代码，包含了 Bootstrap 的 Javascript 代码，可以安全忽略。

```py
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">
    <script src='https://cdn.plot.ly/plotly-latest.min.js'></script>
</head>
<body>

    <header class="bg-primary text-white text-center py-4">
        <h1 class="display-4">{{ params.title }}</h1>
        <p class="lead">{{ params.subtitle }}</p>
    </header>

    <div id = 'content' class="container mt-4">
        <div id='chart'></div>
        <div class="lead">{{params.content}}</div>
        </div>
    <script type='text/javascript'>
      var figure = JSON.parse({{params.graph | safe}})
      Plotly.newPlot('chart', figure, {});
    </script>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM" crossorigin="anonymous"></script>

</body>
</html>
```

如果我们忽略使页面看起来漂亮的 Bootstrap 属性，我们得到的代码如下，这要简单得多，这就是我从这里开始引用的内容。

```py
 <header>
        <h1>{{ params.title }}</h1>
        <p>{{ params.subtitle }}</p>
    </header>

    <div">
        <div id='chart'></div>
        <div>{{params.content}}</div>
    </div>
    <script type='text/javascript'>
      var figure = JSON.parse({{params.graph | safe}})
      Plotly.newPlot('chart', figure, {});
    </script>
```

我们之前见过使用 Jinja 参数，这次唯一的不同是我们将几个参数打包成一个名为 `params` 的字典。因此，我们通过在参数名称前加上字典的名称来引用它们。因此，`<h1>{{params.title}}</h1>` 只是将 `title` 参数放入一对标题标签中。其他三个文本参数的标签也是类似的。

为了绘制图表，我们需要一个可以放置图表的元素，并且该元素必须有一个 *id* 属性 (`<div id='chart'></div>`)。这个元素被放置在标题下方和描述上方。

下面的脚本元素再次是调用 Plotly Javascript 绘制图表的样板代码。唯一需要注意的是，当我们包含 `graph` 参数时，我们使用了 `safe` 关键字。这指示 Jinja 不要尝试解释 `graph` 中的任何特殊字符，而是将它们按字面意思对待。因此，代码如下：

```py
var figure = JSON.parse({{params.graph | safe}})
```

现在，请记住模板必须在项目目录中的 templates 文件夹中，并且为了使此代码正常工作，数据文件必须位于项目目录本身（当然，你可以移动它，但在 Python 程序中打开时必须更改路径）。

所以运行应用程序并在浏览器中指向 *localhost:5000/simple*，你应该能看到一个如下图所示的网页。

![](img/026a7f8a007aa2461ecefee20a00d8f3.png)

一个静态应用

这就是网页应用的静态版本。

# 一个互动数据可视化应用

但是为什么要限制在夏季？如果我们能够选择除 *JJA* 以外的其他时期不是很好吗？数据还包含全年的列，*J-D*，以及三个时间段：*DJF*、*MAM*、*JJA* 和 *SON*。（这些字母代表英文中的月份名称：十二月、一月、二月；三月、四月、五月；等等。）

为此，我们需要整合一个用户控件，用于选择适当的时间段。我选择了一个下拉菜单，用于显示各种时间段。它将与之前的网页非常相似（参见下图）。

![](img/bf5e22fe0b4b24eb267590c0eb842f85.png)

一个互动应用

代码最初也非常相似。主要区别出现在处理新图表的选择时。

当选择一个新值时，这将调用一个 Javascript 函数，该函数将值发送到服务器上的回调函数并等待响应。这个回调函数将返回一个新的图表，然后由调用的 Javascript 显示。

让我们先处理熟悉的内容。下面是实现新端点 */ddsimple* 的函数。

```py
@app.route('/ddsimple')
def ddsimpleindex():
    # The root endpoint builds the page
    header = "Global Temperature"
    subheader = "Global Temperature changes over the last few centuries"
    description = """The graph shows the increase in temperature year on year.
    The data spans the years 1881 to 2022 and includes temperature anomalies for periods of each year as indicated.
    """
    menu_label = "Select a period"

    params = {    
        'template': 'ddsimpleindex.html',
        'title': header,
        'subtitle': subheader,
        'content' : description,
        'menu_label': menu_label,
        'options' : [{'code':'J-D', 'desc':'Whole year'},
                     {'code':'DJF','desc':'Winter (North)'},
                     {'code':'MAM','desc':'Spring (North)'},
                     {'code':'JJA','desc':'Summer (North)'},
                     {'code':'SON','desc':'Autumn/Fall (North)'}],
        'graph'   : get_graph()
    }
    return template(params)
```

如你所见，它与/*simple*端点非常相似。区别（除了名称）在于额外的参数：菜单的标签和一个表示菜单项列表的字典，首先是一个对应于数据框列的值，其次是该值的文本描述，这些描述将显示在菜单中。

HTML 稍有不同，因为它展示了 Jinja 的更复杂用法。代码如下。

```py
<form id="userForm" name="form1" onChange="getFormValues('form1')">
    <div class="mb-3">
        <label for="dropdown" class="form-label lead">{{params.menu_label}}</label>
        <select class="form-select" id="dropdown" name="dropdown">
            {% for opt in params.options %}
                <option value="{{opt.code}}">{{opt.desc}}</option>
            {% endfor %}
        </select>
    </div>
</form>
<div>
    <!-- Main Content Area -->
    <p class="lead">{{params.content}}</p>
</div>
<div id="graph"></div>
```

在这里，我们构建了一个包含下拉菜单的表单。该表单还包含一个菜单标签，这个标签包含在我们之前见过的双花括号中。

与之前的示例相比，主要区别在于菜单的构建方式。在`<select>`元素内，我们需要放置一系列代表菜单选项的`<option>`标签。`<option>`标签具有一个值和一个描述，这些值和描述是在`params.options`字典中定义的。我们通过执行类似`{% for opt in param.options %}`的 Jinja 循环来包含这些值和描述，该循环遍历字典，将每个元素放入本地变量`opt`中。然后，我们利用这些值和描述，通过使用`opt.code`和`opt.desc`将它们插入到`<option>`标签中。

你可以在[Flask 教程：Pythonbasics.org 网站上的模板](https://pythonbasics.org/flask-tutorial-templates/)中找到 Jinja 模板的简单示例和解释。

表单标签中还有另一个对我们目的至关重要的部分。表单标签内有一个名为`onChange`的属性，它接受某种类型的动作值，在这种情况下，它是一个 Javascript 函数，每当表单中的值发生变化时——在本例中，当选择菜单中的选项时，就会调用这个函数。

这就是有趣的开始。

## 回调

为了用新图表更新网页，我们使用回调机制，下面的图表展示了浏览器与服务器之间的事务。请注意，响应请求新图表的方式是*不是*重新加载页面，而是更新页面——这种方式更快，并且避免了重新加载时出现的瞬时空白屏幕，从而提供了更好的用户体验。

![](img/017940485c124abb31584f388fec0575.png)

使用回调更新网页

回调是通过网页上表单的变化来调用的，正如我们之前提到的。这种机制是一个在表单的`onChange`属性中标识的 Javascript 函数。

我将详细解释 Javascript 函数的工作原理，但基本上，它从表单中获取值，并将这些值发送到 Flask 应用中的回调端点。

现在，如果想到编写 Javascript 让你感到不安，不用担心，你不需要真正了解这些内容，你可以直接复制它，它适用于你可能想在网页上包含的任何表单。因此，对冒险者而言，解释如下，对于其他人——直接跳到下一部分。

实际上有两个函数：从表单中获取值的函数如下所示。

```py
 function getFormValues(f) {
            const form = document.forms.namedItem(f);
            const formData = new FormData(form);
            const value = Object.fromEntries(formData.entries());
            postJSON(value);
        }
```

所有代码都使用内置的 Javascript 函数：第一行从文档（即网页）中获取表单；第二行检索包含表单数据的数据结构；第三行提取该结构中的所有值。

最后，这些值被传递给另一个函数 `postJson`。

```py
 async function postJSON(data) {
            try {
                const response = await fetch("/callback", {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json",
                    },
                    body: JSON.stringify(data),
                });

                const result = await response.json();
                console.log("Success:");//, result);

                drawGraph(result);
            }
            catch (error) {
                console.error("Error:", error);
            }
        }
```

这个函数是一个异步函数，这意味着在调用后，程序执行会立即返回到调用代码，而异步函数会在一个单独的执行线程中继续运行。也就是说，它会与网页的执行并行地继续执行所需的操作。

`postJSON` 函数接收需要发送到 Python 回调代码的数据，并使用异步 `fetch` 函数发送。`fetch` 接收数据应该传递到的端点和数据本身作为参数——我们使用 HTTP POST 机制来发送数据。`postJSON` 等待 fetch 完成，即等待服务器返回一些数据。然后将这些数据传递给 `drawGraph` 函数，更新网页上的图表。

请注意，代码被包含在 `try... catch...` 块中。这与在 Python 程序中看到的基本相同：如果 `try` 块中的代码失败——没有响应或其他通信故障——那么该故障将被记录到控制台中。

## Python 回调

为了使这一切工作，我们需要一个 Flask 代码中的回调函数，该函数将接收数据，对其进行处理（即创建新图表），并返回结果。如下所示：

```py
@app.route('/callback', methods=['POST'])
def callback():
    # The callback updates the page
    if request.is_json:
        data = request.get_json() 
        return get_graph(period=data['dropdown'])
    else:
        return jsonify({"error": "Invalid JSON data"}), 400
```

首先需要定义回调的端点，你可以看到我们还指定了端点期望使用 POST 方法发送数据。

我们也期望数据为 JSON 格式，如果不是，则返回错误。

如果数据*是* JSON，我们从下拉菜单中提取值，并将其传递给`get_graph`函数，该函数绘制图表并以 Plotly 期望的 JSON 格式返回图表数据。这个图表数据由网页上的 Javascript 函数接收，页面得到更新。

这段代码将为你提供上面显示的交互式网页。

## Flapjax —— 名字中有什么

诚然，这个名字有些刻意：它代表了**Fla**sk，**P**ython，**J**avascript 和 **ax**，这些代表了使这种技术得以实现的异步通信。

我希望你能看到，通过使用预先编写的模板和一段模板代码，你可以创建有用的交互式网页，同时主要集中于 Python 代码的逻辑。

这个应用程序仅包含一个图表，当用户选择新选项时，该图表会更新，但可以通过这种方式从 HTML 表单中收集任意数量的值，这些值可以由 Flask 应用程序处理，然后用新信息更新网页（也许我们会在未来的文章中探讨这个问题）。

所有在这里展示的代码和数据可以从我的 [GitHub 仓库](https://github.com/alanjones2/flapjax_public) 下载（查看 *jinja-article* 文件夹）。

*更新：我在 GitHub 仓库的* reuse *文件夹中编写了一个新应用程序，该应用程序使用一组新的数据——HTML 和 Javascript 保持不变，只有 Python 代码和数据发生了变化：* [**重用 Flapjax 模板和代码**](https://medium.com/codefile/reusing-flapjax-templates-and-code-0ee6db58ffc8)

我希望你觉得这些内容有用。如果你想查看更多我的作品，请访问我的 [网站](http://alanjones2.github.io)，并通过订阅我的免费通讯来获取我发布的更新，点击这里：

[## 数据可视化、数据科学和 Python | Alan Jones | Substack

### 关于数据科学、数据可视化及主要使用 Python 进行的动手编码的教程和其他文章。点击…

[technofile.substack.com](https://technofile.substack.com/?source=post_page-----465090fa3fba--------------------------------)

如果 Flask 不是你的首选，我根据我在 Medium 上的文章编写了一本电子书 [*从头开始学习 Streamlit*](https://alanjones.gumroad.com/l/streamlitfromscratch)。

## 说明和参考文献

本文和应用程序中使用的数据来源于下面第 1 和第 2 条说明中的描述。

1.  GISTEMP 团队，2023: GISS 地表温度分析 (GISTEMP)，第 4 版。NASA 戈达德空间研究所。数据集访问日期 2023–09–19，网址：data.giss.nasa.gov/gistemp/。*请注意，NASA 数据集的使用没有特定的许可证。NASA 将这些数据集免费提供用于非商业目的，但应给予归属（如上所述）。*

1.  Lenssen, N., G. Schmidt, J. Hansen, M. Menne, A. Persin, R. Ruedy, 和 D. Zyss, 2019: GISTEMP 不确定性模型的改进。J. Geophys. Res. Atmos., 124, no. 12, 6307–6326, doi:10.1029/2018JD029522。

除非另有说明，所有的图像、图表、截图和代码均由作者创建。
