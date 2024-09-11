# Tidyverse 与 Base-R：如何选择最适合你的框架

> 原文：[https://towardsdatascience.com/tidyverse-vs-base-r-how-to-choose-the-best-framework-for-you-29b702bdb384?source=collection_archive---------3-----------------------#2023-02-27](https://towardsdatascience.com/tidyverse-vs-base-r-how-to-choose-the-best-framework-for-you-29b702bdb384?source=collection_archive---------3-----------------------#2023-02-27)

## R编程中最流行方法的优缺点

[](https://roryspanton.medium.com/?source=post_page-----29b702bdb384--------------------------------)[![Rory Spanton](../Images/6c35a3de7cb516aac09bc5cf417a6c70.png)](https://roryspanton.medium.com/?source=post_page-----29b702bdb384--------------------------------)[](https://towardsdatascience.com/?source=post_page-----29b702bdb384--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----29b702bdb384--------------------------------) [Rory Spanton](https://roryspanton.medium.com/?source=post_page-----29b702bdb384--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39501aa8ce39&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftidyverse-vs-base-r-how-to-choose-the-best-framework-for-you-29b702bdb384&user=Rory+Spanton&userId=39501aa8ce39&source=post_page-39501aa8ce39----29b702bdb384---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----29b702bdb384--------------------------------) ·7分钟阅读·2023年2月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F29b702bdb384&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftidyverse-vs-base-r-how-to-choose-the-best-framework-for-you-29b702bdb384&user=Rory+Spanton&userId=39501aa8ce39&source=-----29b702bdb384---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F29b702bdb384&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftidyverse-vs-base-r-how-to-choose-the-best-framework-for-you-29b702bdb384&source=-----29b702bdb384---------------------bookmark_footer-----------)![](../Images/bae5db2308636e79c69003895db7e8fd.png)

[由 Chris Wynn 提供的照片](https://www.pexels.com/photo/two-deer-clash-antlers-6402320/)

程序员是充满激情的人。他们会就自己喜欢的语言和框架展开热烈的讨论（也就是激烈的争论），为自己钟爱的方式辩护。在R编程者中，一个最大的争论点是选择两个框架中的一个；Base-R和tidyverse。

Base-R 指的是所有内置于 R 编程语言中的功能。[tidyverse](https://www.tidyverse.org/) 是一组附加到 R 上的包，拥有自己的理念和数据分析立场。两者都非常受欢迎，人们对哪个更好争论不休。

Base-R 的粉丝在推特上指责 tidyverse 用户不是真正的“程序员”的情况似乎每年都会发生一次。局面有点激烈。

![](../Images/7e3995f5d3b241c20bf524e67939be73.png)

有些人对整个 R 编程非常认真。来自 Twitter 的截图（作者编辑）

在我看来，这种竞争被夸大了。我认为这两种方法只是不同的工具集，你应该根据自己的需要来使用。

在这篇文章中，我会考虑五个问题，帮助你在 `tidyverse` 和 Base-R 之间做出选择……
