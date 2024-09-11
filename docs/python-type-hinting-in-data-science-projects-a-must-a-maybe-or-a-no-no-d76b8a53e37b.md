# 数据科学项目中的 Python 类型提示：必须的、可能的，还是完全不该使用？

> 原文：[https://towardsdatascience.com/python-type-hinting-in-data-science-projects-a-must-a-maybe-or-a-no-no-d76b8a53e37b?source=collection_archive---------8-----------------------#2023-09-26](https://towardsdatascience.com/python-type-hinting-in-data-science-projects-a-must-a-maybe-or-a-no-no-d76b8a53e37b?source=collection_archive---------8-----------------------#2023-09-26)

## PYTHON 编程

## 我们是否应该在使用 Python 实现的数据科学项目中使用类型提示？

[](https://medium.com/@nyggus?source=post_page-----d76b8a53e37b--------------------------------)[![Marcin Kozak](../Images/d7faf62e48ed81dab5d8ad92819fff54.png)](https://medium.com/@nyggus?source=post_page-----d76b8a53e37b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d76b8a53e37b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d76b8a53e37b--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----d76b8a53e37b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-type-hinting-in-data-science-projects-a-must-a-maybe-or-a-no-no-d76b8a53e37b&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----d76b8a53e37b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d76b8a53e37b--------------------------------) ·6分钟阅读·2023年9月26日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd76b8a53e37b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-type-hinting-in-data-science-projects-a-must-a-maybe-or-a-no-no-d76b8a53e37b&source=-----d76b8a53e37b---------------------bookmark_footer-----------)![](../Images/fb8966e277eba25a6d58485fa18f1c9d.png)

无论你是否是 Python 类型提示的满意用户，你都需要了解这些概念以及如何使用它们。照片来源：[Kerin Gedge](https://unsplash.com/@keringedge?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我们是否应该在使用 Python 实现的数据科学项目中使用类型提示？

需要免责声明吗？在这里：这要看情况。在概念验证类型的项目中，这通常是不必要的。在生产项目中，至少在2023年，这倒是有必要的。但这还是要看情况。

我会尽量简明扼要，尽快切入正题。我不想花费几个小时考虑所有的利弊，简单的原因是数据科学市场对我们的工作有着相当明确的期望。我的目标是向你展示这些期望，而不是详细讨论它们。

首先陈述显而易见的内容。首先，Python 中的类型提示是*可选的*。就像*你不必在 Python 中使用类型提示*。如果是这样，我们主要问题的答案只有一个：你可以，但不，实际上你不必在数据科学项目中使用类型提示！

那么…就这样？我们完成了吗？

等一下。我们确实陈述了显而易见的内容，但我们并未触及任何超出显而易见的东西。

> 我们应该在…
