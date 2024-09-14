# 想成为更好的数据科学家吗？写编程教程！

> 原文：[`towardsdatascience.com/want-to-be-a-better-data-scientist-write-coding-tutorials-9c1312897633`](https://towardsdatascience.com/want-to-be-a-better-data-scientist-write-coding-tutorials-9c1312897633)

## 提升你的技术和沟通技能，学习编写可读的代码，并开始写关于任何话题的文章。

[](https://conorosullyds.medium.com/?source=post_page-----9c1312897633--------------------------------)![Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----9c1312897633--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9c1312897633--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9c1312897633--------------------------------) [Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----9c1312897633--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----9c1312897633--------------------------------) ·阅读时间 5 分钟·2023 年 4 月 15 日

--

![](img/af00521333624dd1d6b994ffd1e2282e.png)

[Mimi Thian](https://unsplash.com/@mimithian?utm_source=medium&utm_medium=referral)拍摄的照片，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上。

当我刚开始写作时，我几乎不会编程。我的一点 Python 经验来自大学项目。与此同时，我认为自己是一个出色的写作者！现在回头看，我对早期的一些文章感到羞愧。

这使我意识到，写教程提升了我的**技术**和**沟通技能**。最终，它让我成为了一个更好的数据科学家。我想分享我的经验，并讨论一些具体的好处。这包括编写可读的代码，并提供写作困难话题的第一步。

# 编程

技术技能对开发人员和数据科学家显然很重要。同时，我们的工作领域不断变化。这意味着我们需要跟上最新的工具。这可能既困难又耗时，因此需要真正的承诺才能不断学习。

> 写作激励我不断学习新的技术技能，并对这些主题进行深入了解。

写作是一种很好的激励方式。我不仅可以学习新东西，还可以将这些经验分享给世界。这种感觉令人上瘾——有人阅读和互动你的作品。不断发布新文章的愿望促使我不断学习新知识。

不仅如此，我发现写作是*最好的*学习方式。通常，只有在尝试解释某些东西之后，我才意识到自己实际上并不理解它。此外，我总是力求为一个主题增加价值。引入新颖性需要良好的理解。通过这种方式，写作成为了我知识的检验。这推动我真正理解我所写的技术概念。

## 编写可读的代码

一个相关的技能是编写可读的代码。好的代码应该自我解释。同样，基于可读代码的教程更容易编写。我花在解释代码做了什么的时间更少，而且通常你根本不需要解释。我发现这种好处扩展到了行业合作中。

> *任何傻瓜都可以写出计算机能理解的代码。优秀的程序员编写人类能理解的代码。*
> 
> *— 马丁·福勒*

一个例子来自我最近的一篇文章——[使用 SHAP 调试 PyTorch 图像回归模型](https://medium.com/towards-data-science/using-shap-to-debug-a-pytorch-image-regression-model-4b562ddef30d)。在教程中，我处理了一个 5 维数组中的 SHAP 值。我想用包的一个函数来可视化这个数组。为此，数组首先需要重新排序。最初，代码是这样的：

```py
# Reshape shap values for plotting
shap_numpy = [np.swapaxes(np.swapaxes(s, 1, -1), 1, 2) for s in shap_values]
```

嵌套的**swapaxes**函数解释起来并不有趣。简而言之，代码将数组的第 3 维与第 5 维进行了转置。我发现自己不得不写一大段文字来解释代码是如何做到这一点的。因此，我重新编写了代码：

```py
# Reshape shap values for plotting
shap_numpy = list(np.array(shap_values).transpose(0,1,3,4,2))
```

转置函数更直观，代码也更容易解释。当为教程编写代码时，我总是寻找这样的例子。可读的代码使得写作过程更加顺畅，教程也更易于理解。

我发现这个技能在与其他专业人士合作时特别有用。你无法逃避交接和代码审查。在这些过程中解释代码与在在线教程中解释代码没有区别。*最终*，编写可读的代码意味着我花在向同事解释代码上的时间更少。

# 写作

当我刚开始工作时，我迅速意识到非技术技能也同样重要。你不仅需要取得结果，还需要解释这些结果和你的过程。我必须每天撰写电子邮件、演示文稿或技术文档。没有办法逃避写作！

> …传统上，不同的人在职业生涯中选择不同的路径——有些人更偏向技术性，有些人则更具创意和沟通能力。数据科学家必须兼具这两者。
> 
> ―莫妮卡·罗加提

编写编码教程是很好的实践。写得越多，我在工作中的沟通能力就越强。复杂的分析变得更容易解释，我需要澄清的工作也少了。我迅速看到了明显的好处。然而，这只是开始。

## 使用教程作为写作的起点

与其他文章相比，编程教程更容易编写。没有深奥的思考或细致的论证。你只需解释代码。这意味着它们可能是进入更复杂写作的一种方式。

写作是需要练习才能变得擅长的。教程帮助我迈出了第一步。我开始尝试在说明和结论中添加吸引人的故事。通过这些磕磕绊绊的过程，我开始转向不同类型的文章。

在更加自信的步伐下，我开始撰写基于数据科学家经验的文章。这些文章基于更抽象的概念、我的个人经历和感受。我发现这些比任何编程教程都难写。然而，如果没有那些最初的教程，我不会达到现在的水平。

回顾过去，编写编程教程带来了许多好处。我提高了技术知识，学会了编写可读的代码。编写教程还让我感到自信，可以写几乎任何内容。我还没有提到对我的声誉和财务补偿的好处。

所以，如果你对写作感兴趣，编程教程是一个很好的起点。如果你在想法上挣扎，你可能会觉得这篇文章很有用：

[## 永不枯竭的文章主题：5 个可靠来源](https://writingcooperative.com/never-run-out-of-article-topics-5-reliable-sources-fb161a77de3e?source=post_page-----9c1312897633--------------------------------)

### 回顾过去的项目、你想学习的东西以及你的职业和个人经历

[writingcooperative.com](https://writingcooperative.com/never-run-out-of-article-topics-5-reliable-sources-fb161a77de3e?source=post_page-----9c1312897633--------------------------------)

希望你喜欢这篇文章！你可以通过成为我的[**推荐会员**](https://conorosullyds.medium.com/membership)来支持我**:)**

[## 通过我的推荐链接加入 Medium — Conor O’Sullivan](https://conorosullyds.medium.com/membership?source=post_page-----9c1312897633--------------------------------)

### 作为 Medium 会员，你的部分会员费会分配给你阅读的作者，并且你可以完全访问每一个故事…

[conorosullyds.medium.com](https://conorosullyds.medium.com/membership?source=post_page-----9c1312897633--------------------------------)

| [Twitter](https://twitter.com/conorosullyDS) | [YouTube](https://www.youtube.com/channel/UChsoWqJbEjBwrn00Zvghi4w) | [Newsletter](https://mailchi.mp/aa82a5ce1dc0/signup) — 免费注册以获得[Python SHAP 课程](https://adataodyssey.com/courses/shap-with-python/)的访问权限
