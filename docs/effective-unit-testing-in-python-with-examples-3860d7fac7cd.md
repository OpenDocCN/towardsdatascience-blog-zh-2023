# Python 中的有效单元测试 — 带实例

> 原文：[`towardsdatascience.com/effective-unit-testing-in-python-with-examples-3860d7fac7cd?source=collection_archive---------2-----------------------#2023-10-01`](https://towardsdatascience.com/effective-unit-testing-in-python-with-examples-3860d7fac7cd?source=collection_archive---------2-----------------------#2023-10-01)

[](https://medium.com/@christopher_karg?source=post_page-----3860d7fac7cd--------------------------------)![Christopher Karg](https://medium.com/@christopher_karg?source=post_page-----3860d7fac7cd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3860d7fac7cd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3860d7fac7cd--------------------------------) [Christopher Karg](https://medium.com/@christopher_karg?source=post_page-----3860d7fac7cd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5fbda6d16c39&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feffective-unit-testing-in-python-with-examples-3860d7fac7cd&user=Christopher+Karg&userId=5fbda6d16c39&source=post_page-5fbda6d16c39----3860d7fac7cd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3860d7fac7cd--------------------------------) ·16 分钟阅读·2023 年 10 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3860d7fac7cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feffective-unit-testing-in-python-with-examples-3860d7fac7cd&user=Christopher+Karg&userId=5fbda6d16c39&source=-----3860d7fac7cd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3860d7fac7cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feffective-unit-testing-in-python-with-examples-3860d7fac7cd&source=-----3860d7fac7cd---------------------bookmark_footer-----------)![](img/4d3e4fc2d2cb528b00e0e5676f4becb5.png)

来源：[`www.pexels.com/photo/red-vintage-car-stopped-on-the-crossroads-15706251/`](https://www.pexels.com/photo/red-vintage-car-stopped-on-the-crossroads-15706251/)

我的下一篇文章将探讨 Python 中的单元测试。这个话题在课程、书籍和在线教程中往往被忽视。然而，这是编写生产级、坚不可摧的代码时至关重要的技能。本文中的场景略微偏向数据/数据科学领域。因此，我对“有效”的单元测试的看法可能与来自不同背景的人有所不同。

> 编写单元测试的最终目的是防止错误被推送到生产环境中。

这会导致许多头痛的问题，并且需要时间来解决。不同的公司有不同的测试方法。我曾经参与过的一些项目（特别是网页抓取项目）虽然不一定需要单元测试，但可以通过实现 Python 的 [内置日志功能](https://docs.python.org/3/library/logging.html) 和 try/except 处理来受益。这篇文章仅涵盖了 [单元测试](https://docs.python.org/3/library/unittest.html?highlight=unittest#module-unittest) 的实际实现，并提供了代码示例和链接，这些代码可以在 [我的 GitHub 仓库](https://github.com/CMaxK/unittesting_examples) 中找到。欢迎克隆并尝试一些测试函数——有关如何操作的说明可以在仓库的 README 中找到。该仓库分为简单、中等和困难三个类别，每个类别都在前一个类别的基础上进行扩展。
