# Python 嵌套字典 — Python 嵌套字典的完全指南

> 原文：[`towardsdatascience.com/nested-dictionary-python-a-complete-guide-to-python-nested-dictionaries-756a7822cb4f?source=collection_archive---------13-----------------------#2023-04-18`](https://towardsdatascience.com/nested-dictionary-python-a-complete-guide-to-python-nested-dictionaries-756a7822cb4f?source=collection_archive---------13-----------------------#2023-04-18)

## 如何在 Python 中使用嵌套字典？本文教会您有关 Python 嵌套字典的一切知识。

[](https://medium.com/@radecicdario?source=post_page-----756a7822cb4f--------------------------------)![Dario Radečić](https://medium.com/@radecicdario?source=post_page-----756a7822cb4f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----756a7822cb4f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----756a7822cb4f--------------------------------) [Dario Radečić](https://medium.com/@radecicdario?source=post_page-----756a7822cb4f--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F689ba04bb8be&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnested-dictionary-python-a-complete-guide-to-python-nested-dictionaries-756a7822cb4f&user=Dario+Rade%C4%8Di%C4%87&userId=689ba04bb8be&source=post_page-689ba04bb8be----756a7822cb4f---------------------post_header-----------) 在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----756a7822cb4f--------------------------------) 发表 ·12 分钟阅读·2023 年 4 月 18 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F756a7822cb4f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnested-dictionary-python-a-complete-guide-to-python-nested-dictionaries-756a7822cb4f&user=Dario+Rade%C4%8Di%C4%87&userId=689ba04bb8be&source=-----756a7822cb4f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F756a7822cb4f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnested-dictionary-python-a-complete-guide-to-python-nested-dictionaries-756a7822cb4f&source=-----756a7822cb4f---------------------bookmark_footer-----------)![](img/393f0346b4e4e357c149ccbef699a1e2.png)

照片由 [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) 提供于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 什么是 Python 中的嵌套字典？

Python 中一种常见的数据结构是嵌套字典，也就是一个字典的值可以是其他字典。初学者讨厌嵌套字典，因为它们需要更多时间来处理和解析，但只要稍加练习，这些都不是问题。

> [*新手 Python？先学习基本字典。*](https://betterdatascience.com/python-dictionaries/)

今天你将学习什么是嵌套字典、为什么在 Python 中使用嵌套字典、如何在 Python 中遍历嵌套字典，以及更多内容。关于库的导入，将此内容放在脚本或笔记本的顶部：

```py
import pprint
pp = pprint.PrettyPrinter(depth=4)
```

这样可以在打印嵌套字典时进行格式化，使其更易于阅读。

# 如何在 Python 中创建嵌套字典

创建嵌套字典的方法有很多，但如果……
