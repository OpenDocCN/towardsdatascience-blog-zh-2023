# 使用 Pytest 测试 Python 代码 — 初学者指南

> 原文：[`towardsdatascience.com/testing-python-code-with-pytest-for-beginners-bcde301e7453?source=collection_archive---------15-----------------------#2023-04-03`](https://towardsdatascience.com/testing-python-code-with-pytest-for-beginners-bcde301e7453?source=collection_archive---------15-----------------------#2023-04-03)

## 使用 Python 的概述与实现

[](https://medium.com/@aashishnair?source=post_page-----bcde301e7453--------------------------------)![Aashish Nair](https://medium.com/@aashishnair?source=post_page-----bcde301e7453--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bcde301e7453--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bcde301e7453--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----bcde301e7453--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3087ba81e065&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftesting-python-code-with-pytest-for-beginners-bcde301e7453&user=Aashish+Nair&userId=3087ba81e065&source=post_page-3087ba81e065----bcde301e7453---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bcde301e7453--------------------------------) ·8 分钟阅读·2023 年 4 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbcde301e7453&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftesting-python-code-with-pytest-for-beginners-bcde301e7453&user=Aashish+Nair&userId=3087ba81e065&source=-----bcde301e7453---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbcde301e7453&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftesting-python-code-with-pytest-for-beginners-bcde301e7453&source=-----bcde301e7453---------------------bookmark_footer-----------)![](img/c8878181dbd849d86d9719658b424a99.png)

图片来自 Pixabay: [`www.pexels.com/photo/blur-business-close-up-code-270557/`](https://www.pexels.com/photo/blur-business-close-up-code-270557/)

## 介绍

在维护数据管道时，做出任何更改后测试底层代码是很重要的。这样，你可以在任何重构的代码无法按预期执行时及时得到警报。

大多数初学者倾向于手动测试他们的代码，一次运行一个函数，使用不同的参数。这种方法很简单，但扩展性不佳。

测试一段代码在一组参数下通常不会耗费太多时间，但要进行 10 次？50 次？100 次呢？

在持续数月或数年的项目中，当数据管道的底层代码经常被重构时，手动测试代码将花费程序员大量时间。

从事长期项目的人将受益于通过使用[**pytest**](https://docs.pytest.org/en/7.2.x/)自动化这一过程，这是一个 Python 测试框架，允许用户用很少的代码执行测试。

在这里，我们揭示了 pytest 的好处，并展示了数据科学家如何通过案例研究利用这个包编写基本测试。

## Pytest 值得付出努力吗？
