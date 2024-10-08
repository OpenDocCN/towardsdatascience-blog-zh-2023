# Tidyverse 与 Base-R：如何为你选择最佳框架

> 原文：[`towardsdatascience.com/tidyverse-vs-base-r-how-to-choose-the-best-framework-for-you-29b702bdb384`](https://towardsdatascience.com/tidyverse-vs-base-r-how-to-choose-the-best-framework-for-you-29b702bdb384)

## R 编程最流行方法的优缺点

[](https://roryspanton.medium.com/?source=post_page-----29b702bdb384--------------------------------)![Rory Spanton](https://roryspanton.medium.com/?source=post_page-----29b702bdb384--------------------------------)[](https://towardsdatascience.com/?source=post_page-----29b702bdb384--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----29b702bdb384--------------------------------) [Rory Spanton](https://roryspanton.medium.com/?source=post_page-----29b702bdb384--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----29b702bdb384--------------------------------) ·阅读时间 7 分钟·2023 年 2 月 27 日

--

![](img/bae5db2308636e79c69003895db7e8fd.png)

[照片来自 Pexels 的 Chris Wynn](https://www.pexels.com/photo/two-deer-clash-antlers-6402320/)

程序员是充满热情的人。他们会就自己喜欢的语言和框架展开热烈的辩论（也就是激烈的争吵），捍卫自己偏爱的做法免受批评。在 R 编程社区中，其中一个最大的争论点就是选择两个框架之一：Base-R 和 tidyverse。

Base-R 指的是 R 编程语言中自带的所有功能。[tidyverse](https://www.tidyverse.org/) 是一个扩展 R 的包集合，拥有自己在数据分析方面的理念和立场。两者都非常受欢迎，人们对哪个更好争论不休。

Base-R 爱好者指责 tidyverse 用户不是“真正的程序员”的推文似乎成了年度惯例。这场争论有点激烈。

![](img/7e3995f5d3b241c20bf524e67939be73.png)

有些人对 R 编程非常认真。截图来自 Twitter（作者编辑）

从我的角度来看，这种对立被过分夸大了。我认为这两种方法只是不同的工具集，应该根据你的需求来选择使用。

在这篇文章中，我将考虑五个问题，以帮助你在 tidyverse 和 Base-R 之间做出选择。根据你的情况，我还会给出我对哪个框架更适合你的判断。

# 1\. 哪个更容易使用？

就像木匠不会用黄油刀来修剪地板一样，当使用 R 时，你应该选择合适的工具。尽管 Base-R 和 tidyverse 提供了大致相同的功能，但在某些方面，一个方法要比另一个更容易使用。

例如，tidyverse 通常是快速且简单的数据操作的最佳选择。通过许多变量对数据集进行分组以创建汇总统计，使用像 dplyr 这样的包比使用 Base-R 函数要容易得多。

[](/going-beyond-group-by-how-to-group-data-in-r-af30f19c1ff2?source=post_page-----29b702bdb384--------------------------------) ## 如何在 R 中分组数据

### 从初学者到高级用户，使用这些分组工作流

[towardsdatascience.com

然而，Base-R 更适合用于运行快速模拟等其他应用。根据你日常工作中 R 的使用情况，你的首选框架可能会有所变化。

在考虑可用性时，还值得考虑你的技能水平和编程背景。

初学者往往更喜欢 tidyverse，因为它比 Base-R 更易读。语法在函数之间一致，使得学习更容易，而且关键函数有描述性的名称，这使得阅读代码像是一组简单的指令。

话虽如此，一些经验丰富的程序员对此感到困惑，更喜欢 Base-R 的感觉。与 tidyverse 不同，Base-R 更加注重程序功能，这对来自其他语言的开发者来说更为熟悉。

# 2\. 哪个更快？

在进行计算开销较大的操作时，执行时间非常重要。在许多情况下，Base-R 和 tidyverse 之间的速度差异非常大。

举个例子，当 Base-R 速度明显更快时，我们可以使用内置于 R 的 mtcars 数据集。执行一个简单的操作，比如筛选数据集以仅显示六缸的汽车，Base-R 的速度比 tidyverse 快超过 40 倍！

```py
library(microbenchmark)
library(tidyverse)

results <- microbenchmark(mtcars %>% filter(cyl == 6), 
                          mtcars[mtcars$cyl == 6,])

summary(results) %>%
  as_tibble() %>%
  select(expression = expr, mean_execution_time = mean)
```

![](img/e2d920c74d7d4ac852e1b41cd58d557d.png)

当然，tidyverse 版本对初学者来说更具可读性，并且有其他优点。但是，如果你运行的脚本中需要重复执行该筛选操作数百次，那么 40 倍的性能提升将非常实用。

尽管在很多情况下 Base-R 的速度比 tidyverse 更快，[但有时情况](https://www.r-bloggers.com/2022/10/efficiency-comparison-of-dplyr-and-tidyr-functions-vs-base-r/)也可能正好相反。虽然 Base-R 通常在速度上占优，但根据具体情况进行检查是值得的。

# 3\. 我的合作者使用哪种工具？

尽管能够独立编写出色的代码很重要，但每个 R 用户的生活中都会遇到必须分享代码的时候。无论你是科学家、开发者还是数据分析师，让别人能够理解并使用你的代码至关重要。

在这里，你应该听取同事对 R 包的偏好。如果你合作的每个人都使用 tidyverse，那么考虑在某些情况下默认使用它以便于合作。同样地，如果他们都使用 Base-R。

与同事有共同的方法也有助于遇到问题或顽固的错误时。以个人经验为例，我在学习了 tidyverse 之后，与我以 tidyverse 为主的同事合作时要容易得多，尽管我在 R 学习的第二年才学会它。

这并不是说你必须根据合作伙伴的心情来限制自己使用 tidyverse 或 Base-R。即使我和我合作的大多数人默认使用 tidyverse，我偶尔也会为他们编写 Base-R 代码。不过，使用他们喜欢的方法作为基础是有帮助的。

# 4\. 哪个社区更易于接触？

除了合作学习外，学习 R 最好的事情之一是它附带的在线社区。很多人和组织分享 R 的技巧和更新，这可以帮助你改进代码。

对于 tidyverse 和 Base-R 爱好者来说，社区精神总是充足的。#RStats 是获取社交媒体技巧的好地方。还有很多博客，包括 Medium 上的，提供 Base-R 和 tidyverse 的技巧。

[## 如何快速学习 R 以进行数据科学](https://example.org/how-to-learn-r-for-data-science-fast-d47408d0becf?source=post_page-----29b702bdb384--------------------------------)

### 快速而免费地深入理解 R。

[towardsdatascience.com](https://example.org/how-to-learn-r-for-data-science-fast-d47408d0becf?source=post_page-----29b702bdb384--------------------------------)

对于 tidyverse 爱好者来说，每周的 [Tidy Tuesday](https://www.tidytuesday.com/) 活动强调使用 tidyverse 包创建惊艳的可视化。R for Data Science 社区也源于 Hadley Wickham 合著的同名开创性书籍，他是 tidyverse 的共同创建者。

许多忠实的 Base-R 爱好者历史上曾在论坛上聚集。尽管许多人也在社交媒体上，但在 Twitter 和 Mastodon 等平台上，tidyverse 似乎有更多的社区存在。根据你在线上花费的时间，你可以对这两种方法有很多了解。

# 5\. 哪个更适合软件开发？

虽然 tidyverse 很棒，但它在软件开发方面可能会有所不足。目前 tidyverse 中有超过 25 个包，每个包都需要自己的更新以保持最新。

如果你依赖这些技巧来编写自己的 R 包或其他软件，你的代码中可能会引入许多额外的依赖。虽然依赖额外的包不一定不好，但也不是理想的。

你的代码功能现在受其依赖的包的更新影响；这些更新是你无法控制的。依赖越多，重现你的环境以便他人运行你的代码就变得越困难。

如果你认真对待 R 开发并想向 CRAN 提交一个包，你会面临严格的依赖限制，这些限制是由于这些（和其他）原因。此情况下，tidyverse 包通常可能不适合。

相比之下，Base-R 不引入额外的依赖。问题解决。

# 你应该选择哪个：Base-R 还是 tidyverse？

综上所述，你应该选择哪一个——Base-R 还是 tidyverse？

两者都可以。

是的，这有点避重就轻。但说真的。了解这两种方法是扩展你的工具集并确保你能够处理 R 中所有类型任务的最佳方式。

话虽如此，许多程序员在日常工作中仍然专注于一种方法，并在需要时添加另一种方法的部分。以下是选择每种方法作为默认方法的一些理由。

## 如果你有以下情况，请将 tidyverse 设为你的默认方法：

+   大多数你的工作涉及数据清理、可视化和常见统计

+   你刚开始接触 R，觉得 Base-R 比 tidyverse 更容易阅读和理解

+   大多数你的合作者和在线网络也在使用它

## 如果你有以下情况，请将 Base-R 设为你的默认方法：

+   大多数你的工作涉及软件或包开发、先进的统计程序，或计算密集型操作

+   你习惯使用与 Base-R 更相似的其他语言

+   大多数你的合作者和在线网络也在使用它

这不是使用每个包的原因的详尽列表，但它们可以帮助你根据你的情况做出正确的选择。

作为心理学研究人员，我在进行大多数数据清理和简单分析时默认使用 tidyverse。然而，在进行更复杂的统计建模和模拟，或者当依赖性是个问题时，我会使用 Base-R。

最重要的是，我认为没有一种正确的方法。使用 tidyverse 并不会阻止你成为一个“真正的 R 程序员”，使用 Base-R 也不会阻止你编写整洁的代码。它们只是工具集，你可以用它们来做出很酷的 R 作品。

学习两者，混合搭配，使用适合工作的工具。

想要阅读我关于 R 编程、数据科学等的所有文章？请在 [这个链接](https://medium.com/@roryspanton/membership) 注册 Medium 会员，获取我所有文章的完整访问权限以及 Medium 上的其他故事。这也直接帮助我，因为我可以从你的会员费中获得一小部分贡献，而你无需额外支付费用。

你还可以通过 [订阅这里](https://roryspanton.medium.com/subscribe) 直接将我所有的新文章送到你的收件箱。感谢阅读！
