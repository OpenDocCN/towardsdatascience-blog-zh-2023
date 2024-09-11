# 关于 Python 数据结构的 3 个必知概念

> 原文：[https://towardsdatascience.com/3-must-know-concepts-about-python-data-structures-641e6f6207fc?source=collection_archive---------9-----------------------#2023-07-13](https://towardsdatascience.com/3-must-know-concepts-about-python-data-structures-641e6f6207fc?source=collection_archive---------9-----------------------#2023-07-13)

## 学习这些概念以编写高效且稳健的程序

[](https://sonery.medium.com/?source=post_page-----641e6f6207fc--------------------------------)[![Soner Yıldırım](../Images/c589572e9d1ee176cd4f5a0008173f1b.png)](https://sonery.medium.com/?source=post_page-----641e6f6207fc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----641e6f6207fc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----641e6f6207fc--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----641e6f6207fc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-must-know-concepts-about-python-data-structures-641e6f6207fc&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----641e6f6207fc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----641e6f6207fc--------------------------------) · 4 分钟阅读 · 2023 年 7 月 13 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F641e6f6207fc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-must-know-concepts-about-python-data-structures-641e6f6207fc&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=-----641e6f6207fc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F641e6f6207fc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-must-know-concepts-about-python-data-structures-641e6f6207fc&source=-----641e6f6207fc---------------------bookmark_footer-----------)![](../Images/9f815b14a6c3013897e9d87d037c87d3.png)

图片来源于 [Ricardo Gomez Angel](https://unsplash.com/@rgaleriacom?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/ZUwXnS7K7Ok?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

+   列表可以包含重复项，但集合不行。

+   你可以更新列表中的项目，但不能更新元组中的项目。

+   你可以从元组中获取第三个项目，但不能从集合中获取。

这些只是关于 Python 数据结构的一些基本知识。这些差异背后肯定有原因，使每种结构适用于特定的任务。

要编写高效的 Python 脚本，必须了解这些数据结构的关键特性以及如何使用它们。

例如，我们可以使用集合来查找两个列表之间的不同项：

```py
lst_1 = ["Jane", "Emily", "John", "Max", "Emily", "Jane", "Matt"]

lst_2 = ["Jane", "Matt", "John", "Jane", "Emily"]

# convert each list to a set, find the difference using subtraction
difference = list(set(lst_1) - set(lst_2))

print(difference)
# output
['Max']
```

我们将学习关于 Python 四种内置数据结构的三个必知概念：[字典](/12-examples-to-master-python-dictionaries-5a8bcd688c6d)，[集合](/12-examples-to-master-python-sets-71802ea56de3)，[列表](/11-must-know-operations-to-master-python-lists-f03c71b6bbb6)，[元组](/10-examples-to-master-python-tuples-6c606ed42b96)。

# 1\. 可变

如果数据结构是可变的，我们可以更新其项或添加新的项。

+   列表：可变（我们可以添加新的…）
