# 为你的下一个数据科学网页应用程序选择合适的工具

> 原文：[`towardsdatascience.com/choosing-the-right-tool-for-your-next-data-science-web-application-3d7ec8293d61`](https://towardsdatascience.com/choosing-the-right-tool-for-your-next-data-science-web-application-3d7ec8293d61)

## Flask、Django、Streamlit：数据科学家进入网页开发的三种选择。

[](https://murtaza5152-ali.medium.com/?source=post_page-----3d7ec8293d61--------------------------------)![Murtaza Ali](https://murtaza5152-ali.medium.com/?source=post_page-----3d7ec8293d61--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3d7ec8293d61--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3d7ec8293d61--------------------------------) [Murtaza Ali](https://murtaza5152-ali.medium.com/?source=post_page-----3d7ec8293d61--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3d7ec8293d61--------------------------------) ·6 分钟阅读·2023 年 1 月 31 日

--

![](img/3ab692f53196eb96db92fbd079ea7b14.png)

图片由 [Campaign Creators](https://unsplash.com/@campaign_creators?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

依靠现代的先进计算能力，我们可以比以往更好地利用数据。从你家里的舒适环境中，通过使用笔记本电脑和通过互联网获得的大量数据，设计和实现可能改变生活的技术变得可能。

但有一个问题。如果一个令人印象深刻的机器学习模型或有洞察力的数据可视化工具仅仅是作为代码文件放在你的电脑上，那它对任何人来说几乎没有用处。要真正产生影响，你需要能够以紧凑、可用的方式与其他人分享你的发明。

传统的方法是通过一个网页应用程序来实现。通过将你的数据科学工具转变为一个应用程序，并将其托管在一个任何人都可以通过互联网访问的服务器上，你可以立即让数以百万计的人使用你的工具。

不幸的是，这并不像听起来那么简单。构建一个网页应用程序是一个相当复杂的编程任务，涉及的技能超出了许多数据科学家的专长。从零开始编写一个确实是一项挑战。

幸运的是，许多公司从事*网页框架*的业务。网页框架本质上是一个广泛的库，它提供了构建网页应用程序的基础结构，使得入门变得非常容易。

在这篇文章中，我将讨论 Python 中三种成熟的 web 框架的优缺点：Flask、Django 和 Streamlit。如果你刚刚开始，可能很难知道该使用哪个工具，我希望能帮助你确定哪个最适合你的技能水平和总体目标。请注意，这篇文章*不*讨论如何实际使用这些工具构建应用程序的细节。

那我们开始吧？ 

## Flask

[Flask](https://flask.palletsprojects.com/en/2.2.x/) [1] 常常是介绍给新手 web 开发者的第一个框架。你常常会听到它被描述为极其轻量且如果你已经了解 Python，则相当容易学习。在这里，“轻量”一词意味着 Flask 没有任何外部依赖，它提供了构建 web 应用程序所需的最基本工具，而不会让你感到功能过多。

Flask 轻量得如此以至于常被称为“微型”框架。你可以从最简单的应用程序开始，根据需要选择更多功能。此外，它足够简单，如果你已经了解 Python，你不会觉得自己在学习一种全新的语言。

然而，这种简单性是把双刃剑。由于 Flask 轻量级，它也会为你自动完成较少的事情。换句话说，当你在构建 web 应用程序时，你将不得不自己解决许多问题。

我自己从未使用过 Flask，但我承认它已经被许多次推荐给我。在一般编程社区的眼中，它的好处似乎超过了成本。

让我们在继续之前回顾一下。

**优点**

+   极其轻量（在某种意义上是“微型”框架）

+   你可以选择所需的功能。

+   由于上述两个特性，学习起来可以说更容易。

**缺点**

+   内置功能不那么丰富。

+   由于上述原因，你需要自己实现 web 应用程序的许多方面。

## Django

这是我个人的框架选择。我最近才开始进行全面的 web 开发，之前只是使用了更简单的替代方案（见下文 Streamlit 部分）。我必须在 Django 和 Flask 之间做出选择，最终决定 Django 更适合我的需求。

[Django](https://www.djangoproject.com/) 是 Flask 的完全对立面，因为它相当沉重。它有一个巨大的功能集——当你构建一个 Django 项目时，你会得到所有这些功能，无论你是否需要。这有积极和消极的方面。

好的一面是，由于许多功能已经内置，你需要从头开始实现的东西会更少。一个很好的例子是 Django 管理页面。所有 Django 项目自动实现一个数据库管理员网页，允许一个“超级用户”添加和删除数据，而无需编写任何代码。如果你想为一个需要管理相关数据库但没有相应编程技能的团队设计一个网络应用，这可能会非常有用。

另一方面，项目庞大、复杂且可能让人感到威慑，因为你一次性获得了一切。最简单的 Flask 应用可以写在一个代码文件中，但任何 Django 项目在你开始时都会自动创建大量的目录和相关文件。

Django 也有非常详细的文档和由开发者亲自编写的[优秀教程](https://docs.djangoproject.com/en/4.1/intro/tutorial01/) [3]。对一些人来说，这是把双刃剑，因为开发者意见非常明确。因此，当你在 Django 中做某事时，你必须按照“Django”的方式去做。然而，如果网页开发不是你主要的编程强项，你只是想开始，那么这不太可能成为问题。

总之 —

**优点**

+   附带许多内置功能

+   Django 管理员是一个非常棒的附加功能，你无需额外工作即可获得

+   Django 提供了一整套由公司自行编写的详尽教程（确保信息准确并符合最佳实践）

**缺点**

+   非常庞大——你在 Django 项目中获得所有功能，无论你是否需要它们

+   Django 的开发者对网页开发有强烈的意见，这些意见反映在使用 Django 编程时必须遵循的方式上。如果你不同意这些意见，可能会发现 Django 很让人沮丧。

## Streamlit

> “[Streamlit](https://streamlit.io/) [4] 将数据脚本在几分钟内转换为可分享的网页应用。
> 
> 完全使用 Python 编写。不需要前端经验。”

上述引言直接摘自 Streamlit 的首页，并且是一个相当扎实的总结。Streamlit 最初的设计目的是为了让缺乏网页编程技能的数据科学家能够轻松地将他们的数据科学工具部署到互联网上。

这是 Streamlit 的最大优势。你可以用 Python 编写你的数据科学工具，对照 Streamlit 的规范进行一些语法上的调整，添加一些头部代码行，然后你的应用程序就准备好了！至少在本地是这样的——不过 Streamlit 也提供了云支持作为下一步。

Streamlit 还具有一些不错的内置功能（滑块、按钮和其他小部件），可以用来为你的应用程序提供良好的提升。

不过，Streamlit 也有两个缺点。显而易见的一个是你实际上被限制在 Streamlit 的默认界面中，因此无法真正自定义应用程序的外观。Streamlit 并不是为了个性化设计的；它只是提供了一种快速发布工具并在需要时收集数据的方式。

此外，Streamlit 在处理大数据集时也存在一些问题。根据我的经验（以及我合作过的其他人的经验），执行潜在的高成本计算过程（例如渲染可视化）在 Streamlit 中可能会有一些延迟。这种经验得到 [Streamlit 论坛讨论](https://discuss.streamlit.io/t/whether-streamlit-can-handle-big-data-analysis/28085/15) [5] 的支持。

总的来说，Streamlit 学习快且容易，但灵活性远不如 Flask 和 Django。

**优点**

+   为数据科学家设计

+   需要最低限度的网页开发知识

+   可以极其快速地构建和部署网页应用

**缺点**

+   在自定义网页应用方面几乎没有自由度

+   对于大数据集或高成本操作可能存在效率问题

## 最终想法

选择正确的框架既是个人偏好的问题，也是你具体需求的考虑。我无法了解你的偏好，因此我会简要地谈谈第二点以作结。

想快速测试一个工具，却没有太多时间来组装它？Streamlit 可能是你的最佳选择。如果你想学习网页开发并提升 Python 技能，选择 Flask 或 Django 会更合适。这两者都能很好地处理大型复杂应用，因此个人偏好在此时显得尤为重要。

与所有编程一样，从一篇文章中只能获得有限的信息。在此，我鼓励你亲自尝试每一种工具。我很想听听你的想法和反思，请在评论中分享。

祝你网页开发愉快！

**想要在 Python 中脱颖而出？** [**点击这里获取我简单易读的免费指南**](https://witty-speaker-6901.ck.page/0977670a91)**。想在 Medium 上阅读无限故事？请通过下面的推荐链接注册！**

[](https://murtaza5152-ali.medium.com/?source=post_page-----3d7ec8293d61--------------------------------) [## Murtaza Ali - Medium

### 阅读 Murtaza Ali 在 Medium 上的文章。他是华盛顿大学的博士生，兴趣涉及人机交互…

murtaza5152-ali.medium.com](https://murtaza5152-ali.medium.com/?source=post_page-----3d7ec8293d61--------------------------------)

## 参考资料

[1] [`flask.palletsprojects.com/en/2.2.x/`](https://flask.palletsprojects.com/en/2.2.x/)

[2] [`www.djangoproject.com/`](https://www.djangoproject.com/)

[3] [`docs.djangoproject.com/en/4.1/intro/tutorial01/`](https://docs.djangoproject.com/en/4.1/intro/tutorial01/)

[4] [`streamlit.io/`](https://streamlit.io/)

[5] [`discuss.streamlit.io/t/whether-streamlit-can-handle-big-data-analysis/28085/15`](https://discuss.streamlit.io/t/whether-streamlit-can-handle-big-data-analysis/28085/15)
