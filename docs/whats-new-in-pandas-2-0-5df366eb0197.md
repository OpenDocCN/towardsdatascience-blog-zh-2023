# Pandas 2.0 有什么新变化？

> 原文：[`towardsdatascience.com/whats-new-in-pandas-2-0-5df366eb0197`](https://towardsdatascience.com/whats-new-in-pandas-2-0-5df366eb0197)

## 关于这次重大发布的五件事

[](https://jeffhale.medium.com/?source=post_page-----5df366eb0197--------------------------------)![Jeff Hale](https://jeffhale.medium.com/?source=post_page-----5df366eb0197--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5df366eb0197--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5df366eb0197--------------------------------) [Jeff Hale](https://jeffhale.medium.com/?source=post_page-----5df366eb0197--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5df366eb0197--------------------------------) ·阅读时间 5 分钟·2023 年 4 月 10 日

--

Pandas 2.0 于 2023 年 4 月 3 日正式发布。让我们看看哪些功能比阳光下的柯基还要炙手可热。☀️

![](img/64c4f87ef9ed00d3cb56323cded1e96c.png)

来源：pixabay.com

三年前，我写了 Pandas 1.0 有什么新变化。经历了一场大流行和一系列 AI 进展之后，我们迎来了 pandas 2.0。

Pandas 是标准的、脑力友好的 Python 数据处理库。2.0 更新的重点是让 pandas 更快、更节省内存。内存是人们需要离开 pandas 转向 Dask、Ray、SQL 数据库、Spark DataFrames 和其他工具的主要原因。减少内存使用会使 pandas 的使用变得更加轻松。🙂

正如你可能期望的那样，作为一个主要版本，pandas 2.0 有许多显著的变化。让我们深入了解一下！

## pyarrow *🐍➡️*

如果用一个词来总结这次发布，那就是 *pyarrow*。

Pandas 是使用 NumPy 数据结构进行内存管理构建的。现在，你可以选择使用 [pyarrow](https://arrow.apache.org/docs/python/index.html) 作为你的后备内存格式。

使用 pyarrow 意味着你可以加快速度并提高内存使用效率，因为你可以利用 [Arrow](https://arrow.apache.org/docs/cpp/index.html) 的 C++ 实现。有趣的是，pandas 的创始人 [Wes McKinney](https://en.wikipedia.org/wiki/Wes_McKinney) 在 2009 年开源 pandas 后，于 2016 年开始参与 Arrow 的开发。

Arrow 是什么？ [Sebastian Raschk](https://twitter.com/rasbt) 解释道，它是“…一种开源且与语言无关的列式数据格式，用于在内存中表示数据。它可以实现进程间的数据零拷贝共享。”

列式数据存储将数据列组装在内存中，从而使得在诸如计算列均值等任务上操作更快。Arrow 数据类型还包含了如空值等有用的概念，如上所述。

使用 pyarrow 的 pandas 比其他 pandas 替代方案更快吗？在一些任务上，[Sebastian Raschka 显示 pandas 可能比](https://twitter.com/rasbt/status/1632090412117532672) [polars](https://github.com/pola-rs/polars) 更快，polars 是一个相对较新的“Rust 和 Python 的 lightning-fast DataFrame 库”，也使用 Arrow。

然后，polars 库的作者 Ritchie Vink 对 [TPCH-10 基准测试](https://docs.deistercloud.com/content/Databases.30/TPCH%20Benchmark.90/index.xml?embedded=true) 进行了比较测试，这是一个更大、更相关的实际数据测试。请查看以下一些结果和讨论：

来源：[`twitter.com/RitchieVink/status/1632334005264580608`](https://twitter.com/RitchieVink/status/1632334005264580608)

看起来 pandas 可能还有一段路要走，如果你还没有尝试过，polars 可能值得一试。

如果你觉得需要速度，[Numba](https://pandas.pydata.org/docs/user_guide/enhancingperf.html#pandas-numba-engine) 是另一个选项。在任何情况下，避免过早优化的普遍建议是合理的。

以下是使用 pyrarrow 后端格式读取 CSV 文件的代码：

```py
pd.read_csv(my_file, dytpe_backend='pyarrow')
```

如果你想深入了解，Marc Garcia 有一篇[不错的文章](https://datapythonista.me/blog/pandas-20-and-the-arrow-revolution-part-i)，介绍了 pandas 内存结构的历史以及 pandas 2.0 中对 pyarrow 的支持（注意，在预发布版本和正式发布之间语法有所更新）。

## 从一开始就支持可空数据类型

在 pandas 中处理缺失（空）值并不总是那么容易。Pandas 是建立在 NumPy 上的，而 NumPy 对某些数据类型不支持空值。

例如，NumPy 整数数据类型无法支持空值。在整数列中引入空值会导致该列自动转换为浮点数据类型。一分钟列是整数，下一分钟变成浮点数。这至少可以说是不理想的。

Pandas 1.0 为我们提供了可空的数据类型，但使用起来需要一些工作。现在，当你将数据读取到 DataFrame 中时，你可以指定你想要可空的 NumPy 支持的数据类型，如下所示：

```py
pd.read_csv(my_file, dtype_backend="numpy_nullable")
```

更新：注意，自 2.0 beta 版本以来，这种语法已被更新。

注意，大多数，但不是所有的 `read_xxx` 函数都支持可空的数据类型。更多信息请见[这里](https://pandas.pydata.org/docs/whatsnew/v2.0.0.html#argument-dtype-backend-to-return-pyarrow-backed-or-numpy-backed-nullable-dtypes)。

`pyarrow` 数据类型从一开始就提供了空值支持。

## 更多 NumPy 数据类型用于索引

现在，你可以为 pandas 索引选择更多的 NumPy 数据类型以减少内存使用。现在你可以选择更低内存的数据类型作为索引。例如，你可以指定索引使用 32 位整数，从而节省之前只能使用 64 位整数时的一半内存。

```py
pd.Index([1, 2, 3], dtype=np.int32)
```

阅读更多[这里](https://pandas.pydata.org/pandas-docs/version/2.0/whatsnew/v2.0.0.html#index-can-now-hold-numpy-numeric-dtypes)。

## 复制-on-写模式

Pandas 变得懒惰了。以一种好的方式。 🙂 大量 DataFrame 和 Series 方法在需要之前不会再创建 pandas 对象的副本。例如，`df.head()` 不会创建一个包含前五行的新 DataFrame，而只是返回前五行的视图。

这些变化将节省时间和内存。只需使用：

```py
pd.set_option("mode.copy_on_write", True)
```

你可以通过几种其他方式设置这个行为，因为 pandas 喜欢给你多种方法来完成任务。 ❤️

## 可安装的额外功能

现在你可以在安装 pandas 时同时安装 pandas 需要的额外 Python 库。例如，运行以下命令将为你提供处理 Google Cloud Platform 和 Parquet 文件格式所需的库。

```py
pip install "pandas[gcp, parquet]==2.0.0"
```

⚠️ 确保不要忘记引号！

这里是可用的安装选项：

+   all (所有附加功能)

+   performance

+   computation

+   timezone

+   fss

+   aws

+   gcp

+   excel

+   parquet

+   feather

+   hdf5

+   spss

+   postgresql

+   mysql

+   sql-other

+   html

+   xml

+   plot

+   output_formatting

+   clipboard

+   compression

+   test

了解安装的依赖项请访问 [这里](https://pandas.pydata.org/docs/getting_started/install.html#install-dependencies)。

# Upgrading

要在你的虚拟环境中从 PyPI 安装 pandas 2.0，请使用以下命令进行升级：

```py
pip install -U pandas
```

现在你已经开始更快、更节省内存的数据处理之旅了！ 🎉

# Wrap

你已经看到 pandas 2.0 的最大变化了。还有其他变化，比如 `read_csv` 和类似函数的新日期参数名称。 [查看发布帖子了解所有细节](https://pandas.pydata.org/pandas-docs/version/2.0/whatsnew/v2.0.0.html#configuration-option-mode-dtype-backend-to-return-pyarrow-backed-dtypes)。

即使有了这些新变化，当你的 pandas DataFrame 没有足够的内存时也会出现问题。针对这种情况，我在 这份指南 中分享了八个帮助你应对的技巧。

如果你是 pandas 新手，可以查看我的入门书籍 [Memorable Pandas](https://memorablepandas.com/)。

希望你觉得这份 pandas 2.0 变化指南对你有帮助。如果有，请分享给其他人，让他们也能找到它！ 🚀

我写关于 Python、数据和其他技术话题的文章。如果你对这些感兴趣，*跟随我* [follow me](https://jeffhale.medium.com/)，这样你就不会错过更新！

![](img/7a28dc434ef34199ac66bccca51578a1.png)

来源：pixabay.com

编程愉快！
