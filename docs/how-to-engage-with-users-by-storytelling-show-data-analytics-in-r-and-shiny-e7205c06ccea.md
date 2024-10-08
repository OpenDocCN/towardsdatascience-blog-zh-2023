# 如何通过讲故事与用户互动：在 R 和 Shiny 中展示数据分析

> 原文：[`towardsdatascience.com/how-to-engage-with-users-by-storytelling-show-data-analytics-in-r-and-shiny-e7205c06ccea`](https://towardsdatascience.com/how-to-engage-with-users-by-storytelling-show-data-analytics-in-r-and-shiny-e7205c06ccea)

## 如何利用 R 和 Shiny 帮助你找到最适合孩子们的 YouTube 视频

[](https://chengzhizhao.medium.com/?source=post_page-----e7205c06ccea--------------------------------)![Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----e7205c06ccea--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e7205c06ccea--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e7205c06ccea--------------------------------) [Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----e7205c06ccea--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e7205c06ccea--------------------------------) ·阅读时间 9 分钟·2023 年 3 月 13 日

--

![](img/7509f012586e8a90a0f1ef5734651113.png)

如何通过讲故事与用户互动：在 R 和 Shiny 中展示数据分析 | 图片由作者提供

数据通过讲故事的方式变得更具吸引力。作为数据专业人士，我寻求更简单的方式来弥合数据分析与沟通之间的差距。仪表板传统上是可视化和分享数据的默认方式。它也承担了沟通的责任。然而，我发现了仪表板的局限性：图表选择有限，自定义自由度较低。我探索了更多互动的方式来展示想法并与用户互动 — *为你的数据构建一个网页应用程序*。

[**Shiny**](https://shiny.rstudio.com/) 是一个 R 包，用于直接从 R 代码构建交互式网页应用程序。使用 R 而不必担心切换到 HTML、CSS 和 JavaScript 的上下文是令人兴奋的。这节省了始终使用一种语言的时间。我们可以重复使用 R 的数据框，并使用如 dply 和 ggplot2 图表等 R 包来处理数据而不会遇到问题。Hadley Wickham 的书 — [*Mastering Shiny*](https://www.amazon.com/Mastering-Shiny-Interactive-Reports-Dashboards/dp/1492047384) 对此有深入的内容。

使用 R 和 Shiny 更令人兴奋的是，我们现在可以构建一个应用程序，让最终用户可以与我们完成的数据分析进行互动。他们可以通过独立探索获得更多的洞察，而不需要了解背后的复杂情况。在这个故事中，我将向你展示如何通过讲故事与用户互动 — 在 R 和 Shiny 中展示数据分析。

# 准备数据以构建 Cocomelon 视频。

在我之前的故事中，我分享了如何使用 Youtube 数据 API 在 R 中获取世界第二大 Youtube 频道——Cocomelon 的统计数据。我们建立了一个过程来检索数据，进行基本清理，并使用 ggplot2 提供了几个数据可视化。在这次分析中，有几个有趣的发现：

+   最受欢迎的 Cocomelon 视频因为每日观看量大，依然保持上升趋势，这些视频是在 4-5 年前制作的。

+   最受欢迎的视频：“Bath Song”播放次数比第二个视频——“Wheels on the Bus”多 20%以上。

+   在 200 天内发布的视频中观看次数最多的是🔴 CoComelon Songs Live 24/7\。这个视频展示了父母可以通过自动循环的视频保持孩子的兴趣，而不需要手动切换视频。

[](/r-for-data-analysis-how-to-find-the-perfect-cocomelon-video-for-your-kids-833d6b2d9267?source=post_page-----e7205c06ccea--------------------------------) ## R 数据分析：如何找到适合你孩子的完美 Cocomelon 视频

### 如何从零开始用 R 探索新趋势的 Cocomelon 视频，构建端到端的数据项目

towardsdatascience.com

使用数据讲述这个故事很有趣。然而，作为最终用户，我假设你对 R 及一些 R 数据分析包有所了解。

如果你不知道以上内容怎么办？作为家长，我发现这些数据很有趣，可以帮助你的孩子不厌倦“始终受欢迎”的视频。是否有一种简单的方法来向不了解 R 或任何编程语言的人展示这些数据？我们能否让更多用户自行探索？

是的。我可以构建一个网页应用程序并公开分享，让每个人体验用 R 和 Shiny 构建的网页应用。

我记录了如何用 R 的 Shiny 构建这个端到端的网页应用程序的过程。（如果你不关心技术细节，可以滚动到页面底部查看最终结果）

# 灵感来源于《Mastering Shiny》

在阅读 Wickham 先生的书《*Mastering Shiny*》（[链接](https://www.amazon.com/Mastering-Shiny-Interactive-Reports-Dashboards/dp/1492047384)）之前，我对 Shiny 一无所知。Wickham 先生是 RStudio 的首席科学家，他的书在 R 社区中都很有名。选择一个特定的包来编写书籍，Shiny 在 R 社区中是一个独特的项目。

> 让用户与数据和分析互动——Shiny

数据讲故事不应该是单向的，它应该是双向的。通过用户参与，这有助于对数据的相互理解，并使得实现数据分析目标变得更加容易，同时减少沟通障碍。

Shiny 包缩短了执行分析的数据专业人员与利用数据做出决策的数据消费者之间的距离。它为用户提供了一个完美的用例，通过启用一个元素：交互性，使用户能够参与你所做的数据分析。

*注意：这个故事不是 Shiny 或 R 101。这里有书籍和在线资源可以帮助你熟悉内容。我将专注于构建端到端项目的过程。正如 Hadley Wickham 在书中建议的那样，Shiny 应用的正常开发涉及多个迭代。*

# 第一次迭代：UI 布局和服务器

构建一个 Shiny 应用通常包括两个主要组件：UI 和服务器。UI 处理屏幕上的显示，服务器运行后台逻辑以准备好将其传递给 UI 进行渲染。

我们可以重用我们从上一个帖子中持久化的数据，并从 CSV 文件中读取它们：R 数据分析：如何为您的孩子找到完美的 Cocomelon 视频。

这是第一次迭代的完整代码。

```py
library(shiny)
library(DT)
library(dplyr)
library(readr)
library(ggplot2)

ui <- fluidPage(titlePanel(
  h1(
    "How R and Shiny Can Help You Find the Best Cocomelon Videos for Your Kids",
    align = "center"
  )
),

sidebarLayout(
  sidebarPanel(
    textInput(
      inputId = 'number_records_display',
      label = 'Number of records to Display',
      value = '10'
    ),
    width = 3
  ),

  mainPanel(
    h1("Explore Cocomelon Youtube Videos", align = "center"),
    strong("The following charts display the scatter plot by views and likes"),
    plotOutput("plot"),
    tableOutput("video_table")
  )
))

server <- function(input, output) {
  data <-
    reactive(
      read_csv("~/Downloads/cocomelon_2023_3_2.csv") %>%
        transform(
          statistics.viewCount = as.numeric(statistics.viewCount),
          statistics.likeCount = as.numeric(statistics.likeCount),
          statistics.favoriteCount = as.numeric(statistics.favoriteCount),
          snippet.publishedAt = as.Date(snippet.publishedAt)
        ) %>% mutate(
          views = statistics.viewCount / 1000000,
          likes = statistics.likeCount / 10000,
          number_days_since_publish = as.numeric(Sys.Date() - snippet.publishedAt)
        ) %>% arrange(desc(statistics.viewCount))
    )

  output$plot <- renderPlot({
    videos <-
      data() %>% head(input$number_records_display)
    ggplot(videos, aes(statistics.viewCount, statistics.likeCount)) +
      geom_point()
  }, res = 96)

  output$video_table <- renderTable(
    data() %>% select(
      snippet.title,
      statistics.viewCount,
      statistics.likeCount,
      contentDetails.duration,
      number_days_since_publish
    ) %>% head(input$number_records_display)
  )
}

shinyApp(ui = ui, server = server)
```

我们应用程序的第一个版本是直接的：

对于 UI 部分，我们在 Shiny 中使用了`sidebarLayout`来分割`sidebarPanel`和`mainpanel`。在`sidebarPanel`中，我们添加了一个输入文本框以获取要显示的行数，这将进一步传递值到服务器部分并执行`head`操作。在`mainpanel`中，我们添加了一个`plotOutput`和一个`tableOutput`作为显示数据的占位符。

对于服务器部分，我们首先进行数据读取。然后使用转换后的数据，找到所有在 UI 中定义的输出，为`renderPlot`和`renderTable`提供内容。

我们调用`reactive`来减少冗余操作并提高性能，以提供更好的用户体验。Shiny 的反应式表达式实现了这一点。反应式编程是声明性的，允许引擎懒惰地评估和优化。在我们的例子中，当应用程序最初加载时，它只读取 CSV 文件一次并缓存中间结果。如果用户将输入从默认的十行更改为 20 行，我们只需回到最早的阶段重新运行`head`操作，而不是再次从 CSV 文件中读取数据。

这是第一次迭代的结果

![](img/da51e5bd194c853ec5cf628c41890c75.png)

第一次迭代：UI 布局和服务器，使用 R 和 Shiny | 作者图片

# 第二次迭代：与用户点击的交互

使输入文本反映图表和表格中的更改是一个令人兴奋的第一步。你可能注意到，当我输入数字`2`时，图表和表格迅速变化。这是由于反应式编程的效率。

Shiny 支持在 `plotOutput` 上进行额外的互动，这样可以接收用户的指针事件作为其他输出的输入。在前一步中，图表上有许多点，但我们需要知道图表上的每个点代表什么。作为用户，提供额外的信息可以帮助他们进行自我探索。

Youtube API 还提供了我们可以添加到 Web 应用中的嵌入 iframe。我们需要在 Youtube 上搜索标题，以便为孩子们提供无缝体验。

我们需要对代码进行一些更改，以便看到这个变化。以下是第二次迭代的完整代码。

```py
library(shiny)
library(DT)
library(dplyr)
library(readr)
library(ggplot2)

ui <- fluidPage(titlePanel(
  h1(
    "How R and Shiny Can Help You Find the Best Cocomelon Videos for Your Kids",
    align = "center"
  )
),

sidebarLayout(
  sidebarPanel(
    textInput(
      inputId = 'number_records_display',
      label = 'Number of records to Display',
      value = '10'
    ),
    uiOutput("iframe"),
    width = 4
  ),

  mainPanel(
    h1("Explore Cocomelon Youtube Videos", align = "center"),
    strong("The following charts display the scatter plot by views and likes"),
    textOutput("video_title"),
    plotOutput("plot", click = "plot_click"),
    verbatimTextOutput("clicked_video"),
    DT::dataTableOutput("video_table")
  )
))

server <- function(input, output) {
  data <-
    reactive(
      read_csv("~/Downloads/cocomelon_2023_3_2.csv") %>%
        transform(
          statistics.viewCount = as.numeric(statistics.viewCount),
          statistics.likeCount = as.numeric(statistics.likeCount),
          statistics.favoriteCount = as.numeric(statistics.favoriteCount),
          snippet.publishedAt = as.Date(snippet.publishedAt)
        ) %>% mutate(
          views = statistics.viewCount / 1000000,
          likes = statistics.likeCount / 10000,
          number_days_since_publish = as.numeric(Sys.Date() - snippet.publishedAt)
        ) %>% select(
          snippet.title,
          statistics.viewCount,
          statistics.likeCount,
          contentDetails.duration,
          number_days_since_publish,
          player.embedHtml
        ) %>% arrange(desc(statistics.viewCount))
    )

  output$plot <- renderPlot({
    videos <-
      data() %>% head(input$number_records_display)
    ggplot(videos, aes(statistics.viewCount, statistics.likeCount)) +
      geom_point()
  }, res = 96)

  output$video_table = DT::renderDataTable({
    req(input$plot_click)
    top_videos <- data() %>% select(
      snippet.title,
      statistics.viewCount,
      statistics.likeCount,
      contentDetails.duration,
      number_days_since_publish
    ) %>% head(input$number_records_display)

    near_point <- nearPoints(top_videos,input$plot_click)

    datatable(top_videos) %>%
      formatStyle(
        columns = 'statistics.viewCount',
        target = 'row',
        backgroundColor = styleEqual(near_point$statistics.viewCount, c('yellow'))
      )
  })

  output$iframe <- renderUI({
    near_point <- nearPoints(data() %>% head(input$number_records_display) ,input$plot_click)
    HTML(near_point$player.embedHtml)
  })
}

shinyApp(ui = ui, server = server)
```

在 `plotOutput` 上，我们可以定义一个 id: plot_click。每当在图表中点击一个位置时，图表将把坐标发送到服务器，可以在 `input$plot_click` 中找到。

这一次我们在 Shiny 应用中导入了 [DT](https://rstudio.github.io/DT/shiny.html)。DT 是一个 R 接口，用于 JavaScript 库 [DataTables](http://datatables.net/)。`DT::renderDataTable` 提供了比 Shiny 默认的 `renderTable` 更强大的功能。一旦我们能够识别指针位置以知道用户点击的位置，我们可以进一步高亮显示该特定行给用户。`formatStyle` 是我们可以更新表格 CSS 属性的一种方式。我们使用了 `styleEqual`，只要点击的值匹配，定义的样式将会应用。

我们还尝试首先获取最近的点 `near_point <- nearPoints(data(),input$plot_click)`。我们不能指望用户精确点击图表上的确切点。`nearPoints` 是一个帮助函数，用于获取最近的点。因此，它可以正确地突出显示被点击的项目。

要显示视频，我们可以利用 `renderUI` 并使用 Youtube API 已提供的 iframe 在侧边栏中。

![](img/7509f012586e8a90a0f1ef5734651113.png)

第二次迭代：与用户点击的互动 | 图片来自作者

如果用户想选择一系列视频观看怎么办？为了选择多个点，我们可以使用 `brushedPoints`，我们需要做的就是将 `plotOutput(“plot”, click = “plot_click”)` 更改为 `plotOutput(“plot”, brush = “plot_brush”)`，然后我们可以通过以下方式收集点列表。

```py
near_points <- brushedPoints(top_videos,input$plot_brush)
```

现在我们可以选择多个点了

![](img/f4ab87939e5264ae5dfaa87bff3fee01.png)

第二次迭代：与 BrushedPoints 的互动 | 图片来自作者

# 第三次迭代：添加一些主题

主题对于使你的 Web 应用看起来更精致至关重要。Shiny 与 [bslib](https://rstudio.github.io/bslib/index.html) 集成良好，这是一个支持 R 中 [Bootstrap 主题](https://getbootstrap.com/docs/4.6/getting-started/theming/) 的包。

要启用主题，我们可以在 `fluidPage` 中添加一行代码来指示我们感兴趣的主题。

```py
theme = bs_theme(bootswatch = "sketchy")
```

![](img/4ee28f19c2b959cb948fe7e81500b9fe.png)

第三次迭代：添加一些主题 | 图片来自作者

现在，是时候发布我们的应用程序并将其公开。关于[Shiny 教程页面](https://shiny.rstudio.com/tutorial/written-tutorial/lesson7/)中提到了多种选项。RStudio 使得按照这些说明来发布应用程序变得很简单。

![](img/59e07cd2d3f132c7a6d8ee169eb0f7d8.png)

通过 RStudio 发布到 shinyapps.io | 图片由作者提供

现在这里是我们为小朋友准备的 Cocomelon 热门视频的 Shiny 网页应用程序：

[`chengzhizhao.shinyapps.io/Cocomelon_Shiny/`](https://chengzhizhao.shinyapps.io/Cocomelon_Shiny/)

[## 探索 Cocomelon 的 YouTube 视频

chengzhizhao.shinyapps.io](https://chengzhizhao.shinyapps.io/Cocomelon_Shiny/?source=post_page-----e7205c06ccea--------------------------------)

# 最后的想法

在这个故事中，我扩展了我之前的故事《R 用于数据分析：如何为你的孩子找到完美的 Cocomelon 视频》，使其更加引人入胜，并与 Shiny 结合。我分享了如何将数据分析带到下一个阶段：讲故事，并让我们的用户（我们的孩子😊）获得更多帮助。

我还发布了这个[网页应用程序](https://chengzhizhao.shinyapps.io/Cocomelon_Shiny/)，供那些想要互动体验这个网页的人使用。我希望这篇文章能帮助将数据分析的理念带给更多人，并让更多人了解 R 和 Shiny。

我希望这个故事对你有帮助。本文是**一系列**工程与数据科学故事中的**一部分**，目前包括以下内容：

![Chengzhi Zhao](img/51b8d26809e870b4733e4e5b6d982a9f.png)

[Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----e7205c06ccea--------------------------------)

## 数据工程与数据科学故事

[查看列表](https://chengzhizhao.medium.com/list/data-engineering-data-science-stories-ddab37f718e7?source=post_page-----e7205c06ccea--------------------------------)53 个故事！[](../Images/8b5085966553259eef85cc643e6907fa.png)![](img/9dcdca1fc00a5694849b2c6f36f038d4.png)![](img/2a6b2af56aa4d87fa1c30407e49c78f7.png)

你还可以[**订阅我的新文章**](https://chengzhizhao.medium.com/subscribe)或成为[**推荐的 Medium 会员**](https://chengzhizhao.medium.com/membership)，无限访问 Medium 上的所有故事。

如果有问题或评论，**请随时在本故事的评论区留言**，或通过[Linkedin](https://www.linkedin.com/in/chengzhizhao/)或[Twitter](https://twitter.com/ChengzhiZhao)直接联系我。
