# 如何使用 React 构建 Web 应用

> 原文：[`towardsdatascience.com/a-step-by-step-guide-to-develop-a-map-based-application-part-ii-6d3fa7dbd8b9`](https://towardsdatascience.com/a-step-by-step-guide-to-develop-a-map-based-application-part-ii-6d3fa7dbd8b9)

## 开发基于地图的应用程序的逐步指南（第二部分）

[](https://medium.com/@jacky.kaub?source=post_page-----6d3fa7dbd8b9--------------------------------)![Jacky Kaub](https://medium.com/@jacky.kaub?source=post_page-----6d3fa7dbd8b9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6d3fa7dbd8b9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6d3fa7dbd8b9--------------------------------) [Jacky Kaub](https://medium.com/@jacky.kaub?source=post_page-----6d3fa7dbd8b9--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6d3fa7dbd8b9--------------------------------) ·阅读时间 20 分钟·2023 年 2 月 9 日

--

![](img/6a0d5cd09cb96bc373b944c80753e707.png)

图片来源于 [Lautaro Andreani](https://unsplash.com/@lautaroandreani?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

地图是可视化和理解地理数据的强大工具，但需要特定的技能才能高效设计。

在这份逐步指南中，我们将深入探讨构建一个基于地图的应用程序，以展示周围加油站的客户价格。我们将涵盖产品的不同关键步骤，从最初的概念验证（POC）到最小可行产品（MVP）。

## 系列文章：

[第一部分：概念验证——构建一个极简演示](https://medium.com/p/757766b04f77)

第二部分：如何使用 React 构建 Web 应用（静态布局）

第三部分：使用 React 为你的 Web 应用添加互动性

[第四部分：使用 PostgreSQL、FastAPI 和 Docker 构建后端](https://medium.com/towards-data-science/build-a-back-end-with-postgresql-fastapi-and-docker-7ebfe59e4f06)

## 这篇文章的背景

在这篇文章中，我们将继续之前开始的加油站定位器应用的工作。这一次，我们将专注于准备可以使用静态数据集在本地运行的 Web 应用程序的第一个版本。该 Web 应用程序将使用**React 框架**构建，本文将尽可能详细地介绍给那些对该框架不太熟悉的读者。

我决定为这个应用程序使用 React，因为它是一个令人信服的框架，目前只有少数数据科学家/数据分析师在使用。如果你曾对其他框架的僵化感到沮丧，我希望你会在本文中找到一种设计 web 应用程序的替代方法。你会发现，学习曲线一开始可能看起来有点陡峭（尤其是如果你对 HTML/CSS/JavaScript 不熟悉），但一旦掌握，将这个框架加入你的工具箱将为构建动态、用户友好且可扩展的 web 应用程序打开许多机会。

注意：本文不会包括所有代码，仅提供片段以说明关键原则，但完整代码可以在 [我的 GitHub 页面](https://github.com/jkaub/fuel-station-viewer-react) 上找到。

到文章结束时，我们将拥有我们的 React 应用程序的布局，在下一部分，我们将专注于为其添加互动性。

![](img/d199843b195d293642b7e1c26eb7b3e6.png)

文章结束时，应用程序将“静态”渲染，作者插图

## 为什么使用 React？

有许多工具和框架可以用来构建漂亮而强大的 web 应用程序。在数据科学生态系统中，一些最常用的框架包括 [**Dash**](https://dash.plotly.com/) 或 [**Streamlit**](https://streamlit.io/)。这些工具的一个大优势是它们的简单性和易用性，对于一些使用场景来说非常高效且足够，特别是如果你主要使用 Python。

然而，这也带来了灵活性和自定义的权衡，如果你的应用程序变得更加复杂，开发这些框架中的功能所需的时间将呈指数级增长。

另一方面，React 是一个性能更强且允许更多灵活性的框架。它还鼓励使用模块化和基于组件的架构，这在添加新功能和维护项目时非常有用。最后，它是 JavaScript 生态系统的一部分，而 JavaScript 生态系统是世界上最大和最受欢迎的生态系统之一。

尽管有这些优势，你可能会对学习该框架感到犹豫，尤其是如果你来自 Python 世界。你不仅需要学习框架本身，还可能需要学习一些 JavaScript、HTML 和 CSS，这可能会非常耗时。

关于 HTML 和 CSS，即使你打算深入应用程序（即使是在 Dash 或 Streamlit 中），也需要对这些语言有基本了解。JavaScript 可能会稍微有些挑战，特别是如果你不太熟悉面向对象和异步编程，但一旦学会，它将可能让你更轻松高效地开发 web 应用程序（同时提高你的编程技能）。

# 设置项目

要启动一个由 React 提供支持的项目，你首先需要在计算机上安装 Node.js，它是用于运行 JavaScript 的运行时环境。

Node.js 可以在[这里](https://nodejs.org/en/)下载。

*附注：事情变化很快。我在 2023 年 3 月写了这篇文章，我正在使用* ***React 18.2.0\.*** *如果你在文章发布多年后阅读它，可能会有些过时。因此，请注意以下内容。*

## 创建一个新项目

一旦安装完毕，我们将使用**npx/npm**（大致相当于 Python 中的 pip）通过命令“create-react-app”来设置 React 项目。

```py
npx create-react-app fuel-station-front
```

在 CLI 中运行此命令将自动下载所有所需的包，并在以下文件夹中设置你的项目：

```py
fuel-station-front/
  |-- node_modules/
  |-- public/
       |-- index.html
       |-- favicon.ico
       |-- ...
  |-- src/
       |-- App.js
       |-- index.js
       |-- ...
  |-- package.json
  |-- package-lock.json
  |-- README.md
```

这看起来可能很复杂，让我们逐文件详细了解：

**node_modules/**：此文件夹包含所有使用 npm 添加到项目中的包和依赖项。通常，你应该保持这个文件夹不变。

**public/**：此文件夹包含项目使用的静态文件。默认情况下，它包含基本文件，以运行在启动新项目时生成的默认页面。

**src/**：此文件夹是应用程序的核心，包含其源代码。我们将在这里编写应用程序的代码。最初，它包含运行默认页面的文件，当你启动一个新项目时会生成这个页面。

**package-lock.js**：此文件相当于 requirement.txt。它包含所有依赖项和通过 npm 安装的包的确切版本。

**package.json**：此文件相当于 setup.py，指定了项目的元数据。

## 运行项目

在项目的这一生命周期阶段（基本上是在本地构建和测试），我们将使用**create-react-app**工具的内置开发服务器。这对于快速开发很有用，但不够稳定，不能用于生产目的，我们将在另一篇文章中讨论这一点。

要运行测试服务器，只需进入项目文件夹并运行以下命令：

```py
npm start
```

![](img/49b43591d6b4245ca1ab3df1a93c5fa9.png)

这将自动在你的默认浏览器中打开一个新网页，显示任何新 React 项目中模仿的默认页面。

“localhost” 意味着我们在自己的计算机上运行服务器，而 “:3000" 表示应用程序的端口号。

![](img/ee1397845e75512bbb1551671423dc81.png)

默认的 React 应用，作者插图

## 安装所需的包

React 拥有一个庞大的生态系统，许多包是开源的，会简化你的工作。

特别地，对于这个项目，我们将使用**react-plotly.js**，这是一个为 React 应用程序制作的 Plotly 封装器。

```py
npm install react-plotly.js plotly.js
```

一些非常有趣的包，我还可以提到 **react-router-dom**，这是一个强大的 URL 映射器，用于管理多页面应用程序，以及 **uuid**，这是一个根据最新标准生成自动 ID 的包。我今天不会使用它们，但它们是必须了解的包。

## 提升编码体验

我将使用 **VSCode** 编写项目。许多扩展可用，以简化开发 React 网页应用程序时的工作。

简要谈谈其中的一些。

**ES7 React/Redux/GraphQL/React-Native snippets：** 这个扩展提供了许多快捷方式，用于快速设置一些模板代码。

![](img/dd233320407e87ff500386bb37ffaa73.png)

ES7 动作，作者插图

**Emmet：** 这个插件通过提供许多快捷方式和缩写来自动生成代码，从而实现快速编码。

![](img/256029e5e1306d8888f71b1a44dc51cc.png)

Emmet 动作，作者插图

**Prettier：** 这个扩展在你保存文件时会自动格式化你的代码。

# 构建布局

现在我们的环境已经正确设置，我们可以开始构建我们的应用程序。正如前面部分所提到的，这主要是在修改 src/ 文件夹中的文件后完成的。

让我们稍微看一下里面的内容：

```py
|-- src/
   |-- App.js
   |-- index.js
   |-- App.test.js
   |-- reportWebVitals.js
   |-- setupTests.js
   |-- index.css
   |-- App.css
   |-- logo.svg
```

对我们来说唯一感兴趣的文件是 **App.js**，我们稍后会详细讨论。关于其他文件，简要说明：

+   **index.js** 负责应用程序的渲染。我们将不会修改这个文件

+   **reportWebVitals.js** 用于将网页的性能指标报告给分析服务，在我们使用 React 的层级中，将不会使用。

+   **setupTests.js** 是一个用于设置测试环境的文件。这也超出了本文的范围，我们将不做修改

+   **logo.svg** 仅仅是创建项目时在默认页面中使用的一张图片，可以删除

+   **App.css** 和 **index.css** 包含一些基本的 .css 文件，用于你的网页应用，如果需要，可以修改以实现不同的样式。

## 应用程序架构

在 React 中，我们通过组合自定义组件来构建应用程序。作为最佳实践，每个组件将由两个文件组成：

+   一个 .js 文件用于编写组件的逻辑：它的功能、如何管理其内部状态以及如何将状态传递给父组件和子组件

+   一个 .css 文件用于组件的美观和格式设置

让我们以我们的应用程序为例，看看我们的逻辑如何应用。我们希望我们的应用程序基本上有三个主要组件：

+   一个搜索区域表单将用于查找地点和燃料类型

+   一个地图，将显示车站，就像我们在上一篇文章中构建的那样

+   一个表格用于快速总结周围车站的主要信息

这将看起来像这样：

```py
|-- src/
  |-- App.js
  |-- App.css
  |-- AppComponents/
    |-- StationsFilter/
      |-- StationsFilter.js
      |-- StationsFilter.css
      |-- CustomTextBoxes/
        |-- ...
    |-- StationsMap/
      |-- StationsMap.js
      |-- StationsMap.css
    |-- StationsTable/
      |-- ...
  |-- index.js
  |-- ...
```

这可能看起来有很多文件和文件夹，但这将有助于保持项目的清晰和整洁，如果你将来需要返回修改或改进应用程序，这将使你的生活变得更加轻松。

## 应用基础布局

在这一点上，你可能想要构建一个应用程序的粗略布局。如果你没有网页设计师帮助，最好的办法是查看现代应用程序的示例，从中获取灵感。互联网充满了构建漂亮组件的模板，通过基本的 .css 知识和这些模板的帮助，你已经能够做出漂亮且现代的元素。

在我们开始项目时，我们想要构建一个基本的初始视图，以大致了解如何组织我们的组件。基本布局完成一半，实际上，网页应用程序基本上是由盒子中的盒子构成的……

对于我们的网络应用程序，我们保持简单：

![](img/a2e720a415e5c6d34f78982ac7911dbc.png)

基本应用布局，作者插图

在进一步讨论之前，我们还需要谈谈设计范式。在 HTML/CSS 中，你有很多组织组件的方式。我最喜欢的范式是[Flexbox 范式](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)，它提供了一种简单的方法来组织容器内的盒子。

使用这种范式，你可以设置“容器盒子”的组织样式：

+   子盒子的对齐方式是按行还是按列？

+   在这个结构中，子盒子是如何在主轴上组织的？

+   在这个结构中，子盒子在正交轴上是如何组织的？

上述布局遵循 Flexbox 范式。

灰色区域是主要容器。我们希望它包含两个“div”（即 HTML 语言中的盒子），按列组织：一个包含头部（一个标题，有时还有一些菜单选项），另一个包含主要组件。

这个 div 将以行的方式组织，从而将空间分成两部分：左侧部分，我们将有一个 div（橙色）用于过滤组件和价格表格，而右侧部分，将有地图组件，占据剩余空间。

```py
export default function App() {
  return (
    // the gray box, we want its "organisation" to be in column
    <div className="App">
      //the header, being the first blue box
      <header className="header">
        <h1 className="h1-searchbar">Gas Station Finder</h1>
      </header>
      //the second blue box, containing the main components, which will be organised in row
      <div className="main-components">
        //Inside that blue box, we have another box "left-section" (the orange box) containing the Filter and the Table
        <div className="left-section">
          <StationsFilter />
          <StationsTable />
        </div>
        //Finally we also have the other component, our map, inside the second blue box
        <StationsMap />
      </div>
    </div>
  );
}
```

<StationsMap />、<StationsTable /> 和 <StationsFilter /> 将是我们将在接下来部分构建的自定义组件。

App.css 将包含我们容器盒子的 .css，来看看吧！

```py
.main-components{
  height:90vh;
  width:100%;
  display: flex;
  flex-direction: row;
  align-items: center;
}

.left-section{
  height:100%;
  width: 39%;
  background-color: #f4f4f4;
  display: flex;
  flex-direction: column;
  align-items: center;
}
```

**display: flex** 指示容器是“弹性盒子”，子盒子将按照这个范式进行组织

**flex-direction** 指示盒子是按行还是按列组织的

**align-items** 用于在次要方向上组织对齐

**justify-content**（这里未使用）用于在主方向上组织对齐

## 初始化组件

现在所有上述内容都已设置，让我们用基本版本初始化所有组件，我们将随着时间的推移进行改进。

在 StationsFilter.js 中，我们将使用快捷方式“rfc”（如前一部分中的 .gif 所示），这将直接生成我们的组件模板，并添加相应 .css 文件的导入语句。

```py
import "./StationsFilter.css";
```

```py
export default function StationsFilter() {
  return (
    <div>StationsFilter</div>
  );
}
```

这段小代码通过“export default”指示函数 StationsFilter 以后可以直接在另一个文件中导入，如：

```py
import StationsFilter from 'pathtofile/StationsFilter';
```

并在其自身的 HTML 标签中使用：

```py
<StationsFilter />
```

让我们为 StationsMap.js 和 StationsTable.js 制作相同的模板，我将在这里传递代码快照，它与我们之前做的完全相同。

## 处理过滤器布局

我们希望过滤器由两个组件组成：

+   一个用于按邮政编码查找地点

+   一个用于选择燃料类型

```py
import "./StationsFilter.css";
export default function StationsFilter() {

  const fuelTypes = ["E10","E85","Gazole","GPLc","SP95","SP98"]

  return (
      <form className="search-form">
        <input type="text" placeholder="Postal Code" className="general-input" />
        <select value="SP98" className="general-input">
          {fuelTypes.map(e => <option value={e}>{e}</option>)}
        </select>
        <button className="send-request-button">Find Stations</button>
      </form>
  );
}
```

让我们详细查看一下：

+   我们首先定义一个类型为“form”的框，在其中放置我们的表单字段和按钮

+   我们创建一个“text”类型的输入框，用于接收文本。我们设置了一个占位符名称“邮政编码”，以给客户提供指示，并为 .css 设置了一个 className

+   我们创建一个带有 <select> 标签的 Dropdown。

+   在这个 Dropdown 组件中，我们需要将选项作为“children”标签添加。这是通过迭代数组（相当于 Python 列表）来自动生成所有可能性。

```py
const fuelTypes = ["E10","E85","Gazole","GPLc","SP95","SP98"]
<select value="SP98" className="general-input">
   {fuelTypes.map(e => <option value={e}>{e}</option>)}
</select>
```

等同于：

```py
const fuelTypes = ["E10","E85","Gazole","GPLc","SP95","SP98"]
<select value="SP98" className="general-input">
    <option value="E10">E10</option>
    <option value="E85">E85</option>
    <option value="Gazole">Gazole</option>
    <option value="GPLc">GPLc</option>
    <option value="SP95">SP95</option>
    <option value="SP98">SP98</option>
</select>
```

我们使用“map”版本，因为它更紧凑，并且可能更容易维护：如果出现新的燃料类型，我们只需修改我们的 fuelTypes 数组，它将自动生成相应的选项。

+   最后，我们创建一个按钮，稍后将成为发送请求到服务器并检索一些数据以显示的入口点

*注意：目前没有任何互动，这将在下一篇文章中详细说明。*

*注意 2：我不会在这里详细介绍 .css，大部分是通过在互联网上寻找灵感获得的，并不一定在这里详细说明很有趣。*

组件单独的样子如下：

![](img/4d9abf9234db338a54e4df24f7a7540f.png)

过滤器组件，作者插图

## 构建地图

为了构建地图，我们将使用 **react-plotly** 插件。

在 React 中，plotly 图形基本上与 Python 中的构建方式相同，都是通过 traces 和 layout。主要的区别在于，我们不使用包装器来构建图表（如 go.Scatter），而是需要以 JSON 格式（一个包含键值对的字典）提供数据。

如果你习惯用 Python 构建图表，不要惊慌，实际上将你的 Python 图形转换为 React 图形是非常简单的。

作为现在的起点和插图，我将展示如何将我们的 Python 图形复制到 React 中。稍后，当我们添加交互时，我们将通过添加良好的数据源来重建此地图。这是我们想从 Python 代码中复制的地图：

![](img/678a3b01b24d71fb1c575fcaf8eac914.png)

从 Python POC 中的地图小部件，作者插图

第一步：让我们创建一个 .json 版的图表：

```py
fig_json_path = "fig.json"

#Convert the file to a json string, and saved in in the computer
fig_as_json = fig.to_json()
with open(fig_json_path, 'w') as f:
    f.write(fig_as_json)
```

如果你想查看你的图形作为 Python 字典的样子，你也可以使用 fig.to_dict()。对于上面的图形，这将生成类似于以下的内容：

```py
{
 "data":[trace1, trace2 ...],
 "layout":figure_layout
}
```

例如，采用布局和我们图形中的一个轨迹：

```py
trace_stations = {
    'lat': stations_lat,
    'lon': stations_lon,
    'marker': {'color': stations_price,
    'colorscale': colorscale
    'size': 40},
    'mode': 'markers',
    'opacity': 0.7,
    'showlegend': False,
    'text': stations_price,
    'type': 'scattermapbox',
    'uid': '911aadec-c42e-4eb7-968a-6e489606224e'
}

figure_layout = {
    'height': 600,
    'width': 600
    'mapbox': {'accesstoken': token,
    'center': {'lon': center_lon, 'lat': center_lat},
    'style': 'streets',
    'zoom': zoom,
    'bearing': 0,
    'pitch': 0},
}
```

如果你习惯使用 plotly，大多数字段应该都很熟悉。注意“type”字段，它指定你正在构建的轨迹类型。在我们的例子中，“type”:”scattermapbox”，相当于我们在 Python 中用 (go.Scattermapbox() )

这可以直接在 react 中使用，并且可以复用到任何类型的图形中，使得从 Python 中的概念验证过渡到 React 中的对应部分变得非常简单。

让我们将之前在项目中生成的完整 JSON 复制到 StationsMap 组件的文件夹中。

```py
|-- src/
  |-- App.js
  |-- App.css
  |-- AppComponents/
    |-- StationsHeader/
    |-- StationsMap/
      |-- StationsMap.js
      |-- StationsMap.css
      |-- fig.json
```

现在我们可以在我们的 StationsMap.js 脚本中简单地导入该文件，并在 react-plotly 图中使用它。

```py
import "./StationsMap.css"
import Plot from "react-plotly.js";
import figJson from "./fig.json"
export default function StationsMap() {

  const data = figJson["data"]
  const layout = figJson["layout"]

  layout["autosize"]=true
  layout["margin"]={"b":0,"t":0,"l":0,"r":0}
  delete layout.template
  delete layout.width
  delete layout.height

  return (
    <Plot
    data={data}
    layout={layout}
    style={{ height: "100%", width: "100%" }}
    useResizeHandler={true}
    />
  )
}
```

让我们详细查看：

首先，我们简单地导入我们的 JSON 对象并将其存储在变量 figJson 中。这样做的话，figJson 是一个 JavaScript 对象，相当于 Python 字典，这将为我们当前的静态视图提供便利。

```py
import figJson from "./fig.json"
```

然后，正如我们在 Python 中所做的，我们更新了我们的布局对象，添加了额外的信息并删除了一些键/值。在我的例子中，我将 plotly 布局设置为“autosize”，这意味着它将占用所有可用空间，去掉图形周围的所有边距，并删除默认由 Python 图形生成的“template”、“width”和“height”键/值。

```py
 layout["autosize"]=true
  layout["margin"]={"b":0,"t":0,"l":0,"r":0}
  delete layout.template
  delete layout.width
  delete layout.height
```

最后，我声明从我的 StationsMap() 函数返回的组件是一个 plotly 组件，包括我儿子所有的轨迹和修改后的布局。

```py
return (
    <Plot
    data={data}
    layout={layout}
    style={{ height: "100%", width: "100%" }}
    useResizeHandler={true}
    />
  )
```

注意，你可以直接在标签中使用以下格式将参数传递给组件：

```py
<MyComponent param1={param1} param2={param2} />
```

对于 plotly 组件，我们传递一个 data 参数，它是一个表示图表不同轨迹的对象列表，一个 layout 参数，它是一个包含图表布局信息的对象，还有像“style”或“useResizeHandler”这样的额外参数，这些参数用于指示 plotly 生成的 div 应该占用所有剩余的空间。

![](img/7838b650aafd89f40f8cd6c97b306065.png)

plotly 如何作为应用的组件进行渲染，与 Python 版本相同，作者插图

## 构建表格

我们还缺少一个最后的组件来完成我们的应用布局：一个总结周围车站信息的表格。

表格是可能难以设计的组件，因为我们需要找到展示信息的正确级别。它们通常比图表更难以解读，但在你想要数据详细信息的总体视图时仍然非常有用。

对于我们的组件，我们想要提供一些可用的原始数据，例如：

+   车站的地址

+   感兴趣的燃料价格以 €/L 表示。

此外，我们还可以将数据转换为对用户更具体的格式，例如：

+   平均加满一箱油的总价格（比如 60L）

+   相对于周围环境的平均位置，潜在的节省或损失

+   为什么不添加一个 Google 地图链接，以便用户可以被重定向到 Google 服务，以获取有关提供地址的更多信息呢？

这看起来没什么，但把自己放在客户的角度是非常重要的。你在一次查看数据时带来的清晰度越多，他们就越喜欢你的应用程序并愿意回来。

鉴于我们仍然处于“静态视图模式”，我将根据默认地图在 Python 中生成一个数据示例，并首先在我的布局中使用它。表示表格的 json 有多种方式，我选择了使用我个人认为更方便的对象列表：

```py
dataTable = [
    {
     "address":address1, "price_per_L":price_per_L1, "price_tank":price_tank1, 
     "delta_average":delta_average1,"google_map_link":google_map_link
    }, 
    {
     "address":address2, "price_per_L":price_per_L2, "price_tank":price_tank2, 
     "delta_average":delta_average2, "google_map_link":google_map_link
    },
    etc...
]
```

我将添加一些提示：

+   如果你对 Python 比对 Javascript 更为熟悉，那也没问题。在这种情况下，可以在 Python 中完成所有数据转换。我们将在接下来的文章中看到如何将我们的 React 应用程序连接到一个 Python API，其中大部分数据转换逻辑将由其处理。

+   使用 pandas，你可以非常轻松地将 DataFrame 转换为对象列表，并使用以下方式将其导出为 json：

```py
with open("sample_table.json", "w") as f:
    json.dump(df.to_dict("records"), f)
```

![](img/ec2461d4f416fb3fb803d2570ea4eb38.png)

在导出为 json 之前的 Python 中 DataFrame 的示例

就像 plotly 图形一样，让我们将文件复制到相关文件夹中。

```py
|-- src/
  |-- App.js
  |-- App.css
  |-- AppComponents/
    |-- StationsHeader/
    |-- StationsMap/
    |-- StationsTable/
      |-- StationsTable.js
      |-- StationsTable.css
      |-- sample_table.json
```

现在我们有了工作基础，让我们编写表格。这是 HTML 中的整体布局：

```py
<table className="price-table">
  <thead>
    <tr>
      <th>Address</th>
      <th>Price</th>
      <th>Full Tank</th>
      <th>Saved</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td 
        <td>Some values</td>
        <td>Some values</td>
        <td>Some values</td>
        <td>Some values</td>
      </tr>
      <tr>...</tr>
      <tr>...</tr>
  </tbody>
</table>
```

`<table>` 标签用于初始化表格。然后我们按行定义表格。我们从表头开始，这是一个特殊的部分，由 `<thead>` 标签指示，接下来是表体（表格的不同行），位于 `<tbody>` 标签中。

然后每一行都按相同的方式定义。`<tr>` 用于定义整行，`<th>` 用于定义该行中的一个单元格。这意味着如果你的表头有 4 个单元格，你在行中也应定义 4 个单元格。

现在让我们使用我们的静态数据。和图形的最后部分一样，我们开始导入预生成的 json 数据：

```py
import tableJson from "./sample_table.json"
```

记住，这个 tableJson 是一个对象数组。我们可以再次使用 Array.map() 方法来迭代数组中的对象，并动态生成我们的表格。

```py
<tbody>
  {tableJson.map((row) => {
    return (
      <tr key={row.address}>
        <td>{row.address}</td>
        <td>{row.price_per_L.toString()+" €/L"}</td>
        <td>{row.price_tank.toString()+" €"}</td>
        <td>{row.delta_average+" €" }</td>
      </tr>
  )})}
</tbody>
```

这里有几个评论：

+   由于我们动态生成表格的行，React 将生成警告，如果我们没有使用唯一的键来标识每一行。我决定传递地址，因为在我这儿是唯一的，但另一种方法是使用像 **uuid** 这样的外部库自动生成它们。

+   注意，在 JSX 逻辑中，标签内的变量必须使用 {} 表示，以指示它们是变量（它们的值应在标签或标签参数中访问）。

+   我们在 JavaScript 中访问对象的元素的方式与在 Python 中相同，因此如果你来自 Python 领域，你应该对这种表示法很熟悉。

+   我无法重复强调，保持格式清晰。在这种情况下，我不仅显示原始数据，还添加了一个后缀（€ 或 €/L），使数据更具可读性

我们现在有了一个非常简单的表格，经过一些 .css 格式化后如下所示：

![](img/2d5e33d5bf9434232a7b833f4bf1f0f6.png)

我们的数据表经过一些格式化处理后，作者插图

这个版本虽然不是一个坏的开始，但我们肯定可以改进。首先，我们可以添加前面提到的 Google Maps 链接。你可以使用以下公式从站点地址预先计算这些链接：

```py
google_link = f'https://www.google.com/maps/search/?api=1&query={address.replace(" ","+")}'
```

然后我们可以从表格中的地址单元格生成一个超链接，将 Google 链接放入 <a href> 标签中：

```py
 <td>
    <a href={row.google_map_link}>{row.address}</a>
  </td>
```

我们希望对这个表格做最后的改进。你可能已经注意到我在 Python 表格中添加了一个名为“比平均值更好”的列？这是一个将帮助我们创建颜色映射，以快速区分便宜的站点和昂贵的站点的值。让我们在映射函数中创建一个默认为黑色的变量颜色，但根据“比平均值更好”字段可以变为红色或绿色，并将该颜色传递给每行最后一个单元格的样式：

```py
 if (row.better_average == 1) {
      color = "green";
    } else if (row.better_average == -1) {
      color = "red";
    }

    ...

    <td style={{ color: color }}>{row.delta_average + " €"}</td>
```

让我们看看最终结果：

![](img/ca6ca6a392967acc82cfc83d01bd110b.png)

我们的最终表格组件，作者插图

我们的表格已经准备好，我们现在可以很容易地查看哪些站点在价格方面更具竞争力，并通过简单的点击检查它们在 Google Maps 上的位置！

## 最终应用布局

到此为止，我们的布局已定型。我们在第一章中已经实现了主要的应用布局：

```py
export default function App() {
  return (
    // the gray box, we want its "organisation" to be in column
    <div className="App">
      //the header, being the first blue box
      <header className="header">
        <h1 className="h1-searchbar">Gas Station Finder</h1>
      </header>
      //the second blue box, containing the main components, which will be organised in row
      <div className="main-components">
        //Inside that blue box, we have another box "left-section" (the orange box) containing the Filter and the Table
        <div className="left-section">
          <StationsFilter />
          <StationsTable />
        </div>
        //Finally we also have the other component, our map, inside the second blue box
        <StationsMap />
      </div>
    </div>
  );
}
```

所以我们可以简单地查看 [`localhost:3000/`](http://localhost:3000/) 并检查其外观：

![](img/ae7743a4967e9d6f11b15e61b53171de.png)

最终应用布局，作者插图

请注意，如果您对这个布局不满意，考虑到我们代码的模块化，改变为不同配置是非常简单的。（在下一篇文章中，我们也将讨论应用程序的响应性）

# 结论

在这篇文章中，我们继续了上次关于站点查找器应用程序的工作。特别是，我们开始详细了解 React 框架，以构建强大且高度可定制的 Web 应用程序。

我们已经达到了可以在本地测试服务器上运行应用程序静态版本的阶段，但距离拥有完全运行的应用程序仍然很远。

我们的 React 原型尚未完成，在下一篇文章中，我们将继续探索 React，这次重点关注交互功能。
