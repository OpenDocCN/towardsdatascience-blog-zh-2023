# 这 7 个编程习惯让你成为一个低效的数据科学家

> 原文：[`towardsdatascience.com/these-7-programming-habits-are-making-you-a-less-productive-data-scientist-6d9767fd8ff3?source=collection_archive---------9-----------------------#2023-02-03`](https://towardsdatascience.com/these-7-programming-habits-are-making-you-a-less-productive-data-scientist-6d9767fd8ff3?source=collection_archive---------9-----------------------#2023-02-03)

## 修正这些习惯可以让你成为一个更高效的数据科学家

[](https://madison13.medium.com/?source=post_page-----6d9767fd8ff3--------------------------------)![Madison Hunter](https://madison13.medium.com/?source=post_page-----6d9767fd8ff3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6d9767fd8ff3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6d9767fd8ff3--------------------------------) [Madison Hunter](https://madison13.medium.com/?source=post_page-----6d9767fd8ff3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6a8c6841e521&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthese-7-programming-habits-are-making-you-a-less-productive-data-scientist-6d9767fd8ff3&user=Madison+Hunter&userId=6a8c6841e521&source=post_page-6a8c6841e521----6d9767fd8ff3---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----6d9767fd8ff3--------------------------------) ·9 分钟阅读·2023 年 2 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6d9767fd8ff3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthese-7-programming-habits-are-making-you-a-less-productive-data-scientist-6d9767fd8ff3&user=Madison+Hunter&userId=6a8c6841e521&source=-----6d9767fd8ff3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6d9767fd8ff3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthese-7-programming-habits-are-making-you-a-less-productive-data-scientist-6d9767fd8ff3&source=-----6d9767fd8ff3---------------------bookmark_footer-----------)![](img/745feeba3d2e35ac0d0ccd075f510ce9.png)

图片来源于[Rémi Jacquaint](https://unsplash.com/@jack_1?utm_source=medium&utm_medium=referral)的[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我相信在我们共同的数据科学之路上，大家至少都曾经犯过其中一个这些坏习惯。

无论我们是在刚开始学习时做这些注释，还是因为我们已经比较擅长而变得懒惰时做这些注释，我们都曾犯过这些编程错误。

无论出于何种原因，清理你的编程习惯，成为更高效的数据科学家永远不会太晚。幸运的是，你已经完成了最困难的部分，学会了编程。现在，我们只需要完善你的技巧，养成一些良好的习惯，让你在数据科学项目中享受更多乐趣。至少你今天学到的知识会让你避免被软件部门责备。

从软件开发学生的经验来看，这些习惯无疑会妨碍你发挥最佳生产力——让我们现在就改变这一点。

# 1\. 未对代码进行注释

对代码进行注释是代码文档的重要部分，确保以下三点：

1.  其他人能够理解你的代码。

1.  其他人能够维护你的代码。

1.  当你在一段时间后重新访问你的代码时，你能理解它。

至少，你的代码中必须有三种类型的注释——这些是不可协商的：

1.  第一点是对你个人或团队代码库中任何新提交或共享代码的注释。这个注释可以作为你的 Git 提交消息的一部分，但也应该出现在代码中，通常位于你刚刚提交的代码块上方。我喜欢通过在代码和注释周围创建虚线来进一步分隔这个代码段——只是为了更清晰的查看，仅此而已。这个注释应该描述代码的功能。

1.  第二点是对代码中每个函数的注释。该注释应直接位于每个函数的上方，并解释其输入、输出以及函数逻辑。

1.  最终的注释应该位于代码中任何单行代码的上方。单行代码描述了通常分散在几行代码中的逻辑，但却写成了一行。由于这些单行代码有时可能难以理解，因此一个描述其功能及每个部分如何工作的注释是保持组织性的好方法。

从与其他软件开发者的合作中，我看到将好代码与优秀代码区分开来的因素是代码注释的层次和细节。换句话说，简洁、准确、充分地注释你的代码。如果某些内容对你来说很明显，最好还是做个注释，以防对其他人（或将来你自己）不够明显。

# 2\. 未使用 GitHub 进行版本控制

现在已经没有理由让从事技术工作的人不使用 GitHub 了。GitHub 不仅是一个版本控制工具，而且还是一个生产力工具，帮助你轻松处理代码和与他人协作。

GitHub 通常被认为是黄金标准，大多数科技公司都通过它来进行代码的版本控制。即使你是一个在公司里工作的单独数据科学家，如果你将代码分享给一个将其转化为生产代码的软件部门，GitHub 也是一个重要的工具。

在某个时候，我们都曾宣布要*真正*学会如何使用 GitHub 一劳永逸。GitHub 具有许多有用的功能（包括跟踪代码的变化以及处理代码的旧版本），但说实话，数据科学家通常可以仅仅使用它来进行简单的主分支提交，配合几个分支以运行替代场景。就是这么简单。

好吧，这就是你真正开始使用它的标志——这次是认真的。

[](/comprehensive-guide-to-github-for-data-scientist-d3f71bd320da?source=post_page-----6d9767fd8ff3--------------------------------) ## 数据科学家的全面 GitHub 指南

### 数据科学家的 GitHub 教程，包括 UI 和命令行

[towardsdatascience.com

# 3\. 不测试你的代码

我们都经历过这种情况：因为害怕可能发生的事情而尽可能避免运行和测试代码。1% 的时候我们可能运气好，一切正常运行，但我可以保证，其他 99% 的时候，一切都会出问题。

从软件开发的教育背景中，我们经常接受测试的训练。不定期测试代码被视为一种罪过，我们很快就能做到。不仅帮助我们立即发现错误和缺陷，还意味着我们不需要筛查数百行代码来找出问题所在。

测试你的代码就像编写 [单元测试](https://www.guru99.com/unit-testing-guide.html) 一样简单，这种测试涉及检查函数、对象或类（单元）是否正常工作。进行单元测试的一种简单方法是根据你提供的输入打印函数的输出。

[](https://www.freecodecamp.org/news/how-to-write-unit-tests-for-python-functions/?source=post_page-----6d9767fd8ff3--------------------------------) [## 如何为 Python 函数编写单元测试

### 本指南将教你如何为 Python 函数编写单元测试。但你为什么应该考虑编写单元测试……

[www.freecodecamp.org](https://www.freecodecamp.org/news/how-to-write-unit-tests-for-python-functions/?source=post_page-----6d9767fd8ff3--------------------------------)

# 4\. 没有将复杂问题拆解为简单变量和函数

数据科学问题通常很复杂，涉及许多活动的部分。这些问题可能让人感到气馁，如果不将其拆解成简单的部分，可能会导致你盯着计算机屏幕发呆，直到该回家时却没有写下一行代码。

诀窍是 [以终为始地开始解决问题](https://www.franklincovey.com/the-7-habits/)，然后将其拆解成简单的变量和函数——因为实际上，代码就是由变量和函数组成的。

开始之前，你需要问自己这个问题试图解决什么，以及这个解决方案的结果将是什么。这将帮助你开始确定你需要哪些代码片段来达到最终目标。一旦你确定了解决方案的样子，你可以开始规划你需要的各个变量和函数，以实现它。

看，你已经把复杂的问题拆解成了可管理的任务！这感觉不是更好吗？

在将这些任务添加到你的看板上（我在处理复杂问题时保持组织的最爱方式）之后，你可以开始创建将导致完整解决方案的项目小部分。

# 5\. 不重构你的代码

[代码重构](https://en.wikipedia.org/wiki/Code_refactoring)是指在不改变原有功能的情况下，对代码进行重组。虽然重构通常出现在 软件开发场景中，但数据科学家也可以使用重构来清理他们的代码。

重构 比听起来要简单：查看一些你以前写的旧代码，问问它可以如何更有效地编写。然后，应用良好的编码实践，清理你的代码，直到它看起来比以前更好。

重构最好是在你编写了有效的代码之后进行。例如，当你刚开始处理一个 数据科学问题时，你要确保你的代码能正常工作，不管它是否优雅。然后，一旦你确保它产生了正确的输出，你可以回过头来明确变量名称，正确缩进代码，使用 Python 语法标准创建消除冗余的函数，并且总体上重写任何看起来像一堆意大利面的代码。

我建议在你从专注状态中出来并使代码正常工作之后再进行任何重构。每次写一行代码就停下来重构会让你脱离专注状态，并且使完成代码的时间延长十倍。就像建议在你把想法写到纸上之前不要担心拼写或语法一样，等到你写完所有的代码后再美化它。

# 6. 不保持代码的组织

在我大学学习软件开发期间，我经常遇到糟糕的组织技能。尽管我的许多同学都是优秀的开发者，但组织能力并不是他们的强项，这使得他们的代码在查看时留下了很多遗憾。

早期学习适当的代码组织技能可以帮助你创建易于导航和处理的代码，从而减少你推动项目的时间。

根据[Karl Broman](https://kbroman.org/steps2rr/pages/organize.html)的说法，代码和数据组织可以简单到如下几点：

+   **将项目的所有内容保存在一个目录中。** 该目录应包含项目的所有数据、代码和结果，这样将来在继续工作或交给他人时会更方便。

+   **将原始数据与派生数据分开。** 保持两个子目录，一个包含原始数据，另一个包含派生数据。包含数据总结的子目录也有助于保持数据的组织。

+   **将数据与代码分开。** 将代码放在一个子目录中，将数据放在另一个子目录中（或者像上面描述的那样，三个子目录）。

+   **避免使用绝对路径，改用相对路径：** 当与其他人合作时，他们可能没有在完全相同的位置复制你的项目目录，因此使用相对路径很重要，以便他们能够打开和访问你的所有文件。

+   **选择好的文件（以及变量和函数）名称：** 原始数据文件名应保持不变，但代码文件名应尽可能具有描述性。变量和函数名称也是如此。

+   **文件名中永远不要使用“final”：** 正如 Broman 所说，“没有什么是最终的”。多个版本的文件应附加版本号，但“最终”版本不应如此标记。

+   **编写文档和 README 文件。** 文档是解释所有内容及其功能的必要手段。好的文档包括描述文件及其包含的过程。保持 README 更新很重要，同时如果有人有进一步的问题，也要包含你的联系信息。

我在之前的项目中承担的任务之一是组织团队的代码。这不仅是一个学习代码如何工作的好方法，而且是成为熟练开发者的重要技能。建立一个适合你的组织系统（我的系统与上述描述的相似）并在每个项目中付诸实践，无论大小。

# 7. 不休息

我告诉你一个小秘密：当你不休息时，你写的代码往往很糟糕。

在技术领域，拼命文化依然存在，工作 90 小时的周数并不少见，你可能只会偶尔起身去加点饮料，几天不见太阳也很正常。

虽然你可能觉得老板支配了你，但他们并不支配你的健康（包括身体、心理和情感健康）。

所以我希望你现在就承诺每天至少进行一个小时的活动，每天至少喝两次除了苏打水以外的饮品（开始时可以选择水，咖啡不算），每天至少出去一次，并且尽量吃不是外卖的食物。

相信我，我记不清多少次是在遛狗的时候解决了编码或逻辑问题。新鲜空气似乎对大脑的解决问题部分有奇效。

每天坐在电脑前连续编写代码八小时或更长时间而没有任何休息，会使你感到精疲力竭，生产力也会比之前更低。换句话说，生活短暂，你的工作并不是一切，照顾好自己从长远来看会让你成为更优秀的数据科学家。

订阅以便直接将我的故事发送到你的收件箱：[故事订阅](https://madison13.medium.com/subscribe)

请通过我的推荐链接成为会员，以获得对 Medium 的无限访问（这对你没有额外费用，我会获得少量佣金）：[Medium 会员](https://madison13.medium.com/membership)

通过捐赠支持我的写作，帮助创造更多这样的故事：[捐赠](https://ko-fi.com/madisonhunter13)
