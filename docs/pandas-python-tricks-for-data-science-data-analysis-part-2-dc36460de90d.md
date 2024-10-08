# Pandas & Python 数据科学与数据分析技巧 — 第二部分

> 原文：[`towardsdatascience.com/pandas-python-tricks-for-data-science-data-analysis-part-2-dc36460de90d`](https://towardsdatascience.com/pandas-python-tricks-for-data-science-data-analysis-part-2-dc36460de90d)

## 这是我的 Pandas & Python 技巧的第二部分

[](https://zoumanakeita.medium.com/?source=post_page-----dc36460de90d--------------------------------)![Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----dc36460de90d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dc36460de90d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dc36460de90d--------------------------------) [Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----dc36460de90d--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dc36460de90d--------------------------------) ·阅读时间 5 分钟·2023 年 1 月 17 日

--

![](img/11eeeaf9d71dbc5826179cfa3cd1c5e2.png)

图片来源：[Andrew Neel](https://unsplash.com/@andrewtneel) 在 [Unsplash](https://unsplash.com/photos/cckf4TsHAuw)

# 介绍

几天前，我分享了[一些 Python 和 Pandas 技巧](https://medium.com/towards-data-science/pandas-and-python-tips-and-tricks-for-data-science-and-data-analysis-1b1e05b7d93a)，帮助数据分析师和数据科学家快速学习他们可能不知晓的新有价值概念。这也是我每天在[LinkedIn](https://www.linkedin.com/in/zoumana-keita/)上分享的技巧合集的一部分。

这些技巧的视频系列可以在我的 YouTube 频道上查看。

我的系列中的 3 个技巧

# Python

## 从列表中移除重复项

尝试从列表中移除重复项时，你可能会尝试使用𝗳𝗼𝗿 循环方法。

这样做虽然有效，但在处理非常大的数据时效率不高 ❌。

相反，使用𝘀𝗲𝘁() ✅，它本身不接受重复项。

以下是一个说明 💡

![](img/7a25a88658e8953ba771ad1e6cdf055c.png)

移除重复项（图片来源：作者）

## 原始列表中的顺序

使用𝘀𝗲𝘁()来从列表中移除重复项是一个很好的方法。

🚨 但要小心使用，因为它不会❌ 保留原始顺序。仅在不关心列表中元素顺序时使用。

相反，使用𝗱𝗶𝗰𝘁.𝗳𝗿𝗼𝗺𝗸𝗲𝘆𝘀() ✅ 保持原始顺序。

以下是一个说明 💡

![](img/ab0d765c3db368512e0d1132c6cb8749.png)

原始顺序由 dict.fromkeys 保持（图片来源：作者）

## 检查元素是否存在于列表中

[#Python](https://www.linkedin.com/feed/hashtag/?keywords=python&highlightedUpdateUrns=urn%3Ali%3Aactivity%3A7011293609643716608) 技巧 ✨🐍✨

尝试 𝗰𝗵𝗲𝗰𝗸 𝗶𝗳 𝗮𝗻 𝗶𝘁𝗲𝗺 𝗲𝘅𝗶𝘀𝘁𝘀 𝗶𝗻 𝗮 𝗹𝗶𝘀𝘁 时，你可能会尝试使用𝗳𝗼𝗿 循环和 𝗶𝗳 条件方法。

这样做有效，但处理非常大数据时效率不高 ❌。

相反，使用 𝗶𝗻 ✅ 方法，它本地返回布尔值。

以下是一个示例 💡

![](img/baf9630156bad0ab19a7121f5eb0547f.png)

检查元素是否存在于列表中（图像作者）

## 获取 Python 列表中的 N 个最大和最小值

在 Python 中，可以使用 𝗺𝗮𝘅() 和 𝗺𝗶𝗻() 函数分别找到列表的最大值和最小值。

然而，当涉及到获取 𝗡 𝗹𝗮𝗿𝗴𝗲𝘀𝘁 或 𝘀𝗺𝗮𝗹𝗹𝗲𝘀𝘁 值时，你可能会考虑双向方法：

1️⃣ 按递减或递增顺序对列表进行排序。

2️⃣ 检索 N 个最大或最小值。

好策略，但处理大数据时效率不高 ❌。

✅ 相反，你可以使用 𝗻𝗹𝗮𝗿𝗴𝗲𝘀𝘁 和 𝗻𝘀𝗺𝗮𝗹𝗹𝗲𝘀𝘁 函数来自内置的 Python 模块 𝗵𝗲𝗮𝗽𝗾，它是快速 🚀 和内存高效的 👍。

![](img/d40da68eac922c15166e5b5c5c09e982.png)

𝗻𝗹𝗮𝗿𝗴𝗲𝘀𝘁 和 𝗻𝘀𝗺𝗮𝗹𝗹𝗲𝘀𝘁 函数示例（图像作者）

# Pandas

## 使用同一单元格显示多个数据框

大多数时候，我们倾向于使用不同的笔记本单元格来显示不同的数据框，比如相同数据的 head() 和 tail()。

这是因为在同一单元格中使用它们时，只有最后一个会被显示，之前的所有指令都会被忽略 ❌。

✅ 解决此问题，你可以使用 𝗱𝗶𝘀𝗽𝗹𝗮𝘆() 函数。

以下是一个示例 💡

![](img/ab3cf7e045cbd33f29fac472321bda81.png)

从同一笔记本单元格中获取多个数据框（图像作者）

## 描述数值列和分类列

应用 𝗱𝗲𝘀𝗰𝗿𝗶𝗯𝗲() 函数没有参数时，自然只返回与数值列相关的统计数据。

这限制了 🚫 我们对数据集的理解，因为大多数时候我们也处理分类列。

✅ 为了解决这个问题，你可以采取双向方法：

1️⃣ 使用 𝗱𝗲𝘀𝗰𝗿𝗶𝗯𝗲() 对数值列进行描述。

2️⃣ 设置参数 𝗶𝗻𝗰𝗹𝘂𝗱𝗲=[𝗼𝗯𝗷𝗲𝗰𝘁] 以提供有关分类列的信息。

以下是一个示例 💡

![](img/ce8e88c565b6152290078ab245d5bbfe.png)

**描述** 包括分类列（图像作者）

## 创建新列时避免使用 for 循环

在处理 Pandas 数据框时，从现有列创建新列主要是过程的一部分。

这些列的创建方式会影响整体计算时间的效率 ⏰。

有些人可能会使用循环来生成这些派生列。

然而，这可能不是正确的方法 ❌，因为时间复杂度 📈，特别是在处理大数据时。

✅ 采用向量化方法要好得多。

![](img/e6a26db9a840701eb41dc49afb6a4987.png)

向量化与 for 循环的示例

## 保存 Pandas 列的子集

有时我们只对从原始数据框中保存子集列感兴趣，而不是整个数据。

一种方法是创建一个包含感兴趣列的新数据框。

但是，这种方法增加了另一个复杂性 ❌。

✅ 这个问题可以通过指定 columns 参数来解决。

![](img/a773627576845f9001df9b33599a58ba.png)

获取 Pandas 列的子集（图像作者提供）

## 将网页上的表格数据转换为 Pandas Dataframe

如果你想从网页 🌐 中提取表格作为 Pandas Dataframes，你可以使用 Pandas 的 𝗿𝗲𝗮𝗱_𝗵𝘁𝗺𝗹() 函数。

✅ 它返回网页上所有表格的列表。

![](img/c3f1ad9d423fdc4e0e75d92a644f6f39.png)

将网页表格转换为 Pandas Dataframe（图像作者提供）

# 结论

感谢阅读！ 🎉 🍾

我希望你觉得这份 Python 和 Pandas 技巧的清单对你有帮助！请继续关注，因为内容将每天更新更多技巧。

此外，如果你喜欢阅读我的故事并希望支持我的写作，可以考虑 [成为 Medium 会员](https://zoumanakeita.medium.com/membership)。每月 $5 的承诺，你就能解锁 Medium 上的无限故事访问权限。

随时关注我的 [Medium](https://zoumanakeita.medium.com/)、[Twitter](https://twitter.com/zoumana_keita_) 和 [YouTube](https://www.youtube.com/channel/UC9xKdy8cz6ZuJU5FTNtM_pQ)，或者在 [LinkedIn](https://www.linkedin.com/in/zoumana-keita/) 上打个招呼！讨论 AI、ML、数据科学、NLP 和 MLOps 总是令人愉快的！
