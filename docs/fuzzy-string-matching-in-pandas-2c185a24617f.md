# 如何在 Pandas 数据框中进行模糊字符串匹配

> 原文：[`towardsdatascience.com/fuzzy-string-matching-in-pandas-2c185a24617f`](https://towardsdatascience.com/fuzzy-string-matching-in-pandas-2c185a24617f)

## 匹配没有完美匹配的文本

[](https://thuwarakesh.medium.com/?source=post_page-----2c185a24617f--------------------------------)![Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----2c185a24617f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2c185a24617f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2c185a24617f--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----2c185a24617f--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----2c185a24617f--------------------------------) ·阅读时间 6 分钟·2023 年 4 月 17 日

--

![](img/0e2257b146dc3409abf7ce5b742d7da1.png)

图片由[Lucas Santos](https://unsplash.com/@_staticvoid?utm_source=medium&utm_medium=referral)提供，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

现实世界是不完美的。

人们使用相同信息的不同形式。即便是成熟的系统也使用不同的标准。你可能会见过城市名称拼写错误的情况，比如“Santana”而不是“Santa Ana”或“St. Louie”而不是“St. Louis”。

在处理现实世界的数据时，这是不可避免的。因此，我们必须确保我们在管道的下一步中使用的数据是标准化的。

我们可以通过模糊字符串匹配来解决这个问题。这也不完美，但非常有用。

[](/python-decorators-for-data-science-6913f717669a?source=post_page-----2c185a24617f--------------------------------) ## 我在几乎所有数据科学项目中使用的 5 个 Python 装饰器

### 装饰器提供了一种新的便利方式，从缓存到发送通知都有应用

towardsdatascience.com

## Python 中的模糊字符串匹配

如果你是 Python 程序员，你可能会使用 Pandas 数据框来处理数据。除了 pandas，你还可以使用“thefuzz”来进行模糊字符串匹配。

```py
pip install thefuzz
```

[TheFuzz](https://github.com/seatgeek/thefuzz)是一个开源 Python 包，正式名称为“FuzzyWuzzy”。它使用**Levenshtein 编辑距离**来计算字符串相似度。

这是它的基本用法：

```py
from thefuzz import fuzz, process

process.extractBests(
    "my precious",
    [
        "My brushes",
        "my purses",
        "my prices",
        "me priceless",
        "my prcios",
        "My bruises",
        "My praises",
        "My precursors",
        "My process",
        "My princess",
        "My progresses",
        "My prospects",
        "My producers",
        "My precisions",
        "My presuppositions",
    ],
)

# Output
>> [('my prcios', 90),
   ('My presuppositions', 86),
   ('My precisions', 83),
   ('My precursors', 75),
   ('my purses', 70)]

process.extractOne(
    "my precious",
    [
        ...
    ],
)

# Output
>> ('my prcios', 90)
```

[](/sql-on-pandas-usign-duckdb-f7cd238a0a5a?source=post_page-----2c185a24617f--------------------------------) ## SQL 在 Pandas 上 — 我新宠，速度提升 10 倍。

### 将两者的最佳之处结合起来

[towardsdatascience.com

## 如何测量文本相似度？

Levenshtein 距离，也称为编辑距离，是一种用于测量两个字符串之间差异或相似度的度量。它计算将一个字符串转换为另一个字符串所需的最小操作数（插入、删除或替换）。

> Levenshtein 距离越小，两个字符串越相似。

例如，考虑两个字符串“chat”和“chart”。它们之间的 Levenshtein 距离是 1，因为将“chat”转换为“chart”所需的唯一操作是将字母“a”替换为“r”。

现在考虑字符串“intention”和“execution”。它们之间的 Levenshtein 距离是 5。以下是将“intention”转换为“execution”的一种可能方式，所需的操作最少：

1.  将‘i’替换为‘e’：entention

1.  将’n’替换为‘x’：extention

1.  删除‘t’：exention

1.  将’n’替换为‘u’：exection

1.  插入‘u’：execution

这些操作的总成本是 5，这也是两个字符串之间的 Levenshtein 距离。

通常，两个字符串之间的 Levenshtein 距离越小，它们越相似。例如，如果两个字符串的 Levenshtein 距离为 0，则它们是完全相同的。相反，如果距离较大，则表示字符串之间有显著差异。

请参阅[Educative 关于 Levenshtein 距离的文章](https://www.educative.io/answers/the-levenshtein-distance-algorithm)，因为这不是本文的重点。

但这里有一个简单的示例，展示了如何在 Python 中为两个字符串获取相似度评分。

```py
# Example 2: Calculate Jaccard similarity
ratio = fuzz.ratio("apple", "banana")
print(ratio)  # Output: 18

# Example 3: Calculate cosine similarity
cosine_sim = fuzz.token_sort_ratio("apple", "banana")
print(cosine_sim)  # Output: 18

# Example 4: Calculate partial ratio
partial_ratio = fuzz.partial_ratio("apple", "banana")
print(partial_ratio)  # Output: 20

# Example 5: Calculate token set ratio
token_set_ratio = fuzz.token_set_ratio("apple is a fruit", "a fruit is an apple")
print(token_set_ratio)  # Output: 100
```

链接 ## 使用 Black 和 GitHub Actions 保持 Python 代码的整洁。

### 没有人愿意面对混乱的代码库；很少有人有耐心去清理它。

[链接

## 使用模糊字符串匹配来删除 Pandas 数据框中的重复项

在处理用户创建的数据时，常见的挑战之一是删除重复项。但当重复项不是完全匹配时，这个任务会变得更加困难。

我们可以编写一个小函数来检查字符串相似度并删除重复项。以下是一个示例：

```py
import pandas as pd
from thefuzz import fuzz, process

data = {
    "Name": ["John Smith", "Jon Smtih", "Jane Doe", "James Johnsan", "Janes Johnson"],
    "Age": [25, 25, 30, 40, 40],
    "Gender": ["M", "M", "F", "M", "M"],
}

df = pd.DataFrame(data)

display(df)

# Output
|    | Name          |   Age | Gender   |
|---:|:--------------|------:|:---------|
|  0 | John Smith    |    25 | M        |
|  1 | Jon Smtih     |    25 | M        |
|  2 | Jane Doe      |    30 | F        |
|  3 | James Johnsan |    40 | M        |
|  4 | Janes Johnson |    40 | M        |

def compare_strings(a, b):
    return fuzz.token_sort_ratio(a, b)

def remove_duplicates(df, threshold=90):
    duplicates = set()
    processed = []

    for i, row in df.iterrows():
        if i not in duplicates:
            processed.append(row)

            for j, other_row in df.iterrows():
                if i != j and j not in duplicates:
                    score = compare_strings(row["Name"], other_row["Name"])

                    if score >= threshold:
                        duplicates.add(j)

    return pd.DataFrame(processed)

remove_duplicates(df, threshold=80)

# Output
|    | Name          |   Age | Gender   |
|---:|:--------------|------:|:---------|
|  0 | John Smith    |    25 | M        |
|  2 | Jane Doe      |    30 | F        |
|  3 | James Johnsan |    40 | M        |
```

我们的原始数据集包含了类似的名字——John Smith 和 Jon Smith，James Johnson 和 James Johnsan。但我们能够去除这些重复项。

链接 ## 我的 Python 脚本如何更像自然对话

### 管道是更人性化编码的极佳技巧

[towardsdatascience.com

## 在 Pandas 数据框中标准化模糊重复项

这是一个实现函数的示例，基于相似度评分阈值，使用 thefuzz 包将“Name”列中的重复行替换为该行的第一次出现：

```py
def replace_duplicates(df, column_name='Name', threshold=90):
    processed = []
    duplicates = set()
    first_occurrence = {}

    for i, row in df.iterrows():
        row_text = row[column_name]

        if i not in duplicates:
            processed.append(row)
            first_occurrence[row_text] = i

            for j, other_row in df.iterrows():
                if i != j and j not in duplicates:
                    other_text = other_row[column_name]
                    score = fuzz.token_set_ratio(row_text, other_text)

                    if score >= threshold:
                        duplicates.add(j)
                        first_occurrence[other_text] = i

    return df.iloc[list(first_occurrence.values())]
```

我们可以使用这个函数将数据集中所有的 Jon Smith 替换为 John Smith。

```py
replace_duplicates(df, threshold=80)

# Output
|    | Name          |   Age | Gender   |
|---:|:--------------|------:|:---------|
|  0 | John Smith    |    25 | M        |
|  0 | John Smith    |    25 | M        |
|  2 | Jane Doe      |    30 | F        |
|  3 | James Johnsan |    40 | M        |
|  3 | James Johnsan |    40 | M        |
```

## 结论

对于现实世界的数据，模糊字符串匹配是必不可少的。除非你自动生成数据，否则几乎总是期望数据集中存在非标准值。即使在自动生成的系统中，约定也可能有所不同。

当涉及到文本数据时，我经常看到我们在某些点上卡住了，这时需要模糊匹配。

在这种情况下，我们可以使用本文中强调的技术。我们使用了 Python 包“thefuzz”来使用 Levenshtein 距离匹配字符串，并从 Pandas 数据框中删除了重复项。如果需要，我们可以将它们替换为一个合适的值，而不是简单地删除。

但模糊字符串匹配并不完美。例如，在替换重复项的最后一个示例中，我们的脚本将所有的“James Johnson”替换为“James Johnsan”。如果我们更喜欢 Johnson，我们的脚本就没有做好。

因此，我们需要把模糊匹配作为最后的手段，或作为一个有用的指南。但是过度依赖它们是不明智的。

希望这对你有帮助。

> 感谢阅读，朋友！在 [**LinkedIn**](https://www.linkedin.com/in/thuwarakesh/)，[**Twitter**](https://twitter.com/Thuwarakesh)，和 [**Medium**](https://thuwarakesh.medium.com/) 上跟我打个招呼吧。
> 
> 还不是 Medium 会员？请使用这个链接 [**成为会员**](https://thuwarakesh.medium.com/membership)，因为对你没有额外费用，我通过推荐你赚取少量佣金。
