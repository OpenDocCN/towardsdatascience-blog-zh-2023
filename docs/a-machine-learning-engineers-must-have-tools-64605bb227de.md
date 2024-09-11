# 机器学习工程师的必备工具

> 原文：[https://towardsdatascience.com/a-machine-learning-engineers-must-have-tools-64605bb227de?source=collection_archive---------4-----------------------#2023-10-19](https://towardsdatascience.com/a-machine-learning-engineers-must-have-tools-64605bb227de?source=collection_archive---------4-----------------------#2023-10-19)

## 从技术和生产力两个方面

[](https://medium.com/@cereniyim?source=post_page-----64605bb227de--------------------------------)[![Ceren Iyim](../Images/5a774ece76cff16b65cdf4fcfe01eb06.png)](https://medium.com/@cereniyim?source=post_page-----64605bb227de--------------------------------)[](https://towardsdatascience.com/?source=post_page-----64605bb227de--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----64605bb227de--------------------------------) [Ceren Iyim](https://medium.com/@cereniyim?source=post_page-----64605bb227de--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F287e9909d3b5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-machine-learning-engineers-must-have-tools-64605bb227de&user=Ceren+Iyim&userId=287e9909d3b5&source=post_page-287e9909d3b5----64605bb227de---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----64605bb227de--------------------------------) ·4 min read·2023年10月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F64605bb227de&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-machine-learning-engineers-must-have-tools-64605bb227de&user=Ceren+Iyim&userId=287e9909d3b5&source=-----64605bb227de---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F64605bb227de&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-machine-learning-engineers-must-have-tools-64605bb227de&source=-----64605bb227de---------------------bookmark_footer-----------)![](../Images/ecd4d387ac807736e75943b74a1fe68d.png)

图片由 [JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

大约四年前，我将职业从 [SAP顾问转为数据科学家](/why-i-decided-to-become-a-data-scientist-eec6f8cd435e)。在遵循我为自己设计的课程之后，我在一年内获得了一家初创公司的机器学习工程师职位。

将我过去四年所学到的知识、使用过的工具和经历凝结成一篇文章并不容易。不过，我会重点介绍那些特别对我有帮助的内容。

随着我角色的进展，我通过使用各种工具和在工作中遵循一些最佳实践来培养软件开发技能：

1.  Git与版本控制

1.  编写可读且清晰的代码

1.  探索不同的开发工具

我不仅会在本文中解释它们，还会提到它们如何帮助我提高软技能和生产力。让我们开始吧🚀

## Git与版本控制

[Git](https://en.wikipedia.org/wiki/Git)是一个开源版本控制系统，广泛用于软件开发。它组织项目并管理开发者之间的协作。我在独自工作时并没有使用Git，而是手动管理我的代码和笔记本🙃

当涉及到协作时，Git变得必不可少。它有助于跟踪项目进展，并促进协作。

这是一个广泛的学习主题，很多优秀的资源可供参考（比如[这个](https://www.freecodecamp.org/news/git-and-github-for-beginners/)）。今天，我将重点讲解“commit”这个术语，以及它如何帮助我整理思路。

> Git提交就像是对你代码的一个时间快照。

我在早期学习的第一件事之一就是要有组织的Git提交和简洁的提交消息。

后来我意识到，提前考虑你的提交及其结构也有助于你组织工作并用更好的逻辑模式设计代码。

这是一个例子，展示了如何在数据科学的背景下组织Git提交，来自我最近的一个项目：

![](../Images/4f788d949ddc8ee622c370fda7e88d21.png)

作者提供的图片

从协作的角度来看，分解提交——每次代码更改应有一个目的——也将帮助你的同事更快地审查你的代码。

## 编写可读且清晰的代码

我之前的导师曾经告诉我：“即使是你奶奶也应该能读懂你的代码”——免责声明：这不是从年龄歧视的角度出发，而是意味着每个人都应该能够轻松阅读和理解你的代码。

开玩笑说，反映你的思维过程在代码中并编写自解释的代码将帮助任何人更快地审查和理解你的工作。

我通过工作和阅读几本行业内的书籍学会了如何编写可读且清晰的代码：

+   **《代码整洁之道》** 由**罗伯特·C·马丁**著作

+   **《软件设计哲学》** 由**约翰·奥斯特豪特**著作。

> “所以如果你想快速完成任务，如果你希望你的代码容易编写，就让它易于阅读。”
> 
> **罗伯特·C·马丁**。

以及我最近工作中这一做法的实现：

相信我，你的未来自己会感谢你编写可读且清晰的代码，不仅仅是你的团队！

## 探索不同的开发工具

当涉及到机器学习模型的实验或原型测试其可行性时，使用笔记本通常是首选。

而Jupyter笔记本是一个很好的媒介。

在担任机器学习工程师角色之前，我主要使用 Jupyter 笔记本进行项目开发。当时，我的前团队强制使用 PyCharm，这使我首次接触到集成开发环境（IDE）。

作为一个主要使用一次性笔记本解决方案的数据科学家，我对 PyCharm 的众多功能和用户界面感到有些不知所措。

随着时间的推移，PyCharm 已经成为我的第二天性工具。

其代码补全和错误高亮功能已经成为我不可或缺的助手，为我的生产力做出了**很大**的贡献。

在 IDE 中掌握 Git 大大提高了我组织工作的能力，加快了我的编码速度，并使我能更高效地专注于手头的任务。此外，每当我几个月后重温代码时，我总是对自己编写的干净代码心怀感激😉

感谢您的阅读！这篇博客文章对我而言具有特别的意义。它标志着我重返博客和技术写作的旅程🤗

对于评论或建设性的反馈，您可以通过回复、[Twitter](https://twitter.com/cereniyim) 或 [Linkedin](https://www.linkedin.com/in/ceren-iyim) 联系我！
