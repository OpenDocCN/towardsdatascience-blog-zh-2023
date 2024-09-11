# 《使用 Devtools 创建和发布 R 数据包的深度指南》

> 原文：[https://towardsdatascience.com/in-depth-guide-to-creating-and-publishing-an-r-data-package-using-devtools-245b0fd4c359?source=collection_archive---------8-----------------------#2023-10-19](https://towardsdatascience.com/in-depth-guide-to-creating-and-publishing-an-r-data-package-using-devtools-245b0fd4c359?source=collection_archive---------8-----------------------#2023-10-19)

## 详细讲述了我开发的“Richmondway” R 数据包的步骤，其中包含 Roy Kent 的咒骂词统计

[](https://medium.com/@menghani.deepsha?source=post_page-----245b0fd4c359--------------------------------)[![Deepsha Menghani](../Images/56a6ed8597c36e8c76d8a29a449325a4.png)](https://medium.com/@menghani.deepsha?source=post_page-----245b0fd4c359--------------------------------)[](https://towardsdatascience.com/?source=post_page-----245b0fd4c359--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----245b0fd4c359--------------------------------) [Deepsha Menghani](https://medium.com/@menghani.deepsha?source=post_page-----245b0fd4c359--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb0c00845bcfa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fin-depth-guide-to-creating-and-publishing-an-r-data-package-using-devtools-245b0fd4c359&user=Deepsha+Menghani&userId=b0c00845bcfa&source=post_page-b0c00845bcfa----245b0fd4c359---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----245b0fd4c359--------------------------------) · 9 分钟阅读 · 2023 年 10 月 19 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F245b0fd4c359&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fin-depth-guide-to-creating-and-publishing-an-r-data-package-using-devtools-245b0fd4c359&user=Deepsha+Menghani&userId=b0c00845bcfa&source=-----245b0fd4c359---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F245b0fd4c359&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fin-depth-guide-to-creating-and-publishing-an-r-data-package-using-devtools-245b0fd4c359&source=-----245b0fd4c359---------------------bookmark_footer-----------)![](../Images/95dac23a0f2031aee4d139234160bdb3.png)

图片由 [Erda Estremera](https://unsplash.com/@erdaest?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，来源于 [Unsplash](https://unsplash.com/photos/sxNt9g77PE0?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

当我被邀请在2023年Posit会议上就[动画和互动讲故事](https://deepshamenghani.github.io/posit_plotly_crosstalk/#/title-slide)进行演讲时，我花了几个月时间考虑完美的数据集。似乎每个有趣的数据集都已经用尽了，我没有灵感用一个平淡无奇的数据集来做演讲。然后有一天，在看“泰德·拉索”这部美国体育喜剧剧集时，罗伊·肯特巧妙地插入的脏话引发了我的灵感。我重新观看了这部剧（顺便说一下，是以2倍速观看的），并统计了罗伊使用或示意“F**k”这个词的次数。那就是我的数据集！在这篇文章中，我将带你逐步了解如何将这个数据集转变为一个R数据包，使你也能轻松创建一个。

欢迎来到`[Richmondway](https://github.com/deepshamenghani/richmondway)`的制作过程，这是我第一个R数据包，允许你下载数据，逐集、逐季深入探讨罗伊·肯特的词汇。最终，回答一个从未有人问过的问题——罗伊·肯特在哪一季说“F**k”最多？

![](../Images/6c98ac49eb033536c4c55052573f82c7.png)

来源：[Posit Conference 2023 presentation](https://deepshamenghani.github.io/posit_plotly_crosstalk/#/title-slide) 由Deepsha Menghani提供

## 我为什么要创建一个包？

+   我一直渴望学习如何创建R包。从一个简单的数据包开始似乎是一个很好的初步尝试。

+   嵌入数据以进行函数测试是至关重要的。它使用户熟悉包的功能——这是我在未来处理任何复杂包时所需要的步骤。

+   这个数据集太有趣了，不想自己独享。打包它确保了会后大家都能轻松访问。

所以，无论你是对R数据包的创建感到好奇，还是你是“泰德·拉索”迷，泡杯茶，咱们开始吧！

# 我用来创建R数据包的详细步骤

## 第0步：数据集和包名称

这是我的数据集“richmondway”的快照。它有34行，对应于每一集，还有16列，包含各种值，这些值在我们将要创建的包中进行了描述。

给你的包命名就像给宠物命名一样——非常特别。虽然你会希望选择一个易于记忆的名字，但也要确保它简单，特别是如果你打算公开发布的话。我给我的包命名为`Richmondway`——这是对“泰德·拉索”中罗伊·肯特曾经效力的足球俱乐部AFC Richmond的致敬。而且，因为它以“R”开头，这也是一种幸运的巧合。我还希望这个名字能够清晰地指示包的内容。

## 第1步：安装工具包

安装这些R包：`devtools`、`usethis`和`roxygen2`。它们使得构建和记录你的新包变得非常简单。

```py
install.packages(c("devtools", "usethis", "roxygen2"))
```

## 第2步：将新包创建为项目

你可以使用`devtools`创建包，有两种方法。Devtools处理了许多初始包结构设置所需的工作。

> 方法1：直接从RStudio控制台

![](../Images/d086a4a2c90b9c43d7ed8515762bb6c2.png)

使用devtools创建R包的项目录制

> 方法2：使用`usethis`包

使用`usethis::create_package()`命令，你可以通过提供要创建包目录的路径来直接创建一个新包。在本文的其余部分，我将继续展示其他`usethis`命令，这些命令使得完成大量包创建和文档步骤变得更简单和更快。

```py
usethis::create_package("path/richmondway")
```

你刚刚创建了一个包含R包基本需求的文件夹。如果你窥视其中，会发现一些神秘的文件。不用担心，我们将逐一了解它们。以下是你在项目中会看到的一些文件。

![](../Images/da5bc030f8c26ffce73194e71be98726.png)

项目的初始主目录

## 第3步：添加数据集

我将数据集保存在本地环境中，命名为“richmondway”。运行以下命令将在包的根目录下添加一个`‘/data’`目录，并将一个`“.rda”`文件放入其中。

```py
usethis::use_data(richmondway)
```

![](../Images/6e52a46c9f91c506597137acb3f1cc65.png)

“data”文件夹中的一个名为“richmondway.rda”的单一文件

## 第4步：创建数据字典`data.R`：

这是描述你的数据集的地方。相信我，描述越好，其他人越容易发掘其潜力。这也会在后续步骤中纳入你的文档。你可以使用以下命令创建此文件，然后稍后更新所有所需的详细信息。

```py
usethis::use_r("data")
```

此命令将创建一个`data.R`文件在`R`文件夹内。更新该文件的内容以包含数据集中的格式和列。虽然我的数据集中有更多列，但在这个例子中，我只展示了三列。你应尽可能清楚地添加所有列的描述，因为它将出现在数据集包文档中。下面的格式在`data.R`文件内用于创建描述。

```py
#' Data to showcase f**k count
#'
#' A dataset containing the number of times the word f**k was used in Ted Lasso by Roy Kent.
#'
#' @format A data frame with 34 rows and 16 columns.
#' \describe{
#' \item{Character}{Single value - Roy Kent}
#' \item{Episode_order}{The order of episodes from the 1st to the last}
#' \item{Season}{The season 1,2 or 3 associated with the count}
#' }
#' @source Created by Deepsha Menghani by watching the show and counting the number of F**ks used in sentences and as gestures
#'
#' @examples
#' data(richmondway)
"richmondway"
```

让我们将这个文件拆解为它的组成部分：

> 描述注释：

```py
#' Data to showcase f**k count
#'
#' A dataset containing the number of times the word f**k was used in Ted Lasso by Roy Kent.
```

这是数据集的简短标题和描述。以`#'`开头的注释用于以特殊方式注解R对象，这些对象将被`roxygen2`包识别，该包在R中用于生成文档。

> 格式注释：

```py
#' @format A data frame with 34 rows and 16 columns.
```

这指定了数据的格式。在这种情况下，数据集是一个包含34行和16列的数据框。

> 变量描述：

```py
#' \describe{
#' \item{Character}{Single value - Roy Kent}
#' \item{Episode_order}{The order of episodes from the 1st to the last}
#' \item{Season}{The season 1,2 or 3 associated with the count}
#' }
```

这一部分详细描述了数据框中的一些主要变量/列。`\describe`块用于列出并描述每个由`\item`标签表示的变量，包括变量的名称及其描述。

> 来源注释：

```py
#' @source Created by Deepsha Menghani by watching the show and counting the number of F**ks used in sentences and as gestures
```

这提供了关于数据的来源或出处的信息。重要的是要注明创作者并提供数据收集的背景。

> 示例评论：

```py
#' @examples #' data(richmondway)
```

这提供了一个示例，说明用户如何访问和使用数据集。在这种情况下，它只是演示了如何在安装你的包后将数据加载到 R 中。

> 数据名称：

```py
"richmondway"
```

这是数据集的名称。它用引号括起来，因为这表明该文档与包中同名的数据集相关联。

当用户在 R 中安装和加载你的包后，输入 `?richmondway`，他们会看到以结构化格式呈现的文档，帮助他们了解数据集的内容、结构以及如何使用它。

## 步骤 5：更新 "`DESCRIPTION`" 文件

描述文件是包的更高层次的文档。查看你的包主文件夹中的 `DESCRIPTION` 文件，它应该预先填充了说明，需要更新为正确的描述。我在描述文件中更新的字段如下，其余保持默认。

```py
Package: richmondway
Title: A dataset containing the number of times the word f**k was used in Ted Lasso by Roy Kent
Authors@R: person("Deepsha", "Menghani", email = "abc@gmail.com", role = c("aut", "cre"))
Description: Downloads the dataset containing the number of times the word f**k was used in Ted Lasso by Roy Kent.
License: file LICENSE
```

## 步骤 6：创建“LICENSE”文件

这个文件类似于你包的简历。请注意，在我的 DESCRIPTION 文件中，我提到一个叫 `LICENSE` 的文件。这个文件还不存在，因此我们现在将创建它。它会指向一个存储许可证信息的文件。许可证信息告知用户如何使用通过此包提供的数据，即使用权。在我的案例中，我使用了 CC0 许可证，并使用下面的命令将标准描述添加到 `LICENSE` 文件中。

```py
license_text <- 'CC0 1.0 Universal (CC0 1.0) Public Domain Dedication
The person who associated a work with this deed has dedicated the work to the public domain by waiving all of his or her rights to the work worldwide under copyright law, including all related and neighboring rights, to the extent allowed by law.
You can copy, modify, distribute and perform the work, even for commercial purposes, all without asking permission.
For more information, please see
<http://creativecommons.org/publicdomain/zero/1.0/>'

writeLines(license_text, con = "packagepath/LICENSE")
```

![](../Images/de998c41a835e3184a1e8762f11a0da9.png)

新创建的 LICENSE 文件在项目主目录中突出显示

## 步骤 7：加载文档

现在所有文件已创建并更新，我们使用下面的便捷命令加载文档。此命令将使用 `data.R` 文件创建文档，该文件中添加了数据的描述。文档被放置在一个新创建的文件夹 `man` 中，代表“manual”。

```py
devtools::document()
```

![](../Images/f70fc27979fbab8fbfc0c724c1e3c7e7.png)

新创建的 “man” 文件夹在项目主目录中突出显示

一旦运行文档命令，你可以使用帮助命令 `?richmondway`，它应打开包文档。确保文档清晰，`data.R` 文件中的必要细节按预期显示。

## 步骤 8：检查

你现在可以通过运行下面的命令来测试一切是否顺利运行并正确创建。该命令执行广泛的检查，以确保你的包的一致性和有效性。

```py
devtools::check()
```

`devtools::check()` 的输出会给出 NOTES、WARNINGS 和 ERRORS，每个都需要不同程度的关注：

错误：必须立即修复，因为它们表明存在重大问题。

警告：应予以解决，以确保功能性和 CRAN 合规性。

注意事项：提供有用的建议和提示，有时需要在 CRAN 提交时处理。

![](../Images/5c90468ad2f2abafdcf3d35d8cb5aef3.png)

## 步骤 9：在本地安装包并测试

以下来自 `devtools` 的命令可用于在本地安装包。然后，使用您在 `data.R` 文件中分享的相同方法来测试数据访问。

```py
devtools::install() # Install the package locally
data(richmondway) # Access the data through the package
```

## 第 10 步。将您全新的包发布到 GitHub

现在我们需要初始化我们的 Git 仓库，并使用一些更方便的 `usethis` 命令将包推送到 GitHub。在运行这些命令之前，请确保您拥有一个 GitHub 账户，并且已为 GitHub 设置了 SSH 密钥或个人访问令牌（PAT）。

```py
usethis::use_git() # Git integration
usethis::use_github() # Github integration
```

> usethis::use_git() 是做什么的

+   **初始化 Git：** 此功能在您的项目中初始化一个新的 Git 仓库。

+   **第一次提交**：它使用项目的当前状态进行初始提交。

> usethis::use_github() 是做什么的

+   **GitHub 仓库创建：** 此功能帮助创建一个新的 GitHub 仓库，并将您的本地 Git 仓库连接到远程 GitHub 仓库。

+   **身份验证**：帮助设置与 GitHub 的身份验证。它会检查您是否已通过 GitHub 进行身份验证。如果没有，它可能会提示您进行身份验证。

+   **推送：** 将您的本地 git 提交推送到远程 GitHub 仓库。

## 第 11 步：最后与世界分享您的包

现在您可以分享您的包仓库链接。任何人都可以直接从 GitHub 使用 `devtools::install_github(“your_username/packagename”)` 安装您的包并访问数据。

例如，可以使用以下命令访问我 GitHub 仓库包中的 `richmondway` 数据：

```py
devtools::install_github("deepshamenghani/richmondway")
```

# 你做到了！

如果您已经到达这一点，恭喜您！您刚刚把一个有趣的狂欢观看时光变成了既有教育意义又令人愉快的经历。

随意玩弄这个有趣的数据集，并在您的分析和可视化中标记我。或者，您可以在 GitHub 上 fork `[richmondway](https://github.com/deepshamenghani/richmondway)` 并为 Roy Kent 词汇表做贡献。继续打包吧！

# 资源

1.  [Richmondway package](https://github.com/deepshamenghani/richmondway) 仓库

1.  [R Packages](https://r-pkgs.org/) 由 Hadley Wickham 和 Jennifer Bryan 编写

1.  [Roxygen2](https://roxygen2.r-lib.org/articles/roxygen2.html) 文档

1.  [Usethis](https://usethis.r-lib.org/) 包

1.  [Devtools](https://devtools.r-lib.org/) 包

快乐编码！如果愿意，可以在 [Linkedin](https://www.linkedin.com/in/deepshamenghani/) 上找到我。

所有图片和屏幕录像，除非另有说明，均由作者提供。
