# 5 种代码优化技术来加速你的程序

> 原文：[https://towardsdatascience.com/5-code-optimization-techniques-to-speed-up-your-programs-cc7740381bcb?source=collection_archive---------2-----------------------#2023-11-29](https://towardsdatascience.com/5-code-optimization-techniques-to-speed-up-your-programs-cc7740381bcb?source=collection_archive---------2-----------------------#2023-11-29)

## 使用这些与语言无关的方法，使你的代码更高效、更专业

[](https://medium.com/@nic-obert?source=post_page-----cc7740381bcb--------------------------------)[![Nicholas Obert](../Images/d70330063c9edc2f63e53f62a78f82ec.png)](https://medium.com/@nic-obert?source=post_page-----cc7740381bcb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cc7740381bcb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cc7740381bcb--------------------------------) [Nicholas Obert](https://medium.com/@nic-obert?source=post_page-----cc7740381bcb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Feeb884cbf705&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-code-optimization-techniques-to-speed-up-your-programs-cc7740381bcb&user=Nicholas+Obert&userId=eeb884cbf705&source=post_page-eeb884cbf705----cc7740381bcb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cc7740381bcb--------------------------------) ·7分钟阅读·2023年11月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcc7740381bcb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-code-optimization-techniques-to-speed-up-your-programs-cc7740381bcb&user=Nicholas+Obert&userId=eeb884cbf705&source=-----cc7740381bcb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcc7740381bcb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-code-optimization-techniques-to-speed-up-your-programs-cc7740381bcb&source=-----cc7740381bcb---------------------bookmark_footer-----------)![](../Images/1538a19522e68cf19f04a0b3e4287df6.png)

照片由 [Shubham Dhage](https://unsplash.com/@theshubhamdhage?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

首先让它能工作，然后让它更快。这是许多专业程序员遵循的一个共同原则。开始时，你可以使用任何最直观的方法来编写代码，以节省草稿开发时间。在你有了一个可用的实现后，你可能会想通过仔细选择在特定情况下最有效的数据结构和技术来优化它。

在本文中，我们将探讨五种与语言无关的方法，您可以使用这些方法来提高代码运行时间。以下概念是通用的，可以应用于任何编程语言。

## 循环不变提取

请考虑以下 Python 代码，该代码检查一个字符串列表以找到所有匹配项：

```py
import regex as re

# Get some strings as input
strings = get_strings()
# List to store all the matches
matches = []

# Iterate over the input strings
for string in strings:

  # Compile the regex
  rex = re.compile(r'[a-z]+')

  # Check the string against the regex
  matches = rex.findall(string)

  # Finally, append the matches to the list
  matches.extend(matches)
```
