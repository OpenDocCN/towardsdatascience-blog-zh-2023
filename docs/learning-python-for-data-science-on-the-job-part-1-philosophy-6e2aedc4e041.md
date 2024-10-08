# 在职学习 Python 数据科学 第一部分：哲学

> 原文：[`towardsdatascience.com/learning-python-for-data-science-on-the-job-part-1-philosophy-6e2aedc4e041`](https://towardsdatascience.com/learning-python-for-data-science-on-the-job-part-1-philosophy-6e2aedc4e041)

## 关于如何在没有正式教育的情况下成为一名熟练数据科学家的实用建议和理念

[](https://nrlewis929.medium.com/?source=post_page-----6e2aedc4e041--------------------------------)![Nicholas Lewis](https://nrlewis929.medium.com/?source=post_page-----6e2aedc4e041--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6e2aedc4e041--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6e2aedc4e041--------------------------------) [Nicholas Lewis](https://nrlewis929.medium.com/?source=post_page-----6e2aedc4e041--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6e2aedc4e041--------------------------------) ·8 分钟阅读·2023 年 1 月 14 日

--

![](img/f6156bd577abc49b42723a0ea50db58b.png)

图片由 [Fatos Bytyqi](https://unsplash.com/@fatosi?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

随着数据科学在各个行业中的普及，值得注意的是，许多自学成才的程序员已经进入了职场。很多人正在寻求职业转型，希望用数据科学这一强大工具来补充自己的专业知识，或者只是对所有的炒作感到好奇。尽管我回到学校接受了一些正式的数据科学教育，但在本科阶段我从未参加过编程课程，在回到学校之前我已经在数据科学领域工作了 2 年；我绝对会认为自己是一个自学成才的程序员！自从我开始使用 Matlab 和 Python 探索系统生物学应用中的优化技术以来，已经快 10 年了。回顾过去，我想分享一些我尝试过的东西——有些成功了，有些失败了——这些都帮助我在工作中学习 Python（和 Matlab）编程以及数据科学。

本文是关于在没有正式教育情况下学习数据科学的两部分系列文章中的第一部分。第一部分是我经验的反思，提供了一些实践建议，包括尝试和可能需要避免的事项（你会发现我不喜欢 YouTube 教程！）。[第二部分](https://nrlewis929.medium.com/learning-python-for-data-science-on-the-job-part-2-practice-b4ece80488da) 通过一个简单的线性回归问题，分别在 Excel 和 Python 中进行操作，讲解了一些基本的 Python 语法，并比较了每种方法的优缺点。希望你喜欢！

## 入门指南

我仍然清楚地记得我作为新生研究助理的第一周，周围都是以代码为生、为梦的研究生。我不明白为什么有人能理解像`print('Hello World!')`这样的简单代码。这是为什么？我究竟要如何做更多高级的事情？

我很快意识到，编程基本上是用特定的词汇和结构表达的逻辑思想，以产生结果。它与在计算器中输入内容是一样的，只是更强大。唯一困难的部分是学习语法，即将你脑中的逻辑转换为计算机可执行任务的关键字和字符。

这是我学习语法时的一些有效方法：

+   Google 是你最好的朋友！要学习如何做一个`for`循环，只需 Google “Python for loop example。”你可能会看到一些很好的教程，详细讲解使用哪些关键字和字符。随着你问题的深入，你会发现 Stack Overflow，这是一个人们提出问题并从社区获取答案的论坛。你会看到很多创造性的方法来做同样的事情，并最终培养对常见做法的敏感性。

+   从你熟悉的事物开始对待 Python。例如，我们可能都用过计算器，因此首先就像使用计算器一样输入方程式。然后开始增加复杂性，比如分配变量、打印结果和提高计算复杂性。本系列第二部分的练习是一个很好的深入案例研究。

+   学会阅读文档。不幸的是，没有一个好的标准来规范人们编写文档的方式，有些文档写得非常差。但是每当你开始使用一个新包（如 pandas 或 numpy）时，如果你对如何做某事有疑问，首先查看它们的文档，然后再 Google 查找答案。

关于入门的其他几点：

+   确保对你的代码进行注释！这可能是你应该学习的第一个语法。Google “how to comment code in Python。”注释是不会被执行的代码行，所以你可以解释你写代码的逻辑和理由。不时回顾你早期的代码，认识到你的进步。

+   熟练调试。这可以简单到添加`print`语句，以便你能看到程序正在做什么，并检查是否符合你的预期。

+   阅读！大量阅读！当我开始我的数据科学工作时，我对 Python 生疏，并且从未进行过数据科学。所以我每天工作开始的第一个小时都会阅读教程并进行副项目实践。这对于我了解有哪些选项非常有价值。在阅读的同时，考虑你所阅读的内容如何应用到你的工作项目中。我发现了很多创新方法的灵感，这些灵感是通过阅读中的想法激发的（例如，参见我关于模拟 PID 控制器的系列）。

+   有很多平台可以运行 Python 代码。目前，对于初学者（以及那些尝试新想法的高级用户）来说，Jupyter notebook 是最好的选择。如果你在课堂上学习，你的老师很可能会使用 Jupyter notebook，因为它更直观，并且具有简单文本文件所没有的内置功能。

## 使用入门教程和材料

在我看来，一般的教程或 YouTube 视频不值得花时间。它们对我不起作用，而且过于笼统，实际上并不具备帮助性。如果你完全是编程新手，它们*可能*对你了解语法的运作方式有些帮助。如果你使用这些教程，你必须积极参与，边看边写代码。我们大多数人无法仅仅通过观看和吸收来有效学习……你可能会观看一个 4 小时的教程，但如果你没有积极参与，教程结束后你可能不会成为一个更好的程序员。

我通过沉浸在我关心的项目中来学习编程。我会获取一些起始代码，然后逐行阅读以理解它。我会添加`print`语句来更好地了解发生了什么。接着，我会开始进行一些小的调整和定制。即使现在，我几乎从不从头开始编写代码；而是会找到一些类似的代码（通常是我之前写过的，但作为起点，可以从在线资源开始），复制粘贴，然后根据我的特定项目进行定制。

从一个项目开始而不是教程有两个好处：首先，它给你一个你关心的任务，因此你会更投入（Towards Data Science 对于寻找项目演练非常好）。这也可能与你以后工作中要做的事情更相关。其次，它给你提供了定制代码以实现你想要的功能的机会，而不是仅仅生成数百万已经完成的代码。就像做一个有答案的学校作业和做一个没有单一正确答案的开放式工作项目之间的区别。你将能够认识到完成同一任务的不同方式。最终，你会开始认识到更高效的编码方法，但现在，更重要的是多加练习。

## 从基础开始

简单的事实是，没有人能在一夜之间成为 Python 大师。即使经过 10 年，我仍在不断学习。但是，有一件事真的很有帮助，那就是先从代码中的基本功能开始，然后再增加复杂性。这不仅是初学者编码的好框架，对高级编码者也是如此。实际上，当我最近进行[求职](https://my-data-science-job-search-6deb4117e7b5)时，从一次编程挑战中得到的最有价值的反馈之一就是在尝试解决边界情况之前，先从基本功能开始。想到如何编写一个完美工作的整个脚本可能会让人感到无从下手（我可能会争辩说，这几乎是不可能的），但只需稍加练习，你就可以编写一个完全正常的`for`循环或字典来完成大部分任务。

我说的是什么意思？考虑一个经典的编程挑战——[将罗马数字转换为整数](https://www.geeksforgeeks.org/python-program-for-converting-roman-numerals-to-decimal-lying-between-1-to-3999/)。有一些非常合理的规则，但与其试图一次性考虑所有规则，不如从基础开始，然后增加复杂性。首先编写一些代码，将每个罗马数字翻译为整数（例如 X = 10）。然后弄清楚如何编程处理一个较大值后跟一个较小值的情况（例如 XI = 11）。最后，编写代码处理一个较大值前面有一个较小值的情况（例如 IX = 9）。

## 发展自信

无论是教别人一门语言、一种乐器，还是任何其他技能，我总是告诉学生这一点：要有自信！你不是*仅仅*在学习小提琴，你是在演奏它。你不是*仅仅*在学习俄语，你是在说它。编程也是如此：你不是*仅仅*在学习编程，你已经在做了。当然，你在不断发展和学习，也许在不同的技能水平上。但即使作为一个初学的小提琴手，我仍然会说我在演奏小提琴，尽管那时的水平远不如今天。编程也是一样。虽然我比 10 年前知道的更多，10 年后我会比现在知道得更多，但这并没有减少我现在作为程序员的事实，就像我在 10 年前开始时一样。这种心态的关键在于它让你有信心去展现自己，去解决困难的问题（或说语言或演奏乐器）。我们通过实践、挑战自己、以及拥有自信来学习。是的，你绝对应该尽可能多地寻求反馈，但不要因为害怕犯错而感到瘫痪。不要因为不知道如何做而给自己找借口*不*去接受任务。整个要点在于你正在学习，甚至很可能是在工作中学习。

## 案例研究和结论

当我开始我的第一份数据科学工作时，我给自己设定了一个目标：我想比我对 Excel 的了解更深入地掌握 Python。我在 Excel 方面非常拿手，但即使是使用 VBA，也确实开始遇到一些限制。我们中的大多数人对 Excel 很熟悉，甚至是专家；第二部分的案例研究是一个绝佳的机会，让你通过在 Excel 和 Python 中解决问题来初步涉猎。我希望你不仅能够看到它们的相似之处和优缺点，还能建立起自信。

希望这些对你有所帮助。我知道我们每个人的学习方式不同，你可能会尝试 YouTube 教程或虚拟课堂，并觉得它们是最有价值的。我要表达的观点是，你*可以*在工作中学习这一宝贵的技能，并且培养出与那些接受过正式编程教育的同行一样的信心。像往常一样，你可以通过[LinkedIn](https://www.linkedin.com/in/nicholas-lewis-0366146b/)与我联系，也可以随时关注我在[Towards Data Science](https://nrlewis929.medium.com/)上的数据科学案例研究定期更新。
