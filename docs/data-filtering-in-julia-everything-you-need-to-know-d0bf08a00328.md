# Julia 中的数据过滤：你需要知道的一切

> 原文：[`towardsdatascience.com/data-filtering-in-julia-everything-you-need-to-know-d0bf08a00328`](https://towardsdatascience.com/data-filtering-in-julia-everything-you-need-to-know-d0bf08a00328)

## 当涉及到在 Julia 中进行数据过滤时，你需要知道的一切

[](https://emmaccode.medium.com/?source=post_page-----d0bf08a00328--------------------------------)![Emma Boudreau](https://emmaccode.medium.com/?source=post_page-----d0bf08a00328--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d0bf08a00328--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d0bf08a00328--------------------------------) [Emma Boudreau](https://emmaccode.medium.com/?source=post_page-----d0bf08a00328--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d0bf08a00328--------------------------------) ·阅读时间 7 分钟·2023 年 7 月 11 日

--

![](img/0d8be971b1ee16caf88f89d7affa2ca9.png)

图片由 [Najib Kalil](https://unsplash.com/@nkalil?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在数据科学中，进行假设检验、机器学习甚至分析时，获得结果的最重要因素是拥有良好的数据。数据通常需要满足多种不同的要求。数据过滤是一种非常常见且在数据领域中经常使用的技术。数据过滤可以是移除不属于的数据组件的过程，也可以是获取符合某些参数或多个参数的样本的过程。

我们可以说，当我们从数据中移除不属于该数据的内容时，比如从数据中去除缺失值，这就是一种数据过滤。这是数据科学过程中的一个重要步骤，通常通过过滤技术来完成。一个符合某些设定参数的样本的例子是，如果我们试图测试身高和撞头之间的统计显著性。我们会过滤掉所有身材矮小的数据，以便只留下高个子的数据进行测试。

这种技术有许多应用。过滤可能对于执行一些常见的数据科学任务也很重要，因此确实需要注意。幸运的是，在 Julia 中进行过滤相对简单。如果你想尝试本文中的代码，这里有一个以笔记本格式呈现的概述链接：

[](https://github.com/emmettgb/Emmetts-DS-NoteBooks/blob/master/Julia/data%20filtering%20in%20julia.ipynb?source=post_page-----d0bf08a00328--------------------------------) [## Emmetts-DS-NoteBooks/Julia/data filtering in julia.ipynb 在 master 分支 · emmettgb/Emmetts-DS-NoteBooks

### 各种项目的随机笔记本。通过创建帐户，参与 emmettgb/Emmetts-DS-NoteBooks 的开发……

github.com](https://github.com/emmettgb/Emmetts-DS-NoteBooks/blob/master/Julia/data%20filtering%20in%20julia.ipynb?source=post_page-----d0bf08a00328--------------------------------)

要真正理解 Julia 中的过滤，我们需要了解一些不同的内容。**匿名函数** 通常作为参数提供，尽管包括 `do` 语法在内的任何形式的函数作为参数都是可能的。在某些情况下，我们也可能使用一种叫做 `**BitArray**` 的 `Vector` 类型。

## BitArray

`BitArray` 是一个只包含 `Bool` 类型值的 `Vector`。在 Julia 中，`Array` 只是 `Vector` 的别名。通常在数学中，我们认为 `Vector` 特指一维数组……通常是一列特征。在 `BitArray` 的情况下，我们应该注意到它也可能被称为 `BitVector`。这是大多数按位布尔操作符在使用 `.` 进行广播时返回的结果，例如 `.==`：

```py
julia> y = [5, 10] .== [5, 10]
2-element BitVector:
 1
 1

julia> BitArray
BitArray

julia> BitVector
BitVector (alias for BitArray{1})
```

尽管这个类型与 `Vector{Bool}` 非常相似，但重要的是要记住，这种类型确实存在，因为它将是从操作符中返回的结果，例如。

## 匿名函数

在 Julia 中，匿名函数起初可能看起来有些难以理解，但一旦掌握了语法，它们相对简单。如果你想要阅读有关 Julia 中匿名函数的更深入概述，这里有一篇我写的文章：

[](/what-on-earth-is-an-anonymous-function-f8043eb845f3?source=post_page-----d0bf08a00328--------------------------------) ## 什么是匿名函数？

### 揭示 Julia 中匿名函数和类型的方方面面

towardsdatascience.com

匿名函数与普通函数类似，只是函数没有在某个类型下定义。让我通过进一步解释 Julia 函数来使这个概念更清晰。每当我们在 Julia 中正常创建一个函数时，这个函数会被定义在一个名称下，并具有一个类型。这是一个单例，这个类型拥有与该方法类型（`typeof`）注册的方法。

```py
function myfunc()

end
```

```py
typeof(myfunc)

typeof(myfunc) (singleton type of function myfunc, subtype of Function)
```

我们可以使用 `methods` 方法来访问新函数的方法。

```py
methods(myfunc)

# 1 method for generic function myfunc from [35mMain[39m:myfunc() in Main at In[4]:1
```

每当我们创建一个匿名函数时，函数名称会变得未定义——出于这个原因，我们只能为匿名函数拥有一个方法。如果我们只想让一个函数快速完成某事，例如过滤，这一点非常有用。要编写匿名函数，我们使用逻辑右操作符。在很多方面，这就像我们将正常函数声明中的 `=` 替换了一样。

```py
f = x -> x += 5

f2(x) = x += 5
```

匿名函数的参数放在操作符的左侧，函数本身放在右侧。我们可以使用注释来使这一点更加清晰：

```py
x::Int64 -> 5 +  x
```

Julia 的一个伟大之处在于语言的可扩展性。借助 Julia，所有模块都可以利用 `Base` 模块提供的函数来添加新方法。换句话说，模块可以无缝地与 `Base` 结合，并且通常像 `Base` 类型一样被处理。这意味着如果我们学习了这些方法如何与 `Base` 一起工作，我们可能会将很多这方面的知识带到其他模块中。今天我们将通过从 `Base` 开始，然后扩展到来自 `DataFrames` 的 `DataFrame` 过滤不同结构，来演示这一点。

## 过滤基本类型

有几种不同的技术可以用来过滤简单的 `Vector`。我认为 Julia 中相对较新的一个特性是能够提供条件掩码作为索引。我不确定这个功能在 `Base` 中已经存在多久了，但这无疑是一个很棒的特性，因为我喜欢条件掩码。要创建条件掩码，我们需要制作一个之前提到的 `BitArrays`。在这种情况下，我们将再次广播一个比较操作符。在这里，我们将过滤出 x 中大于 14 的任何值：

```py
x = [5, 10, 15, 20]
```

```py
xmask = x .< 14
```

```py
x[xmask]
2-element Vector{Int64}:
  5
 10
```

另外，我们可以利用 `filter` 方法。这些方法有 `filter` 和 `filter!`。这两个方法做完全相同的事情，唯一的区别是 `filter!` 是一个变异方法。这正是函数名中的 `!` 所代表的含义。我觉得这是一个非常酷的标准，因为它确实使我们更容易区分何时数据被变异，何时没有。我认为这是一个非常值得了解的事情，特别是在数据科学方面。`filter` 方法提供了一个 `Function` 作为第一个位置参数，然后是我们的 `Vector` 作为第二个位置参数。如果类型不是 `Vector`，这种情况可能会有所变化，所以请记住这一点。

```py
filter(x::Int64 -> x < 14, x)

2-element Vector{Int64}:
  5
 10
```

由于我们在这里使用的是 `filter` 而不是 `filter!`，我们需要将 `x` 设置为返回值以强制执行这些更改。我们还可以使用这种技术过滤字典。与其提供 `Vector` 中每个元素的类型，不如使用 `Pair`。

```py
mydict = Dict(:A => [5, 10], :B => [4, 10])

filter(k::Pair{Symbol, Vector{Int64}} -> k[2][1] != 5, mydict)

Dict{Symbol, Vector{Int64}} with 1 entry:
  :B => [4, 10]
```

因为函数是第一个位置参数，这也开启了利用 `do` 语法的能力，所以一定要记住这一点。

```py
x = [5, 10, nothing, nothing, 40]

filter!(x) do number
    ~(isnothing(number))
end

3-element Vector{Union{Nothing, Int64}}:
  5
 10
 40
```

## 过滤数据框

另一种可能需要过滤的常见结构是`DataFrame`。这有点不同，因为它是一个依赖项和模块，而不仅仅是`Base`的一部分。

```py
using DataFrames

df = DataFrame(:X => [1, 2, 3, 4], :Y => [1, 2, 3, 4])
```

当在`DataFrame`上使用`filter`方法时，它会向函数提供一个`DataFrameRow`。这是一个很酷的类型，我们可以很容易地对其进行索引，这使得过滤变得轻而易举。

```py
filter!(df) do row
    if row[:X] > 3
        return(false)
    end
    true
end
```

这确实就是全部内容，结合`Base`中的现有知识，可能很难找到无法用这种技术过滤的内容！

## 总结

数据过滤是处理数据过程中的一个重要步骤。过滤技术使我们能够移除可能妨碍任何实际建模分析的观察数据。幸运的是，作为科学计算语言的 Julia 考虑了这一点，语言提供的`Base`工具对此效果很好！此外，Julia 的构建方式允许在`Base`中包含的函数添加额外的方法以增加功能。

这创造了我最喜欢的 Julia 特点之一。在编程世界中，熟悉某个生态系统或一组包是很常见的。例如，知道如何使用 NumPy 并不意味着知道如何使用 Dask。在 Julia 中情况略有不同，因为完全可能 Dask 或 NumPy 的等效方法会使用相同的方法。我们在`DataFrames`上的`filter`应用中可以看到这一点。这有助于节省单例的成本，也帮助包的用户重用旧知识。不用多说，Julia 是一个处理数据的出色语言——数据过滤的应用也不例外。
