# 如何使用 Streamlit 的 st.write 函数来改进您的 Streamlit 仪表板

> 原文：[`towardsdatascience.com/how-to-use-streamlits-st-write-function-to-improve-your-streamlit-dashboard-1586333eb24d`](https://towardsdatascience.com/how-to-use-streamlits-st-write-function-to-improve-your-streamlit-dashboard-1586333eb24d)

## Streamlit 函数的瑞士军刀

[](https://andymcdonaldgeo.medium.com/?source=post_page-----1586333eb24d--------------------------------)![Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----1586333eb24d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1586333eb24d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1586333eb24d--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----1586333eb24d--------------------------------)

·发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----1586333eb24d--------------------------------) ·8 分钟阅读·2023 年 1 月 17 日

--

![](img/fc9fcf4c1510ad514f6ae258e5ee6e19.png)

照片由[Bram Naus](https://unsplash.com/es/@bramnaus?utm_source=medium&utm_medium=referral)提供 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

当开始使用[Streamlit](https://streamlit.io/)时，构建仪表板或应用程序时，您将首先遇到的一个函数是`st.write()`。[Streamlit 文档描述了这个函数](https://docs.streamlit.io/library/api-reference/write-magic/st.write)为“Streamlit 命令的瑞士军刀”。

这是一个非常多才多艺的函数，允许您显示文本、表情符号、Markdown 等等。

在本文中，我们将探讨`st.write()`函数如何用于改进您的 Streamlit 仪表板的方法。

[](https://docs.streamlit.io/library/api-reference/write-magic/st.write?source=post_page-----1586333eb24d--------------------------------) [## st.write — Streamlit Docs

### 函数签名 st.write(*args, unsafe_allow_html=False, **kwargs) 参数要打印的一个或多个对象…

docs.streamlit.io](https://docs.streamlit.io/library/api-reference/write-magic/st.write?source=post_page-----1586333eb24d--------------------------------)

本文是我在 Streamlit 上创建的一系列文章的一部分。您可以在下面的链接中探索它们。

+   开始使用基于 Web 的 Streamlit 应用程序

+   创建真正的多页面 Streamlit 应用程序-新方法（2022）

+   [开始使用 Streamlit：刚开始时需要了解的 5 个函数](https://medium.com/towards-data-science/getting-started-with-streamlit-5-functions-you-need-to-know-when-starting-out-b35ed7d872b9)

+   Streamlit 颜色选择器：在 Streamlit 仪表板上轻松更改图表颜色的简便方法

# 使用 st.write 显示简单和格式化的文本

`st.write()` 命令的第一个和最明显的用途是显示文本。如果我们使用下面的简单示例，我们将在我们的应用程序中显示该文本。

```py
st.write('Here is some simple text')
```

![](img/e5e5e8d6bfbdcf7a66aaa721e5b06225.png)

使用 st.write 显示简单文本的结果。作者提供的图片。

## 如何格式化并为 st.write 生成的文本添加颜色

上面的文本看起来有点无聊。我们可以通过添加一些颜色来增加一些趣味。为此，我们需要利用一些 HTML 和 CSS 知识，将 `unsafe_allow_html` 选项设置为 `True`。

然后我们使用 HTML `<p>` 标签创建一个段落，并使用 `style` 参数设置颜色

```py
st.write('<p style="color:red;">Here is some red text</p>', 
unsafe_allow_html=True)
```

![](img/eaff5b9981bad24bd020bb6e2e6dd990.png)

在 st.write 函数中更改字体颜色。作者提供的图片。

如果我们想要增加字体大小，我们可以添加一个 `font-size`

```py
st.write('<p style="font-size:26px; color:red;">Here is some red text</p>',
unsafe_allow_html=True)
```

![](img/730fc02b6be6aca4782a448f5f68e85e.png)

在 st.write 函数中更改字体的颜色和大小。作者提供的图片。

## 将文本拆分成多行

我们可以将文本显示在多行而不是单行。st.write 函数允许你传入一个用三个单引号（’’’）或三个双引号（”””）括起来的字符串。[这种语法在 Python 中用于允许字符串跨越多行。](https://www.digitalocean.com/community/tutorials/how-to-format-text-in-python-3)

只使用三个单引号或三个双引号并不足以使我们的文本在使用 Streamlit 时跨越多行。为了做到这一点，我们必须修改字符串。

有几种不同的方法可以实现这一点。

第一种方法是使用换行符 `\n`，它会将下一行文本推到新的一行。

```py
st.write("""This is an example \n
of writing text \n
on multiple lines \n
""")
```

这将生成以下输出：

![](img/c463eb9e158adac0790e6054ddb4cead.png)

使用 st.write 函数将文本拆分成多行。作者提供的图片。

第二个选项是在每行末尾添加两个空格。这在[markdown 语言](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)中常用于将文本移动到新行。

![](img/cc61e276241648ea9a97044daae96184.png)

在字符串末尾添加两个空格允许文本显示在多行上。注意每行末尾的两个淡淡的点表示两个空格。作者提供的图片。

这将生成类似于以下的多行文本输出。请注意，行间距比前面的例子要小得多。

![](img/257eb2f95a012cecbd09a48871f64b25.png)

使用 st.write 函数将文本拆分成多行。作者提供的图片。

# 使用 Markdown 语法与 st.write。

[Markdown](https://en.wikipedia.org/wiki/Markdown) 是一种流行的标记语言，允许作者在纯文本格式中简单轻松地应用格式。这是我最喜欢的写作方式之一，也被许多笔记应用程序使用，如[Obsidian](https://obsidian.md/)和[Notion](https://www.notion.so/)。

要在 `st.write()` 函数中使用 markdown，我们只需像这样输入：

```py
st.write('## Markdown')
st.write('Here is some more **markdown** text. *And here is some more in italics*')
```

第一行创建了一个二级标题（H2），第二行创建了一些格式化文本。在文本周围使用双星号（**）使其加粗，而单个星号（*）使其斜体。

当我们刷新应用程序时，我们会得到以下输出。

![](img/09a95433ccb5cbf7e4ec948f0d189366.png)

使用 markdown 语法与 Streamlit 的 st.write 函数的示例。作者提供的图片。

如果你想了解更多关于如何使用 markdown 写作的知识，那就查看[基本语法 Markdown 指南](https://www.markdownguide.org/basic-syntax/)。

Streamlit 使用的特定类型的 markdown 是[GitHub Flavoured](https://github.github.com/gfm/)，这是 markdown 的一个方言，具有自己的特点和细微差别。

# 使用 st.write 创建 LaTeX / KaTeX 格式的方程式。

[LaTeX](https://en.wikibooks.org/wiki/LaTeX) 已经存在很长时间（自 1984 年以来），是另一种用于排版文档并为打印准备的标记语言。除了排版，[它还可以用于准备和格式化数学方程式。](https://en.wikibooks.org/wiki/LaTeX/Mathematics)

[Streamlit 使用 KaTeX 的版本来使用 LaTeX 语法渲染方程式。](https://docs.streamlit.io/library/api-reference/text/st.latex) [KaTeX 本质上是一个用于渲染数学符号和方程式的 Javascript 库。](https://www.reddit.com/r/LaTeX/comments/n7zs8j/difference_between_latex_and_katex_also_asking/)

我们可以将方程式格式化，使其更易读，而不是将方程式作为线性文本——这样很难阅读。

要在 Streamlit 中显示 LaTeX 方程式，我们需要在方程式的开头和结尾各加上两个美元符号（$$）。

然后我们可以利用 LaTeX 的强大功能来书写我们的方程式。

```py
st.write("""Here is a simple equation using LaTeX
$$
ax² + bx + c
$$
""")

st.write(r"""And a slightly more complex equation
$$
Sw = \bigg({\frac{a \cdot Rw}{\phi^m \cdot Rt} \bigg)^\frac{1}{n}}
$$
""")
```

![](img/201a5d5d1b57443df385711024967df9.png)

使用 Streamlit 的 st.write 函数结合 KaTeX 的示例，显示简单和更复杂的数学方程式。作者提供的图片。

如果你不确定如何在 LaTeX 中做某事，这里有一些资源可以帮助你：

+   [使用 Codecogs 编辑器构建 LaTeX 方程式](https://latex.codecogs.com/eqneditor/editor.php)

+   [Overleaf 数学表达式参考指南](https://www.overleaf.com/learn/latex/Mathematical_expressions)

+   [Wikibooks LaTeX 数学指南](https://en.wikibooks.org/wiki/LaTeX/Mathematics)

Streamlit 使用的支持函数源自 KaTeX，并且可以在[这里](https://katex.org/docs/supported.html)找到所有支持的函数列表。

# 写代码块

如果我们正在创建一个应用程序或仪表板，我们想要显示代码供用户检查或学习，我们可以使用[markdown 语法创建代码块](https://www.markdownguide.org/extended-syntax/#fenced-code-blocks)。这是通过在我们想要显示的代码之前和之后使用三个反引号来实现的。

我们还可以提供一种语言来创建一些[语法高亮](https://www.markdownguide.org/extended-syntax/#fenced-code-blocks)。

```py
st.write("""
```python

如果我们想要显示代码：

    use_back_ticks == True

```py
""")
```

![](img/3c304816c87a01245093dd826e379169.png)

使用 st.write 和 markdown 语法显示的代码块。作者提供的图片。

# 使用 st.write 显示 Pandas 数据框

[Pandas 数据框](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html)是数据科学中常用的格式。它们可以用于存储、操作和创建新数据。

在构建 Streamlit 应用程序时，我们很可能会使用数据框来处理数据。st.write 函数可以通过传入数据框对象或相关方法（如 describe 方法）来显示交互式数据框。

```py
arr_data = np.random.default_rng().uniform(0, 100, size=(5,4))
df = pd.DataFrame(arr_data, columns=list('ABCD'))

st.write('Displaying the dataframe: `df`')
st.write(df)

st.write('Using the `describe` method:')
st.write(df.describe())
```

![](img/160812df93172b18bc5f507c2c2d776c.png)

Streamlit 应用程序显示了一个 pandas 数据框和 describe 方法。作者提供的图片。

# 结合文本和变量与 st.write

通过使用[f-strings](https://realpython.com/python-f-strings/)，我们可以将字符串和变量组合成单个输出。Streamlit 还进一步允许我们传入多个参数，然后将它们组合在一起。

在下面的示例中，我们使用 f-strings 来显示`a`和`b`的值，然后最后一行将文本与`a+b`的总和结合在一起。

```py
a = 3
b = 3

st.write(f'a is: {a}')
st.write(f'b is: {b}')
st.write('And if we sum them together we get:', a + b )
```

![](img/392546e7bb20e27a2d85efaf3d5f362e.png)

使用 Streamlit 的 st.write 函数将文本和变量组合在一起。作者提供的图片。

# 使用 st.write()显示图表图。

使用图表显示数据是数据科学和数据分析的基本部分。它使我们能够以读者易于理解和做出明智决策的方式传达数据。

Streamlit 处理多个绘图库，包括[matplotlib](https://matplotlib.org/)、[seaborn](https://seaborn.pydata.org/)、[altair](https://altair-viz.github.io/)和[plotly](https://plotly.com/)。

首先，我们需要有一个数据源，可以来自文件或现有数据框，其次，我们需要创建我们的图。在下面的示例中，我创建了一个简单的随机数据集，并选择了两列在散点图中显示。

然后将 matplotlib 图传递给 st.write 函数。

```py
# Creating a dataframe with random values in 5 columns
arr_data = np.random.default_rng().uniform(0, 100, size=(5,5))
df = pd.DataFrame(arr_data, columns=list('ABCDE'))

# Creating a simple figure
fig, ax = plt.subplots(1,1)
ax.scatter(x=df['A'], y=df['B'])
st.write(fig)
```

当我们运行上面的代码时，我们得到以下显示。

![](img/59c77504301a38e328c05e0b2fafb4d2.png)

使用 st.write()显示的 matplotlib 图。作者提供的图片。

# 结合表情符号

最后，我们可以通过使用表情符号为我们的文本增添一些趣味。这很容易，只需在表情符号名称前后加上冒号即可。

```py
st.header('Emojis')
st.write('Emojis can be used to add a bit of character 
to your text :smile: :grin: :thumbsup:')
```

![](img/2d38de6ed54b161a1d8113ab520000dc.png)

在 Streamlit 应用程序中使用 st.write()显示表情符号。作者提供的图片。

# 摘要

[S[treamlit st.write() function](https://docs.streamlit.io/library/api-reference/write-magic/st.write)]是一个强大的工具，可以以多种不同的方式使用，从编写普通文本到显示图表和方程式。它绝对名副其实地成为了 Streamlit 库的瑞士军刀功能。

一定要查看[Streamlit Magic](https://docs.streamlit.io/library/api-reference/write-magic/magic)上的文档，将您的应用程序提升到下一个水平。

*感谢阅读。在您离开之前，您绝对应该订阅我的内容，并将我的文章发送到您的收件箱。* [***您可以在这里做到！***](https://andymcdonaldgeo.medium.com/subscribe)*或者，您可以* [***订阅我的通讯***](https://fabulous-founder-2965.ck.page/2ca286e572) *免费获取额外的内容直接发送到您的收件箱。*

*其次，您可以通过注册会员来获得完整的 Medium 体验，并支持成千上万的其他作家和我。每月只需花费$5，您就可以完全访问所有精彩的 Medium 文章，以及有机会通过写作赚钱。*

*如果您使用* [***我的链接***](https://andymcdonaldgeo.medium.com/membership)***注册***，*您将直接支持我获得部分费用，而且不会增加您的费用。如果您这样做了，非常感谢您的支持。*
