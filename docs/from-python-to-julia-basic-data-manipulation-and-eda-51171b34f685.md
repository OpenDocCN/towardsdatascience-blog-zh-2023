# 从Python到Julia：基础数据操作和探索性数据分析

> 原文：[https://towardsdatascience.com/from-python-to-julia-basic-data-manipulation-and-eda-51171b34f685?source=collection_archive---------13-----------------------#2023-06-20](https://towardsdatascience.com/from-python-to-julia-basic-data-manipulation-and-eda-51171b34f685?source=collection_archive---------13-----------------------#2023-06-20)

![](../Images/775edd50e2dd483b847e2e3422ad51eb.png)

图片由作者提供

[](https://medium.com/@wangshenghao1993?source=post_page-----51171b34f685--------------------------------)[![王晟豪](../Images/c59ca7f4fc77ca81f6b670ea5435ac19.png)](https://medium.com/@wangshenghao1993?source=post_page-----51171b34f685--------------------------------)[](https://towardsdatascience.com/?source=post_page-----51171b34f685--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----51171b34f685--------------------------------) [王晟豪](https://medium.com/@wangshenghao1993?source=post_page-----51171b34f685--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F75535ec0f14c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-python-to-julia-basic-data-manipulation-and-eda-51171b34f685&user=Wang+Shenghao&userId=75535ec0f14c&source=post_page-75535ec0f14c----51171b34f685---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----51171b34f685--------------------------------) ·7分钟阅读·2023年6月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F51171b34f685&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-python-to-julia-basic-data-manipulation-and-eda-51171b34f685&user=Wang+Shenghao&userId=75535ec0f14c&source=-----51171b34f685---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F51171b34f685&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-python-to-julia-basic-data-manipulation-and-eda-51171b34f685&source=-----51171b34f685---------------------bookmark_footer-----------)

作为一种新兴的统计计算编程语言，Julia近年来获得了越来越多的关注。有两个特点使得Julia在其他编程语言中更为优越。

+   Julia是一种高级语言，类似于Python。因此，它容易学习和使用。

+   Julia是一种编译语言，设计速度与C/C++一样快。

当我第一次了解Julia时，它的计算速度吸引了我。因此，我决定尝试Julia，看看是否能在日常工作中实际使用它。

作为数据科学从业者，我使用Python为各种目的开发原型ML模型。为了快速学习Julia，我将模拟我用Python和Julia构建简单ML模型的常规过程。通过将Python和Julia代码并排比较，我可以轻松捕捉到这两种语言的语法差异。这就是博客将按照以下章节安排的方式。

# 设置

在开始之前，我们需要先在工作站上安装Julia。Julia的安装包括以下两个步骤。

+   从[官方网站](https://julialang.org/downloads/)下载安装程序文件。

+   解压安装程序文件并创建指向Julia二进制文件的符号链接。
