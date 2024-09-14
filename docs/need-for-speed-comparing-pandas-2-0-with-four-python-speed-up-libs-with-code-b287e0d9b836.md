# 需要速度：将 Pandas 2.0 与四个 Python 加速库进行比较（附代码）

> 原文：[`towardsdatascience.com/need-for-speed-comparing-pandas-2-0-with-four-python-speed-up-libs-with-code-b287e0d9b836`](https://towardsdatascience.com/need-for-speed-comparing-pandas-2-0-with-four-python-speed-up-libs-with-code-b287e0d9b836)

## *Polars*、*Dask*、*RAPIDS.ai cuDF* 和 *Numba* 与使用 *pyarrow* 的 *Pandas 2.0* 在后台处理、向量化以及 *itertuples()*、*apply()* 方法上进行了比较。

[](https://theomitsa.medium.com/?source=post_page-----b287e0d9b836--------------------------------)![Theophano Mitsa 博士](https://theomitsa.medium.com/?source=post_page-----b287e0d9b836--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b287e0d9b836--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b287e0d9b836--------------------------------) [Theophano Mitsa 博士](https://theomitsa.medium.com/?source=post_page-----b287e0d9b836--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b287e0d9b836--------------------------------) ·阅读时间 9 分钟·2023 年 5 月 19 日

--

![](img/3e9e1405f4a9150be078fd1dfff4beda.png)

图片由 alan9187 提供，来源于 Pixabay

到 2023 年为止，这一年给机器学习领域带来了显著的进展，先进的*大语言模型*（*LLMs*）得到了开发和全球分布。然而，机器学习的专业能力不仅限于对*LLLMs*的微调和提示工程。一位机器学习专家了解系统内部的运作，能够解释/优化各种系统行为，并最终负责机器学习解决方案的整体质量、业务需求适配性和性能。

> 除了性能比较，文章还旨在通过提供代码示例来教育读者如何实现各种 Python 加速操作，突出关键点。

说到了解系统内部运作和性能，本文将对比新发布的 *Pandas 2.0* 与四个其他加速 Python 库和操作：*Polars*、*RAPIDS.ai cuDF*、*Dask* 和 *Numba*。代码在 *ASUS Rig Strix Scar*（2022 款）游戏笔记本电脑上执行，配置为 *NVIDIA GeForce RTX 3080 Ti Laptop GPU* 和 *Intel Core i9 12900Hz* 处理器。除了性能比较，文章还旨在通过提供代码示例来教育读者如何实现各种 Python 加速操作，突出关键点。

在我们的代码示例中，我们将使用一个合成的*.csv*文件，该文件包含 500K 行，模拟一个超级商店的两地商品价格。文件有三列；第一列，称为*Store1*，包含第一个超级商店位置的库存价格，第二列，称为*Store2*，包含第二个超级商店位置的库存价格。*Store2*的价格略高（最高可高 20%）。最后，第三列，称为*Discountability*，仅包含 0 和 1，指示一个项目是否可以打折。0 表示该项目不能打折，1 表示可以打折。创建*Pandas 2.0 DataFrame*和合成*.csv*文件的代码如下。

***关键点***：注意到*col2*（*Store2*的价格）是通过使用矢量化操作从*numpy*数组*col1*和*var*创建的，而无需使用循环。我们之所以能这样做，是因为 N*umPy*支持矢量化操作。

# A. Dataframe 创建性能比较

我们将比较不同方法性能的第一个操作是*DataFrame*的创建。具体来说，我们将比较*Pandas 2.0*、*Dask*和*Polars*的性能。在接下来的两个部分中，我们将讨论代码实现，然后在 A.3 节中，我们将比较性能。

## **A.1\. 使用 Pandas 2.0**

*Pandas 2.0*中的一个重要新特性是添加了 A*pache Arrow (pyarrow)*支持的内存格式。*Apache Arrow*的主要优点是它可以执行更快、更节省内存的操作。下面的代码展示了使用和不使用*pyarrow*构建*Pandas DataFrame*的方式。

***关键点：*** 注意，一旦我们将后端类型(*dtype_backend*)指定为*pyarrow*，我们还需要将*engine*指定为*pyarrow*。

## A.2 使用 Polars 和 Dask

*Polars*是一个加速的 Python *DataFrame*库，实施了*Apache Arrow*内存模型，并用*Rust*编写。与*Apache Arrow*类似，它既快速又内存高效，特别适合数据密集型计算。为了创建*Polars* *DataFrame*，我们只需导入*Polars*库并调用其*read_csv()*方法。

*Dask*是一个开源框架，支持 Python 中的并行和分布式计算，因此特别适合数据密集型应用程序。为了创建一个*Dask* *DataFrame*，我们必须首先导入*dask.dataframe*库，然后调用其*read_csv()*方法。

**关键点。** 安装*Dask*库时要小心，因为*Pandas*是其依赖项之一，它会安装它。在撰写本文时，*Dask*的安装不会安装*Pandas 2.0*，而是一个较旧的版本。因此，如果你想使用*Dask*而不覆盖你的*Pandas 2.0*安装，请创建一个虚拟环境并在其中安装*Dask*。

## A.3 Dataframe 创建执行时间比较

操作时间使用*timeit*库进行实现，如下所示。

下方展示了从最快到最慢的*DataFrame*创建执行时间。最快的是*Polars*和*Dask*，紧随其后的是具有*pyarrow*后端的*Pandas 2.0*。至于传统的*Pandas DataFrame*创建，它比前者慢 4.37 倍。因此，即使从创建*Pandas DataFrame*的第一步开始，*pyarrow*也能发挥作用。

+   使用*Dask*：0.020656200009398162

+   使用*Polars*：0.027874399966094643

+   使用*pyarrow*支持的*Pandas* 2.0：0.04491699999198317

+   使用*Pandas* 2.0（不使用*pyarrow*）：0.196299700008239

除了测量执行时间，我还进行了一些内存使用的估算。*cuDF DataFrame*的内存占用最小（0.12MB），而*Polars DataFrame*的内存占用约为 7.63MB，*Pandas DataFrame*的内存占用约为 11.44MB。注意，*cuDF DataFrame*的创建时间未提供，因为*.csv*文件存储在 CPU 中，因此我们无法利用*cuDF*的 GPU 能力进行此操作。我们将在文章后面讨论这一点。

# B. *DataFrame*操作的性能比较

为了比较上述库在执行*DataFrame*操作时的性能，假设如下情况：所有*Store1*的合格商品将折扣 20%，所有*Store2*的合格商品将折扣 30%，折扣后的价格将保存在一个新的数据框中。我们使用“合格”这个词，因为如上所述，*Discountability*为 0 的项目不能享受折扣。因此，我们将对应用函数到*DataFrame*的行上进行执行时间比较，这是一个常见任务。

代码实现展示在 B.1-B.8 节，性能比较展示在 B.9 节。

## B.1 使用 pyarrow 的 Pandas

在我们对*DataFrame*操作的第一次实验中，我们将利用*Apache Arrow*的能力，鉴于它最近与*Pandas 2.0*的互操作性。如下面代码的第一行所示，我们将一个*Pandas DataFrame*转换为*pyarrow Table*，这是一种在内存中高效表示列数据的方式。每一列单独存储，这允许高效的压缩和数据查询。然后，将*Store1*、*Store2*和*Discountability*列传递给*scale_columns()*函数，该函数通过适当的折扣（0.2 或 0.3）和*Discountability*列的掩码值（0 或 1）来缩放这些列。缩放后的列作为元组由函数返回。最后，将表*result_table*转换为*Pandas DataFrame*。

***关键点：** 在函数*scale_columns()*中，乘法操作是通过*pyarrow.compute* 函数*multiply()* 实现的。注意*每一个*乘法操作都必须使用*multiply()* 实现。例如，如果我们将上面的*pc.multiply(0.2,mask_col)* 替换为*pc.multiply(0.2*mask_col)*，我们将会遇到错误。最后，我们使用*pyarrow.compute* 的*subtract()*函数进行减法。

**B2. 使用 Pandas 方法 apply()**

我们的第二个实验将使用*Pandas DataFrame*的*apply()*方法来执行逐行操作。函数*scale_columns()*在下面的代码中执行缩放。请注意，实际的缩放是在嵌套函数*discount_store()*中完成的。

**关键点：** 嵌套函数返回一个包含字典的*Pandas Series*，其中*key*是列名，*value*是缩放后的商店价格。你可能会好奇为什么我们要返回这种类型的对象。原因是，*Pandas apply()* 方法返回一个*Series*，其索引是列名。因此，将结构类似于其返回值的对象传递给*apply()*有助于计算。为了教育目的，在我的代码的*github*目录中（文章末尾有链接），我提供了两个使用*apply()*的实现。这里展示的是其中一个，还有一个实现，其中嵌套函数将缩放值作为元组返回。这个实现计算上更为密集，因为返回的类型与*apply()*需要返回的结构不同。

## B.3. 使用 Pandas itertuples()

*itertuples()* 是一个快速的*Pandas* 行*迭代器*，生成*namedtuples*（列名，相应行的值）。实现商店价格缩放并使用*itertuples()* 计算折扣价格的代码如下所示。

**关键点：** *Pandas itertuples()* 方法通常比 *Pandas apply()* 更快，特别是对于较大的数据集，就像我们的代码示例一样。原因是*apply()*为每一行调用一个函数，而*itertuples()*则不会，并且可以利用向量化操作，同时返回一个轻量级迭代器，这不会创建新对象，且内存效率高。

## **B.4 Pandas 向量化操作**

现在，我们已经到了计算折扣商店价格的最优雅和最快的方法：向量化！这是一种允许对整个数组一次性应用所需操作的方法，而不是使用循环进行迭代。实现如下所示。

**B.5 使用 Numba**

*Numba* 是一个即时（*JIT*）编译器，用于将 Python 代码在**运行时**翻译成优化的机器代码。它在优化涉及循环和*NumPy* 数组的操作中非常有用。

*@numba.njit* 符号是一个*装饰器*，它告诉*Numba* 随后的函数要编译成机器代码。

## B.6 使用 Dask

类似于*NumPy*，*Dask*提供了向量化操作，额外的优势是这些操作可以以并行和分布的方式应用。

*Dask*的额外优势包括：(a) 懒惰计算，即*Dask*数组和操作建立在任务图上，仅在请求结果时执行，例如调用*compute()*。(b) 超出内存处理，即可以处理不适合内存的数据集。(c) 与*Pandas*的集成。

## B.7 使用 Polars

*Polars*提供了利用*Rust*语言的速度和内存效率的向量化操作。类似于*Dask*，它提供了懒惰计算、超出内存处理和与*Pandas*的集成。实现如下所示。

## B.8 使用 RAPIDS.ai cuDF

最后，我们使用*cuDF*通过向量化操作实现了价格折扣功能。*cuDF*库建立在*CUDA*之上，因此其向量化操作利用了 GPU 的计算能力以加快处理速度。*cuDF*的一个方便功能是提供*CUDA* *kernels*。这些是优化的函数，利用了 GPU 的并行特性。*cuDF*的实现如下所示。

## B.9 函数应用的执行时间比较

类似于*DataFrame*的创建时间，*timeit*被用来测量价格折扣功能的执行时间及其在新*DataFrame*中的节省。下面是执行时间，按从最快到最慢排序。

+   使用*RAPIDS.ai, cuDF:* 0.008894977000011295

+   使用*Polars:* 0.009557300014421344

+   使用*pyarrow Table*: 0.028865800006315112

+   使用*Pandas 2.0*向量化操作: 0.0536642000079155

+   使用*Dask:* 0.6413181999814697

+   使用*Numba:* 0.9497981000000095

+   使用*Pandas 2.0 itertuples()*: 1.0882243000087328

+   使用*Pandas 2.0 apply()*: 197.63155489997007

不出所料，支持 GPU 的*Rapids.ai cuDF*取得了最快的时间，其次是闪电般快速的*Polars*库。*pyarrow Table*和*Pandas 2.0*的向量化操作的执行时间也紧随其后；*cuDF*的执行时间平均比后两者快约 4.64 倍。*Dask*、*Numba*和*Pandas*的*itertuples()*方法的表现较差但仍合理（分别比*Polars*慢约 72、107 和 122 倍）。最后，*Pandas*的*apply()*相对于所有其他方法表现极其*差*，这也不令人惊讶，因为此方法在处理大型数据集时以逐行方式操作，速度相对较慢。

**关键点**：（a）一般来说，*apply()*是对小到中型数据集上的*Pandas DataFrames*应用函数的优秀方法。但是，当处理大型数据集时，如我们所例，最好考虑其他实现函数的方法。（b）如果你决定在*Pandas 2.0*中使用*apply()*，请确保这在标准创建的*DataFrame*上进行，而不是在后台使用*pyarrow*的。原因是，显著的数据类型转换开销会大大减慢你的计算速度。

# **C. 结论**

在这篇文章中，我描述了几种加速应用于大型数据集的 Python 代码的方法，特别关注新发布的*Pandas 2.0*和*pyarrow*后端。不同的加速技术在两个任务中进行了性能比较：（a）*DataFrame*创建和（b）对*DataFrame*行应用函数。**对于*Pandas 2.0*用户来说，确实是好消息；** *Pandas DataFrame*的向量化操作和*pyarrow Table*（从*Pandas DataFrame*中提取）实现了**与*Rapids.ai cuDF*和*Polars*库相当的性能**。

整个代码在*github*目录下：[link](https://github.com/theomitsa/Pandas2.0-Performance-comparison)

感谢阅读！
