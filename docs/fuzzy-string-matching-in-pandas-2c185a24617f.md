# 如何在Pandas数据框中进行模糊字符串匹配。

> 原文：[https://towardsdatascience.com/fuzzy-string-matching-in-pandas-2c185a24617f?source=collection_archive---------10-----------------------#2023-04-17](https://towardsdatascience.com/fuzzy-string-matching-in-pandas-2c185a24617f?source=collection_archive---------10-----------------------#2023-04-17)

## 匹配文本时没有完美的匹配。

[](https://thuwarakesh.medium.com/?source=post_page-----2c185a24617f--------------------------------)[![Thuwarakesh Murallie](../Images/44f1a14a899426592bbd8c7f73ce169d.png)](https://thuwarakesh.medium.com/?source=post_page-----2c185a24617f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2c185a24617f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2c185a24617f--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----2c185a24617f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F93ce19993bef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffuzzy-string-matching-in-pandas-2c185a24617f&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=post_page-93ce19993bef----2c185a24617f---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2c185a24617f--------------------------------) ·6分钟阅读·2023年4月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2c185a24617f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffuzzy-string-matching-in-pandas-2c185a24617f&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=-----2c185a24617f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2c185a24617f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffuzzy-string-matching-in-pandas-2c185a24617f&source=-----2c185a24617f---------------------bookmark_footer-----------)![](../Images/0e2257b146dc3409abf7ce5b742d7da1.png)

[Lucas Santos](https://unsplash.com/@_staticvoid?utm_source=medium&utm_medium=referral)拍摄的照片，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

现实世界并不完美。

人们使用相同信息的不同形式。即使是成熟的系统也使用不同的标准。你可能会见到城市名称拼写错误，如“Santana”代替“Santa Ana”或“St. Louie”代替“St. Louis”。

在处理真实世界的数据时，这是不可避免的。因此，我们必须确保我们在管道的下一步中使用的数据是标准化的。

我们可以通过模糊字符串匹配来解决这个问题。这虽然也不完美，但非常有帮助。

[](/python-decorators-for-data-science-6913f717669a?source=post_page-----2c185a24617f--------------------------------) [## 我在几乎所有数据科学项目中使用的 5 个 Python 装饰器

### 装饰器提供了一种新的便利方式，从缓存到发送通知都能使用。

towardsdatascience.com](/python-decorators-for-data-science-6913f717669a?source=post_page-----2c185a24617f--------------------------------)

## Python 中的模糊字符串匹配

如果你是一个 Python 程序员，你可能会使用 Pandas 数据框进行数据处理。除了 pandas，你还可以使用“thefuzz”进行模糊字符串匹配。

```py
pip install thefuzz
```

[TheFuzz](https://github.com/seatgeek/thefuzz) 是一个开源 Python 包，正式名称为“FuzzyWuzzy”。它使用**Levenshtein 编辑距离**来…
