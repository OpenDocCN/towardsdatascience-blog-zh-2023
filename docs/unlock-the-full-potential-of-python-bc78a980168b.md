# 解锁 Python 的全部潜力

> 原文：[https://towardsdatascience.com/unlock-the-full-potential-of-python-bc78a980168b?source=collection_archive---------4-----------------------#2023-05-10](https://towardsdatascience.com/unlock-the-full-potential-of-python-bc78a980168b?source=collection_archive---------4-----------------------#2023-05-10)

## 提高代码可读性和效率的小贴士

[](https://tinztwinspro.medium.com/?source=post_page-----bc78a980168b--------------------------------)[![Janik 和 Patrick Tinz](../Images/a08aa54f553f606ef5df86f9411c36ac.png)](https://tinztwinspro.medium.com/?source=post_page-----bc78a980168b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bc78a980168b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bc78a980168b--------------------------------) [Janik 和 Patrick Tinz](https://tinztwinspro.medium.com/?source=post_page-----bc78a980168b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4eb5d9652d9e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlock-the-full-potential-of-python-bc78a980168b&user=Janik+and+Patrick+Tinz&userId=4eb5d9652d9e&source=post_page-4eb5d9652d9e----bc78a980168b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bc78a980168b--------------------------------) ·9 min read·2023年5月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbc78a980168b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlock-the-full-potential-of-python-bc78a980168b&user=Janik+and+Patrick+Tinz&userId=4eb5d9652d9e&source=-----bc78a980168b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbc78a980168b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlock-the-full-potential-of-python-bc78a980168b&source=-----bc78a980168b---------------------bookmark_footer-----------)![](../Images/f2a8e5fcd50cccc27e31966429fead88.png)

图片由 [Maxwell Nelson](https://unsplash.com/@maxcodes?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**Python** 是开发者和数据科学家最受欢迎的编程语言之一。开发者使用该语言进行后台和前台开发。数据科学家和分析师使用 Python 来分析数据。主要的机器学习库都可以在 Python 中找到。因此，Python 在数据科学家中非常受欢迎。

在这篇文章中，我们展示了一些 Python 的技巧和窍门。这些技巧和窍门将帮助你使你的 Python 代码更高效且更具可读性。你可以直接在下一个 Python 项目中使用这些技巧。我们将技巧结构化，使你可以独立阅读每一个技巧。阅读标题并决定哪个技巧或窍门对你有兴趣！**让我们马上开始吧！**

# 使用 `enumerate()`

在 Python 中，你通常会在一个可迭代对象上写一个 `for()` 循环。因此，你不需要计数变量来访问元素。然而，有时需要一个计数变量。有几种方法可以实现这样的 `for()` 循环。我们今天将展示两种变体。以下示例代码展示了变体 A。

```py
# variant A

company_list = ["Tesla", "Apple", "Block", "Palantir"]

i = 0
for company in company_list:
    print(i, company)
    i+=1

# Output:
#…
```
