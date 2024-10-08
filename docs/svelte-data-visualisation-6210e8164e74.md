# Svelte & 数据可视化

> 原文：[`towardsdatascience.com/svelte-data-visualisation-6210e8164e74`](https://towardsdatascience.com/svelte-data-visualisation-6210e8164e74)

![](img/81954de2afaf7180320431e00696f500.png)

Svelte 条形图（来源：作者，2023 年）

## 使用 Svelte 创建交互式条形图

[](https://sutan.co.uk/?source=post_page-----6210e8164e74--------------------------------)![Sutan Mufti](https://sutan.co.uk/?source=post_page-----6210e8164e74--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6210e8164e74--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6210e8164e74--------------------------------) [Sutan Mufti](https://sutan.co.uk/?source=post_page-----6210e8164e74--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6210e8164e74--------------------------------) ·阅读时间 6 分钟·2023 年 3 月 20 日

--

# 介绍

数据可视化揭示了我们数据中的信息。通常的方法是制作图表或地图。我们可以在诸如 Microsoft Excel 或 Google Sheets 的电子表格软件中完成这项工作。通常这已经足够了，但我们可以通过添加交互性来使其更具吸引力。一个简单的交互性添加例子是通过添加切片器来根据类别筛选数据。在 Microsoft Excel 中，我们可以启用“开发者”选项卡并添加表单元素，如按钮、复选框等。

使用现代软件技术，创建这种交互式数据体验的经典工具是使用 d3.js 以及原生 JavaScript。它的工作原理如此简单，却能产生美丽的结果，真是令人惊叹。然而，与 React、Angular、Vue 等现代框架相比，原生 JavaScript 感觉有些过时。虽然大家都知道这些框架，但我觉得有一个用于数据可视化的 UI 框架被低估了；你从标题中猜到它就是 Svelte。考虑到它的年轻生态系统，我认为这个出色的框架需要被推广，以便让更多人使用或开发它；尤其是在数据科学领域。

本文展示了 Svelte 在交互式数据可视化中的应用（代码链接见下文的 GitHub 链接）。首先，我将简要说明其区别于其他框架的特性。其次，我将概述我们将要可视化的数据和我们的特性。最后，我将解释一些代码片段的工作原理；我更侧重于理念而不是语法。最后，我们将制作一个如文章主要 gif 图片所示的交互式数据 UI。

## 代码

代码可在以下链接获取。

[](https://github.com/sutanmufti/svelte-data-visualisation?source=post_page-----6210e8164e74--------------------------------) [## GitHub - sutanmufti/svelte-data-visualisation: 使用 svelte 可视化数据

### 由 Sutan Mufti（2023）创建的这段代码是 RekayasaData.co.uk 项目的一部分。这个代码库托管了代码……

[github.com](https://github.com/sutanmufti/svelte-data-visualisation?source=post_page-----6210e8164e74--------------------------------)

## 演示

你可以在这里找到演示：

[## 伦敦人口数据 2023

### 演示

[ceritapeta.co.uk](https://ceritapeta.co.uk/svelte-visualisation/?source=post_page-----6210e8164e74--------------------------------)

# 什么是 Svelte？

Svelte 是一个通过将其文件编译成纯 HTML + JavaScript 和 CSS 来创建用户界面的框架。Svelte-kit 是开发 svelte 应用程序的新元框架。它刚刚在[去年 12 月（2022）](https://svelte.dev/blog/announcing-sveltekit-1.0)达到 v1.0。这是一个很好的工具来开始和管理 svelte 应用程序。

在使用 Svelte 几个月后，我爱上了这个框架，因为它的惊人功能。个人认为，这些功能让它脱颖而出，并提供了出色的开发体验。对于数据科学和可视化，我认为这些功能最为有用：

+   **HMR（热模块替换）**：在开发过程中，应用程序在我们更改代码时保持其状态。这意味着实时更改会直接反映在浏览器中，同时保持变量。

+   **原生响应性**：这将作为本文中我们应用的主要功能进行演示！我们不需要在 svelte 中管理状态。如果你熟悉 React，这相当于`useState`钩子。虽然，我们仍然需要管理多个组件之间的状态连接。一个非常简短的代码来演示这个功能；**点击一个按钮，生成一个随机数，并在 HTML 元素中显示它**：

```py
<!-- ./randomiser.svelte -->
<script lang='ts'>
    let theValue: number = 40;
    function randomiser(){theValue = Math.floor(Math.random() * 100);}
</script>
<button on:click={randomiser}>random!</button><br>
<p>the value: {theValue}</p>
```

![](img/d98753051c54ec7e9df9b76168ffe5de.png)

随机器应用（来源：作者，2023）

你可以按原样将上述代码粘贴到 svelte 文件中！

![](img/1f89153e75ff179d9a9bb4a50184c022.png)

随机器应用代码（来源：作者，2023）

+   **数据绑定**：类似于`Document.querySelector`，`bind`语法将一个元素绑定到脚本的变量上。我认为这种方式管理 HTML 元素更简单。我在 github 代码库中提供了一个循环和绑定的示例。

![](img/78c5c440d1409534a41a8c3d626ef219.png)

绑定（来源：作者，2023）

# 构建界面

构建界面需要 HTML、JavaScript 和 CSS 的知识。我更喜欢 typescript 来减少 bug 开发，因为它使我们编写的代码比纯 JavaScript 更严格。现在，让我们使用这些功能（绑定、响应性和 HMR）。我将讨论的步骤是：

+   使用`+layout.svelte`设置主题

+   使用 ES6 语法设置要导入的数据

+   整合各个部分

## 设置主题

首先，我喜欢从根布局的样式开始。`+layout.svelte`文件定义了所有 Svelte 文件的行为。在这个文件中，我只设置了字体、颜色、边距和背景颜色。这基本上是应用的“主题”。为了这个演示，我们保持简单，至少它不是默认的。

```py
<!-- +layout.svelte -->
<svelte:head>

    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            color: #303030;
            margin: 0;
            background-color: #ebebeb;

        }

    </style>
</svelte:head>

<slot></slot>
```

## 设置数据

在这个演示中，我们将使用来自英国大伦敦地区的人口数据。你可以在[这里](https://data.london.gov.uk/dataset/land-area-and-population-density-ward-and-borough)找到这些数据。数据格式为 csv，建议将其转换为 JSON 格式。使用`pandas`非常简单，因为有`to_json("data.json", orient="record")`。请注意`orient`参数，它表示一种无模式的风格数据，类似于 MongoDB。这样，我们可以在 Javascript 中使用`forEach`或`map`方法来处理一个数组。

在 ES6 语法中，我们可以使用`import`从其他脚本文件中导入变量或对象。由于我们的数据基本上是 JSON，我们可以像这样将其定义为一个变量：

```py
// data.ts
export const data = [
  {name: "sutan", article: "Visualising Data with Svelte"},
  {name: "sutan", article: "Spatial Data Science"},
  {name: "someone", article: "somedata"}
  //... and many more rows
]
```

然后我们可以将数据导入并解构到主脚本文件中。

```py
// main.ts

import {data} from './data.ts'

interface Record {
  name: string;
  article: string;
}

function main(data: Record[]){
  data.forEach(record=>console.log(`${record.name} wrote ${record.article}`))
}
main(data)
```

在我的示例中，我们将伦敦人口数据存储在`./src/routes/population_london.ts`文件中，变量名为`LondonData`。

# 构建应用

让我们将这一部分分成两个小节：处理用户事件和实际可视化数据。

## 绑定与用户事件

首先，我们创建一个选项表单字段，用户可以用来选择一个区。请查看以下代码，我们使用`bind`将表单值绑定到`selectionIndex`。在未来的组件中，我们可以使用`selectionIndex`，它会根据用户的事件动态变化。

```py
<div>
    density of
    <select bind:value={selectionIndex}>
        {#each data as d,i}
            <option value={i}>{d.Name}</option>
        {/each}
    </select>
    is {selectedDensity} per square km
</div>
```

## 数据可视化

借用 d3.js 开发范式的理念，我使用 SVG 来可视化图形。Svelte 提供了原生的`each`语法来循环变量。对于我们的人口数据中的每条记录，我们想要创建一个矩形，并将高度设置为密度数据。查看`rect`的`height`属性。

然后，`on:click`属性处理用户点击条形图时的点击事件。它的回调执行一个函数，该函数接受矩形索引作为参数。`activateSelection(index: number)`基本上设置了前一节中绑定的`selectionIndex`。当`selectionIndex`改变时，选择选项表单的值也会改变。

```py
<svg width={data.length * barwidth / 100} height={maxHeight} >
    <g >
        {#each data as d,i}
        <rect  height={d.Population_per_square_kilometre / scaling} y={maxHeight - d.Population_per_square_kilometre / scaling} width={barwidth/100*0.8}  x={(i * barwidth / 100 * 1)} fill={(i == selectionIndex)? "black": "grey"} on:click={()=>{return activateSelection(i)}} on:keydown={()=>{}}></rect>
        {/each}
    </g>
</svg>
```

d3.js 中的等效代码如下：

```py
// usingD3.js
const graph = d3
.select("svg")
.selectAll("rect")
.data(LondonData)
.enter()
.append("rect")
.attr("width", (d)=>d.Population_per_square_kilometre)
.on("click",handleClick)
```

# 演示

使用静态适配器，我们可以将其构建为普通的`index.html`文件，以便在如 apache httpd 这样的 Web 服务器中静态服务。以下链接是已发布的演示，也是在文章顶部的主要图片。

[## 伦敦人口数据 2023

演示使用 Svelte! [ceritapeta.co.uk](https://ceritapeta.co.uk/svelte-visualisation/?source=post_page-----6210e8164e74--------------------------------)

# 结论

我认为 Svelte 是一个被低估的工具，特别是在交互式数据可视化方面。在这个演示中，我们甚至不需要额外的库来进行数据可视化，只需导入数据并使用 Svelte 进行显示。目前一切都是静态的，因为我认为这样足以展示可视化部分。更有趣的是，我们在这篇文章中还没有探讨 Svelte 的全部潜力。例如，可以利用 `+server.js` 构建 API 并将其集成到我们的应用中；或者使用过渡效果；或者构建组件以增强图表的外观。可以确定的是：

Svelte 在数据可视化方面的未来令人兴奋。

我希望你觉得这篇文章有用。谢谢阅读。
